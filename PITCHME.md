### Code quality

---

Agenda:
* We will look at a script and see some drawbacks of the "script style"
* We try to explain the benefits of many small functions rather than few big functions

+++
* We will see how to
  * use git (some experience is assumed)
  * write tests
  * setup automatic testing
  * make a setup script that can be used to test and install the project


---

### Why Code quality is important!

+++

3 & 6 June, 1980: __[NORAD and Nuclear Attack Warnings](https://en.wikipedia.org/wiki/North_American_Aerospace_Defense_Command)__

> "... On 3 June 1980, and again on 6 June 1980, a computer communications device failure caused warning messages to sporadically flash in U.S. Air Force command posts around the world that a nuclear attack was taking place. ..."

+++

26 September, 1983: __[The man who saved the world by doing nothing](https://www.wired.com/2007/09/dayintech-0926-2/)__

> "... One of the satellites signaled Moscow that the United States had launched five ballistic missiles at Russia ..."

+++

__Productivity vs. Time__
  - being __slow down__ by __messy code__

+++

![codequality](https://imgs.xkcd.com/comics/code_quality.png)  

---

### What's code quality

+++

- Doing the right thing, right
  - Well-tested & bug-free code

- Clear & simple
  - Easy to understand
  - Easy to maintain
  - Self-explaining


---

### git lightning intro

* we use git for _all_ code
* we use git for _all_ code

+++

* commits
  * patch = diff
* branches
* remotes
  * forks
* pull requests / merge requests

+++

![git-branch](https://git-scm.com/book/en/v2/images/basic-branching-6.png)

---

### Script style development

In Statoil we often see there are scripts written that have no functions

span 200-2000 lines of code.

+++

Some examples recently include
* 764 LOC else block
* 700 LOC function called `run()`
* 500 LOC function called `named_plot` taking 14 parameters
* 400 LOC script, no functions

+++

![orelse](https://raw.githubusercontent.com/pgdr/tdd/master/orelse.png)

+++

* recommend: at most 30 LOC / function
  * but more is ok if it does _one_ thing!
* fewer LOC if complex control flow
* maximum nesting: 4 levels

+++

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

---


#### Why do we program?


---


Primary reason:
* pure pleasure

+++

We program to:

* automate work that is
  * tedious
  * slow
  * error-prone
  * boring?


---


In the process we are solving many smaller problems!

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


#### Reusable programs

When our solution is a 500 lines long script

we ensure no reusability.

+++


* Spend 3 months writing a
* 500 LOC function taking
* more than 10 parameters,

guaranteed not used by anyone else.

+++

Those three months are much more expensive

than if you write code that many others can use.

+++

> given enough eyeballs, all bugs are shallow

More users gives better testing gives better quality

+++

> given enough eyeballs, all bugs are shallow

... is the mantra of open source

---

### Long functions

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

#### Command-Query Separation

* A function should
  * either modify its input _(command)_
  * or return something _(query)_

+++

#### Command-Query Separation

* The best case is _immutable objects_
  * no function modifies data
  * all functions are queries

---

#### The behavior of a function

When a function is not well-defined
* it is difficult to use
* it is difficult to test
* it is difficult to debug
  * because its scope is unclear


---

### Test driven development


+++

#### Code QA

To ensure that code runs correctly, we write _unit test_.

```python
def root_mean_square(lst):
    rms = sum(map(sq, lst))/len(lst)
    return sqrt(rms)

def test_rms():
    assert root_mean_square([4,5,6,7,8]) == 6.1644
```


+++


##### Unit test

Unit tests ensure several things, amongst
* ensure _correctness_
* inform you when behavior changes
* force you into using your own program (dogfood)


---

#### Test Driven Development (TDD)


* When writing code for Statoil
  * we always employ _test driven development_
  * always

+++

##### Tests first

We write the test first!

```python
def test_rms():
    assert root_mean_square([4,5,6,7,8]) == 6.1644
```

There is no such function!  So the tests will fail!


+++


##### Test first -- then implement


```python
def root_mean_square(lst):
    # ...
```

Hopefully written correctly, tests pass!

+++


##### Test first -- then implement

Simultaneously, you got to

* use your own interface before writing it
* making it easy to understand
* reusable
* well defined

+++

EOF
