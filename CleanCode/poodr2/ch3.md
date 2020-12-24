vimChapter 3 Managing Dependencies



## 3.1 Understanding Dependencies

```ruby
# Listing 3.1
class Gear

  attr_reader :chainring, :cog, :rim, :tire
  def initialize(chainring, cog, rim, tire)
    @chainring = chainring @cog = cog @rim = rim @tire = tire
  end
  def gear_inches 
    ratio * Wheel.new(rim, tire).diameter
  end
  def ratio 
    chainring / cog.to_f
  end
    # ...
end

class Wheel
  attr_reader :rim, :tire
  def initialize(rim, tire)
    @rim = rim
    @tire = tire
  end
  def diameter 
    rim + (tire * 2 )
  end
  # ...

end

puts Gear.new(52, 11, 26, 1.5).gear_inches # => 137.0909090909091
```



### 3.1.1 Recognizing Dependencies

An object has a dependency when it knows:

* The name of another class. Gear expects a class named `Wheel` to exist
* The name of a message that it intends to send to someone other than self.Gear expects a Wheel instance to respond to diameter
* The arguments that a message requires
  * Gear knows that `Wheel.new` requires a rim and a tire.
* 参数的顺序.

这些依赖会导致，Gear将会因为Wheel的改变，被迫改变。两个类间某种程度的依赖是不可避免的，毕竟，他们必须合作。但以上的那些依赖是可以避免的

Your design challenge is to manage dependencies so that each class has the fewest possible; a class should know just enough to do its job and not one thing more.

设计关键是管理依赖关系，使每个类的依赖关系尽可能少。一个类应该只知道他的工作就够了，而不是多做一件事。

### 3.1.2 Coupling Between Objects

