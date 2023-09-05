
---

# Gilhooley Dev Blog

9.1.23

So today marks my first day of blogging about my journey as a developer.

A lot has happened lately, and there are some topics I'd like to cover:

**Good**: 3 application deployments, portfolio demo setup.
**Neutral**: Brushing up on my Python skills, PostgreSQL chops being tested.
**Ugly**: Knex, Docker, Middleware. Trying to keep the faith.

I have one more app to finish, then my static/github portfolio should be good to go. Much easier than hosting databases publicly. Wish me luck.

---

9.4.23

How not to develop a React Node.js application:

1. Rush through your data structurization. As long as they contain the necessary columns and are relationally sound, you'll be fine, right? No need to plan further.
2. Never proof your API routes. If you mess up a variable name or return the wrong value in a route, surely you will catch that later.
3. Those deprecation warnings you keep seeing in NPM install? Probably nothing.
4. Certainly, there are no hooks outside of useState worth investigating for manipulating dynamic variables. React has nothing to offer outside of that.
5. On the same note, separation of concerns is an unnecessary hindrance. What you need is a 500-line god-level function that nobody can read or comprehend, including yourself.

Obviously, these are silly examples, but they are all things I had believed at one point albeit implicitly. Looking back, these 5 points are what sunk a lot of my productivity (and sanity) while fumbling my way through developing my first app from start to finish. If I could go back and speak to my slightly younger self, I think I would impart the following:

1. DATA TABLES/MODELS:Take your time organizing your SQL structures and models. The more robust and comprehensive they are, the more you can leverage them for better execution of front-end code. Try and visualize the features you'd like to have in your application, list them out (all of them, even the ones you're tentative about), and list the tables and methods you would need to achieve them. Consider within each table every column you would want to include or exclude, your relationships among them, and the models needed to execute those queries. Be as thorough as possible. The worst thing that could happen is you come up with backend functionality you don't need, which is a much better situation than trying to execute a client-side function and not having backend models you DO need.
2. APIS: For the love of God, check your API routes. Before doing anything. Before testing a function, before launching the client server, make sure everything is sound. Did you omit capitalization or pluralization? Are you querying your databases for what you think they should return? Are you referencing API functions correctly? Are you error handling correctly?
3. DEPENDENCIES: Upgrade your dependencies. I know it's scary, but this might mean actually referencing the documentation and not just sifting through Stack Overflow. But that's okay! Reading about concepts that are beyond your understanding and delving into elusive error messages are valuable and make you more resilient against setbacks and technical issues. Two quick examples: after developing on create react app forever (which has been deprecated for a while now), I switched to a more updated React framework (Vite). After spending countless hours looking through forums to resolve polyfill errors that riddle CRA apps, I just took a short detour through the React documentation, read through their recommendations, which ultimately led down a road that ended up in me being able to develop on a more secure, lightweight, and responsive iteration of React.
4. HOOKS: React is really good at consolidating complex logic and dynamic variables into succinct pairs (hooks). There's so much return on investment in learning them: useEffect, useCallback, useContext, useMemo. There's so much out there along with any custom hooks you might want to implement. Sometimes I feel like we get stuck in these ruts where we try and solve nuanced problems with tools that are familiar to us, then get frustrated when things don't work out. That's your time to research! Read through documentation! Watch a YouTube tutorial! There are things you don't know that you don't know. The only way to find out what that might be is to stop being so hyperfocused on the actual issue and to consider that if you investigated your tech stack better, a solution might come up where you weren't expecting.
5. SEPARATION OF CONCERNS: Lastly, simplify your functions. Seriously. Make them readable, succinct, and make their interconnection to existing logic plainly obvious. Your job is not to come off as smart, but rather to provide a working solution for whatever the issue is at hand. Then, you can make it more secure, reducing edge cases and vulnerabilities. Then, you can make things efficient. But until you have a working solution, make things simple! Sometimes we confuse complexity with efficacy. What looked like an advanced-level LeetCode problem can quickly turn into a one-liner using an existing JavaScript method if you take a step back and consider the problem and break it into its smallest components. There's no shame in breaking a simple React component into 4-5 subcomponents with succinct logic methods if it makes your app more robust and readable. Swallow your pride, simplify things for everyone's sake.

Thank you for reading! Looking forward to delving into the world of open source projects next. I'll break down my experience next week.

---

