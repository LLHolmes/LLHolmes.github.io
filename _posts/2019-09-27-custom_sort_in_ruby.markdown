---
layout: post
title:      "Custom sort in Ruby"
date:       2019-09-27 14:25:18 -0400
permalink:  custom_sort_in_ruby
---


When working on a JavaScript project, I came across the need to build a custom sort for an array of objects.  [This blog](https://flaviocopes.com/how-to-sort-array-of-objects-by-property-javascript/) sent me in the right direction.  But it also had me wondering if there was a way to build it in Ruby so I could sort by two different property values.  Let’s say I have an array:


```
array = [
  {name: "Paul", age: 30},
  {name: "Paul", age: 10},
  {name: "Ringo", age: 20},
  {name: "Ringo", age: 10},
  {name: "John", age: 10},
  {name: "George", age: 20}
]
```

I want to sort my array with all items of the lowest age first, and all ages sorted alphabetically by name.  Of course I could string a few custom ​[`.sort` calls](https://ruby-doc.org/core-2.6/Array.html#method-i-sort) together and get what I was looking for:

```
sorted = array.sort{ |a, b| a[:name] <=> b[:name] }.sort{ |a, b| a[:age] <=> b[:age] }

=> [
  {name: "John", age: 10},
  {name: "Paul", age: 10},
  {name: "Ringo", age: 10},
  {name: "George", age: 20},
  {name: "Ringo", age: 20},
  {name: "Paul", age: 30}
]
```

But is there a more efficient way than needing to sort the same array twice?

Of course!  ​​

`.sort` uses the [Comparable (spaceship <=>) operator](https://ruby-doc.org/core-2.5.0/Comparable.html) which returns -1, 0, or +1 depending if b is greater than, equal to, or less than a.  Let’s start by building out a custom sort for just one property like the ages in our array:

```
sorted = array.sort{ |a, b| a[:age] <=> b[:age] }
```

Now, instead of using the comparable operator, let's use a ternary:

```
sorted = array.sort{ |a, b| a[:age] > b[:age] ? 1 : -1 }
```

Both give us the same result:

```
=> [
  {name: "Paul", age: 10},
  {name: "Ringo", age: 10},
  {name: "John", age: 10},
  {name: "Ringo", age: 20},
  {name: "George", age: 20},
  {name: "Paul", age: 30}
]
```

But the ternary gives us more flexibility.  So, if we then want to alphabetize the names ( `a[:name] <=> b[:name]` ) once they’re sorted by age, we need to replace that `-1`.  But be careful!  We actually need to replace it with *another* ternary to check if the ages are equal and *then* we need to alphabetize ( `a[:age] === b[:age] ? (a[:name] <=> b[:name]) : -1` ).  Otherwise the sorting of the names will take precedence since it's the last sort that takes place.  So we finally settle on:

```
sorted = array.sort{ |a, b| (a[:age] > b[:age] ? 1 : (a[:age] === b[:age] ? (a[:name] <=> b[:name]) : -1)) }

=> [
  {name: "John", age: 10},
  {name: "Paul", age: 10},
  {name: "Ringo", age: 10},
  {name: "George", age: 20},
  {name: "Ringo", age: 20},
  {name: "Paul", age: 30}
]
```

You can get as crazy as you want adding ternary into ternary.  Have fun!
