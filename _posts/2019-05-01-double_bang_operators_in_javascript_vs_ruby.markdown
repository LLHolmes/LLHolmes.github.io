---
layout: post
title:      "Double Bang Operators in JavaScript vs. Ruby"
date:       2019-05-01 16:35:09 +0000
permalink:  double_bang_operators_in_javascript_vs_ruby
---


![https://media.giphy.com/media/v8k9PaAQphzwI/giphy.gif](https://media.giphy.com/media/v8k9PaAQphzwI/giphy.gif)

After starting the JavaScript block here at FlatIron I found the switch to a new programming language a bit disorienting.  But it also gave me some extra clarity on at least one basic Ruby question that has been simmering in the back of my mind for a while: what’s with the double bang (!!) ?!!

I couldn’t figure out why I needed to call ```if !!current_user``` when working with authentication.  How was that different than just referencing the current_user?

I had the vague idea that the single bang meant “not”, and therefore the double bang meant “not not”…so basically the same value as not calling it at all?  That, however, is no where near true.

In both JavaScript and Ruby (and, in fact, in most programming languages) the single bang is more like “the opposite of it’s truthy-ness”, and therefore the double bang is akin to “the truthy-ness of the object”.  They’re realted to the objects you call them on, but they’re not one and the same.  They create booleans.

Let take a quick look:

```
# Ruby:              | JavaScript
-------------------------------------------
string = “hello”     | const string = “hello”
                     |
puts string          | string
# => “hello”         | // => “hello"
                     |
puts !string         | !string
# => false           | // => false
                     |
puts !!string        | !!string
# => true            | // => true
                     |
puts !!!string       | !!!string
# => false           | // => false
​```
​and so on…

Cool!  So when you’re in need of a boolean value, that’s when you use your bangs!

The only area where it gets a bit weird between JavaScript and Ruby (at least in the double bang context) is what each language inherently considers to have a truthy value.  In both languages, a variable pointing to the value of false or null (JavaScript) / nil (Ruby) are falsy.  Any string, number, array (even an empty array = []), and hash/object (even an empty hash = {}) are considered truthy.  So logic prevails and all is good and right in the world.

But!  There are a few diversions.  So keep these few weirdos in mind:

```
# Ruby:              | JavaScript
-------------------------------------------
zero = 0             | const zero = 0
                     |
puts zero            | zero
# => 0               | // => 0
                     |
puts !!zero          | !!zero
# => true            | // => false
-------------------------------------------
empty = “"           | let empty = “”
                     |
puts empty           | empty
# =>                 | // => “”
                     |
puts !!empty         | !!empty
# => true            | // => false
-------------------------------------------
blank                | let blank
# => undefined local |
 variable or method  | blank
 ‘blank’...          | // => undefined
                     |
                     | !!blank
                     | // => false
-------------------------------------------
                     | // and the mother of all weirdos:
                     |
                     | let not = NaN
                     |
                     | not
                     | // => NaN
                     |
                     | !!NaN
                     | // => false
```


Thank goodness what's true and false is so clear in this day and age...

![https://media.giphy.com/media/2CcKiHPDsJqVi/giphy.gif](https://media.giphy.com/media/2CcKiHPDsJqVi/giphy.gif)


For more, lovely references:  https://medium.com/@ryanponte/bang-bang-3bedde21ec8f
https://stackoverflow.com/questions/524658/what-does-mean-in-ruby
