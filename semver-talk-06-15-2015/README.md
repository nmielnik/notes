### Version 0.1.0 of my Semantic Versioning Knowledge
Semantic Versioning has a website: [Semantic Version](http://semver.org/)



# Disclaimer
* <!-- .element class="fragment" -->I don't know a whole lot about semantic versioning
* <!-- .element class="fragment" -->I've been trying to learn how to use it while helping maintain [medium-editor](https://github.com/yabwe/medium-editor)
* <!-- .element class="fragment" -->Most of the reference info came from one [Wikipedia page](https://en.wikipedia.org/wiki/Software_versioning)
* <!-- .element class="fragment" -->I've run into interesting circumstances and read some interesting stuff, so I'm sharing
* <!-- .element class="fragment" -->I'm hoping I can learn more about this from you guys, so please jump in



## Software Versioning
* [What it is goes here]



## Example: Increasing Numbers/Letters
* Every build, generate an increasing number
* Easy to implement
* Can't tell much about features or compatability looking at the version number


## Example: Pi/e based
* Start with 3, then add decimal digits of pi until you converge (Donald Knuth)

> <!-- .element style="font-size: 80%;" -->At the time of my death, it is my intention that the then-current versions of TEX and METAFONT be forever left unchanged, except that the final version numbers to be reported...should become:

<!-- .element style="font-size: 80%;" -->
```
TeX, Version $\pi$
METAFONT, Version $e$
```

> <!-- .element style="font-size: 80%;" -->From that moment on, all “bugs” will be permanent “features.”


## Example: Date Based (Microsoft)
* Tie the release to the date when it was launched
  * Windows 95
  * Adobe Illustrator 88


## Example: NumVersion Struct (Apple)
* A [gist](https://gist.github.com/sjehutch/98f31b79495901e8e0ef) about this
* <!-- .element style="font-size: 60%" --><code style="font-size: 80%;">[major revision].[minor revision].[bug revision]-[stage][stage revision]</code>
  * **Major:** significant changes and/or jumps in functionality and/or programming
  * **Minor:** minor feature changes and/or significant bug fixes
  * **Bug Revision:** indicate minor bug fixes
  * **Stage:** 
    * "dev" stages are internal private releases
    * "alpha" stages are very early releases that may or may not be plublic releases
    * "beta" stages are public releases intended for early adopters and other "beta testers"
    * "rc" stages are release candidates indended for more widespread testing
    * "final" stages are stable releases and should be "production worthy"
  * **Stage Revision:** Starts at 1 & keeps incrementing until a release


## Example: Launch Based
* Per software launch (used for Microsoft account)
  1. Start working on 1.0 until you launch
  2. At some point, start working on 2.0 (branched off of 1.0)
  3. Launch 1.0
  4. Merge 1.0 code into 2.0
  5. Repeat
* All hotfixes of released code (1.0) are merged into 2.0 code
* During development, append the build number on each built candidate
  * ie: 1.0.1234


## Example: Other Versions "Strategies"
* Odd # for development / Even # for stable
  * 2.3 vs 2.4 of Linux
* Represent how close to next major release you are
  * 7.1 vs 7.5 of MacOS (50% of the way to version 8)
* "Add" Backwards Compatability
  * Winamp v5 supported both 2 + 3 (= 5)
* Superstition
  * 12.0 vs 14.0 of Microsoft Office (2007 vs. 2010)



## Semantic Versioning (semver)
* X.Y.Z
  * X = major
  * Y = minor
  * Z = patch/revisioning
* npm packages use this (well, they're supposed to anyway) 
* There is an established standard



## Jeremy Ashkenas is not a fan
* Creator Backbone & Underscore was very [vocal about SemVer](https://gist.github.com/jashkenas/cbd2b088e20279ae2c8e) last summer
  * [Hacker News](https://news.ycombinator.com/item?id=8244700)
  * [Follow SemVer (Backbone)](https://github.com/jashkenas/backbone/issues/2888)
  * [Underscore does not follow SemVer](https://github.com/jashkenas/underscore/issues/1684)

