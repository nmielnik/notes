# MEDIUM-EDITOR <!-- .element: class="presentationHeading" -->
### an open source story <!-- .element:  -->

<em><small>featuring the baffling blabber of <span class="alt-color">Nate Mielnik</span></small></em>

* TODO: NEED A REAL INTRO



## Once <span class="alt-color">Upon</span> A Time
![Tower](http://proofofthought.io/cdn/talks/work-tower.png)<!-- .element: class="tower-inline" --> our new website builder needed an open-source WYSIWYG text editor...

![WYSIWYG Editor](http://proofofthought.io/cdn/talks/wysiwyg-demo.gif)<!-- .element: style="width: 600px" -->



## <span class="alt-color">Open</span>-Source
* Why Open Source?
  * Other people knew text editing better than we did
  * Wanted help from others with maintaining it long term (who wouldn't want that?)
* Types of Open Source projects
  1. build it yourself, share it, hope someone cares
  2. use it, don't contribute, complain when it breaks



## The <span class="alt-color">Search</span>
* Alternatives explored:
  * scribe (platform w/ plugins from The Guardian)
  * quill (coffeescript editor, now maintained by slab)
  * wsyihtml5 (most popular, handed-off, untouched for 2 years)
  * CKEditor (custom, huge, "licensed" open-source)
  * ArteJS (vistaprint?)



<!-- .slide: data-background-image="http://proofofthought.io/cdn/talks/artejs-screenshot.png" data-background-size="cover" -->
## ArteJS <!-- .element: class="over-image" -->
<ul class="over-image-list">
<li class="fragment over-image">Build it and see what happens (common "open-source" mentality)</li>
<li class="fragment over-image">11 contributors (all Vistaprint)</li>
<li class="fragment over-image">No commits for 4 years</li>
<li class="fragment over-image">No maintenance needed</li>
</ul>



## medium-editor
* Medium-inspired, looked nice, just worked
* Approachable code base w/ vibrant community (4000+ stars, 100+ contributors)
* Won the bake-off over ArteJS (sorry Nick Swider)
* Began opening pull-requests for fixes/features we needed
  * [First PR](https://github.com/yabwe/medium-editor/pull/342)
* Really my first dive into open-source contributing



## A Surprising <span class="alt-color">Email</span>

![Email From Davi](http://proofofthought.io/cdn/talks/medium-editor-davi-email.png)

<em><small>Nate & Noah Chase became collaborators on 1/27/15</small></em><!-- .element: class="fragment" -->



## High Energy <span class="alt-color">Rise</span>
* Modularized the codebase
* Created a cross-browser testing framework for each PR (SauceLabs)
* Code coverage test and feedback on PRs (coveralls)


<!-- .slide: data-background-image="http://proofofthought.io/cdn/talks/medium-editor-landing2.png" -->
### Milestones <!-- .element: class="over-image" -->
<ul class="over-image-list">
<li class="fragment over-image"><span class="alt-color">3/5/15</span> 90% Code Coverage</li>
<li class="fragment over-image"><span class="alt-color">6/28/15</span> v5.0.0 w/ documentation + plug-in framework</li>
<li class="fragment over-image"><span class="alt-color">10/8/15</span> Formation of YABWE group</li>
<li class="fragment over-image"><span class="alt-color">1/24/16</span> #4 Product of the Day on ProductHunt</li>
<li class="fragment over-image"><span class="alt-color">1/31/16</span> Most starred editor on github (~8k i think)</li></ul>
</section>


### Lots <span class="alt-color">of</span> Attention
* <!-- .element: class="fragment" -->Featured on the front page of [ProductHunt](https://www.producthunt.com/tech/medium-editor) (437+ upvotes)
* <!-- .element: class="fragment" -->Smashing Magazine (nearly 1M followers) [Tweeted about Medium-Editor](https://twitter.com/smashingmag/status/697057132324073472)
  * Endless Tweets if you search for medium-editor in twitter
* <!-- .element: class="fragment" -->Creators of Medium.com [asked us to change the name!](https://github.com/yabwe/medium-editor/issues/949)


### Famous <span class="alt-color">Folks</span>
* <!-- .element: class="fragment" -->[Peter Higgins](https://github.com/phiggins42), ex-Dojo project lead reaches out to contribute to medium-editor (5th most contributions)
* <!-- .element: class="fragment" -->[Dave Winer](https://en.wikipedia.org/wiki/Dave_Winer), creator of [Scripting News](http://www.scripting.com)
  * Used medium-editor for a project, [opened issues](https://github.com/yabwe/medium-editor/issues/737), and wrote a [nice post](http://myword.io/users/davewiner/essays/045.html) about his experience


### More Famous <span class="alt-color">Folks</span>
* <!-- .element: class="fragment" -->[John David-Dalton](https://twitter.com/jdalton), creator of lo-dash, maintainer of jsPerf and Benchmark.js
  * [Helped Us](https://github.com/yabwe/medium-editor/issues/771#issuecomment-180613622) support Edge and opened 3 bugs internally for Edge, hoping to be fixed in upcoming Edge releases!
* <!-- .element: class="fragment" -->[Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee), creator of World Wide Web, is using medium-editor in [his Dokieli project](http://csarven.ca/dokieli#figure-dokieli-article-fragment)



## Awesomeness
* TODO: What project did we pass to get to #1?
* TODO: Screenshot of github editor rankings
* TODO: Davi mentions state-based editor...Later I realize this too



## Foreshadowing
* I've rewritten a ton of the code and documentation, but now others don't know how to do much
* I'm spending most of my out-of-work time on medium-editor
* (Things are going great...but Davi, the founder, leaves) 



### Other <span class="fragment">Not-So-</span><span class="alt-color">Glamourous</span> Parts
* Code Quality vs Low Barrier To Entry
* Encouraging documentation and best practices
* Responding to unhelpful issues / feature requests



## Cracks <span class="alt-color">Are</span> Showing
* My sitebuilder team begins working on other things
  * Noah hasn't contributed in 1+ years
* Now I'm supporting it on my own
* Start trying to recruit others to YABWE to help
  * Engineering Lightning Talk (03-22-16)
  * Pushing on yabwe folks



## Things Get <span class="alt-color">Worse...</span>
* 0% of time at work is on medium-editor
* medium-editor is at v5.23.0, Tower is using v5.8.2! ([Jira Ticket](https://jira.digital.vistaprint.io/browse/DG-15568))
* ProseMirror + DraftJS + Carbon emerge, as state-backed editors
* TODO's on my home dry erase board are so old they won't erase



## Decision <span class="alt-color">Time</span>
* 7/25/16 - Receive email offering to fund yabwe
* TODO: Email about funding
* Do I pause (or end) my VP career to work on this open-source editor with 10k+ stars and lots of attention?
* (Spoiler Alert) I abandoned the editor
* TODO: What does abandoning mean, could be funny to talk about (ie Dictionary definition)



## medium-editor <span class="alt-color">Today</span>
* Sitebuilder still isn't using the latest version
* medium-editor is up over 12000 stars
* endless un-triaged issues, very painful for me
* I've popped in once or twice and tried to get things moving and then lost momentum quickly



## Lessons
* Things to consider when working on open-source
* Importance of vibrant community and sharing ownership/contribution
* Maintaining a popular project is a full-time job
* Work and life initiatives change, always need an exit strategy
  * Davi recruited me quickly, I waited too long to do the same



### tl; <span class="alt-color">dl;</span> (too long; didn't listen;)
* We found and were invited to take over most starred text-editor on Github (medium-editor)
* Lots of contributors, publicity, glamour, etc.<!-- .element: class="fragment" -->
* Org moved on but I was still supporting it<!-- .element: class="fragment" -->
* I abandoned the project and didn't hand it off well<!-- .element: class="fragment" -->
* <!-- .element: class="fragment" -->Shame, regret, soul-searching, tears, and a **Developer Days Talk** 

* still popular (12.6k stars | 11k npm dl/week)<!-- .element: class="fragment" -->



## Questions?

![Questions](https://s3.amazonaws.com/proofofthought.io/cdn/talks/medium-editor-nate-commit.png)
* TODO: Add Yet Another Broken WYSIWYG Editor comment