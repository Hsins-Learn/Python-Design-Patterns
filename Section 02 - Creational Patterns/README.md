# Section 02: Creational Patterns

- [Section 02: Creational Patterns](#section-02-creational-patterns)
  - [Factory](#factory)
    - [Problems](#problems)
    - [Scenario](#scenario)
    - [Example](#example)
  - [Abstract Factory](#abstract-factory)
    - [Problems](#problems-1)
    - [Scenario](#scenario-1)
    - [Example](#example-1)
  - [Singleton](#singleton)
    - [Problems](#problems-2)
    - [Scenario](#scenario-2)
    - [Example](#example-2)
  - [Builder](#builder)
    - [Problems](#problems-3)
    - [Scenario](#scenario-3)
    - [Example](#example-3)
  - [Prototype](#prototype)
    - [Problems](#problems-4)
    - [Scenario](#scenario-4)
    - [Example](#example-4)

## Factory

### Problems

**The Factory Pattern** encapsulates object creation. That is, a **Factory** is an object specializing in creating other objects.

- **Uncertain Types of Objects**: when we're not sure about what type of objects we'll be needing eventually in our system.
- **Decisions to Be Made at Runtime**: the situation in which our application needs to decide on what class to use at run time.

### Scenario

Our pet shop was only selling dogs but nowwe need to sell cats too. Therefore, our system needs to be able to handle cats as well as dogs. The system supposed to show how each of the pets we sell speak.

### Example

```python
class Dog:
    """ A simple dog class """
    def __init__(self, name):
        self._name = name

    def speak(self):
        return "Woof!"


class Cat:
    """ A simple cat class """
    def __init__(self, name):
        self._name = name

    def speak(self):
        return "Meow!"

def get_pet(pet="dog"):
    """ The Factory Method"""
    pets = dict(dog=Dog("Hope"), cat=Cat("Peace"))

    return pets[pet]

dog = get_pet("dog")
print(dog.speak())

cat = get_pet("cat")
print(cat.speak())
```

Since we have the `get_pet()` method, which is our factory method, the addition of new types such as `Cat` is really easy. Moreover, as the user of the `get_pet()` method, we don't really see what's going on in terms of adding all these additional new types of objects.

Note that the factory pattern we implemented here is slightly different from the factory method in a typical programming language like C++ or Java. We're trying to fully take advantage of the features of Python.

<br/>
<div align="right">
  <b><a href="#section-02-creational-patterns">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Abstract Factory

### Problems

- **The User Expectation Yields Multiple, Related Objects**: when a client expects to receive a family of related objects at a given time, but don't have to know which family it is until run time.

### Scenario

We'll first build a pet factory whose concrete factories include Dog factory and Cat factory. Both dog and cat factories produce related products such as dog food and cat food. Our solution would be consists of:

- **Abstract Factory**: Pet factory
- **Concrete Factory**: Cat factory, Dog factory
- **Abstract Product**
- **Concrete Product**: dog and dog food; cat and cat food

### Example

```python
class Dog:
    """ One of the objects to be returned """
    def speak(self):
        return "Woof!"

    def __str__(self):
        return "Dog"


class DogFactory:
    """ Concrete Factory """
    def get_pet(self):
        """ Returns a Dog object """
        return Dog()

    def get_food(self):
        """ Returns a Dog Food object """
        return "Dog Food!"


class PetStore:
    """ PetStore houses our Abstract Factory """
    def __init__(self, pet_factory=None):
        """ pet_factory is our Abstract Factory """
        self._pet_factory = pet_factory

    def show_pet(self):
        """ Utility method to display the details of the objects returned by the DogFactory """
        pet = self._pet_factory.get_pet()
        pet_food = self._pet_factory.get_food()

        print("Our pet is '{}'!".format(pet))
        print("Our pet says hello by '{}'".format(pet.speak()))
        print("Its food is '{}'!".format(pet_food))


# Create a Concrete Factory
factory = DogFactory()

# Create a pet store housing our Abstract Factory
shop = PetStore(factory)

# Invoke the utility method to show the details of our pet
shop.show_pet()
```

We implement our Abstract Factory without using inheritance, mainly because Python is a dynamically typed language and therefore doesn't require abstract classes. The Abstract Factory is related to factory method and the Concrete Factories are often singletons.

Finally, if we want to add another concrete factory such as a Cat Factory, adding that new concrete factory is not a big problem. That's one of the reason why we use The Abstract Factory Pattern.

<br/>
<div align="right">
  <b><a href="#section-02-creational-patterns">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Singleton

### Problems

### Scenario

### Example

```python
class Borg:
    """Borg class making class attributes global"""
    _shared_state = {}                      # attribute dictionary
    def __init__(self):
        self.__dict__ = self._shared_state  # make it an attribute dictionary

class Singleton(Borg):                      # inherit from the Borg class
    """This class now shares all its attributes among its various instances"""
    def __init__(self, **kwargs):
        Borg.__init__(self)
        self._shared_state.update(kwargs)

    def __str__(self):
        return str(self._shared_state)


```

<br/>
<div align="right">
  <b><a href="#section-02-creational-patterns">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Builder

### Problems

- The Builder Pattern is a solution to an Anti-Pattern called Telescoping Constructor.
- Telescoping Constructor occurs when software developer attempts to build a complex object using an excessive number constructors.

### Scenario

Let's assume that we are building a car. It require the various car parts to be first constructed individually and then assembled.

The Builder partitions the process of building a complex object into the four different roles and steps:

- **Director** is in charge of actually building a product using the builder object.
- **Abstract Builder (Interfaces)** provides all the necessary interfaces required in building an object.
- **Concrete Builder (Implements the interfaces)** class inherits from the Abstract Builder class and actually implements the details of the interfaces of the Abstract Builder class, for a specific type of a product.
- **Product** represents an object being built.

The Builder Pattern does not rely on polymorphism, unlike Factory and Abstract Factory. The focus of the Builder Pattern is rather on reducing the complexity of building a complex object through a divide and conquer strategy.

### Example

```python
class Director():
    """Director"""
    def __init__(self, builder):
        self._builder = builder

    def construct_car(self):
        pass

    def get_cat(self):
        pass


class Builder():
    """Abstract Builder"""
    def __init__(self):
        self.car = None
    
    def create_new_car(self):
        self.car = Car()


class SkyLarkBuilder(Builder):
    """Concrete Builder: provides parts and tools to work on the parts"""
    def add_model(self):
        self.car.model = "Skylark"

    def def_tries(self):
        self.car.tries = "Regular tries"


class Car():
    """Product"""
    def __init__(self):
        self.model = None
        self.tries = None
        self.engine = None

    def __str__(self):
        return f'{self.model} | {self.tries} | {self.engine}'
```

<br/>
<div align="right">
  <b><a href="#section-02-creational-patterns">[ ↥ Back To Top ]</a></b>
</div>
<br/>

## Prototype

### Problems

- The Prototype Pattern clones objects according to a prototypical instance.
- It's especially useful when creating many identical objects individually.

### Scenario

Let's assume that we are building a car. We can mass produce cars if the cars have the same color and the same options. So in this case, we can simply clone the objects instead of creating the individual objects one at a time.

The solution in this case consists of:

- Create a Prototypical Instance
- Simply Clone it Whenever We Need a Replica

### Example

```python
import copy


class Prototype:
    def __init__(self):
        self._objects = {}

    def register_object(self, name, obj):
        """ Register an Object """
        self._objects[name] = obj

    def unregister_object(self, name):
        """ Unregister an Object """
        del self._objects[name]

    def clone(self, name, **attr):
        """ Clone a Registered Object and Update Its Attributes """
        obj = copy.deepcopy(self._objects.get(name))
        obj.__dict__.update(attr)
        return obj


class Car:
    def __init__(self):
        self.name = "Skylark"
        self.color = "Red"
        self.options = "Ex"

    def __str__(self):
        return '{} | {} | {}'.format(self.name, self.color, self.options)


car = Car()
prototype = Prototype()
prototype.register_object("skylark", car)

car1 = prototype.clone("skylark")
print(car1)
```

<br/>
<div align="right">
  <b><a href="#section-02-creational-patterns">[ ↥ Back To Top ]</a></b>
</div>
<br/>