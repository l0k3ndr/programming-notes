# 02-jan-2021

### 4 - minimal virtualenv

without any tooling we can make it like follows

https://meribold.org/python/2018/02/13/virtual-environments-9487/

```bash
➜   touch pyvenv.cfg
➜   echo "home = /usr/bin" >> pyvenv.cfg
➜   mkdir -p lib/python3.8/site-packages
➜   mkdir bin
➜   cp <your python3.8 exec> bin/python3
➜   pip3 install -t lib/python3.8/site-packages left-pad==0.0.3
Collecting left-pad==0.0.3
  Downloading https://files.pythonhosted.org/packages/a0/34/cd668981dc6818d8a39f1185af8113268ddc71d99b0ba4aa8ceee2a123e7/left-pad-0.0.3.tar.gz
Installing collected packages: left-pad
  Running setup.py install for left-pad ... done
Successfully installed left-pad-0.0.3
➜   tree
.
├── bin
│   └── python3
├── lib
│   └── python3.8
│       └── site-packages
│           ├── __pycache__
│           │   └── left_pad.cpython-38.pyc
│           ├── left_pad-0.0.3-py3.8.egg-info
│           │   ├── PKG-INFO
│           │   ├── SOURCES.txt
│           │   ├── dependency_links.txt
│           │   ├── installed-files.txt
│           │   └── top_level.txt
│           └── left_pad.py
└── pyvenv.cfg

6 directories, 9 files

➜   ./bin/python3 -c "import left_pad"

➜   python3 -c "import left_pad"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'left_pad'

```


### 3 - lexiographic fixed length strings generation

https://sahandsaba.com/combinatorial-generation-for-coding-interviews-in-python.html

```python
def strings_2(A, n):

    index_of = {x: i for i, x in enumerate(A)}
    s = [A[0]] * n

    while True:
        yield "".join(s)

        for i in range(1, n + 1):
            if s[-i] == A[-1]:
                s[-i] = A[0]
            else:
                s[-i] = A[index_of[s[-i]] + 1]
                break
        else:
            break


Y = strings_2("abc", 2)

for y in Y:
    print(y)
```

### 2 - inplace permutation iterator

https://sahandsaba.com/combinatorial-generation-for-coding-interviews-in-python.html

```python
def permutations(pi, i):

    if i == len(pi):
        while True:
            yield False

    sub_permutations = permutations(pi, i + 1)

    while True:

        for j in range(i, len(pi) - 1):
            pi[j], pi[j + 1] = pi[j + 1], pi[j]
            yield True

        last = pi[-1]

        for j in range(len(pi) - 1, i, -1):
            pi[j] = pi[j - 1]
        pi[i] = last

        yield next(sub_permutations)


pi = [1, 2, 3]


perm_generator = permutations(pi, 0)


print(pi)
while next(perm_generator):
    print(pi)
print("------")

print(pi)
while next(perm_generator):
    print(pi)
print("------")
```

### 1 - counting balanced paranthesis

https://sahandsaba.com/interview-question-generating-all-balanced-parentheses.html

```python
def count_balanced(n):
    table = [1]
    for j in range(1, n + 1):
        result = 0
        for i in range(j):
            x = table[i]
            y = table[j - i - 1]
            result += x * y
        table.append(result)
    return table[n]
```

cooler iterative algo, it relates to how we generate next number in our number systems
```python
def get_balanced_iterative(n):

    s = ["("] * n + [")"] * n

    while True:
        yield "".join(s)

        o, c = 0, 0

        for i in range(1, 2 * n):
        
            if i == 2*n:
                return #this is to end probaby
        
            if s[-i] == "(":
                o += 1
                if c > o:
                    s[-i:] = [")"] + ["("] * o + [")"] * (c - 1)
                    break
            else:
                c += 1


G = get_balanced_iterative(5)

for i in range(1, 10):
    print(next(G))
```

counting operations per string
```python
def balanced_iterative_operations(n):

    T, C = 0, 0

    s = ["("] * n + [")"] * n

    while True:

        C += 1
        o, c = 0, 0

        for i in range(1, 2 * n + 1):

            if s[-i] == "(":
                o += 1
                if c > o:
                    T += i
                    s[-i:] = [")"] + ["("] * o + [")"] * (c - 1)
                    break
            else:
                c += 1
        if o == n:
            T += 2 * n
            break
    return T, C


for n in range(1, 20):
    T, C = balanced_iterative_operations(n)
    print(n, T, C, T / C)
```




