# focal-hello-package
debian packaging practice

Prequisite

sudo apt install build-essential fakeroot devscripts git git-buildpackage debhelper debmake lintian pbuilder

sudo apt install ubuntu-dev-tools




A. To get and modify package

A-1. download hello package from Debian offical packge

pull-lp-source -a i386 hello focal


A-2. create base.tgz is corresponded to your unduntu distribution (focal as example here)
this step would create focal version of base.tgz under /var/cache/pbuilder/
and build result is at /home/pbuilder/focal_result

sudo pbuilder create --distribution focal



A-3. put maintainner script in debian folder

cp postinst hello-2.10/debian


A-4 to pack maintainer scripts into hello_2.10-2ubuntu2.debian.tar.xz

dpkg-buildpackage --no-sign  -S -sa -rfakeroot


A-5. cd to the hello folder then build, use gernerated base.tgz as base and the result is gererated at specified location

sudo pbuilder build --basetgz /var/cache/pbuilder/base.tgz --buildresult /home/allen/pbuilder/focal_result/ ../hello_2/hello_2.10-2ubuntu2.dsc




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
