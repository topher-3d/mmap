Interface for mmap syscall to provide safe and efficient access to memory.
`*mmap.File` satisfies both `io.ReaderAt` and `io.WriterAt` interfaces.

**Only works for darwin OS, Linux and Little Endian 64 bit architectures.**

## Fork of...

I forked this from [elamre/mmap](https://github.com/elamre/mmap), which was a fork
of [grandecola/mmap](https://github.com/grandecola/mmap). I am using this to write
to 32-bit AXI-Lite registers via mmap of /dev/mem on a Xilinx Zynq-based system. I
need the "register" writes to be 32-bits in one operation.

## Safety & Efficiency
Golang mmap syscall function exposes the mapped memory as array of bytes.
If the array is referenced even after the memory region is unmapped,
this can lead to segmentation fault. `mmap package` provides safe access
to the array of bytes by providing `ReadAt` and `WriteAt` functions.

`WriteAt` function copies a slice into the memory mapped region
whereas `ReadAt` function copies data from memory mapped region to
a given slice, therefore, avoiding exposing the array of bytes referring
to mapped memory. This also avoids any extra data copy providing efficient
access to the memory mapped region.

We have also added functions such as `WriteUint64At`, `ReadUint64At` that
can directly typecast the mmaped memory to Uint64 and avoids an extra copy.
We will add more functions in the library based on our use cases. If you need
support for a particular function, let us know or better, raise a pull request.

## Similar Packages
* golang.org/x/exp/mmap
* github.com/riobard/go-mmap
* launchpad.net/gommap
* github.com/edsrzf/mmap-go
