# MEDIUM-EDITOR <!-- .element: class="presentationHeading" -->
### An open source story



# Once Upon A Time
* The Vistaprint Digital Team was building a website builder from scratch
* Needed a WYSIWYG (What You See Is What You Get) text editor at the core
* (maybe notes about project, team)


## WYSIWYG Text Editing Is Hard
* (Talk about the problem space, which drove wanting open source)



# Open-Source
* Wanted to tap into open source community
* Other people knew text editing better than we did
* Wanted help from others with maintaining it long term (who wouldn't want that?)



# The Search
* Alternatives explored: quill, scribe, wysihtml, ArteJS (VP open source), medium-editor



# ArteJS
* https://github.com/vistaprint/ArteJS
* This is a different way of going about open-source (build it and hope people come)
* Very little contribution or community



# medium-editor
* medium inspired text editor
* Found a vibrant, active community with 4000+ stars and 100+ contributors
* Began opening pull-requests for fixes/features we needed
  * [First PR](https://github.com/yabwe/medium-editor/pull/342)
* Really my first dive into open-source contributing



# A Surprising Email
* Show email I received from Davi asking if I want to become a collaborator
* Became collaborator on 01/27/15



# High Energy Rise
* Modularized the codebase
* Created a cross-browser testing framework for each PR (SauceLabs)
* Code coverage test and feedback on PRs (coveralls)
* (Things are going great...but Davi, the founder, leaves)


## Milestones
* 3/5/15 - 90% Code Coverage
* (TODO: Date) Formation of YABWE group
* (TODO: Date) Include on product hunt
* (TODO: Date) Most starred editor on github (~8k i think)
* (TODO: Date) v5.0.0 w/ documentation + plug-in framework
  * link-insert + editing feature


## Famous<span>Folks</span>
* <!-- .element: class="fragment" -->[Peter Higgins](https://github.com/phiggins42), ex-Dojo project lead reaches out to contribute to medium-editor (5th most contributions)
* <!-- .element: class="fragment" -->[Dave Winer](https://en.wikipedia.org/wiki/Dave_Winer), creator of [Scripting News](http://www.scripting.com)
  * Used medium-editor for a project, [opened issues](https://github.com/yabwe/medium-editor/issues/737), and wrote a [nice post](http://myword.io/users/davewiner/essays/045.html) about his experience
* <!-- .element: class="fragment" -->[John David-Dalton](https://twitter.com/jdalton), creator of lo-dash, maintainer of jsPerf and Benchmark.js
  * [Helped Us](https://github.com/yabwe/medium-editor/issues/771#issuecomment-180613622) support Edge and opened 3 bugs internally for Edge, hoping to be fixed in upcoming Edge releases!
* <!-- .element: class="fragment" -->[Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee), creator of World Wide Web, is using medium-editor in [his Dokieli project](http://csarven.ca/dokieli#figure-dokieli-article-fragment)


## Lots<span>of</span>Attention
* <!-- .element: class="fragment" -->Featured on the front page of [ProductHunt](https://www.producthunt.com/tech/medium-editor) (437+ upvotes)
* <!-- .element: class="fragment" -->Smashing Magazine (nearly 1M followers) [Tweeted about Medium-Editor](https://twitter.com/smashingmag/status/697057132324073472)
  * Endless Tweets if you search for medium-editor in twitter
* <!-- .element: class="fragment" -->Creators of Medium.com [asked us to change the name!](https://github.com/yabwe/medium-editor/issues/949)



# Foreshadowing
* I've rewritten a ton of the code and documentation, but now others don't know how to do much
* I'm spending most of my out-of-work time on medium-editor


## Other Not-So-Glamourous-Parts
* Code Quality vs Low Barrier To Entry
* Encouraging documentation and best practices
* Responding to unhelpful issues / feature requests



# Cracks Are Showing
* My sitebuilder team begins working on other things
  * Noah hasn't contributed in 1+ years
* Now I'm supporting it on my own
* Start trying to recruit others to YABWE to help
  * Engineering Lightning Talk (03-22-16)
  * Pushing on yabwe folks



# Things Get Worse
* 0% of time at work is on medium-editor
* medium-editor is at v5.23.0, Tower is using v5.8.2! ([Jira Ticket](https://jira.digital.vistaprint.io/browse/DG-15568))
* ProseMirror + DraftJS + Carbon emerge, as state-backed editors
* TODO's on my home dry erase board are so old they won't erase



# Decision Time
* Do I pause (or end) my VP career to work on this open-source editor with 10k+ stars and lots of attention?
* (Spoiler Alert) I abandoned the editor



# medium-editor Today
* Sitebuilder still isn't using the latest version
* medium-editor is up over 1200 stars
* endless un-triaged issues, very painful for me
* I've popped in once or twice and tried to get things moving and then lost momentum quickly


## Where Are They Now?
* Me & my career
* Noah
* Davi
* j0ker
* yabwe



# Lessons
* Things to consider when working on open-source
* Importance of vibrant community and sharing ownership/contribution
* Maintaining a popular project is a full-time job
* Work and life initiatives change, always need an exit strategy
  * Davi recruited me quickly, I waited too long to do the same



# tl; dl; (too long; didn't listen;)
* We found and were invited to take over most starred text-editor on Github
* Lots of contributors, publicity, glamour, etc.
* Eventually the work team moved on but I couldn't
* I did too much on my own - had to chose between my job or the project
* Shame, regret, soul-searching, tears, and a Developer Days Talk (so meta)
  * (Link/Snapshot of how popular it still is - 11k npm downloads/week | 12,665 stars)


# Questions?
