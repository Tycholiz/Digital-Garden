
## Overview
*"BDD is an outside-in, pull-based, multiple-stakeholder, multiple-scale, high-automation, agile methodology. It describes a cycle of interactions with well-defined outputs, resulting in the delivery of working, tested software that matters."*

*"It's using examples to talk through how an application behaves... And having conversations about those examples."*

### Approach
The BDD process encourages collaboration among: 
- developers
- quality assurance testers
- customer representatives in a software project

This is done through inter-team conversation and using concrete examples to formalize a shared understanding of how the application should behave

BDD suggests a specific way that unit tests and acceptance tests should be written:
- unit test names be whole sentences starting with a conditional verb ("should" in English for example) and should be written in order of business value.
    - ex. all other music players should pause when pressing play on a music player.
- acceptance tests should be written in the form of a user story ("Being a `[role/actor/stakeholder]` I want to `[feature/capability]` so I can `[benefit]`")

### BDD Focuses on:
- Where to start in the process
- What to test and what not to test
- How much to test in one go
- What to call the tests
- How to understand why a test fails

BDD emerged from [[TDD|testing.TDD]]
- The general techniques and principles of TDD are preserved in BDD,
- further incorporated are ideas from [[DDD|general.principles.DDD]] and [[OOP|paradigm.oop]]

### Implementing BDD Results in:
The result is that software development and other stakeholders (management teams, designers, PMs etc.) now have shared tools and a shared process to collaborate on software development.

To properly implement BDD, special software tools have to be used to ease this collaboration amongst teams.
