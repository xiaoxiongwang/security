# Memory error in wavtool-pl

Tested in Ubuntu 16.04, 64bit

I use the following command:

```shell
wavtool-pl out wavtool-pl_aborted 1 1 1 1 1 1 1 1
```

and get an aborted error:

```
*** Error in `wavtool/install/bin/wavtool-pl': corrupted size vs. prev_size: 0x0000000001869c40 ***
======= Backtrace: =========
/lib/x86_64-linux-gnu/libc.so.6(+0x777e5)[0x7fae87d027e5]
/lib/x86_64-linux-gnu/libc.so.6(+0x80dfb)[0x7fae87d0bdfb]
/lib/x86_64-linux-gnu/libc.so.6(cfree+0x4c)[0x7fae87d0f53c]
/usr/lib/x86_64-linux-gnu/libsndfile.so.1(+0x639b)[0x7fae8805b39b]
/usr/lib/x86_64-linux-gnu/libsndfile.so.1(+0xa93d)[0x7fae8805f93d]
wavtool/install/bin/wavtool-pl[0x406d1a]
wavtool/install/bin/wavtool-pl[0x40118d]
/lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xf0)[0x7fae87cab830]
wavtool/install/bin/wavtool-pl[0x4012e9]
======= Memory map: ========
00400000-0040a000 r-xp 00000000 103:01 20220818                      wavtool/install/bin/wavtool-pl
00609000-0060a000 r--p 00009000 103:01 20220818                          wavtool/install/bin/wavtool-pl
0060a000-0060b000 rw-p 0000a000 103:01 20220818                          wavtool/install/bin/wavtool-pl
0185a000-0187b000 rw-p 00000000 00:00 0                                  [heap]
7fae80000000-7fae80021000 rw-p 00000000 00:00 0 
7fae80021000-7fae84000000 ---p 00000000 00:00 0 
7fae86e1a000-7fae86e30000 r-xp 00000000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7fae86e30000-7fae8702f000 ---p 00016000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7fae8702f000-7fae87030000 rw-p 00015000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7fae87030000-7fae8705a000 r-xp 00000000 103:01 14820378                  /usr/lib/x86_64-linux-gnu/libvorbis.so.0.4.8
7fae8705a000-7fae87259000 ---p 0002a000 103:01 14820378                  /usr/lib/x86_64-linux-gnu/libvorbis.so.0.4.8
7fae87259000-7fae8725a000 r--p 00029000 103:01 14820378                  /usr/lib/x86_64-linux-gnu/libvorbis.so.0.4.8
7fae8725a000-7fae8725b000 rw-p 0002a000 103:01 14820378                  /usr/lib/x86_64-linux-gnu/libvorbis.so.0.4.8
7fae8725b000-7fae87262000 r-xp 00000000 103:01 14820014                  /usr/lib/x86_64-linux-gnu/libogg.so.0.8.2
7fae87262000-7fae87462000 ---p 00007000 103:01 14820014                  /usr/lib/x86_64-linux-gnu/libogg.so.0.8.2
7fae87462000-7fae87463000 r--p 00007000 103:01 14820014                  /usr/lib/x86_64-linux-gnu/libogg.so.0.8.2
7fae87463000-7fae87464000 rw-p 00008000 103:01 14820014                  /usr/lib/x86_64-linux-gnu/libogg.so.0.8.2
7fae87464000-7fae8756c000 r-xp 00000000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7fae8756c000-7fae8776b000 ---p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7fae8776b000-7fae8776c000 r--p 00107000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7fae8776c000-7fae8776d000 rw-p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7fae8776d000-7fae877fa000 r-xp 00000000 103:01 14820380                  /usr/lib/x86_64-linux-gnu/libvorbisenc.so.2.0.11
7fae877fa000-7fae879f9000 ---p 0008d000 103:01 14820380                  /usr/lib/x86_64-linux-gnu/libvorbisenc.so.2.0.11
7fae879f9000-7fae87a15000 r--p 0008c000 103:01 14820380                  /usr/lib/x86_64-linux-gnu/libvorbisenc.so.2.0.11
7fae87a15000-7fae87a16000 rw-p 000a8000 103:01 14820380                  /usr/lib/x86_64-linux-gnu/libvorbisenc.so.2.0.11
7fae87a16000-7fae87a89000 r-xp 00000000 103:01 14818931                  /usr/lib/x86_64-linux-gnu/libFLAC.so.8.3.0
7fae87a89000-7fae87c89000 ---p 00073000 103:01 14818931                  /usr/lib/x86_64-linux-gnu/libFLAC.so.8.3.0
7fae87c89000-7fae87c8a000 r--p 00073000 103:01 14818931                  /usr/lib/x86_64-linux-gnu/libFLAC.so.8.3.0
7fae87c8a000-7fae87c8b000 rw-p 00074000 103:01 14818931                  /usr/lib/x86_64-linux-gnu/libFLAC.so.8.3.0
7fae87c8b000-7fae87e4b000 r-xp 00000000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7fae87e4b000-7fae8804b000 ---p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7fae8804b000-7fae8804f000 r--p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7fae8804f000-7fae88051000 rw-p 001c4000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7fae88051000-7fae88055000 rw-p 00000000 00:00 0 
7fae88055000-7fae880b8000 r-xp 00000000 103:01 14811518                  /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25
7fae880b8000-7fae882b8000 ---p 00063000 103:01 14811518                  /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25
7fae882b8000-7fae882ba000 r--p 00063000 103:01 14811518                  /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25
7fae882ba000-7fae882bb000 rw-p 00065000 103:01 14811518                  /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25
7fae882bb000-7fae882bf000 rw-p 00000000 00:00 0 
7fae882bf000-7fae882e5000 r-xp 00000000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7fae884b3000-7fae884b9000 rw-p 00000000 00:00 0 
7fae884e3000-7fae884e4000 rw-p 00000000 00:00 0 
7fae884e4000-7fae884e5000 r--p 00025000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7fae884e5000-7fae884e6000 rw-p 00026000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7fae884e6000-7fae884e7000 rw-p 00000000 00:00 0 
7ffc266f5000-7ffc26717000 rw-p 00000000 00:00 0                          [stack]
7ffc26746000-7ffc26749000 r--p 00000000 00:00 0                          [vvar]
7ffc26749000-7ffc2674b000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
Aborted
```

I use **valgrind** to analysis the bug and get the below information:

```
==27927== Memcheck, a memory error detector
==27927== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==27927== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==27927== Command: ../../Fuzz/target_progs/target_wavtool/install/bin/wavtool-pl test id:000000,sig:06,src:000018,op:arith32,pos:20,val:-3
==27927== 
==27927== Invalid write of size 4
==27927==    at 0x4E593AB: ??? (in /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25)
==27927==    by 0x4E5B11F: ??? (in /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25)
==27927==    by 0x4E45423: ??? (in /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25)
==27927==    by 0x406D19: wfd_append (wfd.c:151)
==27927==    by 0x40118C: main (wavtool-pl.c:209)
==27927==  Address 0x60d9588 is 0 bytes after a block of size 24 alloc'd
==27927==    at 0x4C2FB55: calloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==27927==    by 0x4E59339: ??? (in /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25)
==27927==    by 0x4E5B11F: ??? (in /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25)
==27927==    by 0x4E45423: ??? (in /usr/lib/x86_64-linux-gnu/libsndfile.so.1.0.25)
==27927==    by 0x406D19: wfd_append (wfd.c:151)
==27927==    by 0x40118C: main (wavtool-pl.c:209)
==27927== 
==27927== 
==27927== HEAP SUMMARY:
==27927==     in use at exit: 76 bytes in 5 blocks
==27927==   total heap usage: 15 allocs, 10 frees, 70,760 bytes allocated
==27927== 
==27927== LEAK SUMMARY:
==27927==    definitely lost: 18 bytes in 3 blocks
==27927==    indirectly lost: 0 bytes in 0 blocks
==27927==      possibly lost: 0 bytes in 0 blocks
==27927==    still reachable: 58 bytes in 2 blocks
==27927==         suppressed: 0 bytes in 0 blocks
==27927== Rerun with --leak-check=full to see details of leaked memory
==27927== 
==27927== For counts of detected and suppressed errors, rerun with: -v
==27927== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
```
