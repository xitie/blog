---
layout: post 
title: Refactoring Ruby Edition 
keywords: refactoring 
tags: ruby 
description: To restructure software by applying a series of refactorings without changing its observable behavior.
---
<h3>First step in Refactoring</h3>
<p>The first step is always the same: build a solid set of tests for that section of code.</p>
<p><span>Tip:</span> Refactoring changes the programs in small steps. If you make a mistake, it is easy to find the bug.</p>
<p><span>Tip:</span> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.</p>
<h3>Code smells</h3>
<p><span>Duplicated Code: 重复的代码</span></p>
<p>Scene: 同一个class内的两个函数含有相同表达式</p>
<p>Refactor: 采用Extract Method方法提炼出重复的代码，让这两个地点调用被提炼出来的那一段代码</p>
<p>&nbsp;</p>
<p><span>Long Method: 过长函数</span></p>
<p>Scene: 感觉需要以注释来对函数说明点什么</p>
<p>Refactor: 把需要说明的东西写进一个独立函数中，并以其用途命名</p>
<p>&nbsp;</p>
<p><span>Large Class: 过大类</span></p>
<p>Scene: 想利用单一class做太多事情，其内往往就会出现太多instance变量</p>
<p>Refactor: 运用Extract Class将数个变量一起提炼至新class内</p>
<p>&nbsp;</p>
<p><span>Long Parameter List: 过长参数列</span></p>
<p>Scene: 太长的参数列难以理解，太多参数会造成前后不一致、不易使用</p>
<p>Refactor: 激活重构准则Replace Parameter with Method(以函数取代参数)</p>
<p>&nbsp;</p>
<p><span>Divergent Change: 发散式变化</span></p>
<p>Scene: A/B/C/D...多种功能变化的时候它都需要修改</p>
<p>Refactor: 某个类负担了多项任务，需要再拆分几个类出来，把变化封装得更细</p>
<p>&nbsp;</p>
<p><span>Shotgun Surgery: 散弹式修改</span></p>
<p>Scene: 完成某个需求的时候，A/B/C/D...多个类都需要修改</p>
<p>Refactor: 多个类之间的耦合太严重,Move Method和Move Field把所有需要修改的代码放进同一个class</p>
<p>&nbsp;</p>
<p><span>Feature Envy: 依恋情结</span></p>
<p>Scene: 函数从另一个对象调用几乎半打的函数</p>
<p>Refactor: 使用Move Method把它移到它该去的地方</p>
<p>&nbsp;</p>
<p><span>Switch Statements: switch惊悚现身</span></p>
<p>Scene: switch语句的问题在于重复</p>
<p>Refactor: 使用Extract Method将switch语句提炼到一个独立函数中，再将它搬移到需要多态性的class里</p>
<p>&nbsp;</p>
<p><span>Message Chains: 过度耦合的消息链</span></p>
<p>Scene: 用户向一个对象索求另一个对象，然后再向后者索求另一个对象，然后再索求另一个对象...</p>
<p>Refactor: 使用Hide Delegate或以Extract Method把使用该对象的代码提炼到一个独立函数中</p>
<p>&nbsp;</p>
<p><span>Inappropriate Intimacy: 狎昵关系</span></p>
<p>Scene: 两个classes过于亲密，花费太多时间去探究彼此的private成分</p>
<p>Refactor: 采用Move Method和Move Field帮它们划清界线</p>
<p>&nbsp;</p>
<h3>Composing Methods</h3>
<p><span>Extract Method: 提炼函数</span></p>
<p>description: 有一段代码可以被组织在一起并独立出来,放进一个函数中，并让函数名解释该函数用途</p>

<pre>
def print_owing(amount)
  print_banner
  puts "name: #{@name}"
  puts "amount: #{amount}"
end

#refactoring code
def print_owing(amount)
  print_banner
  print_details amount
end
def print_details(amount)
  puts "name: #{@name}"
  puts "amount: #{amount}"
end
</pre>

<p>&nbsp;</p>
<p><span>Inline Method: 函数内联化</span></p>
<p>description: 函数本体与其名称同样清楚易懂,在函数调用点插入函数本体，然后移除该函数</p>

<pre>
def get_rating
  more_than_five_late_deliveries ? 2 : 1
end
def more_than_five_late_deliveries
  @number_of_late_deliveries > 5
end

#refactoring code
def get_rating
  @number_of_late_deliveries > 5 ? 2 : 1
end
</pre>

<p>&nbsp;</p>
<p><span>Inline Temp: 临时变量内联化</span></p>
<p>description: 临时变量只被简单表达式赋值一次，将所有对该变量的引用动作替换为表达式本身</p>

<pre>
base_price = an_order.base_price
return (base_price > 1000)

#refactoring code
return (an_order.base_price > 1000)
</pre>

<p>&nbsp;</p>
<p><span>Replace Temp with Query: 以查询取代临时变量</span></p>
<p>description: 程序以一个临时变量保存表达式的运算结果,将这个表达式提炼到一个独立函数中,临时变量的所有引用点替换为对新函数的调用</p>

<pre>
base_price = @quantity * @item_price
if (base_price > 1000)
  base_price * 0.95
else
  base_price * 0.98
end

#refactoring code
if (base_price > 1000)
  base_price * 0.95
else
  base_price * 0.98
end
def base_price
  @quantity * @item_price
end
</pre>

<p>&nbsp;</p>
<p><span>Introduce Explaining Variable: 引入解释性变量</span></p>
<p>description: 有一个复杂的表达式,将该表达式(或其中一部分)的结果放进一个临时变量，以此变量名称来解释表达式用途</p>

<pre>
if (platform.upcase.index("MAC") &&
    browser.upcase.index("IE") &&
    initialized? &&
    resize > 0
)
# do something
end

#refactoring code
  is_mac_os = platform.upcase.index("MAC")
  is_ie_browser = browser.upcase.index("IE")
  was_resized = resize > 0
if (is_mac_os && is_ie_browser && initialized? && was_resized)
  # do something
end
</pre>

<p>&nbsp;</p>
<p><span>Split Temporary Variable: 剖解临时变量</span></p>
<p>description: 程序有个临时变量被赋值超过一次，它既不是循环变量，也不是一个集用临时变量</p>

<pre>
temp = 2 * (@height + @width)
puts temp
temp = @height * @width
puts temp

#refactoring code
perimeter = 2 * (@height + @width)
puts perimeter
area = @height * @width
puts area
</pre>

<p>&nbsp;</p>
<p><span>Remove Assignments to Parameters: 移除对参数的赋值动作</span></p>
<p>description: 代码对一个参数进行赋值动作</p>

<pre>
def discount(input_val, quantity, year_to_date)
  if input_val > 50
    input_val -= 2
  end
end

#refactoring code
def discount(input_val, quantity, year_to_date)
  result = input_val
  if input_val > 50
    result -= 2
  end
end
</pre>

<p>&nbsp;</p>
<p><span>Replace Method with Method Object: 以函数对象取代函数</span></p>
<p>description: 一个大型函数，其中对局部变量的使用，使你无法釆用 Extract Method,将这个函数放进一个单独对象中，如此一来局部变量就成了对象内的值域, 然后你可以在同一个对象中将这个大型函数分解为数个小型函数</p>

<pre>
class Order
  def price
    primary_base_price = 0
    secondary_base_price = 0
    tertiary_base_price = 0
    # long computation
  end
end
</pre>

<p>&nbsp;</p>
<h3>Moving Features Between Objects</h3>
<p><span>Move Method: 搬移函数</span></p>
<p>description: 程序中，有个函数与其所驻之外的另一个class进行更多交流,在该函数最常引用的class中建立一个有着类似行为的新函数,将旧函数变成一个单纯的委托函数，或是将旧函数完全移除</p>
<p>&nbsp;</p>
<p><span>Move Field: 搬移值域</span></p>
<p>description: 程序中,某个field被其所驻class之外的另一个class更多地用到,在target class 建立一个new field，修改source field的所有用户，令它们改用此new field</p>
<p>&nbsp;</p>
<p><span>Extract Class: 提炼类</span></p>
<p>description: 某个class做了应该由两个classes做的事,建立一个新class，将相关的值域和函数从旧class搬移到新class</p>
<p>&nbsp;</p>
<p><span>Inline Class: 将类内联化</span></p>
<p>description: 某个class没有做太多事情,将class的所有特性搬移到另一个class中，然后移除原class</p>
<p>&nbsp;</p>
<p><span>Hide Delegate: 隐藏委托关系</span></p>
<p>description: 客户直接调用其server object的delegate class,在server端建立客户所需的所有函数，用以隐藏委托关系</p>
<p>&nbsp;</p>
<p><span>Remove Middle Man: 移除中间人</span></p>
<p>description: 某个class做了过多的简单委托动作,让客户直接调用delegate</p>

<p>&nbsp;</p>
<h3>Simplifying Conditional Expressions</h3>
<p><span>Decompose Conditional: 分解条件式</span></p>
<p>description: 从if、then、else 三个段落中分别提炼出独立函数</p>

<pre>
if date < SUMMER_START || date > SUMMER_END
  charge = quantity * @winter_rate + @winter_service_charge
else
  charge = quantity * @summer_rate
end

#refactoring code
if not_summer(date)
  charge = winter_charge(quantity)
else
  charge = summer_charge(quantity)
end
def not_summer(date)
  date < SUMMER_START || date > SUMMER_END
end
def winter_charge(quantity)
  quantity * @winter_rate + @winter_service_charge
end
def summer_charge(quantity)
  quantity * @summer_rate
end
</pre>

<p>&nbsp;</p>
<p><span>Recompose Conditional</span></p>
<p>description: You have conditional code that is unnecessarily verbose and does not use the most readable Ruby construct.</p>

<pre>
parameters = params ? params : []

#refactoring code
parameters = params || []

----------------------------------

def reward_points
  if days_rented > 2
    2
  else
    1
  end
end

#refactoring code
def reward_points
  return 2 if days_rented > 2
  1
end
</pre>

<p>&nbsp;</p>
<p><span>Consolidate Conditional Expression: 合并条件式</span></p>
<p>description: 一系列条件测试，都得到相同结果,将这些测试合并为一个条件式，并将这个条件式提炼成为一个独立函数</p>

<pre>
def disability_amount
  return 0 if @seniority < 2
  return 0 if @months_disabled > 12
  return 0 if @is_part_time
  # compute the disability amount
  ...
end

#refactoring code 
def disability_amount
  return 0 if ineligable_for_disability?
  # compute the disability amount
  ...
end
def ineligible_for_disability?
  @seniority < 2 || @months_disabled > 12 || @is_part_time
end
</pre>

<p>&nbsp;</p>
<p><span>Remove Control Flag: 移除控制标记</span></p>
<p>description: 一系列布尔表达式中，某个变量带有控制标记的作用,以break 语句或return 的语句取代控制标记</p>

<pre>
#break
--------------------------
def check_security(people)
  found = false
  people.each do |person|
    unless found
      if person == "Don"
        send_alert
        found = true
      end
      if person == "John"
        send_alert
        found = true
      end
    end
  end
end

#refactoring code
def check_security(people)
  people.each do |person|
    if person == "Don"
      send_alert
      break
    end
    if person == "John"
      send_alert
      break
    end
  end
end

#return
-------------------------
def check_security(people)
  found = ""
  people.each do |person|
    if found == ""
      if person == "Don"
        send_alert
        found = "Don"
      end
      if person == "John"
        send_alert
        found = "John"
      end
    end
  end
  some_later_code(found)
end

#refactoring code
def check_security(people)
  found = found_miscreant(people)
  some_later_code(found)
end
def found_miscreant(people)
  people.each do |person|
    if person == "Don"
      send_alert
      return "Don"
    end
    if person == "John"
      send_alert
      return "John"
    end
  end
  ""
end
</pre>

<p>&nbsp;</p>
<p><span>Replace Nested Conditional with Guard Clauses: 以卫语句取代嵌套条件式</span></p>
<p>description: 函数中的条件逻辑使人难以看清正常的执行路径,使用卫语句(guard clauses)表现所有特殊情况</p>

<pre>
def pay_amount
  if @dead
    result = dead_amount
  else
    if @separated
      result = separated_amount
    else
      if @retired
        result = retired_amount
      else
        result = normal_pay_amount
      end
    end
  end
  result
end

#refactoring code
def pay_amount
  return dead_amount if @dead
  return separated_amount if @separated
  return retired_amount if @retired
  normal_pay_amount
end
</pre>

<p>&nbsp;</p>
<p><span>Replace Conditional with Polymorphism: 以多态取代条件式</span></p>
<p>description: 有个条件式根据对象型别的不同而选择不同的行为,可以将这个条件式的每个分支放进一个subclass内的覆写函数中</p>

<pre>
module MountainBike...
  def price
    case @type_code
      when :rigid
        (1 + @commission) * @base_price
      when :front_suspension
        (1 + @commission) * @base_price + @front_suspension_price
      when :full_suspension
        (1 + @commission) * @base_price + @front_suspension_price +
        @rear_suspension_price
    end
  end
  #...
end
class RigidMountainBike
  include MountainBike
  #...
end
class FrontSuspensionMountainBike
  include MountainBike
  #...
end
class FullSuspensionMountainBike
  include MountainBike
  #...
end

#refactoring code            
#reduces the dependencies in your system and makes it easier to update
class RigidMountainBike
  include MountainBike
  def price
    (1 + @commission) * @base_price
  end
end
class FrontSuspensionMountainBike
  include MountainBike
  def price
    (1 + @commission) * @base_price + @front_suspension_price
  end
end
class FullSuspensionMountainBike
  include MountainBike
  def price
    (1 + @commission) * @base_price + @front_suspension_price +
    @rear_suspension_price
  end
end
module MountainBike...
/*remove
  def price
    case @type_code
      ...
    end
  end
*/
end
</pre>

<p>&nbsp;</p>
<p><span>Introduce Null Object: 引入Null对象</span></p>
<p>description: 需要再三检查某物是否为null value时,可以将null value替换为null object</p>

<pre>
class Site
  attr_reader :customer
  #...
end
class Customer...
  attr_reader :name
  #...
end

customer = site.customer
customer_name = customer ? customer.name : 'occupant'

#refactoring code
class Customer
  def self.new_missing
    MissingCustomer.new
  end
end
class Site
  def customer
    @customer || Customer.new_missing
  end
end
class MissingCustomer
  def name
  'occupant'
  end
end

customer = site.customer
customer_name = customer.name
</pre>

<p>&nbsp;</p>
<h3>Making Method Calls Simpler</h3>
<p><span>Parameterize Method: 令函数携带参数</span></p>
<p>description: 若干函数做了类似的工作，但在函数本体中却包含了不同的值,可以建立单一函数，以参数表达那些不同的值</p>

<pre>
def base_charge
  result = [last_usage, 100].min * 0.03
  if last_usage > 100
  result += ([last_usage, 200].min - 100) * 0.05
  end
  if last_usage > 200
  result += (last_usage - 200) * 0.07
  end
  Dollar.new(result)
end
def last_usage
  #...
end

#refactoring code
def base_charge
  result = (usage_in_range 0..100) * 0.03
  result += (usage_in_range 100..200) * 0.05
  result += (usage_in_range 200..last_usage) * 0.07
  Dollar.new(result)
end
def usage_in_range(range)
  if last_usage > range.begin
    [last_usage, range.end].min - range.begin
  else
    0
  end
end
</pre>

<p>&nbsp;</p>
<p><span>Replace Parameter with Explicit Methods: 以明确函数取代参数</span></p>
<p>description: 有一个函数，其内完全取决于参数值而采取不同反应,可以针对该参数的每一个可能值，建立一个独立函数</p>

<pre>
class Employee
  ENGINEER = 0
  SALESPERSON = 1
  MANAGER = 2
  def self.create(type)
    case type
      when ENGINEER
        Engineer.new
      when SALESPERSON
        Salesperson.new
      when MANAGER
        Manager.new
      else
        raise ArgumentError, "Incorrect type code value"
    end
  end
end

#refactoring code
class Employee
  def self.create_engineer
    Engineer.new
  end
  def self.create_salesperson
    Salesperson.new
  end
  def self.create_manager
    Manager.new
  end
end

kent = Employee.create(Employee::ENGINEER)
#replace to
kent = Employee.create_engineer
</pre>

<p>&nbsp;</p>
<p><span>Preserve Whole Object: 保持对象完整</span></p>
<p>description: 从某个对象中取出若干值，将它们作为某一次函数调用时的参数,改为传递整个对象</p>

<pre>
class Room
  def within_plan?(plan)
    low = days_temperature_range.low
    high = days_temperature_range.high
    plan.within_range?(low, high)
  end
end
class HeatingPlan
  def within_range?(low, high)
    (low >= @range.low) && (high <= @range.high)
  end
end

#refactoring code
class Room
  def within_plan?(plan)
    plan.within_range?(days_temperature_range)
  end
end
class HeatingPlan
  def within_temperature_range?(room_temperature_range)
    @range.includes?(room_temperature_range)
  end
end
class TempRange
  def includes?(temperature_range)
    temperature_range.low >= low && temperature_range.high <= high
  end
end
</pre>

<p>&nbsp;</p>
<p><span>Replace Parameter with Method: 以函数取代参数</span></p>
<p>description: 对象调用某个函数，并将所得结果作为参数，传递给另一个函数,而接受该参数的函数也可以调用前一个函数。可以让参数接受者去除该项参数，并直接调用前一个函数</p>

<pre>
def price
  base_price = @quantity * @item_price
  level_of_discount = 1
  level_of_discount = 2 if @quantity > 100
  discounted_price(base_price, level_of_discount)
end
def discounted_price(base_price, level_of_discount)
  return base_price * 0.1 if level_of_discount == 2
  base_price * 0.05
end

#refactoring code
def price
  return base_price * 0.1 if discount_level == 2
  base_price * 0.05
end
def base_price
  @quantity * @item_price
end
def discount_level
  return 2 if @quantity > 100
  return 1
end
</pre>

<p>&nbsp;</p>
<p><span>Introduce Parameter Object: 引入参数对象</span></p>
<p>description: 某些参数总是很自然地同时出现,以一个对象取代这些参数</p>

<pre>
class Account
  def add_charge(base_price, tax_rate, imported)
    total = base_price + base_price * tax_rate
    total += base_price * 0.1 if imported
    @charges << total
  end
  def total_charge
    @charges.inject(0) { |total, charge| total + charge }
  end
end

account.add_charge(5, 0.1, true)
account.add_charge(12, 0.125, false)
total = account.total_charge

#refactoring step 1
class Charge
  attr_accessor :base_price, :tax_rate, :imported

  def initialize(base_price, tax_rate, imported)
    @base_price = base_price
    @tax_rate = tax_rate
    @imported = imported
  end
end
class Account
  def add_charge(tax_rate, imported, charge)
    total = charge.base_price + charge.base_price * tax_rate
    total += charge.base_price * 0.1 if imported
    @charges << total
  end
  def total_charge
    @charges.inject(0) { |total, charge| total + charge }
  end
end

account.add_charge(0.1, true, Charge.new(9.0, nil, nil))
account.add_charge(0.125, true, Charge.new(12.0, nil, nil))
total = account.total_charge

#refactoring step 2
class Charge
  attr_accessor :base_price, :tax_rate, :imported

  def initialize(base_price, tax_rate, imported)
    @base_price = base_price
    @tax_rate = tax_rate
    @imported = imported
  end
end
class Account
  def add_charge(charge)
    total = charge.base_price + charge.base_price * charge.tax_rate
    total += charge.base_price * 0.1 if charge.imported
    @charges << total
  end
  def total_charge
    @charges.inject(0) { |total, charge| total + charge }
  end
end

account.add_charge(Charge.new(9.0, 0.1, true))
account.add_charge(Charge.new(12.0, 0.125, true))
total = account.total_charge

#refactoring step 3
class Account
  def add_charge(charge)
    @charges << charge
  end
  def total_charge
    @charges.inject(0) do |total_for_account, charge|
    total_for_account + charge.total
  end
end
class Charge
  def initialize(base_price, tax_rate, imported)
    @base_price = base_price
    @tax_rate = tax_rate
    @imported = imported
  end
  def total
    result = @base_price + @base_price * @tax_rate
    result += @base_price * 0.1 if @imported
    result
  end
end

account.add_charge(Charge.new(9.0, 0.1, true).total)
account.add_charge(Charge.new(12.0, 0.125, true).total)
total = account.total_charge
</pre>

<p>&nbsp;</p>
<p><span>Hide Method: 隐藏某个函数</span></p>
<p>description: 有一个函数，从来没有被其他任何class用到,可以将这个函数修改为private</p>

<p>&nbsp;</p>
<p><span>Replace Constructor with Factory Method: 以工厂函数取代构造函数</span></p>
<p>description: 希望在创建对象时不仅仅是对它做简单的构建动作,可以将constructor替换为factory method</p>

<pre>
class ProductController
  def create
    @product =
      if imported
        ImportedProduct.new(base_price)
      else
        if base_price > 1000
          LuxuryProduct.new(base_price)
        else
          Product.new(base_price)
        end
      end
  end
end
class Product
  def initialize(base_price)
    @base_price = base_price
  end
  def total_price
    @base_price
  end
end
class LuxuryProduct < Product
  def total_price
    super + 0.1 * super
  end
end
class ImportedProduct < Product
  def total_price
    super + 0.25 * super
  end
end

#refactoring code
class ProductController
  def create
    @product = Product.create(base_price, imported)
  end
end
class Product
  def self.create(base_price, imported=false)
    if imported
      ImportedProduct.new(base_price)
    else
      if base_price > 1000
        LuxuryProduct.new(base_price)
      else
        Product.new(base_price)
    end
  end
end
</pre>

<p>&nbsp;</p>
<p><span>Replace Error Code with Exception: 用异常取代错误码</span></p>
<p>description: 希望在创建对象时不仅仅是对它做简单的构建动作,可以将constructor替换为factory method</p>
