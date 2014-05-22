Linux modprobe Command Examples to View, Install, Remove Modules

modprobe utility is used to add loadable modules to the Linux kernel. You can also view and remove modules using modprobe command.

Linux maintains /lib/modules/$(uname-r) directory for modules and its configuration files (except /etc/modprobe.conf and /etc/modprobe.d).

In Linux kernel 2.6, the .ko modules are used instead of .o files since that has additional information that the kernel uses to load the modules. The example in this article are done with using modprobe on Ubuntu.

1. List Available Kernel Modules

modprobe -l will display all available modules as shown below.


```

$ modprobe -l | less

```

The Output will be like this

```

kernel/drivers/ata/pata_artop.ko
kernel/drivers/ata/pata_atiixp.ko
kernel/drivers/ata/pata_atp867x.ko
kernel/drivers/ata/pata_cmd64x.ko
kernel/drivers/ata/pata_cs5520.ko
kernel/drivers/ata/pata_cs5530.ko
kernel/drivers/ata/pata_cs5536.ko
kernel/drivers/ata/pata_cypress.ko
kernel/drivers/ata/pata_efar.ko

```

2. List Currently Loaded Modules

While the above modprobe command shows all available modules, lsmod command will display all modules that are currently loaded in the Linux kernel.


```

$ lsmod | less

```

The Output will be like this

```

Module                  Size  Used by
nls_utf8               12557  1 
udf                    94317  1 
crc_itu_t              12707  1 udf
nf_nat                 25891  2 ipt_MASQUERADE,iptable_nat
nf_conntrack_ipv4      19716  3 iptable_nat,nf_nat
nf_defrag_ipv4         12729  1 nf_conntrack_ipv4

```

3. Install New modules into Linux Kernel

In order to insert a new module into the kernel, execute the modprobe command with the module name.

Following example loads vmhgfs module to Linux kernel on Ubuntu.

```

$ sudo modprobe vmhgfs

```

Once a module is loaded, verify it using lsmod command as shown below.

```

$ lsmod | grep vmhgfs
vmhgfs                 50772  0

```

The module files are with .ko extension. If you like to know the full file location of a specific Linux kernel module, use modprobe command and do a grep of the module name as shown below.


```

$ modprobe | grep vmhgfs
misc/vmhgfs.ko

$ cd /lib/modules/2.6.31-14-generic/misc

$ ls vmhgfs*
vmhgfs.ko

```

> Note: You can also use insmod for installing new modules into the Linux kernel.



4. Load New Modules with the Different Name to Avoid Conflicts

Consider, in some cases you are supposed to load a new module but with the same module name another module got already loaded for different purposes.

If for some strange reasons, the module name you are trying to load into the kernel is getting used (with the same name) by a different module, then you can load the new module using a different name.

To load a module with a different name, use the modprobe option -o as shown below.

```

$ sudo modprobe vmhgfs -o vm_hgfs

$ lsmod  | grep vm_hgfs
vm_hgfs                   50772  0

```

5. Remove the Currently Loaded Module

If you’ve loaded a module to Linux kernel for some testing purpose, you might want to unload (remove) it from the kernel.

Use modprobe -r option to unload a module from the kernel as shown below.

```

modprobe -r vmhgfs

```


***
