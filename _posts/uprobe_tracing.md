

# Uprobe based tracing


# Requirements
## Kernel
CONFIG_UPROBE_EVENTS

## Runtime
Mount debugfs on a runnning system

# Usage

## Default format of command
[CMD][:[GRP/]EVENT] PATH:OFFSET [FETCHARGS]

to

/sys/kernel/debug/tracing/uprobe_events


echo 1 > /sys/kernel/debug/tracing/events/uprobes/enable
echo 0 > /sys/kernel/debug/tracing/events/uprobes/enable
cat /sys/kernel/debug/tracing/trace



```bash
$ objdump -tT /bin/bash | grep main
...
000000000041eed0 g    DF .text	000000000000168e  Base        main
...
```

```bash
readelf -e /bin/bash              
...
Program Headers:
  Type           Offset             VirtAddr           PhysAddr
                 FileSiz            MemSiz              Flags  Align
  PHDR           0x0000000000000040 0x0000000000400040 0x0000000000400040
                 0x00000000000001f8 0x00000000000001f8  R E    8
  INTERP         0x0000000000000238 0x0000000000400238 0x0000000000400238
                 0x000000000000001c 0x000000000000001c  R      1
      [Requesting program interpreter: /lib64/ld-linux-x86-64.so.2]
  LOAD           0x0000000000000000 0x0000000000400000 0x0000000000400000
                 0x00000000000f3cac 0x00000000000f3cac  R E    200000
  LOAD           0x00000000000f3df0 0x00000000006f3df0 0x00000000006f3df0
...
```

0x41eed0 - 0x400000 = 0x1eed0

```bash
# echo "p:uprobe_main /bin/bash:0x1eed0" > uprobe_events 
# echo "uprobe_main" > set_event

# echo "1" > tracing_on 
# bash # In another shell
# echo "0" > tracing_on
# cat trace
# tracer: nop
#
#                              _-----=> irqs-off
#                             / _----=> need-resched
#                            | / _---=> hardirq/softirq
#                            || / _--=> preempt-depth
#                            ||| /     delay
#           TASK-PID   CPU#  ||||    TIMESTAMP  FUNCTION
#              | |       |   ||||       |         |
            bash-30678 [000] d... 2233525.915372: uprobe_main: (0x41eed0)
        lesspipe-30680 [002] d... 2233525.921635: uprobe_main: (0x41eed0)
```bash




# Reference
[Official documentation](https://www.kernel.org/doc/Documentation/trace/uprobetracer.txt)
