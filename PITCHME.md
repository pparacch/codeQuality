## Software development

---

Agenda:
* We will look at a script and see some drawbacks of the "script style"
* We try to explain the benefits of many small functions rather than few big functions
* We will see how to
  * use git (some experience is assumed)
  * write tests
  * setup automatic testing
  * make a setup script that can be used to test and install the project


---


## Script style development

In Statoil we often see there are scripts written that have no functions and
span 200-2000 lines of code.

Some examples recently include
* 700 LOC function called `run()`
* 500 LOC function called `named_plot` taking 14 parameters
* 400 LOC script, no functions


---


### Why do we program?


---


Primary reason:
*  pure pleasure
*  automate work that is
  * tedious
  * slow
  * error-prone
  * boring?


---


In the process we are solving many smaller problems!

These smaller problems, the atomic building blocks of the final solution, are
often seen in other problems.


---


### Reusable programs

When our solution is a 500 lines long script

we ensure no reusability.


If you spend three months writing a function on 500 lines that takes more than
10 parameters, guaranteed not used by anyone else.

That means those three months are much more expensive than if you write code
that many others can use.


---

## Long functions

Long functions are not "well-defined".

What does `run` do in those 700 LOC?

If you cannot explain in a couple sentences exactly

* input
* output
* assumptions
* side-effects

it is not reusable and should therefore be split.


---

### The behavior of a function

When a function is not well-defined
* it is difficult to use
* it is difficult to test
* it is difficult to debug, because its scope is unclear


---

## Test driven development


---

### Code QA

To ensure that code runs correctly, we write _unit test_.

```python
def root_mean_square(lst):
    rms = sum(map(sq, lst))/len(lst)
    return sqrt(rms)

def test_rms():
    assert root_mean_square([4,5,6,7,8]) == 6.1644
```


---

### Unit test

Unit tests ensure several things, amongst
* they ensure _correctness_
* they inform you when your program's behavior changed
* dogfooding -- they force you to taste your own food



---

### TDD -- Test Driven Development

When writing code for Statoil, we employ _test driven development_.

---

#### Tests first

We write the test first!

```python
def test_rms():
    assert root_mean_square([4,5,6,7,8]) == 6.1644
```

There is no such function!  So the tests will fail!


---

#### Test first -- then implement


```python
def root_mean_square(lst):
    # ...
```
