# About this project

Read the [The System Design Checklist](https://evelyne24.github.io/system-design-checklist/).


## ✅ What this checklist is

This is a checklist of things to think about when designing or re-designing a software system or parts of it.

For example, a specialised checklist can be used to design:

- an Minimum Viable Product (MVP) for a pre-PMF company
- a microservice for a post-PMF company
- a migration process from a monolith to a microservice architecture
- a logging and monitoring system for external auditing
- a frontend component
- a new API endpoint
- etc.

## ❌ What this checklist is not

This is not a tutorial on software system design. However, I added a few notes about _**why**_ certain questions are important. 

## 💡 How I came up with the checklist

The [AWS Solution Architect](https://aws.amazon.com/certification/certified-solutions-architect-associate/) exam asks 60+ scenario-based questions in 120 minutes. While I was prepping, I came up with a mental checklist to:

- extract the system requirements from the question
- determine the desired system properties
- break down the system layers
- consider trade-offs at each layer

My mental checklist was useful and I passed the exam 🙌.

This checklist has the same structure. I beefed it up with a lot more questions I thought about, during my time as a CTO of an early-stage fintech app, where I designed software for high-street banks.

## 👀 Why I made the checklist public

There are many books and articles on software system design with amazing knowledge. However, I found **they lack a practical format**, to help apply that knowlege immediately in action. Hence, this repository is the first step towards a more practical tool.

> _It's the kind of practical tool I wish I had before I started my first CTO role._

I want to help not only first-time, early-stage CTOs but hopefully any software team who might want to use the checklist before a routine design task.

> _I hope the checklist accomplishes three goals for any engineering team_:

1. Helps them <mark>discover <i>unknown unknowns</i></mark> by _sparking curiosity and reflection_ ahead of the design task.
2. Inspires them to <mark>use it in Pre-Mortems</mark>, to _better assess high-impact, high-risk systems_, <mark>and in Post-Mortems</mark>, to _update it with new learnings_ discovered the hard way.
3. Used and refined frequently, <mark>it builds good habits</mark> leading to _high quality work_.


## 🙋🏻‍♀️🙋🏾 How to contribute to the checklist

If you like the checklist, let's make it more useful together! 💪 

> _Mistakes are human and technology advances so quickly, that what was good enough yesterday is not anymore today. Nobody can know everything._ 

You're very welcome to contribute to fix mistakes, and refine the list of questions and options by submitting a Pull Request.

> _I'm running some A/B tests with real teams from a few companies to benchmark if the usage of a checklist results in_:
> - shorter code commit → review → deploy cycles
> - improved code quality
> - increased number of discovered gotchas 
> - fewer incidents
> - better knowledge loops in and between teams

> _If you want to try it, email me at evelina@vrabie.info. Feel free to leave a comment with suggestions to make it more specific or shorter for certain cases, like the ones I mentioned in the first paragraph_.

### How to run Jekyll locally

1. Install [Docker](https://www.docker.com/) on your machine
2. Run Jekyll: `docker-compose up`
3. The browser should livereload at `http://localhost:4000` 

## 👩🏻‍💻 About me

- [LinkedIn](https://www.linkedin.com/in/evelinavrabie/)
- [Medium](https://medium.com/jump-start)