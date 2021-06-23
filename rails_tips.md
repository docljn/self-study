# Rails tips for new developers

https://www.youtube.com/watch?v=BZbW8FAvf00

## When you start a new Rails project

1. make sample files for reference by future developers
    - database schema

    ```bash
    cp config/database.yml config/database.sample.yml
    echo "config/database.yml" >> .gitignore
    ```

    - secrets

    ```bash
    cp config/secrets.yml config/secrets.sample.yml
    echo "config/secrets.yml" >> .gitignore
    ```

## Modularise your config files

- create `config/payments.yml

    ```yaml
    development:
        stripe_key: ABCXYZ

    test:
        stripe_key: ABCXYZ

    production:
        stripe_key: <%= ENV['STRIPE_KEY'] %>
    ```

- access the data

    ```ruby
    Rails.application.config_for(:payments)['stripe_key']
    ```

## Use constants in place of strings and integers

- give yourself one place to change a string/integer
- instead of

```ruby
if registered_at < 30.minutes.ago
    #...
end
```

- use

```ruby
REGISTRATION_LIMIT = 30

if registered_at < REGISTRATION_LIMIT.minutes.ago
    #...
end
```

- if it's likely to change often, move that constant into a config file

```ruby
if registered_at < Rails.application.config_fir(:registration)['limit'].minutes.ago
    #...
end
```

## Databases

1. Do NOT .gitignore db/schema.rb
    - you need to able to run `rails db:setup` and `rails db:reset` to restore your database

2. Don't change data inside a migration
    - you add a column to an existing `companies` table
        - you want to fill it with data...
        - you grab all the Company entries as objects and interate through them to populate the new columns
    - stop!
        - what if the model changes
        - what if you rerun all the migrations when you have 3,000,000 entries in the table
        - what if a migration fails partway through due to e.g. a nil entry and you end up with some entries with the new data and some without
    - try
        - sql instead of ruby to do the update
            - faster
            - safer
        - use `gem 'rails-data-migrations'`
            - allows for data migrations separate from the standard type
            - first run database migrations
            - then run data migrations

## Background jobs

1. Don't pass objects to background jobs, pass ids instead
    - passing YAML-encoded objects around is risky due to the possibility of special characters causing errors

2. Always deliver your email later
    - if you expect email.send to be part of checkout, then an smtp failure means no checkouts...

## Be Restful all the time

1. Use only the standard verbs
    - index, show, edit, new, update, create, delete

2. If you want a new verb, create a new controller
    - instead of `ProductController::deactivate`
    - try ProductStatesController::update

## Useful gems

1. rubocop
    - <https://github.com/rubocop-hq/rubocop>
    - <https://docs.rubocop.org/en/stable/>
    - you can configure rubocop to ignore known errors

        ```bash
        rubocop --auto-gen-config
        ```

        - creates a `.rubocop_todo.yml` file
    - in your `.rubocop.yml` file add

        ```yml
        inherit_from: .rubucop_todo.yml
        ```

        - rubocop with then only highlight new errors
    - you can also set line length, method length, and files to exclude
    - to run rubocop before you push code create `.git/hooks/pre-push` file

        ```bash
        #!/bin/bash
        rubocop
        ```

        - if it fails, the push will be cancelled

2. annotate
    - <https://github.com/ctran/annotate_models>
    - after the migrations have been run
    - run `annotate`
        - each model, unit test, and factory class will be automatically annotated with information about the scheme
    - run `annotate routes`
        - does what it says on the tin
        - means you have all the names there for autocomplete (!)

3. bullet
   - <https://github.com/flyerhzm/bullet>
   - highlights the N+1 problem as it happens
   - will help you improve application performance by reducing the number of queries made
   - you may need to disable your browser cache for bullet to work properly

    ```ruby
    config.after_initialize do
        Bullet.enable = true
        Bullet.console = true
        Bullet.rails_logger = true
        Bullet.add_footer = true
    end
    ```

4. oink
   - <https://github.com/noahd1/oink>
   - detects memory leaks so you don't have to keep restarting the server
   - can only parse logs in the Hodel3000Logger format
     - <https://github.com/topfunky/hodel_3000_compliant_logger>
   - adds information at the bottom of the log
     - memory usage
     - number of objects instantiated in the request
     - can be used in production short-term

## Weird Bear Traps

- (where User.distinct) will output all users, and none of the following where filters, etc. get hit?