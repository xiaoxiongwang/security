# Segmentation fault error in imgdataopt

I use the following command:

```shell
./imgdataopt imgdataopt_crash
```

and get the error:

```
Segmentation fault
```

I use **valgrind** to analysis the bug and get the below information:

```
==8373== Memcheck, a memory error detector
==8373== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==8373== Using Valgrind-3.15.0 and LibVEX; rerun with -h for copyright info
==8373== Command: ./imgdataopt imgdataopt_crash
==8373== 
warning: png image data too short

==8373== Invalid write of size 8
==8373==    at 0x4C37BD4: memset (vg_replace_strmem.c:1252)
==8373==    by 0x42FBCE: ??? (in imgdataopt/imgdataopt)
==8373==    by 0x402D58: ??? (in imgdataopt/imgdataopt)
==8373==    by 0x4E60872: (below main) (in /usr/lib64/libc-2.28.so)
==8373==  Address 0x0 is not stack'd, malloc'd or (recently) free'd
==8373== 
==8373== 
==8373== Process terminating with default action of signal 11 (SIGSEGV): dumping core
==8373==  Access not within mapped region at address 0x0
==8373==    at 0x4C37BD4: memset (vg_replace_strmem.c:1252)
==8373==    by 0x42FBCE: ??? (in imgdataopt/imgdataopt)
==8373==    by 0x402D58: ??? (in imgdataopt/imgdataopt)
==8373==    by 0x4E60872: (below main) (in /usr/lib64/libc-2.28.so)
==8373==  If you believe this happened as a result of a stack
==8373==  overflow in your program's main thread (unlikely but
==8373==  possible), you can try to increase the size of the
==8373==  main thread stack using the --main-stacksize= flag.
==8373==  The main thread stack size used in this run was 8388608.
==8373== 
==8373== HEAP SUMMARY:
==8373==     in use at exit: 1,591 bytes in 3 blocks
==8373==   total heap usage: 4 allocs, 1 frees, 5,687 bytes allocated
==8373== 
==8373== LEAK SUMMARY:
==8373==    definitely lost: 0 bytes in 0 blocks
==8373==    indirectly lost: 0 bytes in 0 blocks
==8373==      possibly lost: 0 bytes in 0 blocks
==8373==    still reachable: 1,591 bytes in 3 blocks
==8373==         suppressed: 0 bytes in 0 blocks
==8373== Rerun with --leak-check=full to see details of leaked memory
==8373== 
==8373== For lists of detected and suppressed errors, rerun with: -s
==8373== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
Segmentation fault
```

My system is:

```
Red Hat Enterprise Linux release 8.1 (Ootpa)
Linux GPUH3C 4.18.0-80.11.2.el8_0.x86_64 #1 SMP Sun Sep 15 11:24:21 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```