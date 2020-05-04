---
layout: post
title:      "Spaceships in The Spring"
date:       2020-05-03 16:07:03 -0400
permalink:  spaceships_in_the_spring
---


Weird title, right? You’re probably wondering what in the world does a spaceship in the spring have to do with my coding journey. Let me tell you:

Nothing.

Except, if Sinatra was like driving car, it certainly feels like learning Rails was like walking into a spaceship and trying to fly it to a destination of my choice.

![Spaceship controls](https://media.giphy.com/media/3ov9jPghXAsbvLmcjm/giphy.gif)

And that's how big Rails is. It's magical, and I didn't realize just how much so until I began to implement all of the site planning that I had done for my third Project. Enter Ruby on Rails.

### So Let me Tell You About the Spring

Every year in the spring time, Jews around the world begin preparing for a holiday called Passover. That entails selling and searching and cleaning out anything which may have in it leavened grains. Yet when it comes to cleaning, people sometimes decide to go above and beyond the basics. They clean out their vehicles, bookshelves, cabinet drawers and entire homes. This is a lot of work however, and often times help is hired as result. Here in Jerusalem, Israel where I'm studying, it's common practice for students who have the time off to take advantage of this opportunity to go and clean homes for money. So, a friend and I thought:  This would be a great time to make some money, but why not create something that would enable us to help others find work and benefit from it as well? Hence

![Spring `Cleaner`s Logo](https://imgur.com/IkTg8AN.png)

It's one of my favorite things about learning how to code. If there is a tool that I need, I have the ability or the wherewithal now to construct that tool.

### Breaking it down with Model Relations

#### Models

And what I wanted was relatively simple. There are two types of `User`s: a `Cleaner` and a `Customer`. A `Customer` creates an `Appointment`, and a `Cleaner` picks up any available `Appointment`s. Thus, `Appointment`s have three statuses: pending, confirmed, and completed.

So I infered from that, that a `Cleaner` should have many `Appointment`s, and many `Cleaner`s through `Appointment`s, and vice-versa. The only difference is, `Cleaner`s create the `Appointment`s, and an `Appointment` does not need to have a `Cleaner` id.

A `Cleaner` would belong to an `Institution`, and an `Institution` would have many `Cleaner`s. This way, students like me can have access to two addresses depending on whether we're located at a residence or institution.

Finally, a `User` would have many `Conversation`s, and a `Conversation` would have many `Message`s. A `Message` belongs to a `Conversation`, and a `Conversation` would belong to a `User`.

#### Views

For views, a `User` logs in, and is taken to a page that shows 3-4 options. For `Cleaner`s:

1. Find a Home to Clean (view with all pending Appointments)
2. My Current Jobs (view with all Appointments that have that Cleaner's ID and has a status of "Confirmed")
3. My Completed Jobs (view with all Appointments that have that Cleaner's ID and has a status of "Completed")
4. Help

and `Customer`s:

1. Schedule a `Cleaner` (view with Cleaner form to schedle appointment)
2. View Scheduled `Cleaner`s (view with all appointments and their statuses)
3. Help

The Help button would take them to a page that enables them to contact the admin of the site, i.e. me or my aforementioned friend. The Customer/Cleaner views are self-explanatory.

For `Message`s, we'd have a `Conversation`s index that showed a list of all the `Conversation`s, and a `Message`s index that showed all the `Message`s in that particular `conversation`.

These models would have their respective controllers, and we'd add a `Session`s and `Welcome` controller for logging in and a welcome page respectively.

#### Complications

All of that looked beautiful until I realized that there's a need to differentiate between the two `User` types. How do I route them to their respective views? How does that work with signing up? 

What about `Conversation`s? Can that be a join table between two `User`s, or could a `Cleaner` only interact with a `Customer` and vice-versa?

### My Answers

The resolution to these problems resulted in learning about some of the complexities of Rails and Table Relations. For the User class, my options were Single Table Inheritance(STI) or Multi Table Inheritance(MTI) with Polymorphic Referencing also a seemingly viable option.

In the end I decided to forego all three options of the above and instead simply setup my `User` class so that it has_one `Cleaner` and `Customer`. This decision hopefully enables the application to be able to be scaled next year when the application goes live and in production.

As far as `Conversation`s go, I discovered the ability to creating two separate foreign_keys pointing to the same `User` class, e.g.

```

class Conversation < ActiveRecord::Base
    belongs_to :sender, :foreign_key => :sender_id, class_name: 'User'
    belongs_to :recipient, :foreign_key => :recipient_id, class_name: 'User'
end

```

This lets rails know that we have two different types of Users that the Conversation class belongs to, thereby distinguishing between a User that sends the message and a User that receives the message.

### Conclusion

Add a little OAuth, user authentications, and some css design to all of this (things that would require blog posts of their own), and this is what a few dozen hours of building a Ruby on Rails app in a nutshell looks like.

Check it out in this video [Spring Cleaners Walkthrough](https://youtu.be/dpEpxpvgnfs).