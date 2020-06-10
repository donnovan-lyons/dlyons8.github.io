---
layout: post
title:      "The Importance of Big O"
date:       2020-05-27 09:32:32 +0300
permalink:  the_importance_of_big_o
---

One of the most interesting modifications to the way I think about writing my code was the introduction of Big O. It is in essence the language used to discuss the efficiency of algorithms, i.e. as the argument of a function grows towards infinity, the value that the argument tends towards.

What does that mean?

Take the following function, f(n) = n^2 + n.

The greater n (our argument) becomes, the more significant n^2, and thus the less significant n that is added. The way big O works, Is that the notation for the above function would be O(n^2).

What Big O describes