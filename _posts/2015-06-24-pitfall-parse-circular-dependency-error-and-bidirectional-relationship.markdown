---
layout: post
title: "Pitfall: Parse Circular Dependency Error and Bidirectional Relationship"
date: 2015-06-24 12:26:11 +0800
comments: true
categories: [iOS]
---

When you save a `PFObject` and encounter the error `Found a circular dependency when saving`, it could be due to you setting a bidirectional/inverse relationship.

<!-- more -->

Take this example:

```objc
// You have 2 Parse Class Teacher and Student
PFObject *teacher, student;
// You try to set up the bidirectional relationship
teacher.student = student
student.teacher = teacher
```

Upon save, the code will throw the exception `Found a circular dependency when saving`. This is because [bidirectional relationship is not supported](https://parse.com/questions/bidirectional-relationship-one-to-many) on Parse.

The solution requires 3 steps:

- Don't set up any of the relationship, and save `teacher` first. Saving will provide `teacher.objectId`.
- Then save `student` with 1 directional relationship to `teacher`. Saving will provide `student.objectId`.
- Then update `teacher` with the relation to `student`.

This takes 3 API requests to set up.

Not good, but workaround the limitation of Parse.


