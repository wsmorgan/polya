POLYA
======

Numerical Implementation of the Polya Enumeration Theorem
------

This repository has code in both Python and Fortran for counting the number of unique colorings of a finite set under the action of a finite group. An example of the theorem and its application are discussed in the [paper](https://github.com/rosenbrockc/polya/blob/master/docs/polyaenum.pdf), as well as a description of the algorithm being used. Details needed to reproduce the data in the paper's figures is included in [FIGURES.md](FIGURES.md).

Quickstart
------

To get started quickly, clone the git repo: `git clone https://github.com/rosenbrockc/polya.git`. This will create a folder called `polya` in the current directory. Next, `cd polya/python` and execute the script `polya.py`. The script is documented internally with examples and a description of the parameters (by running `./polya.py --help`). We proceed with a short example using the symmetries of the square. It is also the example discussed in the [paper](https://github.com/rosenbrockc/polya/blob/master/docs/polyaenum.pdf).

The generators of the dihedral group of degree four are written in cycle form as:

```
4 3 2 1
2 3 4 1
```

A file containing these generators is included in `fortran/tests/generators.in.paper`. If we want to know how many ways exist to color the corners of the square with 2 colors and two corners of each color, then the positional argument is `2 2` (just list the number of corners that should have each type of color; the number of entries is the number of colors to use). We can then calculate the number of unique colorings using:

```
./polya.py 2 2 -generators generators.in.paper
```

Unit Tests
------

The Fortran implementation is 100% unit tested. All the input and output files for the tests are contained in `polya/fortran/tests`. To automatically compile and run the unit tests, use the [fortpy](https://github.com/rosenbrockc/fortpy) package. We recommend creating a new virtual environment for `fortpy`.

```bash
pip install fortpy
mkdir staging
cd polya
runtests.py fortran/ -staging /path/to/staging
```

`fortpy` will create a directory for each method in the module that has unit tests specified, compile an executable and then run the executable for each of the tests cases specified in [`polya/fortran/classes.xml`](https://github.com/rosenbrockc/polya/blob/master/fortran/classes.xml). If the tests all show 100%, then the code is compiling well on your system. Otherwise, `fortpy` will help debug the compilation errors.

To use the Polya solver in your own code, you can either use the Python version discussed above under "Quickstart" or `cd staging/classes.polya` and look at the driver file `standard.f90` that was auto-generated by `fortpy`. The input files that the driver uses are saved in `staging/classes.polya/tests/standard.*` where `*` represents the test case identifiers listed in the `classes.xml` file. By examining the driver file, you should be able to easily implement it in your own code.

Source Code
------

All the source code is available in the `python` or `fortran` directories. If you use emacs, you can `package-install<RET>fortpy<RET>` to get real-time code development support for the `f90` files in this repo. They have all been documented fully using the XML-based documentation standard of `fortpy`.