---
layout: post
title:      "In A Bind About `bind()`?"
date:       2021-01-05 14:13:06 -0500
permalink:  in_a_bind_about_bind
---

### A Simple (and Hopefully Helpful) Example & Clarification

As my Flatiron School cohortmates and I admired each other's code and projects from our JavaScript unit, I came to realize that I had not been alone in my misunderstanding of `bind()`. As someone who ended up using it to help create my application, *Ear Trainer*, I have decided to take a moment to help you or anyone else who may need some help grasping the concept of `bind()` to understand what exactly is going on here...

So at its simplest, what *is* `bind()`? Well, first of all, `bind()` is a JavaScript method. And what does it do? Well, the MDN Web Docs say the following:
> The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

And while it was all well and good to have read that opening to the Mozilla Docs, it wasn't until I got to apply `.bind()` that I really saw and came to appreciate it's power.

*Ear Trainer* is a full-stack application with a JavaScript front-end and a Rails API back-end and, for any non-musicians out there, is a tool that musicians can use to help them improve their ability to identify melodic intervals (the distance from one note to another) by ear (aka by hearing it). I designed the program to function like a trivia application, where the question is a sound and the user is presented with four multiple-choice options for what the interval may be. So naturally, when building this out, I was going to need [event listeners](https://www.w3schools.com/js/js_htmldom_eventlistener.asp) that would register which (if any) of the buttons was clicked on and then reflect that with the desired outcome.

Within my `renderQuestion()` function, I used a `for` loop to add event listeners for my answers:

```
class Question {
    constructor(id, interval, answer_1, answer_2, answer_3, answer_4, correct_answer, game){
        this.id = id
        this.interval = interval
        this.answer_1 = answer_1
        this.answer_2 = answer_2
        this.answer_3 = answer_3
        this.answer_4 = answer_4
        this.correct_answer = correct_answer
        this.game = game
    }

    renderQuestion(){
        ... 
        for (let index = 0; index < 4; index++) {
            const answer = document.querySelectorAll(".answer")[index];
            answer.addEventListener("click", this.handleAnswer.bind(this))
        }
        ... 
        }
    ... 
}
```

Now, looking at the event listener in that `for` loop, you might be wondering why it is necessary for `this.handleAnswer` to be followed by `.bind(this)`. Without the `.bind(this)`, the intention of the code is pretty clear: when an answer is clicked on, it triggers `handleAnswer`, which uses conditionals to determine what action should be taken for this question:

```
    handleAnswer(){
            if (event.target.innerText === this.correct_answer) {
                this.gameScore()
            }
            else { 
                event.target.style.color = "red";
            }
    }
```


Take this example:
![](https://i.imgur.com/pg3T1l5.png)


We need to add `.bind(this)` because without it, `handleAnswer()`'s `this` isn't actually the entire question, but rather `button.button.answer`. If, for example, we clicked on the upper right answer, `this` would be `<button class="button answer">Tritone</button>`). What this means for us, in this case, is that we can't actually make the comparison of the target's innerText and the question's `correct_answer`, since `this` does not have a `correct_answer`! By adding `.bind(this)`, we are changing the value of `this` for `handleAnswer()` to the value of `this` at the time of the binding! Now, if we were to view the value of `this`, it would be:

```
Question {id: undefined, interval: "/assets/audio/min9.mp3", answer_1: "M2", answer_2: "Tritone", answer_3: "M9", …}
answer_1: "M2"
answer_2: "Tritone"
answer_3: "M9"
answer_4: "m9"
correct_answer: "m9"
game: Game {id: 1, score: 0, questions: Array(17)}
id: undefined
interval: "/assets/audio/min9.mp3"
__proto__: Object
```

We now have access to all of the class properties and can proceed as we were hoping to!

So what's the big takeaway here? We can call `.bind(arg)` and provide as the argument, a new `this` value to the target, giving us increased control and reach throughout our applications!

Hopefully this was a helpful example if you were in a bind about `.bind()`. Now go forth and utilize!
