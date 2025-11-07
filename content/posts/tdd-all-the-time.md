+++
date = '2012-03-17T07:27:32+05:30'
title = 'Tdd All the Time'
+++

Test Driven Development (TDD) is one of the best practices that extreme programming (Xp) suggests. As defined by Wikipedia, TDD is a software development process, in which a developer writes a failing test case, that defines a new improvement, or new functionality, then produce the code to pass that test and finally refactors it to acceptable standards.

Having said that, the series of questions I hear from people who are introduced to TDD are, 

> Is it really important to write tests in first place? I know the functionality. Why can't I just sit down and start coding. What do I gain by writing tests first? The whole idea of TDD is completely counter intuitive. Of course I understand the importance of tests. But I can add them later.

Very valid questions. There are two simple reasons why I think TDD helps in development.

1. First thing - TDD is a very intuitive process. Whenever we write a piece of code everyone of us know, what will be the output of the code given an input. We keep simulating that in our brain. Nobody starts coding without a problem statement in mind. Everybody knows what is expected out of the code at every point I'm time. And we validate it with sample input every time. This, exactly, is the idea behind TDD. Set the expectation for the code you write. Implement it. Validate it. Isn't TDD a intuitive process?

2. I believe in the philosophy of `Self Documenting Code`, code that explains itself without the need for extra documents. The testcases are a very useful add-on to this philosophy. In addition to validating the code, it also captures the intent behind writing a particular code, which comes handy later in the life-cycle of a project, either while debugging or while revisiting a functionality.
