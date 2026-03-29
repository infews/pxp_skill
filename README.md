# Pivotal XP Agentic Coding Skill

If you are coding with an agent, or agents, but you want more of an XP slant to the experience, then this skill is for you.

This skill lets you write up Epics, iterating with your agent to get to Stories, then TDD the tasks for the stories.

This is intended to give you a solid pair on "solo" or small projects. But the discipline could be useful for more complex stories. It assumes that there is no external store of intent - no Jira, or StoryTime here. Instead, your epics, stories, specs, and code live together in your repo.  

There are also file naming rules so epics and stories stay in the right order in the file system. And it uses EARS specs to give a lightweight Spec-Driven-Development flow.

If you're curious more about Pivotal and Pivotal Labs' flavor of Extreme Programming, just ask Claude or ChatGPT. They both have a pretty good breakdown.

## Epics to Stories

Your agent will create a new epic for you. Let your agent resolve any amibguity in the Epic/PRD text with you.

Next the skill instructs your agent to slice the epic into stories, with each story being its own file, and storing them in a directory together.

## Stories to EARS Specs

As you being each story, your agent will break the story down into a task list using [EARS Specs](https://alistairmavin.com/ears/). While not strictly Pivotal-y, this allows for good task context across sessions (e.g., when you hit API limits).  

## TDD Your Specs

As each EARS Spec is tackled as a task, with your agent TDD'ing each step of the way. It will ask you to approve each test, implementation pass, and refactoring.
