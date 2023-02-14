# focal-hello-package
debian packaging practice

all related files are build and shown in this repository

Prequisite

sudo apt install build-essential fakeroot devscripts git git-buildpackage debhelper debmake lintian pbuilder ubuntu-dev-tools

cp testing.sh /usr/bin


A. To get and modify package

A-1. download hello package from Debian offical package, -a means the ARCH option

        pull-lp-source -a i386 hello focal


A-2. create base.tgz corresponded to your unduntu distribution (focal as example here),
this step would create focal version of base.tgz under /var/cache/pbuilder/,
and /home/pbuilder/focal_result is created to locate build result

        sudo pbuilder create --distribution focal


A-3. put maintainner script in debian folder

        cp postinst hello-2.10/debian


A-4 to pack maintainer scripts into hello_2.10-2ubuntu2.debian.tar.xz , build result is based on it (no space between -k and "your_gpg_key")

        dpkg-buildpackage -k"your_gpg_key"  -S -sa -rfakeroot
        
        (generate gpg key according reference: https://help.ubuntu.com/community/GnuPrivacyGuardHowto)
       
using gpg ket to sign is for uploading these building result to ppa , if the result is for local test , signing is not required , use command as 
        
follows:
        
        dpkg-buildpackage -uc -us


A-5. cd to the hello-2.10 folder then build, use gernerated base.tgz as base and the result is gererated at specified location, .deb file in focal_result/ folder

        sudo pbuilder build --basetgz /var/cache/pbuilder/base.tgz --buildresult /home/allen/pbuilder/focal_result/ ../hello_2.10-2ubuntu2.dsc




B. If maintainer scipt is not included in created .deb , inclde it manually in focal_result folder

        mkdir mydeb

        cd mydeb

        mkdir mydeb/DEBIAN

        cd ../../

        dpkg -x hello_2.10-2ubuntu2_amd64.deb mydeb

        dpkg -e hello_2.10-2ubuntu2_amd64.deb mydeb/DEBIAN

        chmod 555 postinst

        cp postinst mydeb/DEBIAN

        dpkg -b mydeb/ test20230212.deb
