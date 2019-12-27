# Segmentation fault detected in function Get32s of exif.c when running jhead 3.04

Tested in Ubuntu 16.04, 64bit. [Jhead](https://www.sentex.ca/~mwandel/jhead/) version is 3.04 and updated on Nov 22 2019.

I use the following command:

```shell
./jhead -mkexif jhead_segv.jpg
```

and get:

```
Nonfatal Error : 'jhead_segv.jpg' Too many components -16777196 for tag 0002 in Exif

Nonfatal Error : 'jhead_segv.jpg' Inappropriate format (2) for Exif GPS coordinates!
Segmentation fault
```

I use **AddressSanitizer** to build jhead 3.04 and running it with the following command:

```shell
./jhead jhead_segv.jpg
```

This is the ASAN information:

```
install/jhead -mkexif jhead_segv.jpg

Nonfatal Error : 'jhead_segv.jpg' Too many components -16777196 for tag 0002 in Exif

Nonfatal Error : 'jhead_segv.jpg' Inappropriate format (2) for Exif GPS coordinates!
ASAN:SIGSEGV
=================================================================
==28329==ERROR: AddressSanitizer: SEGV on unknown address 0x60d00100cf7b (pc 0x00000040ae0c bp 0x7fff3f4085f0 sp 0x7fff3f4085f0 T0)
    #0 0x40ae0b in Get32s jhead-3.04/exif.c:337
    #1 0x4115dc in ProcessGpsInfo jhead-3.04/gpsinfo.c:138
    #2 0x40d9a3 in ProcessExifDir jhead-3.04/exif.c:866
    #3 0x40d91c in ProcessExifDir jhead-3.04/exif.c:852
    #4 0x40e09a in process_EXIF jhead-3.04/exif.c:1041
    #5 0x408318 in ReadJpegSections jhead-3.04/jpgfile.c:287
    #6 0x408581 in ReadJpegFile jhead-3.04/jpgfile.c:379
    #7 0x405039 in ProcessFile jhead-3.04/jhead.c:905
    #8 0x40267d in main jhead-3.04/jhead.c:1756
    #9 0x7f95e5ea182f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #10 0x403c38 in _start (install/jhead+0x403c38)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV jhead-3.04/exif.c:337 Get32s
==28329==ABORTING
```

I use **valgrind** to analysis the bug and get the below information:

```
==872== Memcheck, a memory error detector
==872== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==872== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==872== Command: ./jhead -mkexif jhead_segv.jpg
==872== 

Nonfatal Error : 'jhead_segv.jpg' Too many components -16777196 for tag 0002 in Exif

Nonfatal Error : 'jhead_segv.jpg' Inappropriate format (2) for Exif GPS coordinates!
==872== Invalid read of size 4
==872==    at 0x41D27E: Get32s (exif.c:332)
==872==    by 0x42CB9C: ProcessGpsInfo (gpsinfo.c:138)
==872==    by 0x423C4B: ProcessExifDir (exif.c:866)
==872==    by 0x423A3D: ProcessExifDir (exif.c:852)
==872==    by 0x424324: process_EXIF (exif.c:1041)
==872==    by 0x411471: ReadJpegSections.part.0 (jpgfile.c:287)
==872==    by 0x41289E: ReadJpegSections (jpgfile.c:126)
==872==    by 0x41289E: ReadJpegFile (jpgfile.c:379)
==872==    by 0x408E8E: ProcessFile (jhead.c:905)
==872==    by 0x4022C3: main (jhead.c:1756)
==872==  Address 0x650e38b is not stack'd, malloc'd or (recently) free'd
==872== 
==872== 
==872== Process terminating with default action of signal 11 (SIGSEGV)
==872==  Access not within mapped region at address 0x650E38B
==872==    at 0x41D27E: Get32s (exif.c:332)
==872==    by 0x42CB9C: ProcessGpsInfo (gpsinfo.c:138)
==872==    by 0x423C4B: ProcessExifDir (exif.c:866)
==872==    by 0x423A3D: ProcessExifDir (exif.c:852)
==872==    by 0x424324: process_EXIF (exif.c:1041)
==872==    by 0x411471: ReadJpegSections.part.0 (jpgfile.c:287)
==872==    by 0x41289E: ReadJpegSections (jpgfile.c:126)
==872==    by 0x41289E: ReadJpegFile (jpgfile.c:379)
==872==    by 0x408E8E: ProcessFile (jhead.c:905)
==872==    by 0x4022C3: main (jhead.c:1756)
==872==  If you believe this happened as a result of a stack
==872==  overflow in your program's main thread (unlikely but
==872==  possible), you can try to increase the size of the
==872==  main thread stack using the --main-stacksize= flag.
==872==  The main thread stack size used in this run was 8388608.
==872== 
==872== HEAP SUMMARY:
==872==     in use at exit: 766 bytes in 3 blocks
==872==   total heap usage: 4 allocs, 1 frees, 4,862 bytes allocated
==872== 
==872== LEAK SUMMARY:
==872==    definitely lost: 0 bytes in 0 blocks
==872==    indirectly lost: 0 bytes in 0 blocks
==872==      possibly lost: 0 bytes in 0 blocks
==872==    still reachable: 766 bytes in 3 blocks
==872==         suppressed: 0 bytes in 0 blocks
==872== Rerun with --leak-check=full to see details of leaked memory
==872== 
==872== For counts of detected and suppressed errors, rerun with: -v
==872== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
Segmentation fault
```

I review the source code and try to find why this error occurs. I find line 138 of gpsinfo.c calls function Get32s and its parameter is ValuePtr+4+a*ComponentSize. However, the parameter which doesn't been verified may not be a valid pointer. So we will get a segmentation fault when the pointer be dereferenced with a malicious JPEG file. This may allow a remote attacker to cause a denial-of-service attack or unspecified other impact.

# heap-buffer-overflow detected in function process_DQT of jpgqguess.c when running jhead 3.04

Tested in Ubuntu 16.04, 64bit. [Jhead](https://www.sentex.ca/~mwandel/jhead/) version is 3.04 and updated on Nov 22 2019.

I use the following command:

```shell
./jhead -mkexif jhead_heap_buffer_overflow
```

and get many nonfatal error prompts like the following:

```
Nonfatal Error : 'jhead_heap_buffer_overflow.jpg' Extraneous 32 padding bytes before section DB
......
```

I use **AddressSanitizer** to build jhead 3.04 and running it with the following command:

```shell
./jhead -mkexif jhead_heap_buffer_overflow
```

This is the ASAN information:

```
install/jhead -mkexif jhead_segv.jpg

......
many nonfatal error prompts
......

=================================================================
==20109==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60700001b183 at pc 0x00000040a14f bp 0x7fffcf50e400 sp 0x7fffcf50e3f0
READ of size 1 at 0x60700001b183 thread T0
    #0 0x40a14e in process_DQT jhead-3.04/jpgqguess.c:109
    #1 0x407e02 in ReadJpegSections jhead-3.04/jpgfile.c:223
    #2 0x408581 in ReadJpegFile jhead-3.04/jpgfile.c:379
    #3 0x405039 in ProcessFile jhead-3.04/jhead.c:905
    #4 0x40267d in main jhead-3.04/jhead.c:1756
    #5 0x7fd1a987182f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x403c38 in _start (install/jhead+0x403c38)

0x60700001b183 is located 0 bytes to the right of 67-byte region [0x60700001b140,0x60700001b183)
allocated by thread T0 here:
    #0 0x7fd1a9fbc602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x40798b in ReadJpegSections jhead-3.04/jpgfile.c:173
    #2 0x408581 in ReadJpegFile jhead-3.04/jpgfile.c:379
    #3 0x405039 in ProcessFile jhead-3.04/jhead.c:905
    #4 0x40267d in main jhead-3.04/jhead.c:1756
    #5 0x7fd1a987182f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-buffer-overflow jhead-3.04/jpgqguess.c:109 process_DQT
Shadow bytes around the buggy address:
  0x0c0e7fffb5e0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0e7fffb5f0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0e7fffb600: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0e7fffb610: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0e7fffb620: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
=>0x0c0e7fffb630:[03]fa fa fa fa fa 00 00 00 00 00 00 00 00 03 fa
  0x0c0e7fffb640: fa fa fa fa 00 00 00 00 00 00 00 00 03 fa fa fa
  0x0c0e7fffb650: fa fa 00 00 00 00 00 00 00 00 03 fa fa fa fa fa
  0x0c0e7fffb660: 00 00 00 00 00 00 00 00 03 fa fa fa fa fa 00 00
  0x0c0e7fffb670: 00 00 00 00 00 00 03 fa fa fa fa fa 00 00 00 00
  0x0c0e7fffb680: 00 00 00 00 03 fa fa fa fa fa 00 00 00 00 00 00
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
==20109==ABORTING
```

