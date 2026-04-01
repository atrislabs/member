Introducing Member, a standard for defining agent team members in your project.

As platforms for managing fleets of agents start to mature, it's important to define the member itself. Each agent needs a clear role, scoped tools, and a way to improve over time. That's what MEMBER.md defines.

https://github.com/atrislabs/member?v=2 

It is composed of three parts:

Journal - a daily log that traces what the agent did, what worked, and what the user preferred. Over time this becomes real memory. The agent reads past entries before starting work and stops repeating the same mistakes.

Skills - a focused subset of capabilities the member owns. Each skill is a SKILL.md file the agent actually gets better at through use.

Tools - a scoped set of APIs, CLIs, and integrations the member has access to. Clear boundaries on what it can and can't touch.


In the coming weeks we'll be releasing more around human+agent teams, fleet management, and skill improvement. Member will be the basis and it is designed to work well with other agentic frameworks like OpenClaw. 

Hopefully the concept is useful as you build out your own agent teams.


