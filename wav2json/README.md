# Floating point exception in function compute_waveform in wav2json.cpp

Tested in Ubuntu 16.04, 64bit

I use the following command:

```shell
./wav2json wav2json_floating_exception
```

and get:

```
Floating point exception
```

I use **valgrind** to analysis the bug and get the below information:

```
==6077== Memcheck, a memory error detector
==6077== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==6077== Using Valgrind-3.13.0 and LibVEX; rerun with -h for copyright info
==6077== Command: wav2json wav2json_floating_exception
==6077== 
==6077== 
==6077== Process terminating with default action of signal 8 (SIGFPE)
==6077==  Integer divide by zero at address 0x10030E3FED
==6077==    at 0x14EA27: compute_waveform(SndfileHandle const&, std::ostream&, unsigned long, std::vector<Options::Channel, std::allocator<Options::Channel> >, bool, float, float, bool (*)(unsigned long)) (wav2json.cpp:137)
==6077==    by 0x1145BD: main (main.cpp:74)
==6077== 
==6077== HEAP SUMMARY:
==6077==     in use at exit: 24,588 bytes in 13 blocks
==6077==   total heap usage: 246 allocs, 233 frees, 112,173 bytes allocated
==6077== 
==6077== LEAK SUMMARY:
==6077==    definitely lost: 0 bytes in 0 blocks
==6077==    indirectly lost: 0 bytes in 0 blocks
==6077==      possibly lost: 0 bytes in 0 blocks
==6077==    still reachable: 24,588 bytes in 13 blocks
==6077==         suppressed: 0 bytes in 0 blocks
==6077== Rerun with --leak-check=full to see details of leaked memory
==6077== 
==6077== For counts of detected and suppressed errors, rerun with: -v
==6077== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
Floating point exception
```

As we can see, the error occurred in line 139 of the program wav2json.cpp. This error can be avoided if we check the variable samples is not 0.