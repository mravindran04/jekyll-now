stress testing for ubi and mtd

For slc nand testing, the mtd_tests can be used,
=================================================
1. mtd-utils/tests/fs-tests

2. LTP's fsstress

3. The MTD test-suite contains the following tests:

    mtd_speedtest: measures and reports read/write/erase speed of the MTD device.  
    mtd_stresstest: performs random read/write/erase operations and validates the MTD device I/O capabilities.  
    mtd_readtest: this tests reads whole MTD device, one NAND page at a time including OOB (or 512 bytes at a time in case of flashes like NOR) and checks that reading works properly.  
    mtd_pagetest: relevant only for NAND flashes, tests NAND page writing and reading in different sizes and order; this test was originally developed for testing the OneNAND driver, so it might be a little OneNAND-oriented, but must work on any NAND flash.  
    mtd_oobtest: relevant only for NAND flashes, tests that the OOB area I/O works properly by writing data to different offsets and verifying it.  
    mtd_subpagetest: relevant only for NAND flashes, tests sub-page I/O.  
    mtd_torturetest: this test is designed to wear out flash eraseblocks. It repeatedly writes and erases the same group of eraseblocks until an I/O error happens, so be careful! The test supports a number of options (see modinfo mtd_torturetest) which allow you to set the amount of eraseblocks to torture and how the torturing is done. You may limit the amount of torturing cycles using the cycles_count module parameter. It may be very god idea to run this test for some time and validate your flash driver and HW, providing you have a spare device. For example, we caught rather rare and nasty DMA issues on an OMAP2 board with OneNAND flash, just by running this tests for few hours.  
    mtd_nandecctest: a simple test that checks correctness of the built-in software ECC for 256 and 512-byte buffers; this test is not driver-specific but tests general NAND support code.  

4. simulate power-off-recovery by using UBIFS "failure mode". Set UBIFS debugging module parameter debug_tsts to 4.  There is a script I have used for that below.


5. sysfs interface for ubi in debugfs

root@renault:/sys/kernel/debug/ubi/ubi0# ls -al  
drwxr-xr-x    2 root     root             0 Jul 31  2015 .  
drwxr-xr-x    3 root     root             0 Jan  1  1970 ..  
--w-------    1 root     root             0 Jul 31  2015 chk_fastmap  
--w-------    1 root     root             0 Jul 31  2015 chk_gen  
--w-------    1 root     root             0 Jul 31  2015 chk_io  
--w-------    1 root     root             0 Jul 31  2015 tst_disable_bgt  
--w-------    1 root     root             0 Jul 31  2015 tst_emulate_bitflips  
--w-------    1 root     root             0 Jul 31  2015 tst_emulate_io_failures  
--w-------    1 root     root             0 Jul 31  2015 tst_emulate_power_cut  
--w-------    1 root     root             0 Jul 31  2015 tst_emulate_power_cut_max  
--w-------    1 root     root             0 Jul 31  2015 tst_emulate_power_cut_min  


6. also check for 'nandtest' and 'memteser' commandline tools  

fyi:  
====
https://mytechrants.wordpress.com/2010/01/20/ubiubifs-on-nandsim/  
http://linux-mtd.infradead.narkive.com/ZWVdFwHz/good-stress-test-for-ubifs  
http://www.aleph1.co.uk/lurker/message/20080516.114655.2a368241.en.html  
http://labs.isee.biz/index.php/Board_validation_and_diagnostic_tools#.23001.C2.A0:_Simple_read.2Fwrite_test  

