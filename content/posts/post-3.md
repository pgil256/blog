---
title: "The Good, Bad, and Ugly"
date: 2023-09-08T12:13:32+05:30
tldr: "If there's one takeaway I want people to have, it's that setting up a proxy server in your local enviornment might be a solution if you are facing errors with database migration and seeding to a live server. However, YMMV."
description: "Some thoughts as of late."
tags: [development]
---

...while this is still fresh on my mind: I'd like to talk about the good, neutral (a.k.a. bad ;]), and ugly topics I referenced a week ago:

**Good:** 3 application deployments, portfolio demo setup.

Deploying is one of those things where, like installing package dependencies or resolving conflicts and degrations, or doing a simple pull request in git, that, with a little bit of experience, is not that crazy. But the FIRST time you do it, or rather incorrectly do it 37,814 times, is where the real ~~mania magic is. For me, using fly.io, that meant seetting up a Dockerfile to manage package installations and app execution in frontend an backend directories, as well as setting up deployment settings and enviornment variables in what is called a fly.toml (app configs for fly.io).

Portfolio is up! I found an html template on google somewhere, threw my projects/links on it, added a bunch of animations, set up the form using an imported API, and good to go. Wasn't hard at all. No wordpress, bootstrap, etc. needed.

The good for right now is having tangibles representations of my code. Feels good to see something finished, deployed, and out of my hands (mostly, of course I'll want to maintain/update these apps at somepoint).

**Neutral:** Brushing up on my Python skills, PostgreSQL chops being tested.

Not until you explore other languages do you realize how relatively intuitive and organized Python syntax is. It's essentially the English languages organized into neat little stacks. Was good to get back into Python to revise and deploy my Flask app (Warbler). Good forbid you have a single space indent on line 697 of some test file, because Python will shut you down in your tracks.

PostgreSQL has been my go to database management system for local and live servers. Would like to check out MongoDB to have some versatility under my belt.

**Ugly:** Knex, Docker, Middleware. Trying to keep the faith.

Docker was discussed earlier.

Knex (or Alemibic): Development is a cruel mistress. You write every test, solve every edge case, make everything clean then bam: time for production enviornment. Now it's time for database migration. Those SQL models? Garbage. Db configs? Completely useless. You get to spend the next 4 hours reading documentation and rewriting syntax on everything so that you PG database can see the light of the day. Can you chain these four queries together? Could that have inspired the 40 line message in the error logs? Fun times.

PRO TIP: On fly.io, you can simply use a proxy server rather than running a migration/seed script. Just dump the local db then restore it onto the proxy. This saved when working with SQL Alchemy/Alembic/PostgreSQL.

Middleware: JWT's have become the bane of my existance in production. Causing issues left and right with regards to throwing middleware errors. Not sure if it related to local storage issues or the instantiation of each JWT itself (although JWT decoders seem to point otherwise) but it's become problematic enough where I might check out other libraries/plugins to mitigate this.

Next week: Will make a decision on my first open source issue. ALSO: will be marketing myself for freelance work, starting with upWork. Good luck out there everybody.

