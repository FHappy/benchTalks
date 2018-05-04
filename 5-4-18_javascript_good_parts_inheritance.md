# Intro to OOP

Uncle Bob, describes the three programming paradigms as follows:

*  `Structured programming impsoes discipline on direct transfer of control.`
  * `"The reason encapsulation is cited as part of the
*  `Functional programming imposes discipline upon assignment.`
*  `Object oriented programming imposes discipline upon assignment.`

More importantly, he posits that OOP has three main requirements:

  1. Encapsulation
  2. Polymorphism
  3. Inheritance

# Inheritance?

Crockford notes that classical languages with rigid class structures implement inheritance differently than a loosely typed language such as Javascript that relies on the prototype chain.


## Classical implementation:

```ruby
class Mammal
  def breathe
    puts "inhale and exhale"
  end
end

class Cat < Mammal  # cat 'inherits' from mammal
  def speak
    puts "Meow"
  end
end

cathy = Cat.new
cathy.breathe # logs "inhale and exhale"
cathy.speak # logs "Meow"
```

```ruby
class Bird
  def preen
    puts "I am cleaning my feathers."
  end

  def fly
    puts "I am flying."
  end
end

class Penguin < Bird
  def fly
    puts "Sorry. I'd rather swim."
  end
end

pen_pal - Penguin.new
pen_pal.preen # logs "I am cleaning my feathers."
pen_pal.fly # logs "Sorry. I'd rather swim" => overrides parent class's fly method
```

## Javascript implementation:

Recall, in js:

* ` "Every object is linked to a prototype object form which it can inherit properties."`
* ` "The prototype link has no effect on updating. When we make changes to an object, the object's prototype is not touched"`
* `"The protype link is used only in retrieval."`
* `"If we try to retrieve a property value from an object, and if the object lacks the property name, then JavaScript attempts to retrieve the property value from the prototype object. And if that object is lacking the property, then it goes to its prototype, and so on until the process finally bottoms out with Object.prototype."`
* `"The prototype relationship is a dynamic relationship. if we add a new property ot a prototype, that property will immediately be visible in all of the objects taht are based on that prototype"`

