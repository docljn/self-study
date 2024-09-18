# Naming things well is important - so get good at it

Software development is about discovering and encoding knowledge. One of the best ways that humans have for doing this is to name things and concepts.  However, the chance of two people selecting the same name for a new idea or object is less than 10%!  Natural language is imprecise. Code must be precise, but must also be human-comprehensible if developers are to read and modify it, and users are to receive helpful error messages.

To make shared understanding more likely, it's helpful to work with terms that are understood by technical and non-technical users, developers, sales teams, etc. Part of that is agreeing on a shared vocabulary, ideally with a glossary which is accessible to everyone. This is particularly important when developers will be working on a codebase over many years, or if the developers come from widely differing cultures, or have different first languages. All of these things increase the risk of misunderstanding, or even the  risk that an abbreviated name may be offensive in another language or culture. [See: I got fired over a variable name...](https://www.reddit.com/r/cscareerquestions/comments/dpcfns/i_got_fired_over_a_variable_name/)

## Convention Over Configuration

The fewer decisions you need to make, the more capacity you have for making good decisions when it really counts. This is one reason why linters and autoformatters, particularly those which come with generally accepted defaults, are so popular. Historically, linters have not be able to do much more than enforce conventions such as snake vs camel case, or no-numbers-in-names Large Language Models which integrate with code editors can be helpful, and are improving, so don't discount these tools completely when you are struggling to select a meaningful descriptor for a new abstraction.

### To reduce inconsequential decisions

1. Learn the conventions of your language(s) and framework(s) and agree to follow them as a team
2. Learn and understand the vocabulary of your domain, particularly as used by your customers/clients and sales people.
3. Agree on and install a linter (Rubocop, Prettier, MarkdownLint, etc) and make CI passing conditional on linting success
4. Agree on and install an autoformatter
5. If your codebase allows (and doesn't have too many idiosyncracies) set up a 'format-on-save' hook
6. If your codebase is in bad shape, run an autoformatter on any file you need to change _before_ you change it, and add that as a preparatory commit.
7. Agree on a set of steps to use when naming a new concept, and practice them

## How to Choose a Name

### Describe the thing in plain English sentences

- Why are you creating this particular named object?
- How will you use this new thing?
- What are the expected results of this new method?

### Review the shared vocabulary

- Can you use an existing name from the codebase or problem domain?
- Can you create a derivative of an existing term?
- Can you use a widely-accepted abbreviation? (HTML is commonly understood, SDEP is not)

### Consider Pitfalls

- Does the name communicate all the information needed to understand the purpose / use-case without reading the implementation?
- Are you storing up trouble by describing the same thing in different ways in different places in the codebase?
- Does the name include a word which has multiple potential meanings, depending on context? (bark, die, current, dispatch)
- Does the name include words which are esoteric or otherwise exclusionary? Would it be confusing to a non-native English speaker (flammable, inflammable, non-flammable)
- Does the name include words which have more than one accepted spelling? (synthesise, synthesize)
- Have you checked using a dictionary?
- Can you easily pronounce the name? Does it make sense in an English sentence? You will be talking about this thing.

## When Naming Is Hard

- Create a paragraph-length precise definition of the new thing
- How is this entity different from the entities already in use in the context of the system?
- Have you been too specific, or tried to be too generic?
- Summarise your definition into a variety of four-word phrases, and review them against the criteria above.
- Consider if you can shorten the name without losing critical information.
- Share the name with non-technical as well as technical colleagues. Ask them what they think it is, or does.
- Consider: Is this one thing or many? Do you need to review the overall design? Has the class developed a gravitational field?

## When Naming Feels Impossible

- Ask for help.
- Go for a walk.
- Use an obviously-nonsense name rather than something 'close enough' while you continue to work on the rest of the design and your understanding of the system. Applesauce, Badger, Mushroom, Snake, or something equally incongruous in the context of your domain.

## Further reading

- [Naming Inspiration](https://classnames.paulrobertlloyd.com/)
- [Naming conventions in programming – a review of scientific literature](https://makimo.com/blog/scientific-perspective-on-naming-in-programming/)
- [Your coding conventions are hurting you](http://www.carlopescio.com/2011/04/your-coding-conventions-are-hurting-you.html)
- [Execution in the Kingdom of Nouns](https://steve-yegge.blogspot.com/2006/03/execution-in-kingdom-of-nouns.html)

## Appendix 1: Worked Examples

### Naming a Class

User story
> SO THAT hosts can find companies they might like to invite to sourcing events WE WILL enable them to search Veridion's Complex Search API by product keyword

In order to do this, we need something to coordinate the the interaction between the host user making the search, the view(s) they see, and the data in and business logic around the search results. In other words, we need a Rails controller.

By convention rails controllers are in app/controllers, named in PascalCase with a name ending in `Controller`. Usually the name of the controller is the same as that of the associated model, but pluralized.

Initial name ideas

- `VeridionComplexSearchController`
- `VeridionController`
- `VeridionEventParticipantSearchController`

These names all contain useful information for a developer: in English, the controller interacts with the Veridion Complex Search API to return a collection of data about potential EventParticipants.

It does not, however, use terms that would be familiar to the sales team, or end users, and relies on developers knowing what Veridion offers. It will need to be changed if Veridion has a name change. It will need to be changed if the process of supplier discovery allows for e.g. fetching information about a named company, rather than searching.

The _purpose_ of the controller is to enable our users to discover information about potential suppliers so they can decide whether or not to onboard the suppliers, or invite them to an event. It doesn't matter which third party provides the data, and it doesn't matter what the data will be used for.

The name selected was therefore `SupplierDiscoveryController`

We are not following the convention to use a plural name as it does not make for a gramatically logical name.

### Renaming a Method

Context: An intractable bug where answers were being marked as failed when they were not, causing participants to be excluded from events.

The method `QuestionnaireQuestionAnswer#failed_answer?` was misleading in several ways

- `failed_answer?` overrides a method provided by the Rails framework
- this method, which matches the name of the model attribute `failed_answer` would be expected to query the attribute and return a Boolean.
- the explanatory comment was also misleading (the attribute value was set, but not persisted)
    > This method is used to check if the answer is failed or not. It sets an attribute on the model called failed_answer which can be used in places where we just want to display the answer

What the method actually did was

- calculate whether the selected question choice was an autofail (killer) choice
- set the value of QuestionnaireQuestionAnswer#failed_answer to this value
- leave the modified QuestionnaireQuestionAnswer unsaved i.e. the change was not persisted
- return the Boolean value of the initial calculation

A more correct name (though objectively terrible) is `calculate_and_return_autofail_question_answer_status_and_update_failed_answer_attribute_without_saving`
This is too long for it to be a good name, but it does highlight that the method is doing more than one thing. It would be better to have one method which calculates, one method which updates, and one which queries the attribute

Split into three methods, we have `calculate_autofail_questionnaire_question_answer_status`, `get_boolean_failed_answer_attribute` and `update_failed_answer_attribute_without_saving`

Now we can work on improving each method name separately

`calculate_autofail_question_answer_status` is correct and clear, but can still be improved.
As the method will always be called on a QuestionnaireQuestionAnswer, we do not need to include that information.
`calculate_autofail_status` would be better, and easier to read and pronounce

`update_failed_answer_attribute_without_saving` is standard Rails functionality and can be replaced by `qqa.failed_answer = calculate_autofail_status`.

`get_boolean_failed_answer_attribute` is also standard Rails functionality `qqa.failed_answer?`

With these changes made, it became clear that the failed_answer attribute was being saved at the wrong time. It was calculated and persisted whenever a questionnaire_question_choice was selected in a questionnaire_question_answer. It was not persisted when the questionnaire_answer (the collection of questionnaire_question_answer) was submitted and the failed_answer status was used to calculate if the entire questionnaire should be autofailed.

Changing the code to calculate and persist the failed_answer attribute on each questionnaire_question_answer when the entire questionnaire_answer was submitted, to calculate but not persist the attribute when a summary of unsubmitted questionnaire_answers was generated, and to neither calculate nor persist the attribute when a single question choice was selected resolved the bug.

## Appendix 2: Selected Ruby/Rails Naming Conventions

CARE: These are Ruby-specific and may not apply to your language of choice

### General Guidelines

#### Full Words

Choose dictionary words that clearly describe the purpose or use-case to avoid ambiguity

- Good: `heavy_goods_vehicle`, `total_price_in_pence`
- Bad: `rig`, `total_p`

Use terms that are familiar within the domain of the application, and consistent with end-user mental models.

- Good: `invoice_total`
- Bad: `total_amount`

Only use acronyms which are widely accepted and in common usage and avoid neologisms and slang

- Good: `http`, `sim_dojo_event_participant`, `unique`
- Bad: `hyper_text_transfer_protocol`, `sdep`, `based`

#### Pronounceable

Use names which make sense when spoken as well as written to facilitate collaboration.

- Good: `modified_at`, `unable_to_parse_file`
- Bad: `mod_date_yymmdd`, `file_unable_to_parse`

#### Specific Not Generic

Be specific to avoid confusion and make automated refactoring less risky

- Good: `customer_address`
- Bad: `data`

Ensure the name is not misleading (especially when changing code)

- Good: Contract attribute `alert_date` which stores the date on which the user wishes to be notified
- Bad: Contract attribute `notice_period` which actually stores the _date_ on which the notice_period starts when the name suggests an interval

Avoid homonyms (words which can mean different things depending on context)

- Good: explicit but not esoteric `contemporary`, `alternating_current`, `six-sided-die`, `coining_die`
- Bad: `current` (of the present time, moving electrical charge, moving water), `die` (stop living, small cube with dots on the faces used in a game of chance, an engraved stamp used for e.g. coining money, and so on)

Choose one name for a concept and avoid synonyms

- Good: `current_user`
- Bad: `active_user`, `logged_in_user`, `authenticated_user`, `validated_session_holder` used interchangeably

#### Easy to Reason About

Make booleans positive

- Good: `valid`
- Bad: `not_invalid`

Make names differ by more than word order

- Good: `employee_count` and `counter_staff` in the same context
- Bad: `employee_count` and `count_employees` in the same context

Use singular nouns when naming single objects.

- Good: `product_sku`, `EventParticipant`

Use plural (or collective) nouns when naming collections

- Good: `contact_preferences`, `fleet` (of company cars, ships, spacecraft etc)

Make names as long as they need to be to be clear, but no longer. A rule of thumb is four words, or 20 characters.

- Good: `participant_count`
- Bad: `integer_number_of_participating_users`

#### Easy to Change

Avoid revealing the underlying implementation in the name

- Good: `AccountCreationData`
- Bad: `AccountCreationDataJsonHash`

Avoid using types or encodings as prefixes

- Good: `full_name`
- Bad: `string_display_name`

Name what a value represents, not what it is

- Good: `STANDARD_VAT_RATE = 20%`
- BAD: `ONE_THIRD = 0.25`

Avoid specifying the variable type in the name

- Good: `full_name`
- Bad: `full_name_string`

#### Avoid Reserved Words

Use terms which are not an existing part of the framework or language.

- Good: `user_class`, `app_module`
- Bad: `class`, `module`

Do not override names/methods/concepts provided by the framework

- Good: `calculate_event_status`
- Bad: `active?` (which does not return a boolean active attribute value but calculates and sets the status attribute)

#### Follow Style Conventions

Use snake_case for variables and methods, SCREAMING_SNAKE_CASE for constants, PascalCase for classes.

- Good: `user_name`, `WAIT_TIME`, `ApplicationController`
- Bad: `userName`, `WaitTime`, `application-controller`

### Method Names

Use verbs or verb phrases to indicate what the method does.

- Good: `calculate_total`, `send_email`
- Bad: `total`, `email`

Do not use conjunctions (and, or, but)

- Good: `calculate_invoice_discount`, `calculate_invoice_total`, `invoice_total=`
- Bad: `apply_discount_and_total_invoice`
- Edge Case: `find_and_replace`

Avoid get_- and set_- prefixes.

- Good: `name`, `name=`
- Bad: `get_name`, `set_name`

Use predicate methods rather than is_-, is_not_-, or not_- prefixes when returning a Boolean

- Good: `active?`, `locked?`
- Bad: `is_activated`, `is_not_locked`

Use bang methods for dangerous methods such as those which are irreversible, may raise exceptions, or have side effects

- Good: `sort!` (modifies the existing list in place)
- Good: `save!` (raises an exception if validation fails)

Research has found six main types of method name. It may be helpful to consider where your new name fits into this.

- indirect action, e.g. `error()` or `on_error()`
- direct action, e.g.`open()`, `close()`, `kill()`
- action on object, e.g. `read_line()`, `open_file()`
- double action, e.g. `search_and_replace_text()`
- property check, e.g. `is_active?()` or `active?()`
- transformation, e.g. `convert_to_hex()`

### Classes

Use nouns, or noun phrases, that represent the entity or concept the class encapsulates.

- Good: `User`, `ProcessedOrder`
- Bad: `ProcessOrder`, `Data`

Avoid names which say nothing about the responsibility of the class, encouraging the dumping of random methods

- Good: `ConfirmedOrder`, `AcceptedBid`, `DateTimeLocalizer`
- Bad: `ConfirmationManager`, `DispatchHelper` or `Data`

Use nested modules to group related classes in a way which matches their hierarchy (this is different to the concept of a Module as a mixin)

- Good: `Blog::Post`, `Blog::Platform`; `Bed::Post`, `Bed::Platform`
- Bad: `Post::Blog`, `Post::Bed`, `Post::Box`

Do not duplicate the hierarchy in the class name

- Good: `Validators::Numeric`
- Bad: `Validators::NumberValidator`
