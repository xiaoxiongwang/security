# Memory leaks detected

Tested in Ubuntu 1６.04, 64bit

I use the following command with the [file](lupng_memory_leaks_1):

```shell
./lupng_test lupng_memory_leaks_1 test.png
```

and get:

```
PNG: read error
```

I use **AddressSanitizer** to build Lupng and get memory leaking error with the below command:

```shell
./lupng_test lupng_memory_leaks_1 test.png
```

This is the ASAN information:

```
lupng_test lupng_memory_leaks_1 test.png
PNG: read error

=================================================================
==1027==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 1701 byte(s) in 81 object(s) allocated from:
    #0 0x7fa30721e602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x401cb0 in internalMalloc (lupng_test+0x401cb0)
    #2 0x404392 in parsePlte (lupng_test+0x404392)
    #3 0x407687 in handleChunk (lupng_test+0x407687)
    #4 0x407a40 in luPngReadUC (lupng_test+0x407a40)
    #5 0x407ff3 in luPngReadFile (lupng_test+0x407ff3)
    #6 0x401728 in main (lupng_test+0x401728)
    #7 0x7fa306ddc82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: 1701 byte(s) leaked in 81 allocation(s).
```

Using the same command but different test file, I can also get the the memory leaks error.

With another [file](lupng_memory_leaks_2), I can get the below result:

```
PNG: invalid chunk name, possibly unprintable
```

and the ASAN information:

```
PNG: invalid chunk name, possibly unprintable

=================================================================
==1099==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 32 byte(s) in 1 object(s) allocated from:
    #0 0x7f2bcade4602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x401cb0 in internalMalloc (lupng_test+0x401cb0)
    #2 0x406b67 in readChunk (lupng_test+0x406b67)
    #3 0x407a7b in luPngReadUC (lupng_test+0x407a7b)
    #4 0x407ff3 in luPngReadFile (lupng_test+0x407ff3)
    #5 0x401728 in main (lupng_test+0x401728)
    #6 0x7f2bca9a282f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

Indirect leak of 158353501 byte(s) in 1 object(s) allocated from:
    #0 0x7f2bcade4602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x401cb0 in internalMalloc (lupng_test+0x401cb0)
    #2 0x406dd1 in readChunk (lupng_test+0x406dd1)
    #3 0x407a7b in luPngReadUC (lupng_test+0x407a7b)
    #4 0x407ff3 in luPngReadFile (lupng_test+0x407ff3)
    #5 0x401728 in main (lupng_test+0x401728)
    #6 0x7f2bca9a282f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: 158353533 byte(s) leaked in 2 allocation(s).
```

