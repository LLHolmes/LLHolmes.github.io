---
layout: post
title:      "Recursive Shuffle"
date:       2019-09-13 19:51:05 -0400
permalink:  recursive_shuffle
---


> Recursion occurs when a thing is defined in terms of itself or of its type. Recursion is used in a variety of 
> disciplines...The most common application of recursion is in mathematics and computer science, where a function 
> being defined is applied within its own definition. 
~ [Wikipedia](https://en.wikipedia.org/wiki/Recursion_(computer_science)

The first time I heard of recursion I’m pretty sure my brain melted.  It turned to mush, leaked out my ears, and I sat there trying to put together ANY thought more complicated than, “Whaaaaat???”

The first logical thought I had after that was, “If I ever understand this, I will have won!” 

I’m not sure what I’d win, but I knew it would feel *awesome!* 

So I’ve been practicing.  I haven’t won yet, but I’m getting there.  Here’s what I learned from my latest stab at recursion.  (I really hope this helps someone and I apologize if it makes anyone more confused...)

The non-engineer side of my brain thinks of recursion as opening up a folding fan one rib at a time, doing something with each of the ribs, and then folding it back up. 

![](https://media.giphy.com/media/C5veHexpnAE80/giphy.gif)

I’m not sure how accurate that is, but it’s a visual that I find helpful, so let’s roll with it.

A recursive function is basically a function that calls itself.  To use it, you strip an element (a string, a number, an array…) down to a/each fundamental building block (a letter, the base number 1, an item in the array...) and then use that piece individually in some way.

To practice, I built a function to shuffle a virtual deck of cards (an array of strings).

[Mike Bostock gave an awesome explanation of the Fisher-Yates Shuffle](https://bost.ocks.org/mike/shuffle/), which is essentially what I’m doing but with recursion - take an array of strings, remove one and add it to a new array, remove one of the remaining strings and add it to the new array, remove one of the remaining strings and add it to the new array, and so on until there are no strings left.  He did it with a while loop, I did it non-destructively with a recursive call.
[This post](https://medium.com/@codyfizbuzz/the-recursive-clean-shuffle-e9afc88a8a90) on recursive shuffling was super helpful for me.  This is how I thought about it and then refactored:

```
const cards = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'Jack', 'Queen', 'King', 'Ace’];

const shuffle = (cards) => {
  // Create a new array to add the randomly selected card to.
  let shuffledCards = [];

  // All recursive calls NEED a base case so they don't just continue to call forever.
  // The base case is only one card remaining since no more random selection is needed:
	
  if (cards.length === 1) {
	
    // Simply return the last card.
    return cards[0];
		
  } else {
	
    // Otherwise, select a random index (i) from the cards array.
    let i = Math.floor(Math.random() * cards.length);
		
    // Add the card to the new array.
    shuffledCards.push(cards[i]);
		
    // Create a new array of remaining cards (to keep the original cards array intact).
    let remainingCards = [...cards.slice(0, i), ...cards.slice(i + 1)];

    // Make the recursive call.
    return [...shuffledCards, ...shuffle(remainingCards)];
  };
};


let shuffledCards = shuffle(cards);

// => [‘4', ’10', ‘Ace', ‘3', ‘Queen', ‘2', ‘Jack', ‘9', ‘5', ‘King', ‘6', ‘8', 7']
// THIS WAS JUST ONE OF MANY RANDOM ANSWERS...
```

Making the recursive call there at the end of the function (`return [...shuffledCards, ...shuffle(remainingCards)];`) is what my brain considers folding the fan back up.  This is where I *always* struggle...How do I get the final return I want?  

The above code was not what my first pass looked like.  My first pass failed spectacularly.  For my first attempt, I knew I needed to call the `shuffle` method again and pass in the `remainingCards` array, so I started with:

```
return shuffle(remainingCards);

// => ‘7’
```

Very wrong.  At the end of running the `shuffle` function, this returned a single card -- the last card remaining in the array that was returned from the `if` statement.  I wasn’t folding my fan correctly, I was just returning the individual element from the single rib on the fan.

See where I created the `shuffledCards` array?  Then in the else statement, I push the randomly selected card into that array, correct?  Yes, but it was giving me a false sense of accomplishment.  Remember that everytime the shuffle function is called, `shuffledCards` is reset to an empty array.  I needed to combine the randomly selected cards into an array that would somehow survive the function being called again, hence:  
`return [...shuffledCards, ...shuffle(remainingCards)];`

I'm telling it to return and array of my randomly selected card & an array of the remaining cards after then have been randomly selected.  *Cue brain melting...*

If using an array of only high cards, ```shuffle(['Jack', 'Queen', 'King', 'Ace']);```...for simplicity, the calls/returns actually end up looking like:

```
return ['Queen', ...shuffle(remainingCards)]
                    // Which is the first recursive call, returning:
                       => ['Jack', ...[...shuffle(remainingCards)]]
                                          // Which is the second recursive call, returning:
                                             => ['Ace', ...[...shuffle(remainingCards)]] 
                                                               // Which is the third recursive call...
                                                               // hits the base case…
                                                               // and returns cards[0]:
                                                                  => ‘King'
```

So folding it back up from the bottom, it looks like:

```
return ['Queen', ...shuffle(remainingCards)]
                    => ['Jack', ...[...shuffle(remainingCards)]]
                                       => ['Ace', ...['King']]
```

Stepping back up to:

```
return ['Queen', ...shuffle(remainingCards)]
                    => ['Jack', ...['Ace', 'King']]
```

And back up again to:

```
return ['Queen', ...['Jack', 'Ace', 'King']]
```

Finally returning:

```
['Queen', 'Jack', 'Ace', 'King']
```

Neat!  The folding back up finally clicked for me!

Figuring that out made me realize exactly how useless that `shuffledCards` array is.  Additionally, remembering that the recursive return uses the spread operator, and noticing there's no real point to calling the `else` statement, let me do some refactoring:

```
const shuffle = (cards) => {
  if (cards.length === 1) {
    return cards;
  };

  let i = Math.floor(Math.random() * cards.length);
  let remainingCards = [...cards.slice(0, i), ...cards.slice(i + 1)];

  return [cards[i], ...shuffle(remainingCards)];
};
```

Pretty.


