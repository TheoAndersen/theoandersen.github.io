---
layout: post
title: "Practicing code kata's with code constraints"
comments: true
published: true
---
A year ago I did some Test Driven Development training at work, by doing three two-hour mini code-retreats. The idea was that I would lecture as little as possible, focusing on trying it out instead. TDD is a practical technique, so you will learn quicker, if you practice it it in pairs on a given assignment, rather than following the concept only in slides. 
Each session started with some short theory on the given TDD subtopic of the day and then introduced an assignment, which was carried out in pairs in the style of [Code retreats](http://coderetreat.org/about) (inspired by the work on the subject by Corey Haines and Joe Reinsberger). Before each assignment, I would code a smaller example in the same style as the assignment, on the screen in the room.

The thing I discovered in these sessions, was that using code constrains on top of the normal TDD kata's, is nice learning tool.

Code constraints makes kata's more fun
-------------------------------
A code constraint is an idea of constraining your coding, when doing a code kata (small code assignment), to force the code to ad-hear to some restriction. This can restrict your code in different ways as limiting the number number of lines in methods, or making you avoid specific language constructs (maybe static?) and so on. The benefit of doing this, is that it can make you see other ways to program, than you normally would - and this I've found, can be a really unique learning experience.

I discovered code constraints from reading Corey's ["Understanding the four rules of simple design"](https://leanpub.com/4rulesofsimpledesign) book and researching code retreats, in which it seems to be normal practice.

I have found that Corey's code constraints as explained in his book, will force you to try different coding-styles that you might not be used to, and in some examples force you to write the code more object oriented (or functional). Trying a kata with code constraints, seems to force you to code in a certain way. In particular one of the described constraints seemed to force the code into a more object oriented form which, even though it did seem like overkill for production code, gave me a change in my way of thinking about abstractions in the code.

The rest of this post is an explanation of this constraints along with some of the OO techniques it seems to foster. Please note that while I am investigating code constraints with the context of object oriented programming here, the same concept can be used with functional languages as well.

"No primitives across method boundaries".
-----------------------------------------
The constraint "No primitives across method boundaries" means that, separate from the constructor (which isn't really a 'normal' method), you cannot have a method that returns or is passed a primitive (int, string etc.). You have to use objects.

So this way you're forced to take the Primitive Obsession code smell seriously, and use more objects.

Coding a kata using TDD, this will probably give you some additional work in each refactoring step, making the code ad-hear to the constraint. 

Remember that the second step, is to only make the tests pass:

> "Green -- make test work as quickly as possible, committing whatever sins necessary in the process"  -- Test Driven Development, Kent Beck.

So its okay to forget about the constraint, while making the tests pass, and then fix it while refactoring afterwards, before the next test.

The following is showing a small example in ruby, to show one of the problems i encountered while working on the Bowling kata with this code constraint.

Value object to avoid primitive obsession
------------------------------------------
Doing the bowling kata, the first problem I encountered, was how to represent the typical primitive values of my implementation in an object. In the Bowling kata, its the representation of the _pins_ a player knocks down, and in the typical Code Retreat kata "Conways game of life" it might be the _position_ of a cell.

These are value representations, indicating a number that is not an entity, but something that only makes sense based on its primitive values. In the code-example (ruby) below, you can see them as the number of pins thrown `throw(1)` and the resulting score. How can we represent this, other than just a basic int?

{% highlight ruby %}
def test_aGameOfOnly1s
  game = Game.new()
  20.times do
    game.throw(1)               # <-- constraint violation - passing 1
  end
  assert_equal(20, game.score)  # <-- contraint violation - passing 20
end
{% endhighlight %}

The solution to this, could be a [Value Object](http://martinfowler.com/eaaCatalog/valueObject.html). 

A Value object is created by encapsulating the primitive inside an object with a name (Pins in this example) and making sure that two objects with the same value are equal (normally by overriding the equals method). This could look something like this.

{% highlight ruby %}
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
{% endhighlight %}

In this way we can use the object as it were a primitive that fits the domain, and still use the logical operations on it just as with primitives.
Notice how we create a new _pins_ object with the value passed to the constructor, instead of just passing the value directly.

The benefits of this is multiple. First of you cant mistakingly, use a wrong value from another type, because the methods expect the specific type of _Pins_ instead of _int_ now. I've actually seem pretty big messes created because of exactly these kinds of problems, where two separate status'es were identified by ints, and somebody mistakingly changed the arguments. Another important benefit is that we now have a home for the meaning of a pin, this means that we later will be able to add more functionality to it, if something related to the pin comes along. 

Polymorphism to control state
-----------------------------
Another example comes from the [Conways game of life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) kata (now c#). Here I had an object, which returned a simple bool, representing the state the object was in. In this particular example it was the notion of a Cell being alive or dead. Now returning a bool doesn't ad-hear to the constraint, so I had to do something about it. But how do we get around this kind of state, and avoid the bool?

The example with the violation is the following code.

{% highlight csharp %}
[Test]
public void GetCellAtShouldOnlyReturnLivingCellsWhenSet()
{
  var world = new World();
  world.SetLiveCellAt(new Location(99, 99));
  Assert.AreEqual(false, world.GetCellAt(new Location(1, 1)).IsAlive);
  Assert.AreEqual(true, world.GetCellAt(new Location(99, 99).IsAlive);
}
{% endhighlight %}

Its the ```IsAlive()``` method which has the problem. It cant be allowed to return the bool, but then how can we model if the cell is alive or not?

What if we extend Cell, so that Cell is an abstract class, which can either be instantiated as an AliveCell or a DeadCell object?. This way the `bool IsAlive()` method dissapeares, as the type of the Cell would give the information of the cell being alive or dead - making it possible to return the subtype of the cell instead.

The result looks something like this.

{% highlight csharp %}
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
{% endhighlight %}

This way make the type of the object, help control the state. Notice how we are modeling the problem with more objects than we did earlier? Even though we used inheritance, which can often be a to hard construct, I like how the objects are beginning to model the domain, more than just having a World and a simple Cell class.

Immutable can be simpler than mutable
-------------------------------------

Last thing I did in my kata, was to make some of the objects immutable rather than mutable. Now this wasn't really because of the constraint, but it did seem to simplify the code. 

The World class in Conway's game of life, had a `void Tick()` method, which was calculating the next version of the world. Doing this its altering the current world to get to the state of the next, which quickly became a bit complex. I found it simpler to make the World immutable, so that the method now didn't affect the current World but returned a new one like this `World Tick()`.

Try experimenting with constraints while doing these small coding exercises. These days there a lot of online code kata web-sites, which makes it easy to code and try it out in the browser. I just wish some of them would implement the functionality to give you an error if you don't ad-hear to a specific constraint.

References
----------
<ul>
  <li><a href="https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life">Conways game of life on wiki</a></li>
  <li><a href="https://leanpub.com/4rulesofsimpledesign">Understanding the Four Rules of Simple Design - Corey Haines</a></li>
  <li><a href="https://github.com/TheoAndersen/GameOfLifeKata">Example GameOfLife kata implementation</a></li>
  <li><a href="https://github.com/TheoAndersen/BowlingKataRuby">Example Bowling kata implementation</a></li>
</ul>






