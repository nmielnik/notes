# Semantic Versioning
### My Knowledge v0.1.0
Semantic Versioning has a website/spec: [Semantic Version](http://semver.org/)



# Disclaimer
* <!-- .element class="fragment" -->I don't know a whole lot about semantic versioning
* <!-- .element class="fragment" -->I've been trying to learn how to use it while helping maintain [medium-editor](https://github.com/yabwe/medium-editor)
* <!-- .element class="fragment" -->Most of the reference info came from one [Wikipedia page](https://en.wikipedia.org/wiki/Software_versioning)
* <!-- .element class="fragment" -->I've run into interesting circumstances and read some interesting stuff, so I'm sharing
* <!-- .element class="fragment" -->I'm hoping I can learn more about this from you guys, so please jump in



## Why Have Version Numbers?
* <!-- .element class="fragment" -->Give a bunch of code a name or unique identifier
* <!-- .element class="fragment" -->Used for keeping track of incrementally different versions of electronic information, whether or not this information is computer software.
* <!-- .element class="fragment" -->Used by consumer, or client, to compare their copy of the software product against another copy.
* <!-- .element class="fragment" -->Used by support to ascertain exactly what code a user is running.



## Example: Increasing Number
* Every build, generate an increasing number (ie Build Number)
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

> <!-- .element style="font-size: 80%;" -->From that moment on, all "bugs" will be permanent "features."


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
* npm uses this (well, they're supposed to anyway)
* Major.Minor.Patch is a pretty common syntax



## Define Your API
* What are the public facing pieces of your application?
* Definitely API:
  * Endpoints used to initialize or interact with your code
  * Public methods on your core application
* Until API is stable and well-documented, remain in version 0.y.z



## Major Version (1.x.x)
* Any change to API methods which are not backwards-compatible require a major version increase
* Examples:
  * Renaming a method or initialization option
  * Removing a method
  * Changing the default value of options/parameters



## Minor Version (1.1.x)
* Any new functionality which is backwards-compatible
* Examples:
  * Adding new methods
  * Adding optional parameters to methods (default to previous behavior)
  * Adding new initialization options
  * Exposing new objects or APIs
* <!-- .element class="fragment" -->NOTE: Once something new has been released, it can't be changed without a major release
* <!-- .element class="fragment" -->NOTE: A minor release which accidentally breaks users requires an immediate minor release to fix it



## Patch/Revision Version (1.1.1)
* Any fix for a bug which does not add new functionality
* Bug fixes



## Pre-Release (2.0.0-alpha.1)
* Pre-releases may be denoted by appending a hyphen and a series of dot separated ASCII identifiers
  * 1.0.0-alpha.2
  * 1.0.0-beta
  * 1.0.0-rc.1
  * 1.0.0-0.3.7
  * 1.0.0-x.7.z.92



## Build Numbers (1.1.1+X)
* Build numbers can be added by appending a plus (+) and a series of dot separated ASCII identifiers
  * 1.0.0+20130313144700
  * 1.0.0-beta+exp.sha.5114f85



## Precedence
* Major > Minor > Patch
  * 1.0.0 < 1.1.0 < 1.1.1 < 1.2.0 < 2.0.0
* Pre-releases are LOWER precedence than a Major.Minor.Patch release
  * <!-- .element class="fragment" -->1.0.0-alpha
  * <!-- .element class="fragment" -->< 1.0.0-alpha.1
  * <!-- .element class="fragment" -->< 1.0.0-alpha.beta
  * <!-- .element class="fragment" -->< 1.0.0-beta 
  * <!-- .element class="fragment" -->< 1.0.0-beta.2
  * <!-- .element class="fragment" -->< 1.0.0-beta.11
  * <!-- .element class="fragment" -->< 1.0.0-rc.1
  * <!-- .element class="fragment" -->< 1.0.0



## npm Notation
* You can specify how you want to upgrade in your package.json [node-semver](https://github.com/npm/node-semver)
* A `version range` is a set of `comparators`
* A `compartor` is composed of an `operator` and a `version`
* Operators are for comparing version


### Operators
* `<` Less than
* `<=` Less than or equal to
* `>` Greater than
* `>=` Greater than or equal to
* `=` Equal. (Default if no operator is passed)
* `||` Or.
* By default, multiple comparators are treated as an `AND` so they specify intersection


### Examples
* `>=1.2.7`
  * Matches: `1.2.7` | `1.2.8` | `2.5.3` | `1.3.9`
  * No Match: `1.2.6` | `1.1.0`
* `>=1.2.7 <1.3.0`
  * Matches: `1.2.7` | `1.2.8` | `1.2.99`
  * No Match: `1.2.6` | `1.3.0` | `1.1.0`
* `1.2.7 || >=1.2.9 <2.0.0`
  * Matches: `1.2.7` | `1.2.9` | `1.4.6`
  * No Match: `1.2.8` | `2.0.0`


### Hyphen & X-Ranges 
* Hyphen `-` Ranges
  * `x.y.z - a.b.c`
  * Includes all numbers >= LEFT and <= RIGHT
* X-Ranges (X or *, case-insensitive)
  * `*`, `X`, or `x` can "stand-in" for any version (wildcard)
  * `1.2.x`
  * `1.*`
  * `*`


### Tilde & Caret Ranges
* Tilde Ranges
  * Allows patch upgrades to minor-specific version, minor upgrades otherwise
  * `~1.2.3` equals `>=1.2.3 <1.3.0`
  * `~1` equals `>=1.0.0 <2.0.0` equals `1.x`
* Caret Ranges
  * Allows changes that don't modify the left-most non-zero digit
  * `^1.2.3` equals `>=1.2.3 <2.0.0`
  * `^0.2.3` equals `>=0.2.3 <0.3.0`


### Functions
* `valid(v)` | `inc(v, release)` | `major(v)` | `minor(v)` | `patch(v)`
* `diff(v1, v2)` | `compare(v1, v2)`
* `validRange(range)` | `satisfies(version, range)`
* `outside(version, range, hilo)`



## What's Good
[Talk about Good Things]



## What's Bad
[Talk about Bad Thigns]
[Humans choose releases, so it's not gonna be perfect]


## Jeremy Ashkenas is not a fan
* Creator Backbone & Underscore was very [vocal about SemVer](https://gist.github.com/jashkenas/cbd2b088e20279ae2c8e) last summer
  * [Hacker News](https://news.ycombinator.com/item?id=8244700)
  * [Follow SemVer (Backbone)](https://github.com/jashkenas/backbone/issues/2888)
  * [Underscore does not follow SemVer](https://github.com/jashkenas/underscore/issues/1684)
* You can't blindly rely on SemVer, you have to look at your upgrades
* "Pedantic" or "Romantic" versioning


## Interesting Cases
* Changes to extendable objects
  * What if Backbone Models had a new built-in method named 'render'?
  * What if React exposed a new life-cycle method?
* Change in produced output of a function
  * Tower component.render() now produces slightly different HTML
  * Is that major / minor / patch?
* Any other weird cases?



## Medium-Editor
[Talk about deprecated methods]
[Talk about reducing API surface area]
[Talk about utility methods]
[Talk about base classes]
[Talk about bulding up to a major release]



# Semantic Versioning
## Is your knowledge v1.0.0 now?
* How about 1.0.0-alpha.1?
* Maybe 1.0.0-alpha.1+AFewMoreQuestions?
