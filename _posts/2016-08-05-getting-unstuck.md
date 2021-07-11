---
layout: post
title: Getting Unstuck
---

You're learning, which means you're going to be confused or stuck, and you might need help moving forward. Great! If you're not in that position every now and then, you're even more stuck than you think.

Getting unstuck is a skill – one that's especially important for developers – and it's something many people need guidance on. There are some great articles describing certain approaches to getting unstuck in great detail – the venerable Eric S. Raymond's [How to Ask Questions the Smart Way](http://www.catb.org/esr/faqs/smart-questions.html) is worth a read, for example. However, I'd like to highlight just three broad steps you should take to get unstuck.

## 1. Isolate the issue

Most of the advice anyone can give is going to be pretty useless if you're not sure which problem you're trying to solve. So the first step in the right direction is getting some clarity on that.

### Trace inputs and outputs
All things have a cause. Think broadly about the factors that could be having an impact on the outcome you're seeing. What immediate impacts do each of those factors have? How do they reinforce or work against each other? If you're stuck with something that has clear inputs and a clear process, walk yourself through things step by step. [Say, out loud, exactly what's going on](http://hwrnmnbsol.livejournal.com/148664.html).

### Work from the inside out
Many things can be broken down into constituent parts. When this is possible, it can be helpful to start with the smaller, interior possibilities or issues. Check that each piece of something works as you'd expect, and you'll be able to confidently narrow down the source of your trouble. This is especially applicable to programming: if you're debugging, start by checking your understanding of the function most immediately responsible for the unexpected output, and work backwards.

### Prove yourself wrong
Far too much time is wasted trying to find evidence in *favor* of something that turns out to be wrong – and could easily have been disproven. Each step of the way, ask yourself if you can somehow disprove the explanation you have in mind, so as to eliminate it from consideration.

One way to be systematic about this is to create a [Short, Self-Contained, Correct Example](http://sscce.org/), or SSCCE. If you think the problem might be due to X, remove X from the flow to see if the problem sticks around. Keep this up, removing bits and pieces and adding them back to recreate the problem in its most minimal form. 

## 2. Research

A scary word to some people. But don't worry – I mostly mean "Google", which is something you know how to do. There are a few places you should look for information:

- Canonical or well-known reference sources, such as documentation or textbooks
- Popular forums on the subject (though beware of outdated information, and look for *confirmed* recommendations)
- Tutorials, guides, and blog posts by credible, experienced people

Don't assume that the first information you run into – or the prettiest – is accurate or useful. Nor the opposite, of course! But be sure you understand a proposed solution before diving in. In any case, do a reasonable amount of digging to find what you need before you begin to...

## 3. Ask Good Questions

The article linked above has some excellent advice, but I'll distill some of the main points here. First, be respectful. Not contrite, necessarily; just recognize that you're asking for someone's time – often for free – and that helping them help you is the minimum you can do to respect that. Specifically:

### Share your isolated issue and research so far

Showing that you've worked on the issue is critical. The assumption in most online communities – unfortunately, a warranted one – is that you haven't done much work to solve the problem. You don't need to go into detail regarding every single resource that you've consulted, but a narrow problem statement and a summary of what you've found so far (or where you've looked, and found nothing) goes a long way towards reassuring people that you won't be wasting their time.

If someone tells you to RTFM, it's because they think that will be a useful exercise for you – both in terms of research and in terms of solving your problem. If you don't know what RTFM means, start by applying the process you've been learning about here.

When sharing the isolated issue – ideally, your SSCCE – be sure to include your expectations as well as the actual outcomes. With X, Y, or Z inputs, what outputs did you expect, and what did you get? Similarly...

### Describe the goal, not just the tactic

You'll commonly feel that you have a problem achieving your specific plan of action. Keep in mind that you might not have the background or depth of understanding to realize that your plan is part of the problem. If you're trying to practice a very specific skill, or imposing unusual constraints on yourself, that's fine – but otherwise, the approach you're taking might be the bigger obstacle than the implementation details that you can't work out.

To counter this, share your overall goal that the isolated issue fits into. What is it you're trying to *accomplish* with the step you're taking? That is, instead of saying "I'm trying to use `map` to alter this array" say "I'm trying to transform the data in this array in ______ a way". Small difference, but by expressing the direction you're trying to go, you give others the chance to examine all the layers of your thinking and approach. And, as mentioned above, sharing your expected outputs for given inputs can complement an explanation of your goal.

### Share the symptoms and specific causes

You may have a theory on what it is that's causing your problem. It can be helpful to share this – after all, you might get feedback about an aspect of your theory that you didn't consider, such as a reason that you should have rejected it earlier. But in any case, you must share the *symptoms* of what you're seeing. In part,  that will be accomplished by sharing your isolated issue. More broadly, though – especially when something has blown up, instead of having failed in a way that seems like it should be predictable – it's critical that you describe the specific evidence that you're looking at and the specific steps that led to the outcome you're confronting.

### Be specific about how someone can help

Do you want a hint? A review of your efforts so far? Resources to learn more? Make it clear, rather than leaving it open ended. That doesn't mean you'll get what you want – again, RTFM may be forthcoming – but you'll be helping others help you.

## Don't give up!

Take a break. [Meditate](http://dhammatalks.org/mp3_guidedMed_index.html). [Walk in nature for a bit – *solvitur ambulando*](http://ngm.nationalgeographic.com/2016/01/call-to-wild-text).
