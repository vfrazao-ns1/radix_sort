# Radix Sort

Provides an implementation in C and x86_64 assembly of radix sort for arrays of uint64. The radix sort algorithm implemented here uses a fixed radix of 256.

## Performance
### No Optimization flags passed to GCC
Some benchmarking shows the asm implementation to be substantially faster than the C implementation which is surprising to me. It also shows the C implementation to be about as quick as quicksort from the C standard library which also surprised me. Radix sort should be faster than the STL quicksort for large arrays of random numbers as it has several advantages:

* It should be roughly linear complexity
* It does not make an additional call to a comparison function (unlike C STL quicksort)

Results (no optimization flags passed):

```
$ ./sort_bench $((2**20)) 10
1048576 elements in the array
   C Radix  |  Quicksort  |  ASM Radix  |
     129 ms |      118 ms |       53 ms |
     130 ms |      118 ms |       54 ms |
     130 ms |      117 ms |       51 ms |
     128 ms |      118 ms |       51 ms |
     129 ms |      119 ms |       53 ms |
     129 ms |      118 ms |       52 ms |
     130 ms |      118 ms |       54 ms |
     129 ms |      118 ms |       51 ms |
     129 ms |      117 ms |       55 ms |
     129 ms |      118 ms |       53 ms |
```

### O2 & O3 Optimization Level

At O2/3 optimization the playing field between the C and ASM implementations is leveled (unclear which one has an edge).

O2:

```
$ ./sort_bench $((2**20)) 10
1048576 elements in the array
   C Radix  |  Quicksort  |  ASM Radix  |
      51 ms |      101 ms |       54 ms |
      51 ms |      100 ms |       52 ms |
      51 ms |      101 ms |       48 ms |
      51 ms |      101 ms |       49 ms |
      51 ms |      101 ms |       47 ms |
      51 ms |      101 ms |       49 ms |
      52 ms |      100 ms |       48 ms |
      50 ms |      100 ms |       52 ms |
      51 ms |      101 ms |       50 ms |
      52 ms |      101 ms |       48 ms |
```

O3:

```
$ ./sort_bench $((2**20)) 10
1048576 elements in the array
   C Radix  |  Quicksort  |  ASM Radix  |
      51 ms |       99 ms |       54 ms |
      49 ms |       99 ms |       49 ms |
      50 ms |       99 ms |       55 ms |
      50 ms |      100 ms |       50 ms |
      51 ms |       99 ms |       54 ms |
      48 ms |       99 ms |       55 ms |
      51 ms |      100 ms |       53 ms |
      51 ms |       99 ms |       51 ms |
      50 ms |      100 ms |       53 ms |
      50 ms |      101 ms |       53 ms |
```

It's also interesting to me how much better my implementation of C radix sort got with more optimization levels while quicksort had more modest improvements. I havent looked at the resulting assembly to understand why.
