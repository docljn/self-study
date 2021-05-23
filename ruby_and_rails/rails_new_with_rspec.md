# Setting up a new Rails app with RSpec & FactoryBot

1. create a rails app without a test framework
  - by default, Rails will create a new app with the MiniTest test framework

  `rails new app_name -T`

2. create the (empty) database

  `cd app_name`
  `rake db:create`

3. install the three gems needed in order for RSpec to work
  - rspec-rails, capybara and webdrivers
  - also be installing factory-bot and faker (pretty much essential for Rails testing) 
  - note that Rails 6 requires RSpec 4.0 to avoid the following error `ActionView::Template::Error: wrong number of arguments (given 2, expected 1)`
  - this is done in two steps
    - first modify the `Gemfile` by adding to the following groups

    ```ruby
    # Gemfile

    # ...
    group :development, :test do
      %w[rspec-core rspec-expectations rspec-mocks rspec-rails rspec-support].each do |lib|
        gem lib, git: "https://github.com/rspec/#{lib}.git", branch: 'main' # Previously '4-0-dev' or '4-0-maintenance' branch
      end
      gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
    end

    group :test do
      gem 'capybara'
      gem 'webdrivers', '~> 4.0', require: false
      gem 'factory_bot_rails'
    end
    ```

    - then run `bundle install` to install the gems

4. install RSpec in your rails app

  `rails generate rspec:install`

5. now when you generate using the `scaffold` command RSpec tests will automatically be generated

  `rails generate scaffold ObjectName title:string url:string`

6. run the tests using

  `bundle exec rspec path_to_desired_spec_file.rb`

7. in keeping with Rails' convention over configuration, follow the directory structure recommended by [RSpec](https://relishapp.com/rspec/rspec-rails/docs/directory-structure)

## References

- https://relishapp.com/rspec/rspec-rails/docs/directory-structure
- https://rubyyagi.com/intro-rspec-capybara-testing/
- https://www.matthewhoelter.com/2019/09/12/setting-up-and-testing-rails-6.0-with-rspec-factorybot-and-devise.html

## further reading

- https://longliveruby.com/articles/introduction-to-rspec
- https://www.microverse.org/blog/understanding-rspec-controller-request-specs-integration-test-using-capybara