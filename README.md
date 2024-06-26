# Roman's scripts

This repository includes various tools that I find useful in my daily work as well as
some documentation around them.

## Pre-requisites

- [BCC Tools](https://github.com/iovisor/bcc)

### Installation Steps From Source

- have updated your system kernel (sudo apt update && sudo apt upgrade)

- purge the ubuntu-packed version of the package and its dependencies:  (sudo apt purge bpfcc-tools && sudo apt autoremove)

- install dependencies for compiling: https://github.com/iovisor/bcc/blob/master/INSTALL.md#install-build-dependencies-1

- compile from sources: https://github.com/iovisor/bcc/blob/master/INSTALL.md#install-and-compile-bcc-1

- call the tools with python3 /usr/share/bcc/tools/offcputime or python3 /usr/share/bcc/tools/opensnoop (alternative, install python-is-python3 package with sudo apt install python-is-python3)



## Running

CPU profiling:
```bash
# [sample-rate-hz] [duration-secs] [process-name] 
./perf-cpu 49 10 osmosisd
```

### Tracing
```bash
# [process-name]
./perf-trace osmosisd
```

### Off-CPU (WIP):
```bash
./perf-offcpu
```

### Page faults
```bash
# [process-name] [duration-secs]
./perf-page-fault osmosisd 15
```

**Focus**: Page fault profiling focuses on understanding and analyzing page faults in an application. A page fault occurs when a program tries to access a section of memory that is not currently in physical RAM but instead has been swapped out to disk or is yet to be loaded.

**Purpose**: The aim is to identify parts of the application that cause a high number of page faults, which can significantly degrade performance, especially if the disk is much slower than RAM.

#### Page Fault vs Memory Leak Profiling

**Memory Management**: Heap profiling is more directly concerned with memory management practices within the application, while page fault profiling deals with how the operating system's memory management interacts with the application's memory access patterns.

**Performance Optimization**: Both types of profiling are essential for performance optimization but target different types of performance issues. Heap profiling is used to reduce memory consumption and leaks, while page fault profiling is used to improve access patterns and reduce reliance on disk access for memory.

### Containers

#### Find Container PID by Host PID

```bash
./container-pid [host-pid]
```

#### Find Host PID by Container PID

```bash
./host-pid [container-pid]
```
#### Execute Command In Namespace

```bash
./nsenter [container-pid] [command]
```

## Performance Resources

- [Go Profiler Notes](https://github.com/DataDog/go-profiler-notes?tab=readme-ov-file)

### Debug Symbols

For profiling, debug symbols are necessary.


For go:
```bash
# Make sure this is removed
ldflags += -w -s

# Make sure this is present
-gcflags "all=-N -l"
```

### Other Useful Commands

```bash
# Trace newly started processes
/sbin/execsnoop-bpfcc

# Count system calls
/sbin/syscount-bpfcc

```