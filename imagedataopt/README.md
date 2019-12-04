# Segmentation fault error in imgdataopt

Tested in ubuntu 16.04, 64bit.

I use the following command:

```shell
./imgdataopt imgdataopt_crash test.png
```

and get the error:

```
Segmentation fault
```

I use **valgrind** to analysis the bug and get the below information:

```
valgrind imgdataopt imgdataopt_crash test.png
==29579== Memcheck, a memory error detector
==29579== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==29579== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==29579== Command: ./imgdataopt imgdataopt_crash test.png
==29579== 
warning: png image data too short

==29579== Invalid write of size 8
==29579==    at 0x4C3453F: memset (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==29579==    by 0x42FBCE: ??? (in imgdataopt)
==29579==    by 0x402D58: ??? (in imgdataopt)
==29579==    by 0x4E5A82F: (below main) (libc-start.c:291)
==29579==  Address 0x0 is not stack'd, malloc'd or (recently) free'd
==29579== 
==29579== 
==29579== Process terminating with default action of signal 11 (SIGSEGV)
==29579==  Access not within mapped region at address 0x0
==29579==    at 0x4C3453F: memset (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==29579==    by 0x42FBCE: ??? (in imgdataopt)
==29579==    by 0x402D58: ??? (in imgdataopt)
==29579==    by 0x4E5A82F: (below main) (libc-start.c:291)
==29579==  If you believe this happened as a result of a stack
==29579==  overflow in your program's main thread (unlikely but
==29579==  possible), you can try to increase the size of the
==29579==  main thread stack using the --main-stacksize= flag.
==29579==  The main thread stack size used in this run was 8388608.
==29579== 
==29579== HEAP SUMMARY:
==29579==     in use at exit: 1,597 bytes in 3 blocks
==29579==   total heap usage: 4 allocs, 1 frees, 5,693 bytes allocated
==29579== 
==29579== LEAK SUMMARY:
==29579==    definitely lost: 0 bytes in 0 blocks
==29579==    indirectly lost: 0 bytes in 0 blocks
==29579==      possibly lost: 0 bytes in 0 blocks
==29579==    still reachable: 1,597 bytes in 3 blocks
==29579==         suppressed: 0 bytes in 0 blocks
==29579== Rerun with --leak-check=full to see details of leaked memory
==29579== 
==29579== For counts of detected and suppressed errors, rerun with: -v
==29579== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
Segmentation fault (core dumped)
```

I use **AddressSanitizer** to build imgdataopt, this file can cause SEGV signal with the following command:

```
./imgdataopt imgdataopt_crash test.png
```

This is the ASAN information:

```
warning: png image data too short

=================================================================
==29582==ERROR: AddressSanitizer: unknown-crash on address 0x0000ffffffff at pc 0x7fd44c1ddbec bp 0x7ffed7d51c30 sp 0x7ffed7d513d8
WRITE of size 4294967295 at 0x0000ffffffff thread T0
    #0 0x7fd44c1ddbeb in __asan_memset (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x8cbeb)
    #1 0x40c7c4  (imgdataopt/imgdataopt+0x40c7c4)
    #2 0x401bc3  (imgdataopt/imgdataopt+0x401bc3)
    #3 0x7fd44bda782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #4 0x4032d8  (imgdataopt/imgdataopt+0x4032d8)

Address 0x0000ffffffff is located in the shadow gap area.
SUMMARY: AddressSanitizer: unknown-crash ??:0 __asan_memset
==29582==ABORTING
```

