---
layout: post
title: Why List Comprehension is Quicker Than Append
---

When I took my Python class, I remember my professor saying that list comprehension is much quicker than using `append()` and I never could understand the reason as to why even after it was explained to me more times than I wanted to. Now that I'm working towards my degree in Software Engineering, I revisited this question and decided it was time to have a deep understanding of why this was.

For this post, I'm going to be referencing the following code:

```python
import time
start_time = time.time()
myList = []
for i in range(0, 10000000):
    myList.append(i)
print("For loop: %s seconds" % (time.time() - start_time))

start_time = time.time()
myList = [i for i in range(0, 10000000)]
print("List Comprehension: %s seconds" % (time.time() - start_time))
```  
which produces the following results:
>For loop: 1.307466983795166 seconds  
>List Comprehension: 0.5925030708312988 seconds  

#### Long Story Short  
Down to the bit level, list comprehension performs the exact same operation. However, the for loop has to load and look up the append function, which is a step that's taken out of list comprehension. 

If you're curious as to what the associated C code is [Python Source Code](https://github.com/python/cpython):

```c
app1(PyListObject *self, PyObject *v)
{
    Py_ssize_t n = PyList_GET_SIZE(self);

    assert (v != NULL);
    if (n == PY_SSIZE_T_MAX) {
        PyErr_SetString(PyExc_OverflowError,
            "cannot add more objects to list");
        return -1;
    }

    if (list_resize(self, n+1) < 0)
        return -1;

    Py_INCREF(v);
    PyList_SET_ITEM(self, n, v);
    return 0;
}

int
PyList_Append(PyObject *op, PyObject *newitem)
{
    if (PyList_Check(op) && (newitem != NULL))
        return app1((PyListObject *)op, newitem);
    PyErr_BadInternalCall();
    return -1;
}
```


### The Details - Breif Bytecode Analysis

To simplify the process of the bytecode analysis, I modified the above code as such:

```python
def f1():
    myList = []
    for i in range(0, 10000000):
        myList.append(i)

def f2():
    [i for i in range(0, 10000000)]
```

I "disassembled" the code so I can see what's going on at the bytecode level (that is, the instructions). By looking at the length of what was given, it can be seen right away that there are less instructions that need to be executed for the list comprehensions. Less instructions = quicker code.

Going off of that, suppose that there is one less instruction when a program runs. It will help the speed, but it won't be noticeable. However, when it's scaled to 10 million or so, that's 10 million or so extra instructions that needs to be run. Likewise, if there's 6 less instructions being executed 10 million times, then that's 60 million instructions the computer has to execute. Yay, optimization!

The full dissassembly is shown here:

```python
import dis

dis.dis(f1)
 10           0 BUILD_LIST               0
              2 STORE_FAST               0 (myList)

 11           4 SETUP_LOOP              28 (to 34)
              6 LOAD_GLOBAL              0 (range)
              8 LOAD_CONST               1 (0)
             10 LOAD_CONST               2 (10000000)
             12 CALL_FUNCTION            2
             14 GET_ITER
        >>   16 FOR_ITER                14 (to 32)
             18 STORE_FAST               1 (i)

 12          20 LOAD_FAST                0 (myList)
             22 LOAD_METHOD              1 (append)
             24 LOAD_FAST                1 (i)
             26 CALL_METHOD              1
             28 POP_TOP
             30 JUMP_ABSOLUTE           16
        >>   32 POP_BLOCK
        >>   34 LOAD_CONST               0 (None)
             36 RETURN_VALUE

dis.dis(f2)
 15           0 LOAD_CONST               1 (<code object <listcomp> at 0x114926b70, file "/Users/Brandon/Desktop/untitled1.py", line 15>)
              2 LOAD_CONST               2 ('f2.<locals>.<listcomp>')
              4 MAKE_FUNCTION            0
              6 LOAD_GLOBAL              0 (range)
              8 LOAD_CONST               3 (0)
             10 LOAD_CONST               4 (10000000)
             12 CALL_FUNCTION            2
             14 GET_ITER
             16 CALL_FUNCTION            1
             18 POP_TOP
             20 LOAD_CONST               0 (None)
             22 RETURN_VALUE

Disassembly of <code object <listcomp> at 0x114926b70, file "/Users/Brandon/Desktop/untitled1.py", line 15>:
 15           0 BUILD_LIST               0
              2 LOAD_FAST                0 (.0)
        >>    4 FOR_ITER                 8 (to 14)
              6 STORE_FAST               1 (i)
              8 LOAD_FAST                1 (i)
             10 LIST_APPEND              2
             12 JUMP_ABSOLUTE            4
        >>   14 RETURN_VALUE
```
