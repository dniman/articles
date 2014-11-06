
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


##### after installation

    #  install midnight commander
    sudo apt-get install mc 
    
    # install git
    sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev
    
    cd /tmp
    wget http://[git_version].tar.gz
    tar -zxf [git_version].tar.gz
    cd [git_version]
    make prefix=/usr/local all
    sudo make prefix=/usr/local install
    
    # install node.js
    sudo apt-get install build-essential python libssl-dev
    
    cd /tmp
    wget http://[node_version].tar.gz
    tar -xvf [node_version].tar.gz
    cd [node_version]
    
    create build.sh file
    #!/bin/sh
    export CFLAGS='-march=armv5t'
    export CXXFLAGS='-march=armv5t'
    ./configure
    make install
    
    chmod +x build.sh
    sudo ./build.sh
    
    # install rbenv
    git clone git://github.com/sstephenson/rbenv.git .rbenv
    
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    
    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    
    # install bower
    sudo npm install -g bower
    
    # install sqlite3
    sudo apt-get install libsqlite3-dev
    sudo apt-get install sqlite3 # without it rails dbconsole will not work
    
    # install ruby
    rbenv install [ruby_version]
    rbenv global [ruby_version]
    
    # install gem bundler
    gem install bundler
    
    

