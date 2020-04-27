# kmemleak 101


This docuemnt explains kmemleak, which is memory leak detection tool for linux.

# What is kmemleak
TBD

# Configuration
CONFIG_DEBUG_KMEMLEAK
CONFIG_DEBUG_KMEMLEAK_EARLY_LOG_SIZE  = 4096

Pass cmdline below if knob it not shown
kmemleak=on


## Usage
1. Mount debugfs
mount -t debugfs none /sys/kernel/debug

2. Scan memory leak
echo scan > /sys/kernel/debug/kmemleak

3. Reset
echo clear > /sys/kernel/debug/kmemleak



## Ouput example

```
unreferenced object 0x9eac6dc0 (size 64):
  comm "dd", pid 1545, jiffies 4294952617 (age 854.280s)
  hex dump (first 32 bytes):
    80 67 ac 9e 00 00 00 00 02 00 00 00 12 00 00 00  .g..............
    00 00 00 00 10 b5 a9 9e 98 51 06 9f 10 b5 a9 9e  .........Q......
  backtrace:
    [<7f0023e4>] 0x7f0023e4
    [<805682ac>] __map_bio+0xa0/0x218
    [<80569a8c>] __split_and_process_bio+0x234/0x3f8
    [<80569f90>] dm_make_request+0x5c/0xb8
    [<80387d18>] generic_make_request+0xd8/0x224
    [<80387ee4>] submit_bio+0x80/0x190
    [<801ff6d8>] submit_bh_wbc+0x140/0x178
    [<8020148c>] __block_write_full_page.constprop.17+0x1ec/0x3f4
    [<801780ac>] __writepage+0x14/0x40
    [<80178be8>] write_cache_pages+0x188/0x4a0
    [<80178f44>] generic_writepages+0x44/0x5c
    [<8016e818>] __filemap_fdatawrite_range+0xb4/0xf0
    [<8016e8ec>] filemap_write_and_wait+0x38/0x64
    [<80202658>] __blkdev_put+0x7c/0x308
    [<80202df4>] blkdev_close+0x18/0x20
    [<801cc83c>] __fput+0x80/0x1c8
```

