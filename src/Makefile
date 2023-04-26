#
#  Makefile for Gasket framework and dependent drivers.
#

obj-m += gasket.o
obj-m += apex.o

gasket-objs	:= gasket_core.o gasket_ioctl.o gasket_interrupt.o gasket_page_table.o gasket_sysfs.o
apex-objs	:= apex_driver.o

ifndef KVER
KVER=$(shell uname -r)
endif

ifndef KMODVER
KMODVER=$(shell git describe HEAD 2>/dev/null || git rev-parse --short HEAD)
endif


#all:
#	$(MAKE) -C /lib/modules/$(KVER)/build M=$(PWD) modules

#clean:
#	$(MAKE) -C /lib/modules/$(KVER)/build M=$(PWD) clean

buildprep:
	# elfutils-libelf-devel is needed on EL8 systems
	sudo yum install -y gcc kernel-{core,devel,modules}-$(KVER) elfutils-libelf-devel
all:
	make -C /lib/modules/$(KVER)/build M=$(PWD) EXTRA_CFLAGS=-DKMODVER=\\\"$(KMODVER)\\\" modules
#	gcc -o spkut ./simple-procfs-kmod-userspace-tool.c
clean:
	make -C /lib/modules/$(KVER)/build M=$(PWD) clean
#	rm -f spkut
install:
#	sudo install -v -m 755 spkut /bin/
	sudo install -v -m 755 -d /lib/modules/$(KVER)/
	sudo install -v -m 644 apex.ko        /lib/modules/$(KVER)/apex.ko
	sudo install -v -m 644 gasket.ko /lib/modules/$(KVER)/gasket.ko
	sudo depmod -a
