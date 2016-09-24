we have a kitten class that looks like this:

Kitten.cs
class Kitten
{
  string _name;
  public Kitten(string name)
  {
    _name = name;
  }
  public string GetName()
  {
    return _name;
  }
}
If we ran this assertion:

[Fact]
public void KittiesAreTheSame()
{
  //Arrange
  Kitten firstKittie = new Kitten("Squeakers");
  Kitten secondKittie = new Kitten("Squeakers");

  Assert.Equal(firstKittie, secondKittie);
}
The test would fail. In our eyes both objects are representing the same cuddly little kitten but C# sees two different instances of Kitten.

This causes problems when using databases. Often we'll grab some information out of a database and then use it to instantiate a new object. If we repeat that process at different times, we usually want to treat those objects as equivalent. But they aren't equivalent by default.

Fortunately, we can override this behavior. Here's how:

Kitten.cs
class Kitten
{
...
  public override bool Equals(System.Object otherKitten)
  {
    Kitten otherKitten = (Kitten) otherKitten;
    return this.GetName().Equals(otherKitten.GetName());
  }
...
}
This includes a few new concepts, so let's walk through what's going on very carefully.

When we write Assert.Equal(firstKittie, secondKittie);, the method Equal() comes from a set of default behaviors that all objects have. If we want to override that behavior, we can use the override modifier.

Because the default Equals method accepts any type of object, when we override it, we need to use (System.Object otherKitten) for the argument to match the method signature. Then, we turn the object into a Kitten object with Kitten newKitten = (Kitten) otherKitten;. When we change an object from one type to another, it's called type casting.

Once we have the kitten object, we compare their names. If they're the same, we return true to indicate that they are the same kitten.

Here's a slightly more complex version that takes into account if somebody tries to compare a kitten with an object of another class:

Kitten.cs
public override bool Equals(System.Object otherKitten)
{
  if (!(otherKitten is Kitten))
  {
    return false;
  }
  else
  {
    Kitten newKitten = (Kitten) otherKitten;
    return this.GetName().Equals(newKitten.GetName());
  }
}

public override int GetHashCode()
{
     return this.GetName().GetHashCode();
}
