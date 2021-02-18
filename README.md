# About this project

📖 Read the [The System Design Checklist](https://evelyne24.github.io/system-design-checklist/).


## What this checklist is

This is a checklist of things to think about when designing or re-designing a software system or parts of it.

For example, a specialised checklist can be used to design:

- an Minimum Viable Product (MVP) for a pre-PMF company
- a microservice for a post-PMF company
- a migration process from a monolith to a microservice architecture
- a logging and monitoring system for external auditing
- a frontend component
- a new API endpoint etc.

## What this checklist is not

This is not a tutorial on software system design. However, as a refresher, I added a few notes about _**why**_ certain questions are important and some opinionated suggestions. Feel free to skip over them.

## How I came up with the checklist

The [AWS Solution Architect](https://aws.amazon.com/certification/certified-solutions-architect-associate/) exam (_Why, you ask? Because I felt I needed to contribute to AWS's bottom line, you know...pull them out of the red_) asks 60+ scenario-based questions in 120 minutes. While I was learning, I came up with a mental checklist to:

- extract the system requirements from the question
- determine the desired system properties
- break down the system layers
- consider trade-offs at each layer

My mental checklist was useful and I passed the exam 🙌.

This checklist has the same structure. I beefed it up with a lot more questions I thought about, during my time as a CTO of an early-stage fintech app, where I designed software for high-street banks.

## Why I made the checklist public

There are many books and articles on software system design with amazing knowledge. However, I found **they lack a practical format**, to help apply that knowledge immediately. 

<mark>I noticed a clear move towards checklists and flowcharts in the developer community. I believe that is because we are bombarded by new information every day. Technologies, frameworks, languages, practices change at huge speed. Our brains cannot cope with too many long-form texts, day-to-day.</mark>

Hence, this repository is the first step towards a more practical tool. I want to help not only first-time, early-stage CTOs, but any software team who finds a use for this checklist before a routine design task.

I hope the checklist accomplishes three goals:

1. Helps them **discover unknown unknowns** by sparking curiosity and reflection ahead of the design task.
2. Inspires them to **use it in Pre-Mortems**, to better assess high-impact, high-risk systems, and **in Post-Mortems**, to update it with learnings discovered the hard way.
3. Used and refined frequently, it **builds good habits** leading to high quality work.


## How to contribute to the checklist

If you like the checklist, let's make it more useful together! 💪 

<mark>Mistakes are human nature. Because technology advances so quickly, what was good-enough yesterday may not be tomorrow. Nobody can know everything at all times.</mark>

You're very welcome to contribute to fix mistakes, and refine the list of questions and options by submitting a Pull Request.

I'm running some A/B tests with real teams from a few companies to benchmark if the usage of a checklist results in:

> - shorter `commit → review → deploy` cycles
> - improved code quality
> - increased number of issued discovered ahead
> - fewer severe incidents
> - better knowledge loops in and between teams

If you want to try it, email me at evelina@vrabie.info. Feel free to leave a comment with suggestions to make it more specific or shorter for certain cases, like the ones I mentioned in the first paragraph.

### How to run Jekyll locally

1. Install [Docker](https://www.docker.com/) on your machine
2. Run Jekyll: `docker-compose up`
3. The browser should livereload at `http://localhost:4000` 

## 👩🏻‍💻 About me

- [LinkedIn](https://www.linkedin.com/in/evelinavrabie/)
- [Medium](https://medium.com/jump-start)