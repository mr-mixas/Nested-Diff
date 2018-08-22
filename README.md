# NAME

Nested-Diff - Yet another format for nested structures diff

# VERSION

0.1

# DIFF FORMAT

Diff is a hashmap and may contain following keys:

* **A** stands for 'added', it's value - added item.  
* **D** means 'different' and contains subdiff.  
* **I** index for array item, used only when prior item was omitted.  
* **N** is a new value for changed item.  
* **O** is a changed item's old value.  
* **R** key used for removed item.  
* **U** represent unchanged item.  

Diff metadata alternates with data and may represent any structure of any data
types. Simple types specified as is, arrays and hashmaps contain subdiffs for
their items with native for such types addressing: indexes for arrays and keys
for hashes. Every status type, except **D**, may be (optionally) omitted
during the diff calculation.

Next example uses JSON for simplicity (format itself is language and marshalling
format agnostic).

    old:  {"one": [5,7]}
    new:  {"one": [5], "two": 2}
    opts: {"U": 0} # omit unchanged items

    diff:
    {"D": {"one": {"D": [{"I": 1, "R": 7}]}, "two": {"A": 2}}}
    | |   |  |    | |   || |   |   |   |       |    | |   |
    | |   |  |    | |   || |   |   |   |       |    | |   +- with value 2
    | |   |  |    | |   || |   |   |   |       |    | +- key 'two' was added
    | |   |  |    | |   || |   |   |   |       |    +- subdiff for it
    | |   |  |    | |   || |   |   |   |       +- another key from top-level
    | |   |  |    | |   || |   |   |   +- what it was (item's value: 7)
    | |   |  |    | |   || |   |   +- what happened to item (removed)
    | |   |  |    | |   || |   +- array item's actual index
    | |   |  |    | |   || +- prior item was omitted
    | |   |  |    | |   |+- subdiff for array item
    | |   |  |    | |   +- it's value - array
    | |   |  |    | +- it is deeply changed
    | |   |  |    +- subdiff for key 'one'
    | |   |  +- it has key 'one'
    | |   +- top-level thing is a hashmap
    | +- changes somewhere deeply inside
    +- diff is always a HASH

# IMPLEMENTATIONS

* **Perl:** [Struct::Diff](https://metacpan.org/pod/Struct::Diff)

As command-line tool: [nddiff](https://metacpan.org/pod/nddiff)

# TESTS

[JSON common](https://github.com/mr-mixas/Nested-Diff/tree/master/tests/json)

# BUGS

Please report any bugs or feature requests to
[Tracker](https://github.com/mr-mixas/Nested-Diff/issues)

# AUTHOR

Michael Samoglyadov, `<mixas at cpan.org>`

# SEE ALSO

[JSON Patch (rfc6902)](https://tools.ietf.org/html/rfc6902)  
[JSON Merge Patch (rfc7396)](https://tools.ietf.org/html/rfc7396)  
[DeepDiff](https://deepdiff.readthedocs.io/en/latest/)  

# LICENSE AND COPYRIGHT

 MIT License

 Copyright (c) 2018 Michael Samoglyadov

 Permission is hereby granted, free of charge, to any person obtaining a copy
 of this software and associated documentation files (the "Software"), to deal
 in the Software without restriction, including without limitation the rights
 to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 copies of the Software, and to permit persons to whom the Software is
 furnished to do so, subject to the following conditions:

 The above copyright notice and this permission notice shall be included in all
 copies or substantial portions of the Software.

 THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
 SOFTWARE.
