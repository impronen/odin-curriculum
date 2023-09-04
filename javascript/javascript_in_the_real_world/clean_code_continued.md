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

```javascript
function processUserData(user) {
  if (!user || !user.name || !user.email) {
    throw new Error("User data is incomplete");
  }
  const savedUser = database.save(user);
  emailService.sendWelcomeEmail(user.email);
  logger.logActivity("User registration", user.name, new Date());
}
```

This function is trying to do way too much. If we break it down, it undertakes the following tasks:

1. Validates user data.
2. Saves user data to the database.
3. Sends a welcome email to the user.
4. Logs user activity.

Our poor function is burdened with all kinds of responsibilities. While the code isn't hard to read, violating SRP can make life difficult when fixing bugs down the line.

Let's imagine that there will be a bug in step two, where the user data is added to a database. Will your first instinct to be to look at the function named `processUserData`? Probably not, it's not referring to saving the user to the database but as our function has so many responsibilities, it might be where the problem lies.

So how would we align this function to respect the SRP? By dividing it to smaller functions, like this:

```javascript
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
```

And finally we can wrap it up by calling these smaller functions in one place, where the magic really happens:

```javascript
function addUser(userData) {
  validateUserData(userData);
  const savedUser = saveUserToDatabase(userData);
  sendWelcomeEmailToUser(userData.email);
  logUserRegistration(savedUser.name);
}
```

Now we can safely do changes to each of the steps needed for the process of adding a user. Many of these functions could also come in handy in other parts of the application, if needed. Any errors encountered will specifically pinpoint the related function. Convenient, modular and reusable.

<!-- NEXT WORK ON FUNCTION ARGUMENTS -->

#### Limit the number of arguments passed to a function

Passing arguments to a function is the bread and butter of programming. At the start, you probably had pretty simple stuff to pass in - you might have had something like this in your calculator:

```javascript
function operate(operator, x, y) {
  // Perhaps a switch statement that executes operations
}
```

There's no problem here, this code is easy to read and maintain - just a few self-evident arguments. But let's move onto Todo, where something like this might happen:

```javascript
function renderTask(task, project, dueDate, priority, completion, info) {
  // Code that creates the task in the DOM
}

renderTask(
  "Do Groceries",
  "Chores",
  "11-10-2023",
  "high",
  false,
  "You don't really need additional info for this do you?"
);
```

There's few ways to make this a bit more comfortable. One is to assing the task as an object variable, like this:

```javascript
function renderTask(taskObject) {
  const taskName = document.createElement("div");
  taskName.innerText = taskObject.name;
  parentContainer.appendChild(taskName);
  //more stuff that uses the other values from the object
}
```

Here we are only passing in the object and it's values or methods are accessed with dot notation. It looks a lot cleaner but there is a caveat - the reader has to look elsewhere to get a feel of what does `taskObject` contain. Depending on the codebase, this can be a long search.

One possibility to add clarity is to rely on ES6 destructuring syntax:

```javascript
function renderTask({
  task,
  project,
  dueDate,
  priority,
  completion = false,
  info,
}) {
  // Code that creates the task in the DOM
}

renderTask({
  task: "Do Groceries",
  project: "Chores",
  dueDate: "11-10-2023",
  priority: "high",
  info: "You don't really need additional info for this do you?",
});
```

There's defineatly added clarity here, isn't there? The example also shows how using default values can save time when assigned to some properties.

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
