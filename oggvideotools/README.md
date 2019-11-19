# SEGV in function StreamSerializer::extractStreams() in streamSerializer.cpp

Tested in Ubuntu 16.04, 64bit

I use the following command:

```shell
./oggLength oggLength_SEGV
```

and get:

```
Segmentation fault
```

I use **valgrind** to analysis the bug and get the below information:

```
==7902== Memcheck, a memory error detector
==7902== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==7902== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==7902== Command: /home/wws/Music/Fuzz/target_progs/target_oggvideotools/install/bin/oggLength /home/wws/Music/Fuzz/cmp/fuzz_out_oggvideotools_oggLength/crashes/id:000025,sig:11,src:000146,op:flip1,pos:5
==7902== 
==7902== Invalid read of size 8
==7902==    at 0x41F650: StreamSerializer::extractStreams() (streamSerializer.cpp:163)
==7902==    by 0x4261A2: StreamSerializer::open(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >&) (streamSerializer.cpp:75)
==7902==    by 0x40F6A3: oggLengthCmd(int, char**) (oggLength.cpp:86)
==7902==    by 0x40E412: main (oggLength.cpp:136)
==7902==  Address 0x0 is not stack'd, malloc'd or (recently) free'd
==7902== 
==7902== 
==7902== Process terminating with default action of signal 11 (SIGSEGV)
==7902==  Access not within mapped region at address 0x0
==7902==    at 0x41F650: StreamSerializer::extractStreams() (streamSerializer.cpp:163)
==7902==    by 0x4261A2: StreamSerializer::open(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >&) (streamSerializer.cpp:75)
==7902==    by 0x40F6A3: oggLengthCmd(int, char**) (oggLength.cpp:86)
==7902==    by 0x40E412: main (oggLength.cpp:136)
==7902==  If you believe this happened as a result of a stack
==7902==  overflow in your program's main thread (unlikely but
==7902==  possible), you can try to increase the size of the
==7902==  main thread stack using the --main-stacksize= flag.
==7902==  The main thread stack size used in this run was 8388608.
==7902== 
==7902== HEAP SUMMARY:
==7902==     in use at exit: 157,974 bytes in 18 blocks
==7902==   total heap usage: 22 allocs, 4 frees, 162,629 bytes allocated
==7902== 
==7902== LEAK SUMMARY:
==7902==    definitely lost: 0 bytes in 0 blocks
==7902==    indirectly lost: 0 bytes in 0 blocks
==7902==      possibly lost: 0 bytes in 0 blocks
==7902==    still reachable: 157,974 bytes in 18 blocks
==7902==         suppressed: 0 bytes in 0 blocks
==7902== Rerun with --leak-check=full to see details of leaked memory
==7902== 
==7902== For counts of detected and suppressed errors, rerun with: -v
==7902== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
Segmentation fault (core dumped)
```

I use **AddressSanitizer** to build oggvideotools, this file can cause SEGV signal in function StreamSerializer::extractStreams() with the following command:

```shell
./oggLength oggLength_SEGV
```

This is the ASAN information:

```
ASAN:SIGSEGV
=================================================================
==7905==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x00000043e4b9 bp 0x7ffe756519d0 sp 0x7ffe75651720 T0)
    #0 0x43e4b8 in StreamSerializer::extractStreams() oggvideotools-0.9.1/src/main/streamSerializer.cpp:163
    #1 0x43da8f in StreamSerializer::open(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >&) oggvideotools-0.9.1/src/main/streamSerializer.cpp:75
    #2 0x43b044 in oggLengthCmd(int, char**) oggvideotools-0.9.1/src/binaries/oggLength.cpp:86
    #3 0x43b4e5 in main oggvideotools-0.9.1/src/binaries/oggLength.cpp:136
    #4 0x7f6e840b082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #5 0x43aa78 in _start (target_oggvideotools/install/bin/oggLength+0x43aa78)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV 
oggvideotools-0.9.1/src/main/streamSerializer.cpp:163 StreamSerializer::extractStreams()
==7905==ABORTING
```

# SEGV and heap-overflow

Tested in Ubuntu 16.04, 64bit

I use the following command:

```shell
./oggLength oggLength_SEGV_heap-overflow
```

and get:

```
Segmentation fault
```

I use **valgrind** to analysis the bug and get the below information:

```
==7917== Memcheck, a memory error detector
==7917== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==7917== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==7917== Command: ./oggLength oggLength_SEGV_heap-overflow
==7917== 
MediaConverter::setAvailable(): decoder is not configured or has ended
==7917== Invalid write of size 4
==7917==    at 0x497B98: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:17)
==7917==    by 0x422033: operator= (streamConfig.h:12)
==7917==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe38 is 0 bytes after a block of size 56 alloc'd
==7917==    at 0x4C2E0EF: operator new(unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==7917==    by 0x43A6D8: allocate (new_allocator.h:104)
==7917==    by 0x43A6D8: allocate (alloc_traits.h:491)
==7917==    by 0x43A6D8: _M_allocate (stl_vector.h:170)
==7917==    by 0x43A6D8: std::vector<StreamConfig, std::allocator<StreamConfig> >::_M_default_append(unsigned long) (vector.tcc:557)
==7917==    by 0x424FB2: resize (stl_vector.h:676)
==7917==    by 0x424FB2: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:206)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917== 
==7917== Invalid write of size 4
==7917==    at 0x497B9A: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:18)
==7917==    by 0x422033: operator= (streamConfig.h:12)
==7917==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe3c is 4 bytes after a block of size 56 alloc'd
==7917==    at 0x4C2E0EF: operator new(unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==7917==    by 0x43A6D8: allocate (new_allocator.h:104)
==7917==    by 0x43A6D8: allocate (alloc_traits.h:491)
==7917==    by 0x43A6D8: _M_allocate (stl_vector.h:170)
==7917==    by 0x43A6D8: std::vector<StreamConfig, std::allocator<StreamConfig> >::_M_default_append(unsigned long) (vector.tcc:557)
==7917==    by 0x424FB2: resize (stl_vector.h:676)
==7917==    by 0x424FB2: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:206)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917== 
==7917== Invalid write of size 1
==7917==    at 0x497BA1: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:19)
==7917==    by 0x422033: operator= (streamConfig.h:12)
==7917==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe50 is 16 bytes after a block of size 64 in arena "client"
==7917== 
==7917== Invalid write of size 8
==7917==    at 0x497BAC: operator= (shared_ptr_base.h:867)
==7917==    by 0x497BAC: operator= (shared_ptr.h:93)
==7917==    by 0x497BAC: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:21)
==7917==    by 0x422033: operator= (streamConfig.h:12)
==7917==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe40 is 8 bytes after a block of size 56 alloc'd
==7917==    at 0x4C2E0EF: operator new(unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==7917==    by 0x43A6D8: allocate (new_allocator.h:104)
==7917==    by 0x43A6D8: allocate (alloc_traits.h:491)
==7917==    by 0x43A6D8: _M_allocate (stl_vector.h:170)
==7917==    by 0x43A6D8: std::vector<StreamConfig, std::allocator<StreamConfig> >::_M_default_append(unsigned long) (vector.tcc:557)
==7917==    by 0x424FB2: resize (stl_vector.h:676)
==7917==    by 0x424FB2: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:206)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917== 
==7917== Invalid read of size 8
==7917==    at 0x497BB0: operator= (shared_ptr_base.h:673)
==7917==    by 0x497BB0: operator= (shared_ptr_base.h:867)
==7917==    by 0x497BB0: operator= (shared_ptr.h:93)
==7917==    by 0x497BB0: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:21)
==7917==    by 0x422033: operator= (streamConfig.h:12)
==7917==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe48 is 16 bytes after a block of size 56 alloc'd
==7917==    at 0x4C2E0EF: operator new(unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==7917==    by 0x43A6D8: allocate (new_allocator.h:104)
==7917==    by 0x43A6D8: allocate (alloc_traits.h:491)
==7917==    by 0x43A6D8: _M_allocate (stl_vector.h:170)
==7917==    by 0x43A6D8: std::vector<StreamConfig, std::allocator<StreamConfig> >::_M_default_append(unsigned long) (vector.tcc:557)
==7917==    by 0x424FB2: resize (stl_vector.h:676)
==7917==    by 0x424FB2: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:206)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917== 
==7917== Invalid write of size 1
==7917==    at 0x422041: operator= (streamConfig.h:12)
==7917==    by 0x422041: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe51 is 17 bytes after a block of size 64 in arena "client"
==7917== 
==7917== Invalid read of size 8
==7917==    at 0x429916: size (stl_vector.h:655)
==7917==    by 0x429916: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:191)
==7917==    by 0x422048: operator= (streamConfig.h:12)
==7917==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe58 is 24 bytes after a block of size 64 in arena "client"
==7917== 
==7917== Invalid read of size 8
==7917==    at 0x429919: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:192)
==7917==    by 0x422048: operator= (streamConfig.h:12)
==7917==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe68 is 24 bytes before an unallocated block of size 3,887,456 in arena "client"
==7917== 
==7917== Invalid read of size 8
==7917==    at 0x429974: size (stl_vector.h:655)
==7917==    by 0x429974: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:204)
==7917==    by 0x422048: operator= (streamConfig.h:12)
==7917==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x5aefe60 is 32 bytes before an unallocated block of size 3,887,456 in arena "client"
==7917== 
==7917== Invalid read of size 8
==7917==    at 0x429F87: ~__shared_count (shared_ptr_base.h:658)
==7917==    by 0x429F87: ~__shared_ptr (shared_ptr_base.h:925)
==7917==    by 0x429F87: ~shared_ptr (shared_ptr.h:93)
==7917==    by 0x429F87: _Destroy<std::shared_ptr<OggPacketInternal> > (stl_construct.h:93)
==7917==    by 0x429F87: __destroy<__gnu_cxx::__normal_iterator<std::shared_ptr<OggPacketInternal>*, std::vector<std::shared_ptr<OggPacketInternal> > > > (stl_construct.h:103)
==7917==    by 0x429F87: _Destroy<__gnu_cxx::__normal_iterator<std::shared_ptr<OggPacketInternal>*, std::vector<std::shared_ptr<OggPacketInternal> > > > (stl_construct.h:126)
==7917==    by 0x429F87: _Destroy<__gnu_cxx::__normal_iterator<std::shared_ptr<OggPacketInternal>*, std::vector<std::shared_ptr<OggPacketInternal> > >, std::shared_ptr<OggPacketInternal> > (stl_construct.h:151)
==7917==    by 0x429F87: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:206)
==7917==    by 0x422048: operator= (streamConfig.h:12)
==7917==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  Address 0x88 is not stack'd, malloc'd or (recently) free'd
==7917== 
==7917== 
==7917== Process terminating with default action of signal 11 (SIGSEGV)
==7917==  Access not within mapped region at address 0x88
==7917==    at 0x429F87: ~__shared_count (shared_ptr_base.h:658)
==7917==    by 0x429F87: ~__shared_ptr (shared_ptr_base.h:925)
==7917==    by 0x429F87: ~shared_ptr (shared_ptr.h:93)
==7917==    by 0x429F87: _Destroy<std::shared_ptr<OggPacketInternal> > (stl_construct.h:93)
==7917==    by 0x429F87: __destroy<__gnu_cxx::__normal_iterator<std::shared_ptr<OggPacketInternal>*, std::vector<std::shared_ptr<OggPacketInternal> > > > (stl_construct.h:103)
==7917==    by 0x429F87: _Destroy<__gnu_cxx::__normal_iterator<std::shared_ptr<OggPacketInternal>*, std::vector<std::shared_ptr<OggPacketInternal> > > > (stl_construct.h:126)
==7917==    by 0x429F87: _Destroy<__gnu_cxx::__normal_iterator<std::shared_ptr<OggPacketInternal>*, std::vector<std::shared_ptr<OggPacketInternal> > >, std::shared_ptr<OggPacketInternal> > (stl_construct.h:151)
==7917==    by 0x429F87: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:206)
==7917==    by 0x422048: operator= (streamConfig.h:12)
==7917==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7917==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7917==    by 0x40E412: main (oggLength.cpp:136)
==7917==  If you believe this happened as a result of a stack
==7917==  overflow in your program's main thread (unlikely but
==7917==  possible), you can try to increase the size of the
==7917==  main thread stack using the --main-stacksize= flag.
==7917==  The main thread stack size used in this run was 8388608.
==7917== 
==7917== HEAP SUMMARY:
==7917==     in use at exit: 182,963 bytes in 508 blocks
==7917==   total heap usage: 688 allocs, 180 frees, 259,825 bytes allocated
==7917== 
==7917== LEAK SUMMARY:
==7917==    definitely lost: 104 bytes in 2 blocks
==7917==    indirectly lost: 13,138 bytes in 148 blocks
==7917==      possibly lost: 0 bytes in 0 blocks
==7917==    still reachable: 169,721 bytes in 358 blocks
==7917==         suppressed: 0 bytes in 0 blocks
==7917== Rerun with --leak-check=full to see details of leaked memory
==7917== 
==7917== For counts of detected and suppressed errors, rerun with: -v
==7917== ERROR SUMMARY: 10 errors from 10 contexts (suppressed: 0 from 0)
Segmentation fault (core dumped)
```

I use **AddressSanitizer** to build oggvideotools, this file can cause SEGV signal and heap-overflow error with the following command:

```shell
./oggLength oggLength_SEGV_heap-overflow
```

This is the ASAN information:

```
MediaConverter::setAvailable(): decoder is not configured or has ended
=================================================================
==7920==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60600000e9f8 at pc 0x00000046e493 bp 0x7ffd98330590 sp 0x7ffd98330580
WRITE of size 4 at 0x60600000e9f8 thread T0
    #0 0x46e492 in ExtractorInformation::operator=(ExtractorInformation const&) oggvideotools-0.9.1/src/base/streamExtractor.cpp:17
    #1 0x4407e2 in StreamConfig::operator=(StreamConfig const&) oggvideotools-0.9.1/src/base/streamConfig.h:12
    #2 0x43ea66 in StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) oggvideotools-0.9.1/src/main/streamSerializer.cpp:210
    #3 0x43b0c4 in oggLengthCmd(int, char**) oggvideotools-0.9.1/src/binaries/oggLength.cpp:93
    #4 0x43b4e5 in main oggvideotools-0.9.1/src/binaries/oggLength.cpp:136
    #5 0x7f6f006f282f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x43aa78 in _start (install/bin/oggLength+0x43aa78)

0x60600000e9f8 is located 0 bytes to the right of 56-byte region [0x60600000e9c0,0x60600000e9f8)
allocated by thread T0 here:
    #0 0x7f6f010cd532 in operator new(unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x99532)
    #1 0x4468b4 in __gnu_cxx::new_allocator<StreamConfig>::allocate(unsigned long, void const*) /usr/include/c++/5/ext/new_allocator.h:104
    #2 0x4460b3 in std::allocator_traits<std::allocator<StreamConfig> >::allocate(std::allocator<StreamConfig>&, unsigned long) /usr/include/c++/5/bits/alloc_traits.h:491
    #3 0x4451fd in std::_Vector_base<StreamConfig, std::allocator<StreamConfig> >::_M_allocate(unsigned long) /usr/include/c++/5/bits/stl_vector.h:170
    #4 0x443257 in std::vector<StreamConfig, std::allocator<StreamConfig> >::_M_default_append(unsigned long) (install/bin/oggLength+0x443257)
    #5 0x441d5e in std::vector<StreamConfig, std::allocator<StreamConfig> >::resize(unsigned long) install/bin/oggLength+0x441d5e)
    #6 0x43e9a5 in StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) oggvideotools-0.9.1/src/main/streamSerializer.cpp:206
    #7 0x43b0c4 in oggLengthCmd(int, char**) oggvideotools-0.9.1/src/binaries/oggLength.cpp:93
    #8 0x43b4e5 in main oggvideotools-0.9.1/src/binaries/oggLength.cpp:136
    #9 0x7f6f006f282f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-buffer-overflow oggvideotools-0.9.1/src/base/streamExtractor.cpp:17 ExtractorInformation::operator=(ExtractorInformation const&)
Shadow bytes around the buggy address:
  0x0c0c7fff9ce0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9cf0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9d00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9d10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9d20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c0c7fff9d30: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00[fa]
  0x0c0c7fff9d40: fa fa fa fa fd fd fd fd fd fd fd fd fa fa fa fa
  0x0c0c7fff9d50: fd fd fd fd fd fd fd fd fa fa fa fa 00 00 00 00
  0x0c0c7fff9d60: 00 00 00 00 fa fa fa fa 00 00 00 00 00 00 00 03
  0x0c0c7fff9d70: fa fa fa fa fd fd fd fd fd fd fd fd fa fa fa fa
  0x0c0c7fff9d80: 00 00 00 00 00 00 00 02 fa fa fa fa fd fd fd fd
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
==7920==ABORTING
```

# SEGV and heap-use-after-free

Tested in Ubuntu 16.04, 64bit

I use the following command:

```shell
./oggLength oggLength_SEGV_heap-use-after-free
```

and get:

```
Segmentation fault
```

I use **valgrind** to analysis the bug and get the below information:

```
==7910== Memcheck, a memory error detector
==7910== Copyright (C) 2002-2015, and GNU GPL'd, by Julian Seward et al.
==7910== Using Valgrind-3.11.0 and LibVEX; rerun with -h for copyright info
==7910== Command: ./oggLength oggLength_SEGV_heap-use-after-free
==7910== 
==7910== Invalid write of size 4
==7910==    at 0x497B98: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:17)
==7910==    by 0x422033: operator= (streamConfig.h:12)
==7910==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3870 is 16 bytes before an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid write of size 4
==7910==    at 0x497B9A: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:18)
==7910==    by 0x422033: operator= (streamConfig.h:12)
==7910==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3874 is 12 bytes before an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid write of size 1
==7910==    at 0x497BA1: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:19)
==7910==    by 0x422033: operator= (streamConfig.h:12)
==7910==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3888 is 8 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid write of size 8
==7910==    at 0x497BAC: operator= (shared_ptr_base.h:867)
==7910==    by 0x497BAC: operator= (shared_ptr.h:93)
==7910==    by 0x497BAC: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:21)
==7910==    by 0x422033: operator= (streamConfig.h:12)
==7910==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3878 is 8 bytes before an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid read of size 8
==7910==    at 0x497BB0: operator= (shared_ptr_base.h:673)
==7910==    by 0x497BB0: operator= (shared_ptr_base.h:867)
==7910==    by 0x497BB0: operator= (shared_ptr.h:93)
==7910==    by 0x497BB0: ExtractorInformation::operator=(ExtractorInformation const&) (streamExtractor.cpp:21)
==7910==    by 0x422033: operator= (streamConfig.h:12)
==7910==    by 0x422033: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3880 is 0 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid write of size 1
==7910==    at 0x422041: operator= (streamConfig.h:12)
==7910==    by 0x422041: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3889 is 9 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid read of size 8
==7910==    at 0x429916: size (stl_vector.h:655)
==7910==    by 0x429916: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:191)
==7910==    by 0x422048: operator= (streamConfig.h:12)
==7910==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3890 is 16 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid read of size 8
==7910==    at 0x429919: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:192)
==7910==    by 0x422048: operator= (streamConfig.h:12)
==7910==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae38a0 is 32 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid read of size 8
==7910==    at 0x429974: size (stl_vector.h:655)
==7910==    by 0x429974: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:204)
==7910==    by 0x422048: operator= (streamConfig.h:12)
==7910==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3898 is 24 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid read of size 8
==7910==    at 0x42BFC0: __uninit_copy<std::shared_ptr<OggPacketInternal>*, std::shared_ptr<OggPacketInternal>*> (stl_uninitialized.h:74)
==7910==    by 0x42BFC0: uninitialized_copy<std::shared_ptr<OggPacketInternal>*, std::shared_ptr<OggPacketInternal>*> (stl_uninitialized.h:126)
==7910==    by 0x42BFC0: __uninitialized_copy_a<std::shared_ptr<OggPacketInternal>*, std::shared_ptr<OggPacketInternal>*, std::shared_ptr<OggPacketInternal> > (stl_uninitialized.h:281)
==7910==    by 0x42BFC0: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:213)
==7910==    by 0x422048: operator= (streamConfig.h:12)
==7910==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3890 is 16 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
==7910== Invalid write of size 8
==7910==    at 0x42BFFC: std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > >::operator=(std::vector<std::shared_ptr<OggPacketInternal>, std::allocator<std::shared_ptr<OggPacketInternal> > > const&) (vector.tcc:218)
==7910==    by 0x422048: operator= (streamConfig.h:12)
==7910==    by 0x422048: StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) (streamSerializer.cpp:210)
==7910==    by 0x40F713: oggLengthCmd(int, char**) (oggLength.cpp:93)
==7910==    by 0x40E412: main (oggLength.cpp:136)
==7910==  Address 0x5ae3898 is 24 bytes inside an unallocated block of size 3,938,144 in arena "client"
==7910== 
0
==7910== 
==7910== HEAP SUMMARY:
==7910==     in use at exit: 77,271 bytes in 17 blocks
==7910==   total heap usage: 394 allocs, 377 frees, 233,345 bytes allocated
==7910== 
==7910== LEAK SUMMARY:
==7910==    definitely lost: 208 bytes in 4 blocks
==7910==    indirectly lost: 4,359 bytes in 12 blocks
==7910==      possibly lost: 0 bytes in 0 blocks
==7910==    still reachable: 72,704 bytes in 1 blocks
==7910==         suppressed: 0 bytes in 0 blocks
==7910== Rerun with --leak-check=full to see details of leaked memory
==7910== 
==7910== For counts of detected and suppressed errors, rerun with: -v
==7910== ERROR SUMMARY: 11 errors from 11 contexts (suppressed: 0 from 0)
```

I use **AddressSanitizer** to build oggvideotools, this file can cause SEGV signal and heap-use-after-free error with the following command:

```shell
./oggLength oggLength_SEGV_heap-use-after-free
```

This is the ASAN information:

```
==7912==ERROR: AddressSanitizer: heap-use-after-free on address 0x60600000e9d0 at pc 0x00000046e493 bp 0x7ffcd8574380 sp 0x7ffcd8574370
WRITE of size 4 at 0x60600000e9d0 thread T0
    #0 0x46e492 in ExtractorInformation::operator=(ExtractorInformation const&) oggvideotools-0.9.1/src/base/streamExtractor.cpp:17
    #1 0x4407e2 in StreamConfig::operator=(StreamConfig const&) oggvideotools-0.9.1/src/base/streamConfig.h:12
    #2 0x43ea66 in StreamSerializer::getStreamConfig(std::vector<StreamConfig, std::allocator<StreamConfig> >&) oggvideotools-0.9.1/src/main/streamSerializer.cpp:210
    #3 0x43b0c4 in oggLengthCmd(int, char**) oggvideotools-0.9.1/src/binaries/oggLength.cpp:93
    #4 0x43b4e5 in main oggvideotools-0.9.1/src/binaries/oggLength.cpp:136
    #5 0x7f3e4c40982f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x43aa78 in _start (/home/wws/Music/Fuzz/target_prog_saddresssanitizer/target_oggvideotools/install/bin/oggLength+0x43aa78)

0x60600000e9d0 is located 16 bytes inside of 64-byte region [0x60600000e9c0,0x60600000ea00)
freed by thread T0 here:
    #0 0x7f3e4cde4b2a in operator delete(void*) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x99b2a)
    #1 0x46cf51 in __gnu_cxx::new_allocator<OggStreamDecoder::SegmentElement>::deallocate(OggStreamDecoder::SegmentElement*, unsigned long) /usr/include/c++/5/ext/new_allocator.h:110
    #2 0x46cd3b in std::allocator_traits<std::allocator<OggStreamDecoder::SegmentElement> >::deallocate(std::allocator<OggStreamDecoder::SegmentElement>&, OggStreamDecoder::SegmentElement*, unsigned long) /usr/include/c++/5/bits/alloc_traits.h:517
    #3 0x46c6bf in std::_Vector_base<OggStreamDecoder::SegmentElement, std::allocator<OggStreamDecoder::SegmentElement> >::_M_deallocate(OggStreamDecoder::SegmentElement*, unsigned long) /usr/include/c++/5/bits/stl_vector.h:178
    #4 0x46c0c5 in void std::vector<OggStreamDecoder::SegmentElement, std::allocator<OggStreamDecoder::SegmentElement> >::_M_emplace_back_aux<OggStreamDecoder::SegmentElement const&>(OggStreamDecoder::SegmentElement const&) /usr/include/c++/5/bits/vector.tcc:438
    #5 0x46babf in std::vector<OggStreamDecoder::SegmentElement, std::allocator<OggStreamDecoder::SegmentElement> >::push_back(OggStreamDecoder::SegmentElement const&) /usr/include/c++/5/bits/stl_vector.h:923
    #6 0x46a9dc in OggStreamDecoder::operator<<(std::shared_ptr<OggPageInternal>&) oggvideotools-0.9.1/src/base/oggStreamDecoder.cpp:120
    #7 0x43e4f8 in StreamSerializer::extractStreams() oggvideotools-0.9.1/src/main/streamSerializer.cpp:163
    #8 0x43da8f in StreamSerializer::open(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >&) oggvideotools-0.9.1/src/main/streamSerializer.cpp:75
    #9 0x43b044 in oggLengthCmd(int, char**) oggvideotools-0.9.1/src/binaries/oggLength.cpp:86
    #10 0x43b4e5 in main oggvideotools-0.9.1/src/binaries/oggLength.cpp:136
    #11 0x7f3e4c40982f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

previously allocated by thread T0 here:
    #0 0x7f3e4cde4532 in operator new(unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x99532)
    #1 0x46cfc2 in __gnu_cxx::new_allocator<OggStreamDecoder::SegmentElement>::allocate(unsigned long, void const*) /usr/include/c++/5/ext/new_allocator.h:104
    #2 0x46cd97 in std::allocator_traits<std::allocator<OggStreamDecoder::SegmentElement> >::allocate(std::allocator<OggStreamDecoder::SegmentElement>&, unsigned long) /usr/include/c++/5/bits/alloc_traits.h:491
    #3 0x46c8bb in std::_Vector_base<OggStreamDecoder::SegmentElement, std::allocator<OggStreamDecoder::SegmentElement> >::_M_allocate(unsigned long) /usr/include/c++/5/bits/stl_vector.h:170
    #4 0x46bf0f in void std::vector<OggStreamDecoder::SegmentElement, std::allocator<OggStreamDecoder::SegmentElement> >::_M_emplace_back_aux<OggStreamDecoder::SegmentElement const&>(OggStreamDecoder::SegmentElement const&) /usr/include/c++/5/bits/vector.tcc:412
    #5 0x46babf in std::vector<OggStreamDecoder::SegmentElement, std::allocator<OggStreamDecoder::SegmentElement> >::push_back(OggStreamDecoder::SegmentElement const&) /usr/include/c++/5/bits/stl_vector.h:923
    #6 0x46a9dc in OggStreamDecoder::operator<<(std::shared_ptr<OggPageInternal>&) oggvideotools-0.9.1/src/base/oggStreamDecoder.cpp:120
    #7 0x43e4f8 in StreamSerializer::extractStreams() oggvideotools-0.9.1/src/main/streamSerializer.cpp:163
    #8 0x43da8f in StreamSerializer::open(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >&) oggvideotools-0.9.1/src/main/streamSerializer.cpp:75
    #9 0x43b044 in oggLengthCmd(int, char**) oggvideotools-0.9.1/src/binaries/oggLength.cpp:86
    #10 0x43b4e5 in main oggvideotools-0.9.1/src/binaries/oggLength.cpp:136
    #11 0x7f3e4c40982f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-use-after-free oggvideotools-0.9.1/src/base/streamExtractor.cpp:17 ExtractorInformation::operator=(ExtractorInformation const&)
Shadow bytes around the buggy address:
  0x0c0c7fff9ce0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9cf0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9d00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9d10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c0c7fff9d20: fa fa fa fa fa fa fa fa fa fa fa fa 00 00 00 00
=>0x0c0c7fff9d30: 00 00 00 fa fa fa fa fa fd fd[fd]fd fd fd fd fd
  0x0c0c7fff9d40: fa fa fa fa 00 00 00 00 00 00 00 03 fa fa fa fa
  0x0c0c7fff9d50: fd fd fd fd fd fd fd fd fa fa fa fa 00 00 00 00
  0x0c0c7fff9d60: 00 00 00 02 fa fa fa fa fd fd fd fd fd fd fd fd
  0x0c0c7fff9d70: fa fa fa fa 00 00 00 00 00 00 07 fa fa fa fa fa
  0x0c0c7fff9d80: fd fd fd fd fd fd fd fa fa fa fa fa fd fd fd fd
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
==7912==ABORTING
```

