# Intro to OOP

Uncle Bob, describes the three programming paradigms as follows:

*  `Structured programming impsoes discipline on direct transfer of control.`
*  `Functional programming imposes discipline upon assignment.`
*  `Object oriented programming imposes discipline upon assignment.`

More importantly, he posits that OOP has three main requirements:

  1. Encapsulation
  * `"The reason encapsulation is cited as part of the definiton of OO is that OO languages provide easy and effective encapsulation of data and function. As a result, a line can be drawn around a cohesive set of data and functions. Outside of that line, the data is hidden adn only some of the functions are known. We see this concept in action as the private data members and the public member functions of a class."`
  2. Polymorphism
  3. Inheritance
  * `"Inheritance is simply the redeclaration of a group of variable and functions within an enclosing scope."`


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

From Sandy Metz's `Practical Object Oriented Design in Ruby`:

    "Inheritance is, at its core, a mechanism for automatic message delgation. It defines a forwarding path for not-understood messages. It creaes relationships such that, if one object cannot respond to a received message, it delegates that message to another. You don't have to write code to explicitly delegate the message, instead you define an inheritance relationship between two objects and the forwarding happens automatically."

## Javascript implementation:

Recall, in js:

* ` "Every object is linked to a prototype object form which it can inherit properties."`
* ` "The prototype link has no effect on updating. When we make changes to an object, the object's prototype is not touched"`
* `"The protype link is used only in retrieval."`
* `"If we try to retrieve a property value from an object, and if the object lacks the property name, then JavaScript attempts to retrieve the property value from the prototype object. And if that object is lacking the property, then it goes to its prototype, and so on until the process finally bottoms out with Object.prototype."`
* `"The prototype relationship is a dynamic relationship. if we add a new property ot a prototype, that property will immediately be visible in all of the objects taht are based on that prototype"`


"In a purely prototypal pattern, we dispense with classes. We focus instead on the objects."

From Sandy Metz's `Practical Object Oriented Design in Ruby`:

    "Inheritance provides a way to define two objects as having a relationship such that when the first receives a message that it dos not understand, it automatically forwards, or delegates, the message to the second."

Javascript does the same with the prototype link.

Crockford's example:

```javascript
var myMammal = {
  name: 'Herb the Mammal',
  get_name: function() {
    return this.name;
  },
  says: function() {
    return this.saying || '';
  }
}
```

Employing a `Object.create(newObj)` method from previous chapters which is meant to mimic a constructor and inheritance:

```javascript
var myCat = Object.create(myMammal);
myCat.name = 'Henrietta';
myCat.saying = 'meow';
myCat.purr = function(n) {
  var i, s = '';
  for (i = 0; i < n; i+= 1) {
    if (s) {
      s += '-';
    }
    s += 'r';
  }

  return s;
};

myCat.get_name = function() {
  return this.sasy() + ' ' + this.name + ' ' _ this.says();
}
```

Crockford classifies this as *differential inheritance*.

    "By customizing a new object, we specify the differences from the object on which it is based."

SOUND FAMILIAR????

Crockford's implementation of encapsulation within JS:

```javascript
var mammal = function(spec) {
  var newObj = {};

  newObj.get_name = function() {
    return spec.name;
  };

  newObj.says = function() {
    return spec.saying || '';
  };

  return newObj;
};

var myMammal = mammal({ name: 'Herb' });
```

Using this example, we can redefine our cat constructor as such:

```javascript
var cat = function(spec) {
  spec.saying = spec.saying || 'meow';
  var newObj = mammal(spec);
  // mammal constructor does most of the work of object creation. Cat constructor only has to worry about the differences
  newObj.purr = function(n) {
    var i, s = '';
    for (i = 0; i < n; i+= 1) {
      if (s) {
        s += '-';
      }
      s += 'r';
    }

    return s;
  };

  newObj.get_name = function() {
    return newObj.says() + ' ' + spec.name + ' ' + newObj.says();
  };

  return newObj;
};

var myCat = cat({ name: 'Henrietta' });
```

What about Super methods? Just wait, Crockford, without a predefined super method for accessing properties of the parent class, creates his own functional approach by created a new `superior` method on the Object prototype:

```javascript
Object.method('superior', function (name) {
  var that = this, method = that[name];

  return function() {
    return method.apply(that, arguments);
  }
})
```

```javascript
var coolCat = function(spec) {
  var that = cat(spec), super_get_name = that.superior('get_name');

  that.get_name = function(n) {
    return 'like ' + super_get_name() + ' baby';
  };

  return that;
}
```

This last example has better encapsulation and access to super methods.

"If all of the state of an object is private, then the object is tamper-proof. Properties of the object can be replaced or deleted, but the integrity of the object is not compromised. If we create an object in the functional style, and if all of the methods of the obejct make no use of this or that, then the object is durable. A durable object is simply a collectin of functions that act as capabilities."

