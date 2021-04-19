# Hello, Dawn!

I recently stumbled upon this super-interesting language called [Dawn][dawn-lang]!

[dawn-lang]: https://www.dawn-lang.org/

## Void type

```
{ data Void }
```

## Unit type

```
{ data Unit { cons unit } }
```

```
>>> Unit
{$ Unit}
```

## Boolean

```
{ data Bool { cons T } { cons F } }
```

Apparently I don't yet understand how the pattern matching wildcard works:

```
>>> { fn and => { match { case T T => T } { case _ _ => F } } }
Error: type error: unification error: v0 occurs in v0 Bool Bool
```

But this works for conjunction:

```
{ fn and =>
  { match
    { case T T => T }
    { case T F => F }
    { case F T => F }
    { case F F => F }
  }
}
```

Let's define negation:

```
{ fn not =>
  { match
    { case T => F }
    { case F => T }
  }
}
```

And disjunction:

```
{ fn or =>
  { spread $x $y }
  { $x not pop }
  { $y not pop }
  and not
}
```

## Swap

```
{ fn swap => { spread $x $y } { collect $y $x } }
```

## Option type

```
{ data v1 Opt { cons None } { cons v1 Some } }
```

```
>>> None
{$ None}
>>> Some
{$ (None Some)}
>>> Unit Some
{$ (None Some) (Unit Some)}
```

## Natural numbers

```
{ data Nat { cons Z } { cons Nat S }}
```

```
>>> Z
{$ Z}
>>> S
{$ (Z S)}
>>> S
{$ ((Z S) S)}
```

```
>>> { data Nat { cons Z } { cons Nat S } }
>>> { data Bool { cons T } { cons F } }
>>> { fn zerop => { match { case Z => T } { case _ => F } } }
Error: type error: unification error: v0 occurs in v0 Nat
```

## Tree

```
{ data v1 Tree
  { cons v1 Leaf }
  { cons (v1 Tree) (v1 Tree) v1 Node } }
```

```
>>> Unit Leaf
{$ (Unit Leaf)}
>>> clone
{$ (Unit Leaf) (Unit Leaf)}
>>> Unit
{$ (Unit Leaf) (Unit Leaf) Unit}
>>> Node
{$ ((Unit Leaf) (Unit Leaf) Unit Node)}
>>> clone
{$ ((Unit Leaf) (Unit Leaf) Unit Node) ((Unit Leaf) (Unit Leaf) Unit Node)}
>>> Unit
{$ ((Unit Leaf) (Unit Leaf) Unit Node) ((Unit Leaf) (Unit Leaf) Unit Node) Unit}
>>> Node
{$ (((Unit Leaf) (Unit Leaf) Unit Node) ((Unit Leaf) (Unit Leaf) Unit Node) Unit Node)}
```
