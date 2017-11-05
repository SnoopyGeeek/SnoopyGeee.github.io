---
layout: post
title: Installation of pynauty on a server
date: 2017-11-05 16:36:24.000000000 +09:00
---

#### Introduction

[Pynauty](https://web.cs.dal.ca/~peter/software/pynauty/html/index.html) is a Python/C extension module. It can be used to compare graphs for isomorphism and to determine their automorphism group. It is based on the [Nauty](http://www3.cs.stonybrook.edu/~algorith/implement/nauty/implement.shtml) C procedures.

#### Issues I met
1. I have a server access without root authority. I want to deploy the package on the server but ./configure doesn't work.

#### Deploying procedures
1. Download [pynauty source file](https://github.com/katsiatyna/pynauty_0.6.0-modification) and put them into one dir.
2. Download [nauty source file](http://users.cecs.anu.edu.au/~bdm/nauty/), unzip them, put them into a folder called *nauty* under pynauty dir.
3. cd into the *nauty* dir, input
```bash
$ source activate
$ bash CFLAGS=-fPIC ./configure
$ make
```
4. cd into *pynauty* dir, input:
`ln -s ../nauty* nauty`
5. Modify the extra_objects argument in setup.py:

```python
# from
extra_objects = [ nauty_dir + '/' + 'nauty.so', ],
                      nauty_dir + '/' + 'nautil.o',
                      nauty_dir + '/' + 'naugraph.o'
                    ],
# change it to
extra_objects = [nauty_dir + '/' + 'nauty.a'],
```
You can find extra_objects by searching for key words and change it like the form.

6. `python setup.py build`
7. `python setup.py install`


#### Conclusion

Here it goes, I can import pynauty package in my environment. Before that, I have installed anaconda on my end.
