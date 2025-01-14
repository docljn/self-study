I'll explain declarative programming in Ruby, focusing on the shift in mindset from imperative to declarative patterns that an experienced Ruby developer would need to make.

Rather than detailing what most Ruby devs already know well (objects, blocks, etc.), I'll focus on the declarative patterns and their implementation:

```ruby
# Instead of imperatively building a filtered, mapped array:
filtered_users = []
users.each do |user|
  if user.active? && user.age > 21
    filtered_users << user.name.upcase
  end
end

# Declarative approach using functional composition:
users
  .select(&:active?)
  .select { |user| user.age > 21 }
  .map { |user| user.name.upcase }
```

Key principles to transitioning to declarative Ruby:

1. Compose Operations:
```ruby
# Instead of mutating state:
def process_order(order)
  total = 0
  order.items.each { |item| total += item.price }
  
  if total > 100
    total *= 0.9  # Apply discount
  end
  total
end

# Declare the transformations:
def process_order(order)
  order.items
    .map(&:price)
    .sum
    .then { |total| total > 100 ? total * 0.9 : total }
end
```

2. Express Intent Through Domain-Specific Methods:
```ruby
class OrderProcessor
  def eligible_for_discount?(order)
    order.items.sum(&:price) > 100
  end

  def apply_discount(total)
    total * 0.9
  end

  def process(order)
    total = order.items.sum(&:price)
    eligible_for_discount?(order) ? apply_discount(total) : total
  end
end
```

3. Use Ruby's Built-in Declarative Features:
```ruby
class Product
  # Instead of manual attribute methods:
  # def name; @name; end
  # def price; @price; end
  
  attr_reader :name, :price
  
  # Instead of manual validation logic:
  # def valid?
  #   @price.positive? && @name.present?
  # end
  
  validates :name, presence: true
  validates :price, numericality: { greater_than: 0 }
end
```

4. Leverage Enumerable for Data Transformations:
```ruby
# Instead of mutation-based collection processing:
def categorize_products(products)
  categories = Hash.new { |h, k| h[k] = [] }
  
  products.each do |product|
    categories[product.category] << product
  end
  
  categories
end

# Declare the structure you want:
def categorize_products(products)
  products.group_by(&:category)
end
```

5. Use Pattern Matching (Ruby 2.7+):
```ruby
# Instead of nested conditionals:
def process_result(result)
  case result
  in { success: true, data: { user: { name:, age: } } }
    "Found #{name}, #{age} years old"
  in { success: false, error: msg }
    "Error: #{msg}"
  end
end
```

The key mental shift is moving from "how" to "what":
- Describe the desired data transformations rather than the steps
- Use method names that express business logic
- Leverage Ruby's built-in declarative features
- Compose operations instead of mutating state
- Think in terms of data flow rather than execution steps
I'll demonstrate more advanced declarative patterns in Ruby, focusing on sophisticated real-world scenarios.

1. Advanced Pattern Matching with Data Transformation:

```ruby
# Complex data transformation using pattern matching and declarative composition
class OrderProcessor
  Result = Struct.new(:status, :data, :errors, keyword_init: true)

  def process_order(input)
    case input
    in { customer: { name:, tier: "premium" }, items: items } if items.any?
      Result.new(
        status: :success,
        data: calculate_premium_order(name:, items:)
      )
    in { customer: { name:, tier: "standard" }, items: items } if items.any?
      Result.new(
        status: :success,
        data: calculate_standard_order(name:, items:)
      )
    in { items: [] }
      Result.new(
        status: :error,
        errors: ["Empty order not allowed"]
      )
    else
      Result.new(
        status: :error,
        errors: ["Invalid order format"]
      )
    end
  end

  private

  def calculate_premium_order(name:, items:)
    {
      customer: name,
      items: items,
      subtotal: items.sum(&:price),
      discount: calculate_premium_discount(items),
      total: ->(subtotal, discount) { subtotal - discount }
    }.then do |order|
      order.merge(
        total: order[:total].call(order[:subtotal], order[:discount])
      )
    end
  end
end
```

2. Advanced Composition with Procs and Lambdas:

```ruby
class DataTransformer
  # Define reusable transformations as procs
  TRANSFORMATIONS = {
    normalize_email: ->(email) { email.downcase.strip },
    format_phone: ->(phone) { phone.gsub(/\D/, '').match(/(\d{3})(\d{3})(\d{4})/)&.captures&.join('-') },
    validate_age: ->(age) { age.between?(18, 120) ? age : nil }
  }

  def self.transform(data, *operations)
    operations
      .map { |op| TRANSFORMATIONS.fetch(op) }
      .reduce(data) { |result, transform| transform.call(result) }
  end

  # Compose transformations into higher-order operations
  def self.build_transformer(*operations)
    ->(data) { transform(data, *operations) }
  end
end

# Usage
user_transformer = DataTransformer.build_transformer(:normalize_email, :format_phone)
users.map(&user_transformer)
```

3. Declarative Domain-Specific Language (DSL):

```ruby
class ReportBuilder
  class << self
    def define(&block)
      new.tap { |builder| builder.instance_eval(&block) }
    end
  end

  def initialize
    @sections = []
    @filters = []
    @transformations = []
  end

  def section(name, &block)
    @sections << { name: name, content: block }
  end

  def filter(&block)
    @filters << block
  end

  def transform(&block)
    @transformations << block
  end

  def build(data)
    data
      .then { |d| apply_filters(d) }
      .then { |d| apply_transformations(d) }
      .then { |d| build_sections(d) }
  end

  private

  def apply_filters(data)
    @filters.reduce(data) { |d, filter| d.select(&filter) }
  end

  def apply_transformations(data)
    @transformations.reduce(data) { |d, transform| d.map(&transform) }
  end

  def build_sections(data)
    @sections.map do |section|
      [section[:name], section[:content].call(data)]
    end.to_h
  end
end

# Usage
report = ReportBuilder.define do
  filter { |user| user.active? }
  filter { |user| user.age >= 18 }
  
  transform { |user| user.slice(:name, :email, :age) }
  transform { |user| user.merge(adult: user[:age] >= 21) }
  
  section :summary do |users|
    {
      total_users: users.count,
      average_age: users.sum { |u| u[:age] } / users.count.to_f
    }
  end
  
  section :adults do |users|
    users.select { |u| u[:adult] }
  end
end
```

4. Declarative Validation and Type Checking:

```ruby
module TypeChecking
  def self.included(base)
    base.extend(ClassMethods)
  end

  module ClassMethods
    def attribute(name, type, validations = {})
      define_method(name) { instance_variable_get("@#{name}") }
      
      define_method("#{name}=") do |value|
        unless value.is_a?(type)
          raise TypeError, "#{name} must be a #{type}"
        end
        
        validations.each do |validation, args|
          unless send("validate_#{validation}", value, args)
            raise ArgumentError, "#{name} failed #{validation} validation"
          end
        end
        
        instance_variable_set("@#{name}", value)
      end
    end
  end

  private

  def validate_range(value, range)
    range.include?(value)
  end

  def validate_format(value, regex)
    value.match?(regex)
  end
end

class User
  include TypeChecking

  attribute :name, String, format: /\A[a-zA-Z\s]+\z/
  attribute :age, Integer, range: 18..120
  attribute :email, String, format: /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i
end
```

5. Functional Composition with Monads:

```ruby
class Maybe
  def self.just(value)
    Just.new(value)
  end

  def self.nothing
    Nothing.new
  end
end

class Just < Maybe
  attr_reader :value

  def initialize(value)
    @value = value
  end

  def bind(&block)
    block.call(@value)
  end

  def map(&block)
    Maybe.just(block.call(@value))
  end
end

class Nothing < Maybe
  def bind
    self
  end

  def map
    self
  end
end

# Usage with custom operations
module UserOperations
  extend self

  def find_user(id)
    user = User.find_by(id: id)
    user ? Maybe.just(user) : Maybe.nothing
  end

  def verify_email(user)
    user.email_verified? ? Maybe.just(user) : Maybe.nothing
  end

  def check_subscription(user)
    user.active_subscription? ? Maybe.just(user) : Maybe.nothing
  end
end

# Compose operations
result = UserOperations
  .find_user(123)
  .bind { |user| UserOperations.verify_email(user) }
  .bind { |user| UserOperations.check_subscription(user) }
  .map { |user| user.premium_features }
```

These examples demonstrate how to:
- Build flexible DSLs for configuration
- Handle complex data transformations
- Implement type safety declaratively
- Use functional patterns like monads
- Create composable operations
I'll demonstrate more advanced declarative techniques in Ruby, focusing on sophisticated patterns that leverage Ruby's unique features.

1. Declarative State Machines with Event Handling:

```ruby
module StateMachine
  def self.included(base)
    base.extend(ClassMethods)
  end

  module ClassMethods
    def state_machine(&block)
      class_eval do
        instance_eval(&block)
        
        def self.states
          @states ||= {}
        end

        def self.transitions
          @transitions ||= {}
        end

        def self.state(name, &block)
          states[name] = block
        end

        def self.transition(from:, to:, on:)
          transitions[on] = { from: Array(from), to: to }
        end
      end
    end
  end

  def current_state
    @current_state ||= self.class.states.keys.first
  end

  def trigger(event)
    transition = self.class.transitions[event]
    raise InvalidTransition unless valid_transition?(transition, event)

    @current_state = transition[:to]
    instance_eval(&self.class.states[@current_state]) if self.class.states[@current_state]
    broadcast("on_#{event}")
    self
  end

  private

  def valid_transition?(transition, event)
    transition && transition[:from].include?(current_state)
  end

  def broadcast(event_name)
    return unless respond_to?(event_name)
    send(event_name)
  end
end

class Order
  include StateMachine

  attr_reader :total, :refund_amount

  state_machine do
    state :pending do
      @processed_at = nil
      @refunded_at = nil
    end

    state :processed do
      @processed_at = Time.now
    end

    state :refunded do
      @refunded_at = Time.now
    end

    transition from: :pending, to: :processed, on: :process
    transition from: :processed, to: :refunded, on: :refund
  end

  def initialize(total)
    @total = total
  end

  def on_refund
    @refund_amount = calculate_refund
  end

  private

  def calculate_refund
    # Complex refund calculation logic
    total * 0.9
  end
end
```

2. Declarative Query Builder with Method Chaining:

```ruby
class QueryBuilder
  def self.build(&block)
    new.tap { |query| query.instance_eval(&block) if block_given? }
  end

  def initialize
    @select_clauses = []
    @where_clauses = []
    @joins = []
    @group_by = []
    @having = []
    @order = []
    @limit = nil
    @offset = nil
  end

  def select(*fields)
    @select_clauses.concat(fields)
    self
  end

  def where(condition = nil, &block)
    if block_given?
      @where_clauses << WhereBuilder.build(&block)
    else
      @where_clauses << condition
    end
    self
  end

  def join(table, type: :inner, &block)
    @joins << JoinBuilder.build(table, type, &block)
    self
  end

  def group(*fields)
    @group_by.concat(fields)
    self
  end

  def having(condition)
    @having << condition
    self
  end

  def order(*fields)
    @order.concat(fields)
    self
  end

  def limit(value)
    @limit = value
    self
  end

  def offset(value)
    @offset = value
    self
  end

  def to_sql
    [
      select_clause,
      from_clause,
      joins_clause,
      where_clause,
      group_clause,
      having_clause,
      order_clause,
      limit_clause,
      offset_clause
    ].compact.join(' ')
  end

  private

  def select_clause
    "SELECT #{@select_clauses.empty? ? '*' : @select_clauses.join(', ')}"
  end

  # ... other private methods for clause generation
end

class WhereBuilder
  def self.build(&block)
    new.tap { |builder| builder.instance_eval(&block) }.conditions
  end

  def method_missing(name, *args)
    Condition.new(name, *args)
  end
end

# Usage
query = QueryBuilder.build do
  select :id, :name, :email
  where { (status.eq('active') & age.gt(18)) | premium.eq(true) }
  join(:orders) { user_id.eq(users.id) & status.eq('completed') }
  group :status
  having 'COUNT(*) > 5'
  order :created_at.desc
  limit 10
  offset 0
end
```

3. Advanced Memoization with Dependency Tracking:

```ruby
module SmartMemoization
  def self.included(base)
    base.extend(ClassMethods)
  end

  module ClassMethods
    def memoize(*method_names, depends_on: [])
      method_names.each do |method_name|
        original_method = instance_method(method_name)
        
        define_method(method_name) do |*args, &block|
          cache_key = [method_name, args, depends_on.map { |dep| send(dep) }]
          
          if instance_variable_defined?("@#{method_name}_cache")
            cache = instance_variable_get("@#{method_name}_cache")
            return cache[cache_key] if cache.key?(cache_key)
          else
            instance_variable_set("@#{method_name}_cache", {})
          end

          result = original_method.bind(self).call(*args, &block)
          instance_variable_get("@#{method_name}_cache")[cache_key] = result
        end
        
        define_method("#{method_name}_reset") do
          remove_instance_variable("@#{method_name}_cache") if instance_variable_defined?("@#{method_name}_cache")
        end
      end
    end
  end
end

class PricingCalculator
  include SmartMemoization

  attr_reader :base_price, :tax_rate, :discount_level

  def initialize(base_price, tax_rate, discount_level)
    @base_price = base_price
    @tax_rate = tax_rate
    @discount_level = discount_level
  end

  memoize :total_price, depends_on: [:base_price, :tax_rate, :discount_level]
  def total_price
    (base_price * (1 + tax_rate) * (1 - discount_multiplier))
  end

  memoize :discount_multiplier, depends_on: [:discount_level]
  def discount_multiplier
    case discount_level
    when :premium then 0.2
    when :standard then 0.1
    else 0
    end
  end
end
```

4. Declarative Validation Pipeline:

```ruby
module ValidationPipeline
  def self.included(base)
    base.extend(ClassMethods)
  end

  module ClassMethods
    def validates(field, &block)
      validations[field] ||= []
      validations[field] << block
    end

    def validate_with(*validators)
      validators.each do |validator|
        validate_blocks << validator
      end
    end

    def validations
      @validations ||= {}
    end

    def validate_blocks
      @validate_blocks ||= []
    end
  end

  def valid?
    @errors = {}
    validate_fields
    validate_object
    @errors.empty?
  end

  def errors
    @errors ||= {}
  end

  private

  def validate_fields
    self.class.validations.each do |field, validations|
      value = send(field)
      validations.each do |validation|
        result = instance_exec(value, &validation)
        errors[field] = result unless result == true
      end
    end
  end

  def validate_object
    self.class.validate_blocks.each do |validator|
      instance_exec(&validator)
    end
  end
end

# Usage with complex validation rules
class User
  include ValidationPipeline
  
  attr_accessor :email, :password, :age, :roles

  validates :email do |value|
    next "Email is required" if value.nil?
    next "Invalid email format" unless value.match?(/\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i)
    true
  end

  validates :password do |value|
    next "Password is required" if value.nil?
    next "Password too short" if value.length < 8
    next "Password too weak" unless value.match?(/^(?=.*[A-Z])(?=.*[a-z])(?=.*\d).*$/)
    true
  end

  validate_with ->{
    if age < 18 && roles.include?('admin')
      errors[:base] = "Users under 18 cannot be admins"
    end
  }
end
```

5. Declarative Event System with Middleware:

```ruby
module EventSystem
  class Pipeline
    def initialize
      @middleware = []
    end

    def use(middleware)
      @middleware << middleware
      self
    end

    def call(event)
      @middleware.reduce(event) { |evt, middleware| middleware.call(evt) }
    end
  end

  module ClassMethods
    def event_pipeline
      @event_pipeline ||= Pipeline.new
    end

    def on(event_name, &handler)
      event_handlers[event_name] ||= []
      event_handlers[event_name] << handler
    end

    def event_handlers
      @event_handlers ||= {}
    end
  end

  def self.included(base)
    base.extend(ClassMethods)
  end

  def trigger(event_name, payload = {})
    event = { name: event_name, payload: payload, timestamp: Time.now }
    processed_event = self.class.event_pipeline.call(event)
    
    handlers = self.class.event_handlers[event_name] || []
    handlers.each { |handler| instance_exec(processed_event, &handler) }
  end
end

# Example middleware
class LoggingMiddleware
  def call(event)
    puts "Event: #{event[:name]} at #{event[:timestamp]}"
    event
  end
end

class EnrichmentMiddleware
  def call(event)
    event[:payload].merge!(
      processed_at: Time.now,
      environment: ENV['RACK_ENV']
    )
    event
  end
end

# Usage
class OrderProcessor
  include EventSystem

  event_pipeline
    .use(LoggingMiddleware.new)
    .use(EnrichmentMiddleware.new)

  on :order_created do |event|
    # Handle order created
    notify_warehouse(event[:payload])
  end

  on :order_shipped do |event|
    # Handle order shipped
    notify_customer(event[:payload])
  end
end
```

These examples demonstrate advanced declarative techniques for:
- State management with complex transitions
- Flexible query building
- Smart caching with dependency tracking
- Advanced validation pipelines
- Event handling with middleware

