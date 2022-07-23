
you should not do javascript math on non-integers. If you want to perform math on decimal numbers, you should first convert all values to an integer, do the math, then convert back
- reason because javascript does math in binary, there are rounding errors when doing math.
