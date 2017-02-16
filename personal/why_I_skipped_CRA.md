# Why I Skipped 'Create React App' and Made My Dev Environment from Scratch to Learn React
> Nitish Dayal
> Feb. 15, 2017

### Preface

**Create React App is the shit**. I've got nothing but good things to say about it. There's a tool
  out there that, FOR FREE, will basically walk you through creating your first application all
  the way from development to deployment. That's awesome. Need to modify or customize the 
  configuration? Eject and gain access to clean, structured, well-documented configurations that
  are as easy to reason about as possible. It's great. And it can be great for learning React
  as well, given _your personal style of learning_. It wasn't for me, and Dan was curious
  about what issues I had with it. My response is more than 160chars so the conversation has moved
  to Github. :)

---

tldr; CRA is great, I just need to know what things do and why, and if you're anything like me
  then CRA might end up not being for you (for learning) either. I don't think there's anything
  that needs to be done to address this, because I don't personally think it's a thing to address.
  The tool handles a lot of stuff for you. That's the point. The docs are very clear, provided
  boilerplate is easy to reason about, it's all good. But if you're just dipping your toes in,
  I think the better route to take is to learn the tools and create an environment from scratch.
  There's a lot of value in it.

## My React Learning Experience (thus far)

I'm still new to React. Been playing with it for ~maybe a month. Never used Babel prior to this,
  and had some experience with Webpack from when I was working with Angular & the Angular-CLI
  (TBH all I did was copy the configuration from the Angular docs, and once I discovered the CLI
  the only time I had to do anything with it was when/if I wanted to include any additional
  libraries). Ken (Wheeler) told me to learn React because, and I quote, "react is the shit".
  Started learning React by going through the official docs, taking notes, etc.
  The documentation recommended using Create-React-App to get started, so that's what I did.

_Aside: React documentation is A+ btw 👍🏾. After a while I stopped talking notes because
  the explanations are simplified enough that reading was enough to make things click. 
  10/10, my go-to reference if I have any React-based questions_
  
The first issue (for me) will go back to that whole 'personal learning style' thing,
  and that is too much 'magic'. Yes, I hate me as well. I know the
  entire reason this was created was because the community vocalized that configuration was
  one of the primary detractors. There will always be both crowds, 'too much magic' or 'too
  much config', regardless what you do. <3 Not a complaint about the product, just a personal
  thing. I like to know how things are happening, why, etc. So I ejected.

The second issue was dependency overload. After I ejected, I ran `npm ls --depth=0`. I was
  not amused. '_What the hell is all this? Why? There's no way._' sums up my initial response.
  6 dependencies for Babel, 6 for ESLint, a bunch of Webpack plugins, etc. I had no idea
  what any of this was, what it did, why/if I needed it, how it was set up (the configuration files,
  as well documented as they are, still rely on some existing knowledge of these tools), etc.

Which brings me to my final point: To learn React (or any JS library) in 2017, you need to learn
  JavaScript in 2017, and to work with JavaScript in 2017 you should know these tools. This is, once again, a personal opinion, but I will never understand the purpose or value of a tool unless
  I first understand the problem it's trying to address. There is also an argument to be made for
  having the ability to debug these tools. If I'm new enough to JS and I see **any** error coming
  from some tool that was configured automatically, I'm not going to be able to debug with any
  confidence and will potentially end up making a mess of things.

Once again, this is all very subjective. Not in any way meant to take away from CRA and it's uses,
  and given a different learning approach I'm sure it could be great. Once I get comfortable enough
  with all this stuff I'm sure I'll be using CRA quite often to quickly prototype ideas or to make
  a playground or to steal the npm scripts (👨‍💻). At this point, while I'm still learning, I have
  found a great amount of value in taking the time to understand what these tools offer. An ESLint
  error does not necessarily mean I did something wrong, could just be that a rule needs to be
  adjusted. Learning Babel setup introduced me to some ES6/ESNext features that I otherwise would
  have not known about. Learning how Webpack is used in JavaScript environments explained
  how modularity can not only be a benefit for the developer, but how it can impact performance
  for the end user. And all those dependencies started looking normal and less intimidating once
  I knew what the hell they did.
