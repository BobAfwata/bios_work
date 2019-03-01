Bios work on Tianocore ,uefi,qemu and other tweaks
=================================================


Install required packages 
=========================

sudo apt-get install asciinema

sudo apt-get install qemu binutils-mingw-w64 gcc-mingw-w64 xorriso mtools
wget http://www.tysos.org/files/efi/mkgpt-latest.tar.bz2
tar jxf mkgpt-latest.tar.bz2
cd mkgpt && ./configure && make && sudo make install && cd ..

Run the example test to confirm everything is ok

qemu-system-x86_64 -L output/ -bios OVMF.fd







Tiaonocore Implementation of  UEFI
==================================
1)Compiling / building Tianocore and reuuning it in qemu
2)Compiling the fd file and using it as a payload in coreboot 
3)Building a final iamge for intel quark SoC ,Galileo gen2 ,Intel Atom ,Beaglebone black and rasberry pi
4)Coreboot for Intel Atom ,Minnowmax board ,Upsqaured board ,etc
5)OpenCellular - Rotundu and Elgon
6)Open Compute Project firmware


Install required Packages 
=======================
sudo apt-get install build-essential git uuid-dev iasl nasm git
sudo apt-get install iasl

Clone edk2 repo online 

mkdir bios_work && cd bios_work

git clone git://github.com/tianocore/edk2.git
cd edk2
source ./edksetup.sh #source the environmental settings ,tools etc 
make -C BaseTools/  #build the base tools required



Build the firmware
build -a X64 -t GCC48 -b RELEASE -p OvmfPkg/OvmfPkgX64.dsc

Or use a settings file 
======================
edit the target file under Conf/target.txt for 64 bit using gcc4.8

sed -i -e 's/^TARGET_ARCH.*/TARGET_ARCH = X64/' Conf/target.txt
sed -i -e 's/^TOOL_CHAIN_TAG/TOOL_CHAIN_TAG = GCC48/' Conf/target.txt
sed -i -e 's/^ACTIVE_PLATFORM.*/ACTIVE_PLATFORM = OvmfPkg\/OvmfPkgX64.dsc/' Conf/target.txt

Finally build the firmware 
==========================
$ build


locate the firmware image 
find Build -name OVMF.fd

Build/OvmfX64/RELEASE_GCC48/FV/OVMF.fd

Virtualize in qemu
===================
Install qemu and other virtualization software
and Run the firmware
$ sudo apt-get install qemu-kvm qemu libvirt-bin virt-manager virtinst bridge-utils cpu-checker virt-viewer
$ qemu-system-x86_64 -bios Build/OvmfX64/RELEASE_GCC48/FV/OVMF.fd