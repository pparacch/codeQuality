### Code quality

---

Agenda:

* why __code quality__ is important
* what we __can do__ to __improve__ our __code quality__
* hands-on session

---

#### Why do we program?

+++

Primary reasons:
* pure pleasure
* it is fun

+++

... but also to

* create something valuable
* automate work that is
  * tedious
  * slow
  * error-prone
  * boring?

+++

In the process we are __solving many smaller problems__!

+++

These smaller problems form

> atomic building blocks of the final solution

and are often seen in other problems.

+++

> atomic building blocks

* obtaining data from wherever
* parsing and cleaning csv
* splitting dataset
* reading ECLIPSE files
* and writing them
* plotting production curves
* ...

---

### Why code quality is important!

+++

3 & 6 June, 1980  
__[NORAD and Nuclear Attack Warnings](https://en.wikipedia.org/wiki/North_American_Aerospace_Defense_Command)__

<small>"... On 3 June 1980, and again on 6 June 1980, a computer communications device failure caused warning messages to sporadically flash in U.S. Air Force command posts around the world that a nuclear attack was taking place. ..."</small>

___

26 September, 1983  
__[The man who saved the world by doing nothing](https://www.wired.com/2007/09/dayintech-0926-2/)__

<small>"... One of the satellites signaled Moscow that the United States had launched five ballistic missiles at Russia ..."</small>

+++

__Productivity vs. Time__
  - __messy code__ will __slow you down__

![messiness](./messiness.png)

+++

You are not an __island__ and,  
... your __memory__ will __fail you__!

![code_quality](https://2.bp.blogspot.com/-m-Bkg3LXUW4/V2gAjK9uraI/AAAAAAAC25c/xR3MAwqOLToDOh6k6jDaZZC2qmQT9EwCACLcB/w1200-h630-p-k-no-nu/Poor-Software-Quality.jpg)

---

### What's code quality

+++

It's about ...

- doing __the right thing__, __right__
  - well-tested
  - bug-free code

- writing __clear__ & __simple__ code
  - easy to __understand__
  - easy to __maintain__
  - __self-explaining__

---

### Fundamentals & Best Practice

<small>_... get your machine in order and up to date with the tools you need to do your work._</small>

+++

#### Runtime Environment

- Which __version of python__ is used?
- Which __version of the packages__ are required?

If using __vanilla python__ look into __Virtualenv__, and  
if using __conda__ look into __environment__.

+++

An example of a conda __environment__ <small>(environment.yml)</small> ...

```
name: myEnvironment

dependencies:
- python=3.6.2
- pip:
  - pandas==0.21.0
  - nltk==3.2.5
  - scikit-learn==0.19.1
  - scipy==1.0.0
```

+++

#### Version Control (Git)

__Version control__ is vital when working on a task, whether __alone__ or __with other people__.

* we use git for _all_ code

+++

<small>"_Git thinks about its data more like a stream of snapshots._"</small>

![git-basic](https://git-scm.com/book/en/v2/images/snapshots.png)

+++

<small>"_Git allows to work with branches ... A branch is just an indipendent line of developmnet._"</small>

![git-branch](https://git-scm.com/book/en/v2/images/basic-branching-6.png)

+++


#### Interactive Development Environment (IDE)

_An IDE does not make you a good programmer but_  
_can make your life easier._

- code completion
- syntax highlight
- search
- refactoring
- debugging
- ...

+++

#### Coding style

- [PEP-8 Standard](http://legacy.python.org/dev/peps/pep-0008/)
  - __Indents__
    - 4 spaces (no TAB)
  - __Maximum line length__
    - 80 characters
  - __Blank lines__
    - 2 between imports/ classes/ functions;
    - 1 between methods within a class
  - ...

+++

- [Google Python](https://google.github.io/styleguide/pyguide.html)
  - __Indentation__
    - Indent your code blocks with 4 spaces
  - __Statements__
    - Generally only one statement per line.
  -  __Naming__
    - naming for module (module\_name), ...
  - ...

+++

#### [Clean Coder](https://cleancoders.com/) Rules

- __Use meaningful names__
  - variables, functions, arguments, ...
- __Explain yourself in code__
- __Use comments for special case__
- __Function must be small__
  - try to limit to 20/ 30 lines of code


+++

- __Each function should do one thing__
  - they should do it well and they should do it only
- __Limit the number of arguments for a function__
  - less than 3
- __Limit nesting within a function__
  - less then or maximum _2 nesting level_
- ...

---

### Script style development

In Statoil we often see there are __scripts__ written that have __no functions__ and span __200-2000__ lines of code (LOC).

+++

Some __examples__ recently include
* 764 LOC __else__ block
* 700 LOC function called __`run()`__
* 500 LOC function called __`named_plot`__
  * taking __14 parameters__
* 400 LOC script, __no functions__

+++

![orelse](https://raw.githubusercontent.com/pgdr/tdd/master/orelse.png)

+++

An example ...

```python
# Ugly! Workaround to use multithreading and speed-up plotting
# TODO: create plotting class
def plot_inflow(w, p, lock=''):         # :1175
  #...
  for n, d in enumerate(well_block_prod.get_prt_dates(p)):
    if prod[0][1] == -9999:
        #...
    else:
      if percentage_opt:
        #...
        if flag_tot_contribution:
          #...
          for i, t in enumerate(oil_prod):
            if t <= 0:
              oil_prod[i] = 1.0E-9      # :1332
```

+++

#### Change Strategy: Divide & Conquer

* use __functions__, small function
  * each function should __do one thing__ (SRP)
  * _recommended_ at most __30 LOC / function__
    * but more is ok if it __still__ does __one thing__!
  * fewer LOC if complex control flow
  * maximum nesting
    * 2 levels (_recommended_)
  * maximum no of arguments
    * 2/3 (_recommended_)

---

#### Reusable programs

We want to write __resusable__ programs.

+++

When we

* spend 3 months writing a
* 500 LOC function taking
* more than 10 parameters,

guaranteed not used by anyone else  
(__ensure no reusability__).

+++

Those three months are much more expensive

than if you write code that many others can use.

---

### Short functions

We wnat to write __short functions__.

+++

Long functions _are not well-defined_

What does `run()` do in those 700 LOC?

+++

If you cannot explain in a couple sentences exactly

* input
* output
* assumptions
* side-effects

Then
* not reusable
* should be split.


+++

#### Command-Query Separation (CQS) principle

* A function should either
  * do something _(command)_
    * e.g. change the state of an object
  * or answering something _(query)_
    * e.g. return some information about that object

---

### Test driven development (TDD)


+++

#### Doing the thing right

To ensure that our code runs correctly, we write a _unit test_.

```python
def root_mean_square(lst):
    rms = sum(map(sq, lst))/len(lst)
    return sqrt(rms)

def test_rms():
    assert root_mean_square([4,5,6,7,8]) == 6.1644
```

+++

##### Unit test

In unit testing, each individual unit of code (e.g. a function) is tested in isolation to see if given certain conditions it responds in the expected way

+++

Unit tests ensure several things, amongst
* ensure _correctness_
* act as a _security net_
  * inform when behavior changes
* force you into using your own program (dogfood)

+++

#### The TDD heart-bit

- Write a test
  - __make it fail__
- Write the code (KISS)
  - __make it pass__
- repeat

+++

##### Tests first

We write the test first!

```python
def test_rms():
    assert root_mean_square([4,5,6,7,8]) == 6.1644
```

There is no such function!  So the tests will fail!

+++

##### Test first -- then implement (KISS)


```python
def root_mean_square(lst):
    # ...
```

Hopefully written correctly, tests pass!

+++


##### Test first -- then implement

Simultaneously, you got to

* forced to think about the interface
* use your own interface before writing it
* making it easy to understand
* reusable
* well defined

---

![goodcode](https://imgs.xkcd.com/comics/good_code.png)

EOF
