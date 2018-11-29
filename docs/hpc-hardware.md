# HPC Hardware

* Switches
* Cables
* Storage
* Nodes
* Power
* Cooling

## Switches

* Ethernet
* Infiniband
    * QDR
    * FDR
    * EDR
    * Director Switch
    * Satellite Switches
* FiberChannel
    * HBAs
    * 16G

## Cables

- Ethernet
    - Copper
        - CAT 5/6
    - Fiber
    	- Single
    	- Multi-Mode
- Infiniband
    - QDR
    - FDR
    - EDR
    - Copper
    - Fiber
- FiberChannel
    - Fiber
- SAS
    - Direct Connect SAS
- Power
    - Node to Wall
    - Node to Rack
    - Rack to PDU
    - PDU to UPS

## Storage

* Disks
    * HDD
        * Size and Speed
    * SSD
        * Size, Speed and Useable number of Writes

#### Disk Shelves

##### Controllers

##### ARI Systems

These are storage shelves sold by RAID Inc. in which are rebranded I'm sure. I have used HP MSA 2000's and they seems to be the same thing right down to the web interface. The command line is the same also.

###### Show Versions

- ssh to the controller *manage* is the default username
- `ssh manage@<controller ip or hostname>`

```bash
# show versions
Controller A Versions
---------------------
Bundle Version: GL222R061
Build Date: Tue May 29 14:57:00 MDT 2018

Controller B Versions
---------------------
Bundle Version: GL222R061
Build Date: Tue May 29 14:57:00 MDT 2018

Success: Command completed successfully. (2018-11-29 22:50:53)
```

###### Upgrade Firmware on ARI System backing GPFS 

```bash

- Download firmware from RAID ftp site
        ftp://files.raidinc.com/u-california-riverside/fromRAID
                GL222R061-RaidInc.bin
                GL222R061-RaidInc.md5

- Shutdown IO to cluster

- Unmount all file systems to all nodes
        # mmumount all -a
        # mmlsmount all -a

- Shutdown GPFS on all nodes
        # mmshutdown -a
        # mmgetstate -a

-Get ARI logs before upgrade
        FTP to storage controller to get logs from
        # ftp manage@<storge controller IP>
                password !manage
        #       get logs <name of file>.zip     

- Upgrade firmware on controllers from CLI
    SSH to one of the controller IP addresses: ssh <ip address of controller A or B>
        Username: manage
        Password: !manage

        Run the following command to enable partner firmware upgrade:
        NOTE: This will allow the firmware to automatically both controllers. 
        using FTP. To exit the telnet session type exit.
        # set advanced-settings partner-firmware-upgrade enabled
                # exit

        Run the following command to flash the firmware to controller A or B:
        ftp <ip address of controller A or B>
                Username: manage
                Password: !manage
                # put <firmware file.bin> flash 
        NOTE: Once the firmware is finished flashing on a controller, the controller will automatically reboot.
        NOTE: When the first controller is down rebooting the 2nc controller will automatically upgrade and reboot
        Please wait for both controller to flash and reboot.
        Verify new firmware is loaded on both controllers
                # show versions
                Controller A Versions
        ---------------------
        Bundle Version: GL222R061-xx
        Build Date: xxxxxxxxxxxxxxxxxxx
        Controller B Versions
        ---------------------
        Bundle Version: GL222R061-xx
        Build Date: xxxxxxxxxxxxxxxxxxx

- Get ARI logs after upgrade
        FTP to storage controller to get logs from
        # ftp manage@<storge controller IP>
                password !manage
        #       get logs <name of file>.zip     

- Rebalance vdisks on controllers from CLI
    Telnet to one of the controller IP addresses: telnet <ip address of controller A or B>
        Username: manage
        Password: !manage
        # show vdisks
        # set vdisk owner b vdisk10       to move the controller owner for vdisk10
        # show vdisks

- Restart GPFS
        # mmstartup -a
        # mmgetstate -a

- Remount GPFS
        # mmmount all -a
        # mmlsmount all -a

- Restart IO to cluster

```

##### JBODS

Curretly Supporting ARI JBODS attached to the ARI Storage controller.

Connected with 2 x 12G SAS cables.

* Tape
    * Cold Storage

## Nodes

* Hypervisor

    * CPU

        * High Core Count

    * MEM

        * High Memory

    * Graphical GPU

        * SR-IOV to provide 3D graphics to each VM
        * NVIDIA GRID K2

    * NICs

        * 1g Cat 6
            * Management Interface
        * 10g Fiber Ethernet
            * SR-IOV would be awesome here also
            * For public interface
        * Mellonox Connect X 3 or higher Infiniband NIC 
            * SR-IOV to provide IB to each VM
            * FDR
        * Fiber Channel HBA
            * SAN connection to shared storage

        * 

* Compute

    * CPU

        * Balance between core count and CPU Speed

        * Intel

            * Currently buying:

            * `lscpu` shows

                ```bash
                Vendor ID:             GenuineIntel
                CPU family:            6
                Model:                 79
                Model name:            Intel(R) Xeon(R) CPU E5-2683 v4 @ 2.10GHz
                Stepping:              1
                CPU MHz:               2599.980
                CPU max MHz:           3000.0000
                CPU min MHz:           1200.0000
                BogoMIPS:              4190.19
                Virtualization:        VT-x
                L1d cache:             32K
                L1i cache:             32K
                L2 cache:              256K
                L3 cache:              40960K
                NUMA node0 CPU(s):     0-15,32-47
                NUMA node1 CPU(s):     16-31,48-63
                Flags:                 fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc aperfmperf eagerfpu pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch epb cat_l3 cdp_l3 invpcid_single intel_pt spec_ctrl ibpb_support tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm rdt_a rdseed adx smap xsaveopt cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts
                ```

        * ARM

            * Really watching this.
            * 28-core Cavium ThunderX2 processors running at 2.0 GHz
                * [These are powering a new supercomputer at Sandia National Lab.](https://www.top500.org/news/sandia-to-install-first-petascale-supercomputer-powered-by-arm-processors/)
                * [And another](https://www.top500.org/news/cray-adds-arm-option-to-xc50-supercomputer/)
            * This is becoming more and more main stream.

        * AMD

            * Still keeping up. I manage a few hundered nodes of AMD but we are phasing these out

    * MEM

        * The more the better

        * Check memory on the node

            `cat /proc/meminfo | head -3` 

            ```bash
            i22:~# cat /proc/meminfo | head -3
            MemTotal:       528079828 kB
            MemFree:        495977976 kB
            MemAvailable:   502751960 kB
            ```

            `free -g` 

            ```bash
            i22:~# free -g
                          total        used        free      shared  buff/cache   available
            Mem:            503          31         461           1          10         467
            Swap:             3           0           3
            
            ```

            smem

            * smem  reports physical memory usage, taking shared memory pages into account.  Unshared memory is reported as the USS (Unique Set Size).  Shared memory is divided evenly among the processes sharing that memory.  The unshared memory (USS) plus a process's proportion of shared memory is reported as the PSS  (Proportional  Set Size).  The USS and PSS only include physical memory usage.  They do not include memory that has been swapped out to disk. Memory can be reported by process, by user, by mapping, or system wide.  Both text mode and graphical output are available.

            * Example

                * `smem -w`  

                * ```bash
                    i22:~# smem -w
                    Area                           Used      Cache   Noncache 
                    firmware/hardware                 0          0          0 
                    kernel image                      0          0          0 
                    kernel dynamic memory      14962228    7805104    7157124 
                    userspace memory           17016380    1589884   15426496 
                    free memory               496101220  496101220          0 
                    ```

                * [other examples](https://www.techrepublic.com/article/how-to-install-and-use-the-smem-memory-reporting-tool-in-linux/)

            * 

        * 128G Min.

        * 256G Ok

        * 512G Good

        * 1TB Great

        * Speed:

            * 1600 MHz
            * `dmidecode --type memory | grep Speed | grep -v Unknown | uniq`

    * GPU

    * SSD

    * NIC

* Storage

* Special

## Power

* Rack Power
* Server Room Power

## Cooling

* Server Room HVAC
    * Libert CRAC units
