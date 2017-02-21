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

ubifs debugging
================
http://www.linux-mtd.infradead.org/faq/ubi.html#L_how_debug
http://www.linux-mtd.infradead.org/faq/ubifs.html#L_debug_msgs
http://linux-mtd.infradead.narkive.com/2jtVsnxT/ubifs-error

misc tests:
===========

#insmod mtd_torturetest.ko dev=3 check=1 cycles_count=1 eb=0 ebcnt=4 gran=1 


while(1)  
{  
create and write a random image(with different block size using dd  
command) to nand flash  
  
if "disk space is reached maxim size"  
delete all the files  
  
}  



	#!/bin/sh

	sizecount=0
	localcount=0

	deletelimit=`expr 500 \* 1024 \* 1024` # 500 MB
	FILENAME="1 2 3 4 5 6 7 8 9 10"

	rm -rf /ubifs_rootfs/*

	while [ 1 ]
	do

	for filename in $FILENAME
	do
	size=`expr 4096 \* 10`

	dd if=/dev/urandom of=/ubifs_rootfs/foo1${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo2${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo3${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo4${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo5${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo6${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo7${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo8${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo9${filename} bs=1024
	count=4 2> /dev/null ;
	dd if=/dev/urandom of=/ubifs_rootfs/foo10${filename} bs=1024
	ount=4 2> /dev/null ;
	
	ocalcount=`expr $localcount + $size`
	izecount=`expr $sizecount + $size`
	one
	
	f [ $localcount -gt $deletelimit ] ; then
	cho "Clean up!!!"
	ocalcount=0
	m -rf /ubifs_rootf/* ; sync
	i

	date ; sync
	echo "Bytes Written to Nand flash:`expr $sizecount / 1048576 ` MB"
	
	done


 
