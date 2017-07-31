# Puppet containment (include vs require vs contain)

### Class example:

```
class example::one {
  notify { 'one' :}
}

class example::two {
  notify { 'two a' :}
  notify { 'two b' :}
}
```

If we run
```
puppet apply -e "include example::two"
```

This will create a catalogue similar to this:

```
Whit[Admissible_class[Example::Two]]

  Notify[example::two 1]
  Notify[example::two 2]

Whit[Completed_class[Example::Two]]
```

*NOTE: A Whit is an internal attribute (see: https://github.com/puppetlabs/puppet/blob/master/lib/puppet/type/whit.rb)*



### Class example (include):
```
class example::two {
  include example::one

  notify { 'two a': }
  notify { 'two b': }
}
```

If we run
```
puppet apply -e "include example::two"
```

This will create a catalogue similar to this:

```
Whit[Admissible_class[Example::Two]]

  Notify[example::two 1]
  Notify[example::two 2]

Whit[Completed_class[Example::Two]]

Whit[Admissible_class[Example::One]]

  Notify[example::one]

Whit[Completed_class[Example::One]]

```
NOTE: Order isn't guaranteed in the above, one or two may go first.

**Including a class ensures that the class exists/is declared, but creates to relationship or dependency between the two classes**


### Class example (require):
```
class example::two {
  require example::one

  notify { 'two a': }
  notify { 'two b': }
}
```

If we run
```
puppet apply -e "include example::two"
```

This will create a catalogue similar to this:

```
Whit[Admissible_class[Example::One]]

  Notify[example::one]

Whit[Completed_class[Example::One]]

          ^
          |
          |


Whit[Admissible_class[Example::Two]]

  Notify[example::two 1]
  Notify[example::two 2]

Whit[Completed_class[Example::Two]]

```
NOTE: In this case order is ensured (one before two), and 2 won't start unless 1 is completed.(similar to resource requires)

### Class example (contain):
```
class example::two {
  contain example::one

  notify { 'two a': }
  notify { 'two b': }
}
```

If we run
```
puppet apply -e "include example::two"
```

This will create a catalogue similar to this:

```
Whit[Admissible_class[Example::Two]]

  Whit[Admissible_class[Example::One]]

    Notify[example::one]

  Whit[Completed_class[Example::One]]

  Notify[example::two 1]
  Notify[example::two 2]

Whit[Completed_class[Example::Two]]

```
**NOTE: The contained class can't start until the containing class has started, and has to finish before the containing class finishes**




## When to use which one ?

**include:** When you need the class declared, but order isn't important
**require:** When order is important and you want something run before, but if the class you're requiring has required classes within them they'll be ignored when confirming the success of that classes completion.
**contain:** When you require multiple other classes and won't continue until they are complete.
