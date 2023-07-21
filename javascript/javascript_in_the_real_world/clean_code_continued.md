### Introduction

We touched on the basics of what it means to write clean code all the way back in [Foundations](https://www.theodinproject.com/lessons/foundations-clean-code). In that lesson, we discussed how you might get lost in your own code. It might have felt like a silly notion at the time, but probably by now, you've already had experience with that. Same thing has probably happened with a lot of things we covered back then, wether you remember them or not.

This lesson is an invitation to take a glance back at that earlier lesson for reminders and an opportunity to learn some cool new tricks that can make your code cleaner and just _better_.

Sound good? Let's go!

### Lesson Overview

This section contains a general overview of topics that you will learn in this lesson.

- Find out more ways to improve your code readability.
- Understand why re-usability is golden.
- What it means to write DRY code?
- Brevity vs. readability.

### Back To Functions

Last time we mostly covered functions from the perspective of naming them. But there is much more to what makes a good function (or a method for that matter).

We have just discussed one of the most important principles in an earlier lesson - The Single Responsibility Principle. The discussion there was in the context of Object Oriented Programming, or classes. But the same applies to functions.

#### Have your functions do only one thing

A function that does one thing will be easier to read than one that does many things. This will not only make your code more readable but it will make it easier to refactor. Let's look at an example:

~~~javascript
function processUserData(user) {
  if (!user || !user.name || !user.email) {
    throw new Error("User data is incomplete");
  }
  const savedUser = database.save(user);
  emailService.sendWelcomeEmail(user.email);
  logger.logActivity("User registration", user.name, new Date());
}
~~~

Whoa, that function does _a lot_. If we break it down, the parts are:

1.  We validate user data
2.  Then save user data to the database
3.  Then we send a welcome email to the user
4.  Finally we log the user activity

Our poor function is responsible for all kinds of things. The code as such isn't hard to read but violating SRP can make life difficult, when fixing bugs down the line.

Let's imagine that there will be a bug in step two, where the user data is added to a database. Will your first instinct to be to look at the function named `processUserData`? Probably not, it's not referring to saving the user to the database but as our function has so many responsibilities, it might be where the problem lies.

So how would we bend this function to respect the SRP? By dividing it to smaller functions, like this:

~~~javascript
function validateUserData(user) {
  if (!user || !user.name || !user.email) {
    throw new Error("User data is incomplete");
  }
}

function saveUserToDatabase(user) {
  return database.save(user);
}

function sendWelcomeEmailToUser(email) {
  emailService.sendWelcomeEmail(email);
}

function logUserRegistration(name) {
  logger.logActivity("User registration", name, new Date());
}
~~~

And finally we can wrap it up by calling these smaller functions in one place, where the magic really happens:

~~~javascript
function addUser(userData) {
  validateUserData(userData);
  const savedUser = saveUserToDatabase(userData);
  sendWelcomeEmailToUser(userData.email);
  logUserRegistration(savedUser.name);
}
~~~

Now we can safely do changes to each of the bits needed for the process of adding a user. Many of these functions could also come in handy in other parts of the application, if needed. Convenient, modular and reusable.

<!-- NEXT WORK ON  -->


### Assignment

<div class="lesson-content__panel" markdown="1">

1. A RESOURCE OR EXERCISE ITEM
   - AN INSTRUCTION ITEM

</div>

### Knowledge check

This section contains questions for you to check your understanding of this lesson on your own. If you’re having trouble answering a question, click it and review the material it links to.

- [A KNOWLEDGE CHECK QUESTION](A-KNOWLEDGE-CHECK-URL)

### Additional resources

This section contains helpful links to related content. It isn’t required, so consider it supplemental.

[FCC CLEAN CODE](https://www.freecodecamp.org/news/how-to-write-clean-code/)
[16 TIPS](https://enou.co/blog/how-to-write-clean-code/)

- It looks like this lesson doesn't have any additional resources yet. Help us expand this section by contributing to our curriculum.
