---
layout: post
title:      "An Intro To Algorithms"
date:       2018-06-17 12:50:13 -0400
permalink:  an_intro_to_algorithms
---

What I knew of algorithms prior to coding, was strictly limited to 

> “a formula that computers use to take data and solve a problem.” 

I knew that Google had an amazing search algorithm, and that in the world of biology, they were also used in the sequencing of DNA. However, after venturing into the world of computer programming, one of the things that kept popping up are the importance of knowing how to solve certain algorithmic problems, especially for the coding interviews that I expect to encounter during the job search. After reading the blogs of, and speaking to other bootcamp graduates, it seemed apparent that in general, although we come out of our immersive with the necessary practical programming and web design skills that we need, we’re a little weaker when it comes to algorithms. After all, up until I researched the topic for this blog, I only had a vague idea of what an algorithm actually is! Not to mention, it’s not really covered in our curriculum here at the Flatiron School and other coding bootcamps.

### So what is an algorithm?

An algorithm, on a very basic level can be defined as a set of steps to accomplish a task.

We have algorithms in our every day life. For our daily commutes, preparation of food, or how we’d go about organizing a library of books.

In computer science then, an algorithm is a set of steps for a computer program to accomplish a task. In the web today, algorithms are more popular than ever before. Consider these examples:

Google Hangouts: audio, and visual compression algorithms.

Google Maps: route finding algorithms.

Through our familiarity with algorithms, and knowing when to apply them, we are better able to write interesting and better programs. For example, many of the common search and sorting algorithms are tweaks of well-known algorithms. We’ll also likely use them when selecting which gems ours Ruby on Rails app will need.

This all makes sense, but practically speaking, what does this mean? What does an algorithm look like, and how does it differ from a method?

Straight from the good book, *Cracking the Coding Interview*, by Gayle Laakmaan McDowell, McDowell suggests that a good approach to solving an algorithm problem is understanding what the common sorting and searching algorithms are, and running through them to see if one is suited particularly well as a solution to the problem. Two of the sorting algorithms she listed are rendered here in Ruby edition:

### Bubble Sort

Known as one of the slower algorithms, the bubble sort algorithm iterates through an array, swapping the first two elements, and then each subsequent pair afterwards. It continues to make sweeps through the array until sorting is complete, resulting in an array which is sorted by ascending order. It’s called bubble sort, because the smaller items slowly bubble up to the beginning of the list.

```
def bubble_sort(array)
  n = array.length
  loop do
    swapped = false

    (n-1).times do |i|
      if array[i] > array[i+1]
        array[i], array[i+1] = array[i+1], array[i]
        swapped = true
      end
    end

    break if not swapped
  end

  array
end
```

### Merge Sort

One of the more commonly used algorithms in interviews, divides an array in half, sorts each half using the same algorithm, and then merges the two sorted arrays.
```

def mergesort(array)
  def merge(left_sorted, right_sorted)
    res = []
    l = 0
    r = 0

    loop do
      break if r >= right_sorted.length and l >= left_sorted.length

      if r >= right_sorted.length or (l < left_sorted.length and left_sorted[l] < right_sorted[r])
        res << left_sorted[l]
        l += 1
      else
        res << right_sorted[r]
        r += 1
      end
    end

    return res
  end

  def mergesort_iter(array_sliced)
    return array_sliced if array_sliced.length <= 1

    mid = array_sliced.length/2 - 1
    left_sorted = mergesort_iter(array_sliced[0..mid])
    right_sorted = mergesort_iter(array_sliced[mid+1..-1])
    return merge(left_sorted, right_sorted)
  end

  mergesort_iter(array)
end
```

### What makes a good algorithm? Two primary factors:

1.	Correctness.

2.	Efficiency.

Finally, I want to note that a point of confusion may be the difference between algorithms and methods. There is a distinction. While methods can encapsulate any amount of code, an algorithm is a specific sequence of steps designed to complete it’s task. Algorithms are designed with specificity in mind along with a greater emphasis on computational efficiency. On the other hand, a method could be an algorithm, but can also be more vague or theoretical in its implementation, e.g. printing out a certain message a number of times.

### Why I decided to talk about Algorithms

I was concerned that as someone who is not a CS graduate, this could be a weakness after leaving a coding bootcamp. A study by Triplebyte, a company that interviews developers for hire by other companies confirms this suspicion. Check out their graph below:

![](https://imgur.com/TW4wgcx.png)
 
The author of the blog states, 

> “I want to specifically draw attention to the performance of college grads on algorithm problems. They are not only better than bootcamp grads, they are a lot better. They are significantly better than the average programmer making it to our interview (most of whom have 2+ years of experience), and almost as good at the average engineers who we pass.”

It’s not all bad news though. Triplebyte also says “The first thing to note about this graph is that bootcamp grads do as well as or better than college grads on practical programming and web system design.”

How can we, as bootcamp graduates fill the gap?

I found some resources for you below:
•	https://www.interviewcake.com
•	https://www.leetcode.com/
•	MOOCS such as https://www.edx.org/ or https://www.coursera.org/
•	[Cracking the Coding Interview by Gayle Laakmann Mcdowell](http://www.crackingthecodinginterview.com/)

Good luck.

#### Sources

[Triplebyte: Bootcamps vs. College](https://triplebyte.com/blog/bootcamps-vs-college)

[Sorting Algorithms in Ruby - SitePoint](https://www.sitepoint.com/sorting-algorithms-ruby/)

[Quora: Why and how are algorithms important in our daily life?](https://www.quora.com/Why-and-how-are-algorithms-are-important-in-our-daily-life)

[Quora: What is the difference between a method and an algorithm?](https://www.quora.com/What-is-the-difference-between-a-method-and-an-algorithm)