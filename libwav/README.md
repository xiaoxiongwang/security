# SEGV SEGV in function wav_content_read in libwav.c

Tested in Ubuntu 14.04, 32bit

I use the following command:

```shell
./wav_gain wav_gain_crash_SEGV_wav_content_read.wav test.wav
```

and get:

```
Segmentation fault (core dumped)
```

I use **valgrind** to analysis the bug and get the below information:

```
==6300== Memcheck, a memory error detector
==6300== Copyright (C) 2002-2013, and GNU GPL'd, by Julian Seward et al.
==6300== Using Valgrind-3.10.1 and LibVEX; rerun with -h for copyright info
==6300== Command: /vagrant/target_progs/target_libwav/libwav/tools/wav_gain/wav_gain /vagrant/crash/libwav/wav_gain/cmp/fuzz_out_wav_gain_cmp/crashes/id:000003,sig:11,src:000010,op:havoc,rep:8,cmp,wav_gain test
==6300== 
==6300== Argument 'size' of function malloc has a fishy (possibly negative) value: -218104064
==6300==    at 0x402917C: malloc (in /usr/lib/valgrind/vgpreload_memcheck-x86-linux.so)
==6300==    by 0x804A317: wav_chunk_read (libwav.c:165)
==6300==    by 0x804A317: wav_read (libwav.c:254)
==6300==    by 0x8048861: gain_file (wav_gain.c:20)
==6300==    by 0x8048861: main (wav_gain.c:43)
==6300== 
==6300== Invalid write of size 4
==6300==    at 0x40B8C3C: __GI_mempcpy (mempcpy.S:54)
==6300==    by 0x40AB017: _IO_file_xsgetn (fileops.c:1396)
==6300==    by 0x40ACE07: _IO_sgetn (genops.c:495)
==6300==    by 0x40A0418: fread (iofread.c:42)
==6300==    by 0x804A33A: fread (stdio2.h:295)
==6300==    by 0x804A33A: wav_content_read (libwav.c:11)
==6300==    by 0x804A33A: wav_chunk_read (libwav.c:166)
==6300==    by 0x804A33A: wav_read (libwav.c:254)
==6300==    by 0x8048861: gain_file (wav_gain.c:20)
==6300==    by 0x8048861: main (wav_gain.c:43)
==6300==  Address 0x0 is not stack'd, malloc'd or (recently) free'd
==6300== 
==6300== 
==6300== Process terminating with default action of signal 11 (SIGSEGV)
==6300==  Access not within mapped region at address 0x0
==6300==    at 0x40B8C3C: __GI_mempcpy (mempcpy.S:54)
==6300==    by 0x40AB017: _IO_file_xsgetn (fileops.c:1396)
==6300==    by 0x40ACE07: _IO_sgetn (genops.c:495)
==6300==    by 0x40A0418: fread (iofread.c:42)
==6300==    by 0x804A33A: fread (stdio2.h:295)
==6300==    by 0x804A33A: wav_content_read (libwav.c:11)
==6300==    by 0x804A33A: wav_chunk_read (libwav.c:166)
==6300==    by 0x804A33A: wav_read (libwav.c:254)
==6300==    by 0x8048861: gain_file (wav_gain.c:20)
==6300==    by 0x8048861: main (wav_gain.c:43)
==6300==  If you believe this happened as a result of a stack
==6300==  overflow in your program's main thread (unlikely but
==6300==  possible), you can try to increase the size of the
==6300==  main thread stack using the --main-stacksize= flag.
==6300==  The main thread stack size used in this run was 8388608.
LibWAV v. 0.0.1 A (c) 2016 - 2017 Marc Volker Dickmann

==6300== 
==6300== HEAP SUMMARY:
==6300==     in use at exit: 352 bytes in 1 blocks
==6300==   total heap usage: 1 allocs, 0 frees, 352 bytes allocated
==6300== 
==6300== LEAK SUMMARY:
==6300==    definitely lost: 0 bytes in 0 blocks
==6300==    indirectly lost: 0 bytes in 0 blocks
==6300==      possibly lost: 0 bytes in 0 blocks
==6300==    still reachable: 352 bytes in 1 blocks
==6300==         suppressed: 0 bytes in 0 blocks
==6300== Rerun with --leak-check=full to see details of leaked memory
==6300== 
==6300== For counts of detected and suppressed errors, rerun with: -v
==6300== ERROR SUMMARY: 2 errors from 2 contexts (suppressed: 0 from 0)
Segmentation fault (core dumped)
```

I use **gcc 4.8** and **AddressSanitizer** to build libwav, this file can cause SEGV signal in function wav\_content\_read when running the wav_gain in folder tools/wav_gain with the following command:

```shell
./wav_gain wav_gain_crash_SEGV_wav_content_read.wav test.wav
```

This is the ASAN information:

```
==6303== WARNING: AddressSanitizer failed to allocate 0xf2ffff00 bytes
ASAN:SIGSEGV
=================================================================
==6303== ERROR: AddressSanitizer: SEGV on unknown address 0x00000000 (pc 0xb5fe7c3c sp 0xbfe12d5c bp 0xb5e03e00 T0)
AddressSanitizer can not provide additional info.
    #0 0xb5fe7c3b (/lib/i386-linux-gnu/libc-2.19.so+0x7cc3b)
    #1 0xb5fda017 (/lib/i386-linux-gnu/libc-2.19.so+0x6f017)
    #2 0xb5fdbe07 (/lib/i386-linux-gnu/libc-2.19.so+0x70e07)
    #3 0xb5fcf418 (/lib/i386-linux-gnu/libc-2.19.so+0x64418)
    #4 0x80493cd (/vagrant/target_progs/target_libwav1/libwav1/tools/wav_gain/wav_gain+0x80493cd)
    #5 0x8049ac2 (/vagrant/target_progs/target_libwav1/libwav1/tools/wav_gain/wav_gain+0x8049ac2)
    #6 0x8048982 (/vagrant/target_progs/target_libwav1/libwav1/tools/wav_gain/wav_gain+0x8048982)
    #7 0xb5f84af2 (/lib/i386-linux-gnu/libc-2.19.so+0x19af2)
    #8 0x8048b90 (/vagrant/target_progs/target_libwav1/libwav1/tools/wav_gain/wav_gain+0x8048b90)
==6303== ABORTING
```

