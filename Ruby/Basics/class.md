## Variable Scope Indicators



| Scope    | Def        |
| -------- | ---------- |
| Global   | $variable  |
| Class    | @@variable |
| Instance | @variable  |
| Local    | variable   |
| Block    | variable   |
|          |            |

## Attribute Methods



* attr_* methods
* attr_reader
* attr_writer
* attr_accessor



```ruby
attr_reader :name
# same as
def name
  @name
end

attr_writer :name
# same
def name=(value)
  @name = value
end

attr_accessor :name
# both
```



* Use `self` to reference the current instance from code inside the instance
* Add `self` when calling writer methods (self.first.name=)



## Class Methods

* Behaviors related to a class generally, not to a specific instance
* Called directly on the class, not on an instance
* Animal.new



## Class Attributes

* Properties related to a class genrally, not to a specific instance
* Stored in the class, not on an instance 



## Class Reader/Writer Methods

* Similar to instance reader/writer methods
* 

```ruby
class Animal
  @@species = ['cat', 'cow', 'dog', 'duck', 'horse', 'pig']
  
  def self.species
    @@species
  end
  
  def self.species=(array)
    return unless array.is_a?(Array)
    @@species = array
  end
end

```

## Access the superclass

```ruby
class Chef
  def make_dinner
    puts "Cook food."
  end
end

chef = Chef.new
chef.make_dinner
# Cook food.

class AmateurChef < Chef
  def make_dinner
    puts "Read recipe"
    super
    puts "Clean up mess"
  end
end

chefb = AmateurChef.new
chefb.make_dinner
# Read recipe.
# Cook food.
# Clean up mess.
```



```ruby
class Image
  attr_accessor :resizable
  
  def geometry
    "800x600"
  end
end

class ProfileImage < Image
  def initialize
    @resizable = true
  end
  
  def geometry
    @resizable ? "100x100" : super
  end
end
```

* Can assign return value of super to a variable
* Can pass arguments to super if parent class accepts arguements

























