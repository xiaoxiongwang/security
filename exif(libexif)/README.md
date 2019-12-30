# Double free error detected in function jpeg_data_save_data of jpeg-data.c when running exif with parameter -c

Tested in Ubuntu 16.04, 64bit.

I use the following command:

```shell
./exif -c exif_double_free.jpg
```

and get:

```
*** Error in `./exif': double free or corruption (!prev): 0x0000000000947a40 ***
======= Backtrace: =========
/lib/x86_64-linux-gnu/libc.so.6(+0x777e5)[0x7f1a27d1d7e5]
/lib/x86_64-linux-gnu/libc.so.6(+0x8037a)[0x7f1a27d2637a]
/lib/x86_64-linux-gnu/libc.so.6(cfree+0x4c)[0x7f1a27d2a53c]
./exif[0x40faf4]
./exif[0x410041]
./exif[0x40660a]
./exif[0x403d4f]
/lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xf0)[0x7f1a27cc6830]
./exif[0x404f79]
======= Memory map: ========
00400000-00417000 r-xp 00000000 103:01 21241626                          ./exif
00616000-00617000 r--p 00016000 103:01 21241626                          ./exif
00617000-00618000 rw-p 00017000 103:01 21241626                          ./exif
00943000-00964000 rw-p 00000000 00:00 0                                  [heap]
7f1a20000000-7f1a20021000 rw-p 00000000 00:00 0 
7f1a20021000-7f1a24000000 ---p 00000000 00:00 0 
7f1a272fc000-7f1a27312000 r-xp 00000000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f1a27312000-7f1a27511000 ---p 00016000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f1a27511000-7f1a27512000 rw-p 00015000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f1a27512000-7f1a2799d000 r--p 00000000 103:01 14811834                  /usr/lib/locale/locale-archive
7f1a2799d000-7f1a27aa5000 r-xp 00000000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f1a27aa5000-7f1a27ca4000 ---p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f1a27ca4000-7f1a27ca5000 r--p 00107000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f1a27ca5000-7f1a27ca6000 rw-p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f1a27ca6000-7f1a27e66000 r-xp 00000000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f1a27e66000-7f1a28066000 ---p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f1a28066000-7f1a2806a000 r--p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f1a2806a000-7f1a2806c000 rw-p 001c4000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f1a2806c000-7f1a28070000 rw-p 00000000 00:00 0 
7f1a28070000-7f1a2807b000 r-xp 00000000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f1a2807b000-7f1a2827a000 ---p 0000b000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f1a2827a000-7f1a2827b000 r--p 0000a000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f1a2827b000-7f1a2827c000 rw-p 0000b000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f1a2827c000-7f1a282f0000 r-xp 00000000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f1a282f0000-7f1a284f0000 ---p 00074000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f1a284f0000-7f1a28503000 r--p 00074000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f1a28503000-7f1a28504000 rw-p 00087000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f1a28504000-7f1a2852a000 r-xp 00000000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f1a286f5000-7f1a286f9000 rw-p 00000000 00:00 0 
7f1a28727000-7f1a28729000 rw-p 00000000 00:00 0 
7f1a28729000-7f1a2872a000 r--p 00025000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f1a2872a000-7f1a2872b000 rw-p 00026000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f1a2872b000-7f1a2872c000 rw-p 00000000 00:00 0 
7fff0a4c4000-7fff0a4e6000 rw-p 00000000 00:00 0                          [stack]
7fff0a5cc000-7fff0a5cf000 r--p 00000000 00:00 0                          [vvar]
7fff0a5cf000-7fff0a5d1000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
Aborted (core dumped)
```

I use **AddressSanitizer** to build libexif and exif and running it with the following command:

```shell
./exif -c exif_double_free.jpg
```

This is the ASAN information:

```
=================================================================
==22690==ERROR: AddressSanitizer: attempting double-free on 0x6110000098c0 in thread T0:
    #0 0x7f0fad6492ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x412457 in jpeg_data_save_data ./exif/libjpeg/jpeg-data.c:154
    #2 0x412457 in jpeg_data_save_file ./exif/libjpeg/jpeg-data.c:98
    #3 0x40742e in action_save ./exif/exif/actions.c:183
    #4 0x403d1d in main ./exif/exif/main.c:484
    #5 0x7f0faccf282f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x4053b8 in _start (./exif+0x4053b8)

0x6110000098c0 is located 0 bytes inside of 206-byte region [0x6110000098c0,0x61100000998e)
freed by thread T0 here:
    #0 0x7f0fad6492ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x412457 in jpeg_data_save_data ./exif/libjpeg/jpeg-data.c:154
    #2 0x412457 in jpeg_data_save_file ./exif/libjpeg/jpeg-data.c:98
    #3 0x40742e in action_save ./exif/exif/actions.c:183
    #4 0x403d1d in main ./exif/exif/main.c:484
    #5 0x7f0faccf282f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

previously allocated by thread T0 here:
    #0 0x7f0fad649961 in realloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98961)
    #1 0x41edb4 in exif_data_save_data_content ./libexif/libexif/exif-data.c:600
    #2 0x4210e5 in exif_data_save_data_content ./libexif/libexif/exif-data.c:649
    #3 0x429b93 in exif_data_save_data ./libexif/libexif/exif-data.c:1028
    #4 0x4122a6 in jpeg_data_save_data ./exif/libjpeg/jpeg-data.c:145
    #5 0x4122a6 in jpeg_data_save_file ./exif/libjpeg/jpeg-data.c:98
    #6 0x40742e in action_save ./exif/exif/actions.c:183
    #7 0x403d1d in main ./exif/exif/main.c:484
    #8 0x7f0faccf282f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: double-free ??:0 __interceptor_free
==22690==ABORTING
```

I use **valgrind** to analysis the bug and get the below information:

```
==22707== Memcheck, a memory error detector
==22707== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==22707== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==22707== Command: ./exif -c ./exif_double_free.jpg
==22707== 
==22707== Invalid free() / delete / delete[] / realloc()
==22707==    at 0x4C2EDEB: free (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==22707==    by 0x40FAF3: jpeg_data_save_data (jpeg-data.c:154)
==22707==    by 0x410040: jpeg_data_save_file (jpeg-data.c:98)
==22707==    by 0x406609: action_save (actions.c:183)
==22707==    by 0x403D4E: main (main.c:484)
==22707==  Address 0x59aeb30 is 0 bytes inside a block of size 206 free'd
==22707==    at 0x4C2EDEB: free (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==22707==    by 0x40FAF3: jpeg_data_save_data (jpeg-data.c:154)
==22707==    by 0x410040: jpeg_data_save_file (jpeg-data.c:98)
==22707==    by 0x406609: action_save (actions.c:183)
==22707==    by 0x403D4E: main (main.c:484)
==22707==  Block was alloc'd at
==22707==    at 0x4C2FD5F: realloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==22707==    by 0x4E4EFB7: exif_data_save_data_content (exif-data.c:600)
==22707==    by 0x4E503C6: exif_data_save_data_content (exif-data.c:649)
==22707==    by 0x4E534FA: exif_data_save_data (exif-data.c:1028)
==22707==    by 0x40F9B3: jpeg_data_save_data (jpeg-data.c:145)
==22707==    by 0x410040: jpeg_data_save_file (jpeg-data.c:98)
==22707==    by 0x406609: action_save (actions.c:183)
==22707==    by 0x403D4E: main (main.c:484)
==22707== 
Wrote file './exif_double_free.jpg.modified.jpeg'.
==22707== 
==22707== HEAP SUMMARY:
==22707==     in use at exit: 65 bytes in 1 blocks
==22707==   total heap usage: 247 allocs, 247 frees, 53,761 bytes allocated
==22707== 
==22707== LEAK SUMMARY:
==22707==    definitely lost: 0 bytes in 0 blocks
==22707==    indirectly lost: 0 bytes in 0 blocks
==22707==      possibly lost: 0 bytes in 0 blocks
==22707==    still reachable: 65 bytes in 1 blocks
==22707==         suppressed: 0 bytes in 0 blocks
==22707== Rerun with --leak-check=full to see details of leaked memory
==22707== 
==22707== For counts of detected and suppressed errors, rerun with: -v
==22707== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
```

From the above analysis, we can see that the memory is allocated in function exif_data_save_data_content of `libexif/exif-data.c` and double-free detected in function jpeg_data_save_data of `./exif/libjpeg/jpeg-data.c`.

# heap-buffer-overflow detected in function exif_data_load_data of exif-data.c in libexif when running exif with parameter -c

Tested in Ubuntu 16.04, 64bit. 

I use the following command:

```shell
./exif -c exif_heap_buffer_overflow.jpg
```

and get:

```
*** Error in `./exif': double free or corruption (!prev): 0x000000000210c6c0 ***
======= Backtrace: =========
/lib/x86_64-linux-gnu/libc.so.6(+0x777e5)[0x7f7b644e37e5]
/lib/x86_64-linux-gnu/libc.so.6(+0x8037a)[0x7f7b644ec37a]
/lib/x86_64-linux-gnu/libc.so.6(cfree+0x4c)[0x7f7b644f053c]
./exif[0x40faf4]
./exif[0x410041]
./exif[0x40660a]
./exif[0x403d4f]
/lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xf0)[0x7f7b6448c830]
./exif[0x404f79]
======= Memory map: ========
00400000-00417000 r-xp 00000000 103:01 21241626                          ./exif
00616000-00617000 r--p 00016000 103:01 21241626                          ./exif
00617000-00618000 rw-p 00017000 103:01 21241626                          ./exif
02108000-02129000 rw-p 00000000 00:00 0                                  [heap]
7f7b5c000000-7f7b5c021000 rw-p 00000000 00:00 0 
7f7b5c021000-7f7b60000000 ---p 00000000 00:00 0 
7f7b63ac2000-7f7b63ad8000 r-xp 00000000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f7b63ad8000-7f7b63cd7000 ---p 00016000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f7b63cd7000-7f7b63cd8000 rw-p 00015000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f7b63cd8000-7f7b64163000 r--p 00000000 103:01 14811834                  /usr/lib/locale/locale-archive
7f7b64163000-7f7b6426b000 r-xp 00000000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f7b6426b000-7f7b6446a000 ---p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f7b6446a000-7f7b6446b000 r--p 00107000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f7b6446b000-7f7b6446c000 rw-p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f7b6446c000-7f7b6462c000 r-xp 00000000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f7b6462c000-7f7b6482c000 ---p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f7b6482c000-7f7b64830000 r--p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f7b64830000-7f7b64832000 rw-p 001c4000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f7b64832000-7f7b64836000 rw-p 00000000 00:00 0 
7f7b64836000-7f7b64841000 r-xp 00000000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f7b64841000-7f7b64a40000 ---p 0000b000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f7b64a40000-7f7b64a41000 r--p 0000a000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f7b64a41000-7f7b64a42000 rw-p 0000b000 103:01 10490569                  /lib/x86_64-linux-gnu/libpopt.so.0.0.0
7f7b64a42000-7f7b64ab6000 r-xp 00000000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f7b64ab6000-7f7b64cb6000 ---p 00074000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f7b64cb6000-7f7b64cc9000 r--p 00074000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f7b64cc9000-7f7b64cca000 rw-p 00087000 103:01 21241278                  libexif_install/lib/libexif.so.12.3.3
7f7b64cca000-7f7b64cf0000 r-xp 00000000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f7b64ebb000-7f7b64ebf000 rw-p 00000000 00:00 0 
7f7b64eed000-7f7b64eef000 rw-p 00000000 00:00 0 
7f7b64eef000-7f7b64ef0000 r--p 00025000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f7b64ef0000-7f7b64ef1000 rw-p 00026000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f7b64ef1000-7f7b64ef2000 rw-p 00000000 00:00 0 
7ffdf72d0000-7ffdf72f2000 rw-p 00000000 00:00 0                          [stack]
7ffdf7381000-7ffdf7384000 r--p 00000000 00:00 0                          [vvar]
7ffdf7384000-7ffdf7386000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
Aborted (core dumped)
```

I use **AddressSanitizer** to build exif and libexif and running it with the following command:

```shell
./exif -c exif_heap_buffer_overflow.jpg
```

This is the ASAN information:

```
=================================================================
==23451==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x61200000bd3f at pc 0x7f4e7e0ad676 bp 0x7ffcc91ab730 sp 0x7ffcc91aaed8
READ of size 6 at 0x61200000bd3f thread T0
    #0 0x7f4e7e0ad675 in memcmp (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x77675)
    #1 0x42b4f9 in exif_data_load_data libexif/libexif/exif-data.c:846
    #2 0x42b4f9 in exif_data_new_from_data libexif/libexif/exif-data.c:156
    #3 0x414b1d in jpeg_data_load_data exif/libjpeg/jpeg-data.c:237
    #4 0x4156be in jpeg_data_load_file exif/libjpeg/jpeg-data.c:349
    #5 0x4073f4 in action_save exif/exif/actions.c:168
    #6 0x403d1d in main exif/exif/main.c:484
    #7 0x7f4e7d77782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #8 0x4053b8 in _start (./exif+0x4053b8)

0x61200000bd3f is located 1 bytes to the left of 303-byte region [0x61200000bd40,0x61200000be6f)
allocated by thread T0 here:
    #0 0x7f4e7e0ce602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x415503 in jpeg_data_load_file exif/libjpeg/jpeg-data.c:332
    #2 0x4073f4 in action_save exif/exif/actions.c:168
    #3 0x403d1d in main exif/exif/main.c:484
    #4 0x7f4e7d77782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-buffer-overflow ??:0 memcmp
Shadow bytes around the buggy address:
  0x0c247fff9750: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c247fff9760: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c247fff9770: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c247fff9780: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c247fff9790: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c247fff97a0: fa fa fa fa fa fa fa[fa]00 00 00 00 00 00 00 00
  0x0c247fff97b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c247fff97c0: 00 00 00 00 00 00 00 00 00 00 00 00 00 07 fa fa
  0x0c247fff97d0: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
  0x0c247fff97e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c247fff97f0: 00 00 00 00 00 00 00 00 00 00 00 01 fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Heap right redzone:      fb
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack partial redzone:   f4
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
==23451==ABORTING
```

I use **valgrind** to analysis the bug and get the below information:

```
==23442== Memcheck, a memory error detector
==23442== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==23442== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==23442== Command: ./exif -c ./exif_heap_buffer_overflow.jpg
==23442== 
==23442== Invalid read of size 1
==23442==    at 0x4C33D25: __memcmp_sse4_1 (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==23442==    by 0x4E55093: exif_data_load_data (exif-data.c:846)
==23442==    by 0x4E57147: exif_data_new_from_data (exif-data.c:156)
==23442==    by 0x410CB4: jpeg_data_load_data (jpeg-data.c:237)
==23442==    by 0x411479: jpeg_data_load_file (jpeg-data.c:349)
==23442==    by 0x406565: action_save (actions.c:168)
==23442==    by 0x403D4E: main (main.c:484)
==23442==  Address 0x59acb0f is 1 bytes before a block of size 303 alloc'd
==23442==    at 0x4C2DB8F: malloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==23442==    by 0x411247: jpeg_data_load_file (jpeg-data.c:332)
==23442==    by 0x406565: action_save (actions.c:168)
==23442==    by 0x403D4E: main (main.c:484)
==23442== 
==23442== Invalid read of size 1
==23442==    at 0x4E5510C: exif_data_load_data (exif-data.c:851)
==23442==    by 0x4E57147: exif_data_new_from_data (exif-data.c:156)
==23442==    by 0x410CB4: jpeg_data_load_data (jpeg-data.c:237)
==23442==    by 0x411479: jpeg_data_load_file (jpeg-data.c:349)
==23442==    by 0x406565: action_save (actions.c:168)
==23442==    by 0x403D4E: main (main.c:484)
==23442==  Address 0x59acb0f is 1 bytes before a block of size 303 alloc'd
==23442==    at 0x4C2DB8F: malloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==23442==    by 0x411247: jpeg_data_load_file (jpeg-data.c:332)
==23442==    by 0x406565: action_save (actions.c:168)
==23442==    by 0x403D4E: main (main.c:484)
==23442== 
==23442== Invalid free() / delete / delete[] / realloc()
==23442==    at 0x4C2EDEB: free (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==23442==    by 0x40FAF3: jpeg_data_save_data (jpeg-data.c:154)
==23442==    by 0x410040: jpeg_data_save_file (jpeg-data.c:98)
==23442==    by 0x406609: action_save (actions.c:183)
==23442==    by 0x403D4E: main (main.c:484)
==23442==  Address 0x59adaf0 is 0 bytes inside a block of size 206 free'd
==23442==    at 0x4C2EDEB: free (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==23442==    by 0x40FAF3: jpeg_data_save_data (jpeg-data.c:154)
==23442==    by 0x410040: jpeg_data_save_file (jpeg-data.c:98)
==23442==    by 0x406609: action_save (actions.c:183)
==23442==    by 0x403D4E: main (main.c:484)
==23442==  Block was alloc'd at
==23442==    at 0x4C2FD5F: realloc (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==23442==    by 0x4E4EFB7: exif_data_save_data_content (exif-data.c:600)
==23442==    by 0x4E503C6: exif_data_save_data_content (exif-data.c:649)
==23442==    by 0x4E534FA: exif_data_save_data (exif-data.c:1028)
==23442==    by 0x40F9B3: jpeg_data_save_data (jpeg-data.c:145)
==23442==    by 0x410040: jpeg_data_save_file (jpeg-data.c:98)
==23442==    by 0x406609: action_save (actions.c:183)
==23442==    by 0x403D4E: main (main.c:484)
==23442== 
Wrote file './exif_heap_buffer_overflow.jpg.modified.jpeg'.
==23442== 
==23442== HEAP SUMMARY:
==23442==     in use at exit: 74 bytes in 1 blocks
==23442==   total heap usage: 213 allocs, 213 frees, 45,045 bytes allocated
==23442== 
==23442== LEAK SUMMARY:
==23442==    definitely lost: 0 bytes in 0 blocks
==23442==    indirectly lost: 0 bytes in 0 blocks
==23442==      possibly lost: 0 bytes in 0 blocks
==23442==    still reachable: 74 bytes in 1 blocks
==23442==         suppressed: 0 bytes in 0 blocks
==23442== Rerun with --leak-check=full to see details of leaked memory
==23442== 
==23442== For counts of detected and suppressed errors, rerun with: -v
==23442== ERROR SUMMARY: 3 errors from 3 contexts (suppressed: 0 from 0)
```