# Memory leaks detected 

Tested in Ubuntu 16.04, 64bit

I use the following command:

```shell
./wavegain wavegain_memory_gain.wav
```

and get:

```
Warning: INVALID format chunk in wav header.
 Trying to read anyway (may not work)...
*** buffer overflow detected ***: ./wavegain terminated
======= Backtrace: =========
/lib/x86_64-linux-gnu/libc.so.6(+0x777e5)[0x7f090b3a57e5]
/lib/x86_64-linux-gnu/libc.so.6(__fortify_fail+0x5c)[0x7f090b44715c]
/lib/x86_64-linux-gnu/libc.so.6(+0x117160)[0x7f090b445160]
/lib/x86_64-linux-gnu/libc.so.6(__fread_chk+0x165)[0x7f090b445855]
./wavegain[0x40f797]
./wavegain[0x410ee0]
./wavegain[0x41e20d]
./wavegain[0x41c660]
./wavegain[0x40374e]
/lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xf0)[0x7f090b34e830]
./wavegain[0x403ca9]
======= Memory map: ========
00400000-0042a000 r-xp 00000000 103:01 20397804                          wavegain/wavegain
00629000-0062a000 r--p 00029000 103:01 20397804                          wavegain/wavegain
0062a000-0062b000 rw-p 0002a000 103:01 20397804                          wavegain/wavegain
0062b000-00668000 rw-p 00000000 00:00 0 
00a1c000-00a3d000 rw-p 00000000 00:00 0                                  [heap]
7f090b118000-7f090b12e000 r-xp 00000000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f090b12e000-7f090b32d000 ---p 00016000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f090b32d000-7f090b32e000 rw-p 00015000 103:01 10490464                  /lib/x86_64-linux-gnu/libgcc_s.so.1
7f090b32e000-7f090b4ee000 r-xp 00000000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f090b4ee000-7f090b6ee000 ---p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f090b6ee000-7f090b6f2000 r--p 001c0000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f090b6f2000-7f090b6f4000 rw-p 001c4000 103:01 10487655                  /lib/x86_64-linux-gnu/libc-2.23.so
7f090b6f4000-7f090b6f8000 rw-p 00000000 00:00 0 
7f090b6f8000-7f090b800000 r-xp 00000000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f090b800000-7f090b9ff000 ---p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f090b9ff000-7f090ba00000 r--p 00107000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f090ba00000-7f090ba01000 rw-p 00108000 103:01 10487658                  /lib/x86_64-linux-gnu/libm-2.23.so
7f090ba01000-7f090ba27000 r-xp 00000000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f090bbf5000-7f090bbf9000 rw-p 00000000 00:00 0 
7f090bc25000-7f090bc26000 rw-p 00000000 00:00 0 
7f090bc26000-7f090bc27000 r--p 00025000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f090bc27000-7f090bc28000 rw-p 00026000 103:01 10485855                  /lib/x86_64-linux-gnu/ld-2.23.so
7f090bc28000-7f090bc29000 rw-p 00000000 00:00 0 
7ffde0294000-7ffde02b6000 rw-p 00000000 00:00 0                          [stack]
7ffde0339000-7ffde033c000 r--p 00000000 00:00 0                          [vvar]
7ffde033c000-7ffde033e000 r-xp 00000000 00:00 0                          [vdso]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]
Aborted
```

I use **AddressSanitizer** to build wavegain, this file can memory leak with the following command:

```shell
./wavegain wavegain_memory_gain.wav
```

This is the ASAN information:

```
Warning: INVALID format chunk in wav header.
 Trying to read anyway (may not work)...
Warning: Unexpected EOF in reading WAV header
 Unrecognized file format for wavegain_memory_gain.wav

 WaveGain Processing completed normally

=================================================================
==3874==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 92 byte(s) in 1 object(s) allocated from:
    #0 0x7f6ca685030f in strdup (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x6230f)
    #1 0x4111bc in alloc_node wavegain/main.c:70
    #2 0x4113d3 in add_to_list wavegain/main.c:104
    #3 0x403d7e in process_argument wavegain/recurse.c:583
    #4 0x4144f1 in main wavegain/main.c:718
    #5 0x7f6ca613b82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

Direct leak of 40 byte(s) in 1 object(s) allocated from:
    #0 0x7f6ca6886602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x4097e8 in wav_open wavegain/audio.c:646
    #2 0x407832 in open_audio_file wavegain/audio.c:362
    #3 0x414dec in get_gain wavegain/wavegain.c:181
    #4 0x41198c in process_files wavegain/main.c:197
    #5 0x41453e in main wavegain/main.c:729
    #6 0x7f6ca613b82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: 132 byte(s) leaked in 2 allocation(s).
```

# Floating Point Exception

Tested in Ubuntu 16.04, 64bit

I use the following command:

```shell
./wavegain wavegain_floting_point_exception.wav
```

and get:

```
Floating point exception
```

I use **gdb** to analysis the bug and get the below information:

```
gdb-peda$ set args wavegain_floting_point_exception.wav
gdb-peda$ r
Starting program: wavegain wavegain_floting_point_exception.wav
[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib/x86_64-linux-gnu/libthread_db.so.1".

Program received signal SIGFPE, Arithmetic exception.

[----------------------------------registers-----------------------------------]
RAX: 0x5e00e84b 
RBX: 0x7fffffffd4e0 --> 0x7fffffffd700 --> 0x7fffffffd9b0 --> 0x0 
RCX: 0x60c00000bf00 --> 0x0 
RDX: 0x0 
RSI: 0x3 
RDI: 0x0 
RBP: 0x7fffffffd500 --> 0x7fffffffd540 --> 0x7fffffffd630 --> 0x7fffffffd720 --> 0x7fffffffd9d0 --> 0x4197d0 (<__libc_csu_init>:	push   r15)
RSP: 0x7fffffffd340 --> 0xc00000002 --> 0x0 
RIP: 0x40a5cc (<wav_open+3809>:	idiv   rdi)
R8 : 0x61600000fd60 --> 0x0 
R9 : 0x7fffffffd2d0 --> 0x5e00e84b61746164 
R10: 0x7ffff7fc5780 (0x00007ffff7fc5780)
R11: 0x7ffff692bf90 --> 0xfffda370fffda09f 
R12: 0xffffffffa70 --> 0x0 
R13: 0x7fffffffd380 --> 0x41b58ab3 
R14: 0x7fffffffd380 --> 0x41b58ab3 
R15: 0x0
EFLAGS: 0x10206 (carry PARITY adjust zero sign trap INTERRUPT direction overflow)
[-------------------------------------code-------------------------------------]
   0x40a5c0 <wav_open+3797>:	imul   edx,DWORD PTR [rbp-0x19c]
   0x40a5c7 <wav_open+3804>:	movsxd rdi,edx
   0x40a5ca <wav_open+3807>:	cqo    
=> 0x40a5cc <wav_open+3809>:	idiv   rdi
   0x40a5cf <wav_open+3812>:	mov    rcx,rax
   0x40a5d2 <wav_open+3815>:	mov    rax,QWORD PTR [rbp-0x1b0]
   0x40a5d9 <wav_open+3822>:	add    rax,0x10
   0x40a5dd <wav_open+3826>:	mov    rdx,rax
[------------------------------------stack-------------------------------------]
0000| 0x7fffffffd340 --> 0xc00000002 --> 0x0 
0008| 0x7fffffffd348 --> 0x60200000eff0 --> 0xff042546464952 
0016| 0x7fffffffd350 --> 0x60c00000bf80 --> 0x40a8be (<wav_read>:	push   rbp)
0024| 0x7fffffffd358 --> 0x61600000fc80 --> 0xbebebebefbad2488 
0032| 0x7fffffffd360 --> 0x100000000 --> 0x0 
0040| 0x7fffffffd368 --> 0x60400000dfd0 --> 0xbebebebe00080000 
0048| 0x7fffffffd370 --> 0x24 ('$')
0056| 0x7fffffffd378 --> 0x0 
[------------------------------------------------------------------------------]
Legend: code, data, rodata, value
Stopped reason: SIGFPE
0x000000000040a5cc in wav_open (in=0x61600000fc80, opt=0x60c00000bf80, oldbuf=0x60200000eff0 "RIFF%\004\377", buflen=0xc) at audio.c:790
790				opt->total_samples_per_channel = len/(format.channels*samplesize);
gdb-peda$ bt
#0  0x000000000040a5cc in wav_open (in=0x61600000fc80, opt=0x60c00000bf80, oldbuf=0x60200000eff0 "RIFF%\004\377", buflen=0xc) at audio.c:790
#1  0x0000000000407833 in open_audio_file (in=0x61600000fc80, opt=0x60c00000bf80) at audio.c:362
#2  0x0000000000414ded in get_gain (filename=0x60700000df40 "wavegain_floting_point_exception.wav", track_peak=0x60600000efd8, track_gain=0x60600000efd0, dc_offset=0x60600000efe0, 
    offset=0x60600000eff0, settings=0x7fffffffd8f0) at wavegain.c:181
#3  0x000000000041198d in process_files (file_list=0x60600000efc0, settings=0x7fffffffd8f0, dir=0x41f1a0 ".") at main.c:197
#4  0x000000000041453f in main (argc=0x2, argv=0x7fffffffdab8) at main.c:729
#5  0x00007ffff67b7830 in __libc_start_main (main=0x4139ca <main>, argc=0x2, argv=0x7fffffffdab8, init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>, stack_end=0x7fffffffdaa8)
    at ../csu/libc-start.c:291
#6  0x0000000000401ce9 in _start ()
```


