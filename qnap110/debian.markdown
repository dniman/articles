
##### download the debian installer images

    cd /tmp
    busybox wget http://ftp.debian.org/debian/dists/stable/main/installer-armel/current/images/kirkwood/network-console/qnap/ts-119/initrd.gz
    busybox wget http://ftp.debian.org/debian/dists/stable/main/installer-armel/current/images/kirkwood/network-console/qnap/ts-119/kernel
    busybox wget http://ftp.debian.org/debian/dists/stable/main/installer-armel/current/images/kirkwood/network-console/qnap/ts-119/flash-debian
    busybox wget http://ftp.debian.org/debian/dists/stable/main/installer-armel/current/images/kirkwood/network-console/qnap/ts-119/model
  
##### install debian

1) connect to the machine via SSH

    username: admin
    password: admin
    
2) save the content of qnap flash partitions to a USB stick

    mount | grep external
    /dev/sdi1 on /share/external/sdi type vfat [...]

    cd /share/external/sdi
    cat /dev/mtdblock0 > mtd0
    cat /dev/mtdblock1 > mtd1
    cat /dev/mtdblock2 > mtd2
    cat /dev/mtdblock3 > mtd3
    cat /dev/mtdblock4 > mtd4
    cat /dev/mtdblock5 > mtd5
    cd
    umount /share/external/sdi
  
  add the mtdX files to your regular backup

3) run the script by executing the following command:

    sh flash-debian

    Updating MAC address...
    Your MAC address is 00:08:9B:8C:xx:xx
    Writing debian-installer to flash... done.
    Please reboot your QNAP device.  
  
4) reboot after the command has completed    

    reboot
    exit

5) connect to the machine via SSH

    username: installer
    password: install

  debian installer will start
    
##### reinstall debian

  connect to the machine
 
    cat kernel > /dev/mtdblock1
    cat initrd.gz > /dev/mtdblock2
    
  debian installer will start  
