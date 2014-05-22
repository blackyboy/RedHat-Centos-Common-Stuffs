Troubleshooting Using dmesg Command in Unix and Linux




During system bootup process, kernel gets loaded into the memory and it controls the entire system.

When the system boots up, it prints number of messages on the screen that displays information about the hardware devices that the kernel detects during boot process.

These messages are available in kernel ring buffer and whenever the new message comes the old message gets overwritten. You could see all those messages after the system bootup using the dmesg command.


1. View the Boot Messages

By executing the dmesg command, you can view the hardwares that are detected during bootup process and it’s configuration details. There are lot of useful information displayed in dmesg. Just browse through them line by line and try to understand what it means. Once you have an idea of the kind of messages it displays, you might find it helpful for troubleshooting, when you encounter an issue.


```

# dmesg | more


```


The Out put Will be like this 


```

[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.000000]   AMD AuthenticAMD
[    0.000000]   Centaur CentaurHauls
[    0.000000] BIOS-provided physical RAM map:
[    0.000000]  BIOS-e820: 0000000000000000 - 000000000009d000 (usable)
[    0.000000]  BIOS-e820: 000000000009d000 - 00000000000a0000 (reserved)
[    0.000000]  BIOS-e820: 00000000000ce000 - 00000000000d4000 (reserved)
[    0.000000]  BIOS-e820: 00000000000e0000 - 0000000000100000 (reserved)
[    0.000000]  BIOS-e820: 0000000000100000 - 00000000cff90000 (usable)
[    0.000000]  BIOS-e820: 00000000cff90000 - 00000000cffab000 (ACPI data)
[    0.000000]  BIOS-e820: 00000000cffab000 - 00000000cffcf000 (ACPI NVS)
[    0.000000]  BIOS-e820: 00000000cffcf000 - 00000000e0000000 (reserved)
[    0.000000]  BIOS-e820: 00000000f8000000 - 00000000fc000000 (reserved)
[    0.000000]  BIOS-e820: 00000000fec00000 - 00000000fec10000 (reserved)
[    0.000000]  BIOS-e820: 00000000fee00000 - 00000000fee01000 (reserved)
[    0.000000]  BIOS-e820: 00000000ff000000 - 0000000100000000 (reserved)
[    0.000000]  BIOS-e820: 0000000100000000 - 000000010e000000 (usable)
[    0.000000]  BIOS-e820: 000000010e000000 - 0000000110000000 (reserved)
[    0.000000] NX (Execute Disable) protection: active
[    0.000000] SMBIOS 2.5 present.
[    0.000000] DMI: LENOVO INVALID/LENOVO, BIOS 5CKT67AUS 09/25/2010
[    0.000000] e820 update range: 0000000000000000 - 0000000000010000 (usable) ==> (reserved)
[    0.000000] e820 remove range: 00000000000a0000 - 0000000000100000 (usable)
[    0.000000] No AGP bridge found
[    0.000000] last_pfn = 0x10e000 max_arch_pfn = 0x400000000
[    0.000000] MTRR default type: uncachable


```


2. View Available System Memory


You can also view the available memory from the dmesg messages as shown below.


```

# dmesg | grep Memory


```


The Output will be like this 



```

[    0.000000] initial memory mapped : 0 - 20000000
[    0.000000] Base memory trampoline at [ffff880000098000] 98000 size 20480
[    0.000000] init_memory_mapping: 0000000000000000-00000000cff90000
[    0.000000] init_memory_mapping: 0000000100000000-000000010e000000
[    0.000000] PM: Registered nosave memory: 000000000009d000 - 00000000000a0000
[    0.000000] PM: Registered nosave memory: 00000000000a0000 - 00000000000ce000
[    0.000000] PM: Registered nosave memory: 00000000000ce000 - 00000000000d4000
[    0.000000] PM: Registered nosave memory: 00000000000d4000 - 00000000000e0000
[    0.000000] PM: Registered nosave memory: 00000000000e0000 - 0000000000100000
[    0.000000] PM: Registered nosave memory: 00000000cff90000 - 00000000cffab000
[    0.000000] PM: Registered nosave memory: 00000000cffab000 - 00000000cffcf000
[    0.000000] PM: Registered nosave memory: 00000000cffcf000 - 00000000e0000000
[    0.000000] PM: Registered nosave memory: 00000000e0000000 - 00000000f8000000
[    0.000000] PM: Registered nosave memory: 00000000f8000000 - 00000000fc000000
[    0.000000] PM: Registered nosave memory: 00000000fc000000 - 00000000fec00000
[    0.000000] PM: Registered nosave memory: 00000000fec00000 - 00000000fec10000
[    0.000000] PM: Registered nosave memory: 00000000fec10000 - 00000000fee00000
[    0.000000] PM: Registered nosave memory: 00000000fee00000 - 00000000fee01000
[    0.000000] PM: Registered nosave memory: 00000000fee01000 - 00000000ff000000
[    0.000000] PM: Registered nosave memory: 00000000ff000000 - 0000000100000000
[    0.000000] please try 'cgroup_disable=memory' option if you don't want memory cgroups
[    0.009595] Initializing cgroup subsys memory
[    0.368377] Freeing initrd memory: 13912k freed
[    0.503612] agpgart-intel 0000:00:00.0: detected 131072K stolen memory
[    1.164451] Freeing unused kernel memory: 924k freed
[    1.168764] Freeing unused kernel memory: 1592k freed
[    1.172101] Freeing unused kernel memory: 1188k freed


```


3.View Ethernet Link Status (UP/DOWN)


```

# dmesg  | grep eth


```

The Output Will be like this ...


```
[    0.228209] ACPI Error: Method parse/execution failed [\_SB_.PCI0._OSC] (Node ffff88010762e0a0), AE_ALREADY_EXISTS (20110623/psparse-536)
[    0.228215] ACPI: Marking method _OSC as Serialized because of AE_ALREADY_EXISTS error
[    0.231247] ACPI Error: Method parse/execution failed [\_SB_.PCI0._OSC] (Node ffff88010762e0a0), AE_ALREADY_EXISTS (20110623/psparse-536)
[    0.231291] ACPI Error: Method parse/execution failed [\_SB_.PCI0._OSC] (Node ffff88010762e0a0), AE_ALREADY_EXISTS (20110623/psparse-536)
[    0.236100] i2c-core: driver [aat2870] using legacy suspend method
[    0.236101] i2c-core: driver [aat2870] using legacy resume method
[    1.422304] e1000e 0000:00:19.0: eth0: (PCI Express:2.5GT/s:Width x1) 1c:6f:65:04:dd:8b
[    1.422307] e1000e 0000:00:19.0: eth0: Intel(R) PRO/1000 Network Connection
[    1.422372] e1000e 0000:00:19.0: eth0: MAC: 8, PHY: 8, PBA No: FFFFFF-0FF
[    5.631816] ADDRCONF(NETDEV_UP): eth0: link is not ready
[   10.921326] device eth0 entered promiscuous mode
[   11.072479] ADDRCONF(NETDEV_UP): eth0: link is not ready
[   12.592898] e1000e: eth0 NIC Link is Up 100 Mbps Full Duplex, Flow Control: Rx/Tx
[   12.592902] e1000e 0000:00:19.0: eth0: 10/100 speed: disabling TSO
[   12.593232] ADDRCONF(NETDEV_CHANGE): eth0: link becomes ready
[   12.593285] br0: port 1(eth0) entering forwarding state
[   12.593291] br0: port 1(eth0) entering forwarding state
[   21.600027] br0: port 1(eth0) entering forwarding state
[   23.472020] eth0: no IPv6 routers present


```


5. Clear Messages in dmesg Buffer


Sometimes you might want to clear the dmesg messages before your next reboot. You can clear the dmesg buffer as shown below.


```

# dmesg -c

# dmesg


```



