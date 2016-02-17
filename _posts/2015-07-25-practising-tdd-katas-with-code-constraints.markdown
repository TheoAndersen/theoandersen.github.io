---
layout: post
title: "Practising TDD Kata's with code constraints"
comments: true
---
I've lately been teaching Test Driven Development at work by doing mini code-retreats. The idea was that I would lecture as little as possible, to make room for the attendies getting their hands dirty. Being that TDD is a pretty practical technique, you learn quicker if you try it in pairs on a given assignment, rather than following the concept in slides. Each session started with some short theory on the given TDD subtopic of the day and then introduced an assignment, which was carried out in pairs in the style of [Code retreats](http://coderetreat.org/about) (inspired by the work on the subject by Corey Haines and Joe Reinsbergers).

Code constraints
-------------------------------
Reading Corey's "Understanding the four rules of simple design" I am intrigued by the idea of adding constraints to the assignments/katas. Practising TDD through Kata's can give you a feel for the technique, along with fx practicing to keep the refactorings in the refactoring step, as trying to figure out how to take smaller steps when you need to etc. But you don't really learn Object Oriented programming skills this way. By doing TDD you are still not forced to design the code object oriented. TDD is simply a method to separate the "making it work" part from "making it nice" part whilst making it possible to better control the size of the steps you take when implementing. This is a great technique to have in your belt when coding, but how do you then learn and practice the Object Oriented development skills to go hand in hand with TDD? Well, you can use code constraints.

I have found that Corey's code constraints as explained in his book, will force you to try different coding-styles that you might not be used to, and in some examples force you to write the code more object oriented. Trying the code constraints explained in Corey's book whilst doing a small assignment, seems to force you to code in a certain way. In particular one of the described constraints seemed to force the code into a more object oriented form which, even though it might be overkill for production code, gave me an aha-moment in my way of thinking about abstractions in the code.

The rest of this post is an explanation of this constraints along with some of the OO techniques it seems to foster.

"No primitives across method boundaries".
-----------------------------------------
The constraint "No primitives across method boundaries" means that, separate from the constructor (which isn't a normal method per say), you cannot have a method that returns or is passed a primitive. You have to use objects.
So this way you're forced to take the Primitive Obsession code smell seriously.

Now coding a kata using TDD, this will probably give you some additional work in each refactoring step, making the code ad-hear to the constraint. Remember that the second step is to make the tests pass:

> "Green -- make test work as quickly as possible, commiting whatever sins necessary in the process"  -- Test Driven Development, Kent Beck.

So its okay to forget about the constraint while making the tests pass if thats easier.

Value object to avoid primitive obsession
------------------------------------------
First problem i encountered, was how to represent the typical primitive values of my implementation in an object. In the Bowling kata, its the pins knocked down and in the typical Code Retreat kata "Conways game of life" it might be the position of a cell.

These are value representations, indicating a number that is not an entity. In the code-example (ruby) below you can see them as the number of pins thrown `throw(1)` and the resulting score. How can we represent this, other than just a basic int?

``` ruby
def test_aGameOfOnly1s
  game = Game.new()
  20.times do
    game.throw(1)
  end
  assert_equal(20, game.score)
end
```

The answer could be a Value Object. A Value object is created by encapsulating the primitive inside an object with a name (Pins in this example) and making sure that two objects with the same value are equal (normally by overriding a `equals()`).

``` ruby
class Pins
  protected
  attr_reader :pins

  public
  def initialize(pins)
    @pins = pins
  end

  def ==(otherPins)
    @pins == otherPins.pins
  end
end

def test_aGameOfOnly1s
  game = Game.new()
  20.times do
    game.throw(Pins.new(1))
  end
  assert_equal(Pins.new(20), game.score)
end
```

In this way we can use the object as it were a primitive that fits the domain, and still use the logical operations on it just as with primitives.

Polymorphism to control state
-----------------------------
In the Conways game of life kata (c#), i had an object, which returned a simple bool about the state the object was in. In this particular example it was the notion of a Cell being alive or dead. Now returning a bool dosen't adhear to the constaint, so i had to do something about it. But how to get around this kind of state?

``` csharp
[Test]
public void GetCellAtShouldOnlyReturnLivingCellsWhenSet()
{
  var world = new World();
  world.SetLiveCellAt(new Location(99, 99));
  Assert.AreEqual(false, world.GetCellAt(new Location(1, 1)).IsAlive);
  Assert.AreEqual(true, world.GetCellAt(new Location(99, 99).IsAlive);
}
```

One solution i found was to extend Cell so that Cell was an abstract class which could either be instantiated as an AliveCell or a DeadCell. This way the `bool IsAlive()` method dissapeared as the type of the Cell would give the information of the cell being alive or dead.

``` csharp
public abstract class Cell
{
  public Location Location { get; private set; }
            
  public Cell(Location location)
  {
    this.Location = location;
  }
}
        
public class AliveCell : Cell
{
  public AliveCell(Location location)
    : base(location)
  {
  }
}

public class DeadCell : Cell
{
  public DeadCell(Location location)
    : base(location)
  {
  }
}

[Test]
public void GetCellAtShouldOnlyReturnLivingCellsWhenSet()
{
  var world = new World();
  world.SetLiveCellAt(new Location(99, 99));
  Assert.IsInstanceOfType(typeof(DeadCell), world.GetCellAt(new Location(1, 1)));
  Assert.IsInstanceOfType(typeof(AliveCell), world.GetCellAt(new Location(99, 99)));
}
```

This way we include the type of the object to control the state. Other than this i like the way the objects are beginning to model the problem more than just having a World and a simple Cell class.

Immutable can be simpler than mutable
-------------------------------------

Last thing i did in my kata, was to make some of the objects immutable rather than mutable. Now this wasn't really because of the constraint, but rather to simplify the code. The World class in Conway's game of life, had a `void Tick()` method, which was calculating the next version of the world. Doing this its altering the current world to get to the state of the next, which can be a bit complex. I found it simpler to make the World immutable, so that at least this `Tick()` method didn't affect the current World object but returned a new one.

References
----------
<ul>
  <li><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">Conways game of life on wiki</a></li>
  <li><a href="https://leanpub.com/4rulesofsimpledesign">Understanding the Four Rules of Simple Design - Corey Haines</a></li>
  <li><a href="https://github.com/TheoAndersen/GameOfLifeKata">Example GameOfLife kata implementation</a></li>
  <li><a href="https://github.com/TheoAndersen/BowlingKataRuby">Example Bowling kata implementatino</a></li>
</ul>






