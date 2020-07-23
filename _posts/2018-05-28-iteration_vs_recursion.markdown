---
layout: post
title:      "Iteration vs Recursion"
date:       2018-05-28 11:12:50 -0400
permalink:  iteration_vs_recursion
---


My cohort at the Flatiron School was assigned it’s first technical coding challenge as a segue for when we begin our technical interviews during our job search in the coming months. In this challenge, we were asked to take the following start to a series of numbers: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34…, and write a function that takes n as an argument and prints the nth number in the sequence, with each subsequent number equal to the preceding two numbers in the sequence. This type of sequence is known as a Fibonacci sequence.

At the end was a bonus question, “How could we refactor your solution to use recursion?” 

I was intrigued. Prior to this, I had never heard of recursion, or of using it to solve a problem. So when our instructor went over the concept, I was confused. The same method that we were defining, was in itself a part of the method.

It was a little more abstract than I was used to, and was one of the first real time examples in which I was able to clearly see how choosing how to solve a problem can ultimately affect how well your code will run under certain conditions. I needed to explore this concept further, and so I did that in three steps.

### Defining recursion. What is it?

According to Wikipedia, it is 

> the process a procedure goes through when one of the steps of the procedure involves invoking the procedure itself. 

More formally, it is defined as a class of methods or objects with the following two characteristics: (1) a terminating scenario that does not use recursion to produce an answer, and (2) a set of rules that reduce all other cases toward the base case.

### Let’s contrast recursion to iteration.

By taking a simple problem, print the countdown from a number n until it reaches 0, we can obtain the following solution using an until loop.

```
def countdown_iterator(n)
 until n == 0
  puts n
  n -= 1
 end
end
```

Now recursion requires that the method being defined is called within itself. It would also need to terminate once it reaches a certain condition, as well as get closer to that condition with each recursive call.
So let’s re-write the above iterative method and turn it into a recursive.

```
def countdown_recursive(n)
  return if n.zero?
  puts n
  countdown_recursive(n-1)
end
```
Here, the condition is that our integer, n is equal to zero. After we check for this, we call our method again within the method, which subtracts our argument by 1, checks for our condition, prints our method, and goes another layer deeper, and repeats, until it can go no more.

A great analogy that we can use to explain the concept of the recursion method is the Russian Matryoshka doll:

![](https://imgur.com/b5MDvOZ.jpg)


### Why or when should a recursive method be used?

When I use either an iterative or recursive method for our countdown, at first glance, they seem to work fine. Yet, when I plug 100000 into #countdown_recursive as our argument, we get the error

`SystemStackError: stack level too deep`

As noted in the [video](https://www.youtube.com/watch?v=az5k2m9JXVk), the iterative method had no issues handling the larger integer, but the recursive method did, resulting in a stack overflow, which essentially means that our recursive method attempted to use more memory space than the call stack had available.

Considering it’s limitations, it would seem that there isn’t really a need to use a recursive method when loops or iteration can do what they can do, without having to use the call stack to store function call returns, thereby reducing the likeliness of this unpleasant stack overflow error. Nevertheless use of recursive methods can make one's code cleaner, more easily readable, and less prone to error. After all code is read more than it is written, and recursive methods can be written with just the conditional statement and itself.

On another note, I remembered that I actually have used recursion before in a simple CLI game that I was working on. Check it out the CLI logic in the methods below:

```
def welcome
  puts "Welcome to the Blackjack Table"
end

def deal_card
  rand(1..11)
end

def display_card_total(card_total)
  puts "Your cards add up to #{card_total}"
end

def prompt_user
  puts "Type 'h' to hit or 's' to stay"
end

def get_user_input
  gets.chomp
end

def end_game(card_total)
  puts "Sorry, you hit #{card_total}. Thanks for playing!"
end

def initial_round
  sum = deal_card + deal_card
  display_card_total(sum)
  sum
end

def hit?(card_total)
  prompt_user
  reply = get_user_input
  if reply == "h"
    new_total = deal_card + card_total
  elsif reply == "s"
    new_total = card_total
  else
    invalid_command
    hit?(card_total)
  end
  new_total
end

def invalid_command
  puts "Please enter a valid command"
end

def runner
  welcome
    card_total = initial_round
    until card_total > 21
      card_total = hit?(card_total)
      display_card_total(card_total)
    end
  end_game(card_total)
end
```

In this CLI game, the game starts when the `#runner` method is run. Then a welcome message is initially printed to the screen via the `#welcome` method, and via `#initial_round` an initial number is given based on the sum of two randomly picked numbers between 1 and 11. Then an until loop is entered. This is where our recursive method comes into play. The hit method prompts the user to enter “h” or “s” If anything else is an entered, an invalid response is entered, and we go into the recursive method that remains infinite until a desired response is entered.

This  is only the beginning of my understanding of the use of recursion. The benefits of using recursion will also vary depending on the language used, and appears to be more commonly used to solve more mathematically related problems than what is required for full-stack web development.

Sources:

[Wikipedia: Recursion](https://en.wikipedia.org/wiki/Recursion)