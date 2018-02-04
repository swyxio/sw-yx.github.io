---
layout: post
date: 2018-02-02
tags: storybook
feelings: happy
title: storybook addons
comments: true
description: storybook starts to payoff
---

day 1 as a professional developer. spent all day in orientation/compliance training, but got to set up my new macbook pro at work. first trip-up: i was unable to successfully install `nvm` on my new machine :disappointed: (relatedly, I can’t `npm i -g` without `sudo`)
 some highlights:
- straight up telling you if you do anything on company laptop they own it/no privacy
- no free lunch but lotsa snacks
- meeting the ceo/cto
- finding out that i’m working with ex google/airbnb/appnexus/commonbond people, nobody rubbed me the wrong way/everyone seems nice

day 2 of work
- decided on working in react and typescript but everything else is wiiide open (i was told this would be the case). this means researching and making the calls on whether to adopt everything from redux to styled-components to graphql to build tools. boss doesn't like redux because "its not OO". I silently respond with "thats the point".
- couple hours meeting with other dept heads to learn about what they do. we know surprisingly little about the job before showing up. the product ambtion is massive and this is my first exposure to how to architect multiple layers of backend services that will all feed into the frontend (aka me/my team)
- nerding out with designer about variable fonts
- one thing they dont tell you about starting a job is you still have to keep selling yourself. i forgot about that. today we did the "lets go around the table and talk about our background" lunch. everyone else is like ex airbnb/google/goldman/appnexus/wealthfront. had to fight the urge to not mention i did a bootcamp. its dumb. im hired anyway, they cant take this away from me. yet.
- midday, head to the gym, do 30 pullups with desk neighbors as a form of break. its a fairly fitness friendly culture.
- setting up macbooks. ugh. install git, install brew, install node and python, etc etc etc, install vscode, install all my old git aliases and toys, setup private github. still havent figured out how to install nvm but i dont need it for now.
- clone the mvp app that my colleagues built, make some edits and make my first commit

day 3 - do basic typescript + react tutorials. i browsed a typescript course on pluralsight before but now i do it for a living i have to pay attention. more business meetings trying to understand how the backend team works, including a super helpful API demo. every little thing has to be decided. i want to use yarn. boss dislikes. i want to use Prettier. prettier doesnt support typescript well. styled components vs sass? on and on. the last debate was whether to use Storybook or write our own. Boss doesnt want to use storybook because it has bad DX, eg it doesnt have create-react-app’s error overlay. i dig into the source code and (with help) figure out undocumented functionality to get it done. Open source FTW https://github.com/storybooks/storybook/issues/2890

day 4. morning meeting figuring out backend APIs and getting AWS CLI to work. get assigned first real JIRA tickets at lunch, volunteer for the most laborious but the least abstract task (implementing a bootstrap list item component library with react storybook). by end of day i have a basic component lib up and running, including a nice storybook addon called `storybook-addon-jsx` that shows usage code alongside the rendered component. after yesterday’s shenanigans and today’s work i was called “the storybook expert”. they dont know that this is the first time i’ve touched this but i got the ideas from watching Cory House’s components course on PluralSight. 5pm: company happy hour.

recap of day 5 (the last in this series). really fighting typescript and react. Because typescript needs to know the types of all the things, it is a lot harder to do simple react idioms that I’ve come to take for granted. Eg if you have `const ComponentA = props => <ComponentB {...props} />`, you can’t just use `<ComponentA foo="bar" />` because ComponentA needs to know in advance what prop types are coming in. Also if you want to use `this.state.items.map( item => item.property)` you have to specify AAAAALLL the types all the way down. Separately, I realized that even among React developers there can be a lot of strong differences in code style and preferred solutions for common problems. e.g. HOCs vs render props, styled-components vs SASS. It is important not to be obstinate about your own preferences. After all, you’re being paid to learn new styles!
