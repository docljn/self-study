Here's a practical rubric for implementing features using declarative programming in Ruby on Rails:

```ruby
# QUICK REFERENCE GUIDE: Declarative Programming in Rails
# Use this checklist when building new features

1. BASIC STRUCTURE
   ✓ Start with the "what", not the "how"
   ✓ Define the desired outcome first
   ✓ Break into composable parts

   # Instead of:
   def process_order
     @total = 0
     @items.each { |item| @total += item.price }
     # ... more procedural code
   end

   # Do this:
   def process_order
     OrderProcessor.new(items)
       .calculate_total
       .apply_discounts
       .handle_taxes
   end

2. ACTIVE RECORD / MODELS
   ✓ Use built-in declarative features
   ✓ Define relationships clearly
   ✓ Validate at model level

   class Order < ApplicationRecord
     # Declare relationships
     belongs_to :user
     has_many :items
     
     # Declare validations
     validates :total, presence: true, numericality: true
     
     # Declare scopes
     scope :recent, -> { where('created_at > ?', 30.days.ago) }
     scope :pending_approval, -> { where(status: 'pending') }
   end

3. BUSINESS LOGIC
   ✓ Use service objects for complex operations
   ✓ Make states and transitions explicit
   ✓ Use clear, intention-revealing names

   # app/services/order_processor.rb
   class OrderProcessor
     Result = Struct.new(:success?, :data, :errors, keyword_init: true)

     def self.call(order)
       new(order).process
     end

     def process
       Result.new(
         success?: true,
         data: {
           order: order,
           total: calculate_total,
           discount: apply_discounts
         }
       )
     rescue => e
       Result.new(success?: false, errors: [e.message])
     end
   end

4. CONTROLLERS
   ✓ Keep thin
   ✓ Use declarative patterns for common operations
   ✓ Handle results explicitly

   class OrdersController < ApplicationController
     def create
       result = OrderProcessor.call(order_params)

       if result.success?
         redirect_to order_path(result.data[:order]), notice: 'Order processed'
       else
         flash.now[:error] = result.errors
         render :new
       end
     end
   end

5. QUERIES
   ✓ Use scopes and class methods
   ✓ Chain operations clearly
   ✓ Keep SQL in the model layer

   # app/models/order.rb
   class Order < ApplicationRecord
     scope :for_dashboard, -> {
       includes(:items, :user)
         .where(status: ['pending', 'processing'])
         .order(created_at: :desc)
     }
   end

6. FORMS & VALIDATION
   ✓ Use form objects for complex forms
   ✓ Declare validations explicitly
   ✓ Handle nested attributes declaratively

   # app/forms/order_form.rb
   class OrderForm
     include ActiveModel::Model

     attr_accessor :user_id, :items, :coupon_code
     
     validates :user_id, presence: true
     validates :items, presence: true
     validate :coupon_code_valid?

     def save
       return false unless valid?
       
       ActiveRecord::Base.transaction do
         Order.create!(attributes)
       end
     end
   end

7. ERROR HANDLING
   ✓ Define expected error states
   ✓ Use result objects
   ✓ Handle errors at appropriate levels

   module Orders
     class Error < StandardError; end
     class InvalidStatus < Error; end
     
     def self.transition(order, to:)
       raise InvalidStatus unless valid_transition?(order.status, to)
       order.update!(status: to)
     end
   end

8. TESTING
   ✓ Test behaviors, not implementation
   ✓ Use descriptive contexts
   ✓ Test edge cases explicitly

   RSpec.describe OrderProcessor do
     context 'with valid order' do
       it 'processes successfully' do
         result = described_class.call(valid_order)
         expect(result).to be_success
       end
     end

     context 'with invalid data' do
       it 'returns appropriate errors' do
         result = described_class.call(invalid_order)
         expect(result.errors).to include('Invalid order state')
       end
     end
   end

PRACTICAL TIPS:
- Start simple, add complexity only when needed
- Use built-in Rails features before custom solutions
- Keep related code together (cohesion)
- Use clear naming conventions
- Document complex business rules
- Test edge cases thoroughly

COMMON PITFALLS TO AVOID:
❌ Don't mix business logic in controllers
❌ Avoid global state
❌ Don't bypass model validations
❌ Avoid deep nesting of conditionals
❌ Don't repeat validation logic
```

