---
layout: post
status: publish
published: true
title: Solving 8-puzzle using breadth-first search
author:
  display_name: hisabimbola
  login: hisabimbola
  email: admin@blog.hisabimbola.com
  url: ''
author_login: hisabimbola
author_email: admin@blog.hisabimbola.com
wordpress_id: 23
wordpress_url: http://www.hisabimbola.com/?p=23
date: '2015-05-27 19:44:54 +0100'
date_gmt: '2015-05-27 19:44:54 +0100'
categories:
- Puzzles
tags:
- puzzle
- 8-puzzle
- hash
- breadth-first search
comments:
- id: 6
  author: codeceptacon
  author_email: jivara4shizzle@yahoo.com
  author_url: ''
  date: '2015-05-27 22:22:00 +0100'
  date_gmt: '2015-05-27 22:22:00 +0100'
  content: So please can you give a real-time scenario where this can be applied?
- id: 7
  author: Abimbola Idowu
  author_email: abimbola2010@gmail.com
  author_url: ''
  date: '2015-05-27 22:45:00 +0100'
  date_gmt: '2015-05-27 22:45:00 +0100'
  content: "A simple answer would be to use it to build a game ;), \nThe goal of the
    task is to introduce me to several algorithms and optimizations tips in programming.
    So I am not sure if everything here could be applied in a real project, some principles
    and method used in solving this problem could be applied here.\n\nFor example:\nThe
    breadth-first search algorithm could be used when you want to find the shortest
    path to a node, such as in web crawler, facebook e.t.c This post on quora explained
    more - http://www.quora.com/What-are-the-real-world-applications-of-Breadth-First-Search\n\nThe
    hash table can be used when you want to find a value in the shortest time.\n\nSo
    like I said earlier, when you break it into pieces, the principles and methods
    used can be applied.\n\nI hope I answered your question?\nIf not, please reply
    what I did not answer.\nThanks"
- id: 8
  author: Nick Larsen
  author_email: fodylarsen@gmail.com
  author_url: http://stackoverflow.com/
  date: '2015-05-28 00:05:00 +0100'
  date_gmt: '2015-05-28 00:05:00 +0100'
  content: This game is of great academic interest and serves as one of the benchmark
    scenarios for domain independent planning algorithms.  If you replace the "getSuccessors"
    and "compare" functions with appropriate functions to operate on the state variable,
    it can be applied to a large collection of practical problems such as finding
    driving directions between two addresses in the same city using the fewest number
    of turns, often referred to as the "least confusing directions".
permalink: solving-8-puzzle-using-breadth-first-search/
---

During the [Andela-Stack Overflow mentorship programme](http://blog.stackoverflow.com/2015/05/stack-overflow-and-andela-partner-to-provide-education-beyond-borders/), my mentor [Nick](http://stackoverflow.com/users/178082/nick-larsen), gave me a task to solve [8-puzzle](http://www.8puzzle.com/8_puzzle_problem.html) using the [breadth-first search algorithm](http://en.wikipedia.org/wiki/Breadth-first_search).

In this post, I would try to explain my solution.

You can visit my [gist](https://gist.github.com/andela-aidowu/dbbbd9fb3d3dc3ad01b8) to view the full implementation, but I would explain some methods I used here.

{% highlight js %}
function time() {
  startTime = new Date();

  var puzzle = generatePuzzle();

  console.log('Puzzle to solve', puzzle);

  var result = breadthFirstSearch(puzzle, goalState);

  console.log(result.length);

  endTime = new Date();

  console.log('Operation took ' + (endTime.getTime() - startTime.getTime()) + ' msec');
}
{% endhighlight %}

I am using this function to calculate the total execution time of the program. In this method I am calling the `generatePuzzle` function to generate a random puzzle.

{% highlight js %}
function generatePuzzle() {
  var arr = [];

  while(arr.length < 9){
    var randomNumber = Math.floor(Math.random()*9);
    var found = false;

    for(var i = 0; i < arr.length; i++){
      if(arr[i] == randomNumber){
        found = true;
        break;
      }
    }

    if(!found) {
      arr[arr.length] = randomNumber;
    }

  }

  if(!checkSolvable(arr)) {
    console.log('\n This puzzle: ' + arr + ' is unsolvable. \n Generating a new one \n');
    return generatePuzzle();
  }

  return arr;
}

{% endhighlight %}


The `generatePuzzle` function generates a random puzzle and [almost half of the existing N-puzzles are unsolvable,](http://en.wikipedia.org/w/index.php?title=15_puzzle#Solvability) so I wrote another function to check if the puzzle is solvable or not, so we won't expend stupid mental effort.

{% highlight js %}
function checkSolvable(state) {
  var pos = state.indexOf(0);
  var _state = state.slice();
  var count = 0;

  _state.splice(pos,1);

  for (var i = 0; i < _state.length; i++) {
    for (var j = i + 1; j < _state.length; j++) {
      if (_state[i] > _state[j]) {
        count++;
      }
    }
  }

  return count % 2 === 0;
}
{% endhighlight %}

Thanks to [Tushar's post on stack exchange](http://math.stackexchange.com/a/838818), I was able to write the method to check if the generated puzzle is solvable.

After all that preamble which is very useful, I called the `breadthFirstSearch` method

{% highlight js %}
function breadthFirstSearch(state, goalState) {
  state.prev = null;
  allSuc.push(state);
  allSuc.push.apply(allSuc, getSuccessors(state));

  for (var i = 1; i < allSuc.length; i++) {
    statesPerSecond();
    if (compare(goalState, allSuc[i])) {
      return collateStates(i);
    } else {
      allSuc.push.apply(allSuc, getSuccessors(allSuc[i]));
    }
  }
}
{% endhighlight %}

What I am doing in `breadthFirstSearch` method is to find the successors - possible moves - of a state and add them to the `allSuccessors` array. The `allSuccessors` array is an open list of the states both yet to be evaluated and those I have evaluated.

<span class="s1">Breadth First Search makes use of  a Queue so I push the successors to the end of the `allSuccessors` array to mimic that behavior</span>

One big optimization I did was with here, Initially I wrote this

```js
allSuc = allSuc.concat(getSuccessors(allSuc[i]));
```

but I discovered that it is not scalable because with `concat`, js is creating a new array, which is O(n) runtime. However with the new modification,

```js
allSuc.push.apply(allSuc, getSuccessors(allSuc[i]));
```

`push.apply` is acting on the existing array, and that's O(1) regardless of the size.

After that, I am calling the `statesPerSecond` method to get the number of states evaluated in a second, after which I called a compare method to check if the present state is the same with the goal state, if true call the `collateStates` method

{% highlight js %}
function collateStates(i) {
  var _ = allSuc[i].prev;
  var result = [allSuc[i]];

  while (_) {
    for (var j = 0; j < allSuc.length; j++) {
      if (compare(_, allSuc[j])) {
        _ = allSuc[j].prev;
        result.push(allSuc[j]);
        break;
      }
    }
  }

  console.log(allSuc.length);
  return result.reverse();
}
{% endhighlight %}

Also, I check if the state has been checked before. If the state has been checked before, then I do not want to check it again. Previously, I was using an array and looping through to check if the state exists which is O(n) runtime. To improve that, I used a hash which has O(1) runtime for lookups.

Another method I need to explain is the `getSuccessors` method. This was the initial method that set the ball rolling for me, thanks to Nick for putting me through.

{% highlight js %}
function getSuccessors(state) {
  var successors = [];
  var pos = state.indexOf(0);
  var row = Math.floor(pos / 3);
  var col = pos % 3;

  if (row > 0) {
    //move up
    move(state, successors, pos, -3);
  }

  if (col > 0) {
    //move left
    move(state, successors, pos, -1);
  }

  if (row < 2) {
    //move down
    move(state, successors, pos, 3);
  }

  if (col < 2) {
    //move right
    move(state, successors, pos, 1);
  }

  return successors;

}
{% endhighlight %}

I checked the position of the empty space which is denoted by '0' and find whether it can move either move, up, left, center and I call the `move` method

{% highlight js %}
function move(state, successors, pos, steps) {
  var _state, newState;
  newState = state.slice();

  swap(newState, pos, pos + steps);

  if (!compare(newState, state.prev)) {
    _state = newState.join('');
    if (typeof hash[_state] === 'undefined') {
      hash[_state] = newState;
      newState.prev = state;
      successors.push(newState);
    }
  }
}
{% endhighlight %}

This performs some critical checks

1.  Checks if the current state is not equal to the previous state, this is to avoid infinite loops because if we allow such then a state can be going up and down till the end of the program.
2.  Also, I check if the state has been checked before. If the state has been checked before, then I do not want to check it again. In implementing this, I was using an array and looping through to check if the state exists, this is O(n) which is bad to the program, so instead I used a hash which has O(1), especially when finding.

and with that I solved the puzzle.

[![ah_ha_lrg](assets/ah_ha_lrg-300x300.gif)](assets/ah_ha_lrg-300x300.gif)

Do I think it can be more optimize? Yes!

Am I working on improving it? Yes!

Can you suggest ways to improve this? Of course Yes! Please use the comment section.

Currently working on implementing this with A* algorithm, Nick said that is way faster. So my next blog post should be that


{% include twitter_plug.html %}
