---
comments: true
date: "2013-04-13T23:32:54Z"
description: How much of logic should be in the controller? Should the model be Fat,
  should the controller be so, what? where? - An open question.
tags:
- tech
- mvc
- doubts
title: Fat controller or Fat model???
---
When coding up an app using the MVC model, how much of logic do you actually code up in the controller?

The controller's job in an MVC setup is to pick up action requests from the user acting on the View, pass them to the model for data manipulation and pull out the new view to show to the user.

Now, the question in my mind is: How much of logic do you actually throw into the controller?

* Is the controller supposed to have _ONLY_ routing logic? * Is it allowed to know anything at all about the view and the model it is mediating?
* Is the controller permitted to to have any session logic to quickly handle logged in / logged out users?
* Is pushing any and all processing to the model the right approach?

Just a few questions that I have in mind!
