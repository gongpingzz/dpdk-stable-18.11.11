..  SPDX-License-Identifier: BSD-3-Clause
    Copyright 2018 The DPDK contributors

DPDK Release 18.11
==================

.. **Read this first.**

   The text in the sections below explains how to update the release notes.

   Use proper spelling, capitalization and punctuation in all sections.

   Variable and config names should be quoted as fixed width text:
   ``LIKE_THIS``.

   Build the docs and view the output file to ensure the changes are correct::

      make doc-guides-html

      xdg-open build/doc/html/guides/rel_notes/release_18_11.html


New Features
------------

.. This section should contain new features added in this release.
   Sample format:

   * **Add a title in the past tense with a full stop.**

     Add a short 1-2 sentence description in the past tense.
     The description should be enough to allow someone scanning
     the release notes to understand the new feature.

     If the feature adds a lot of sub-features you can use a bullet list
     like this:

     * Added feature foo to do something.
     * Enhanced feature bar to do something else.

     Refer to the previous release notes for examples.

     Suggested order in release notes items:
     * Core libs (EAL, mempool, ring, mbuf, buses)
     * Device abstraction libs and PMDs
       - ethdev (lib, PMDs)
       - cryptodev (lib, PMDs)
       - eventdev (lib, PMDs)
       - etc
     * Other libs
     * Apps, Examples, Tools (if significant)

     This section is a comment. Do not overwrite or remove it.
     Also, make sure to start the actual text at the margin.
     =========================================================

* **Added support for using externally allocated memory in DPDK.**

  DPDK has added support for creating new ``rte_malloc`` heaps referencing
  memory that was created outside of DPDK's own page allocator, and using that
  memory natively with any other DPDK library or data structure.

* **Added check for ensuring allocated memory is addressable by devices.**

  Some devices can have addressing limitations so a new function,
  ``rte_mem_check_dma_mask()``, has been added for checking that allocated
  memory is not out of the device range. Since memory can now be allocated
  dynamically after initialization, a DMA mask is stored and any new allocated
  memory will be checked against it and rejected if it is out of range. If
  more than one device has addressing limitations, the DMA mask is the more
  restrictive one.

* **Updated the C11 memory model version of the ring library.**

  Added changes to decrease latency for architectures using the C11 memory
  model version of the ring library.

  On Cavium ThunderX2 platform, the changes decreased latency by 27-29%
  and 3-15% for MPMC and SPSC cases respectively (with 2 lcores). The
  real improvements may vary with the number of contending lcores and
  the size of the ring.

* **Added hot-unplug handle mechanism.**

  Added ``rte_dev_hotplug_handle_enable()`` and
  ``rte_dev_hotplug_handle_disable()`` for enabling or disabling the hotplug
  handle mechanism.

* **Added support for device multi-process hotplug.**

  Added support for hotplug and hot-unplug in a multiprocessing scenario. Any
  ethdev devices created in the primary process will be regarded as shared and
  will be available for all DPDK processes. Synchronization between processes
  will be done using DPDK IPC.

* **Added new Flow API actions to rewrite fields in packet headers.**

  Added new Flow API actions to:

  * Modify source and destination IP addresses in the outermost IPv4/IPv6
    headers.
  * Modify source and destination port numbers in the outermost TCP/UDP
    headers.

* **Added new Flow API action to swap MAC addresses in Ethernet header.**

  Added new Flow API action to swap the source and destination MAC
  addresses in the outermost Ethernet header.

* **Add support to offload more flow match and actions for CXGBE PMD.**

  The Flow API support has been enhanced for the CXGBE Poll Mode Driver to
  offload:

  * Match items: destination MAC address.
  * Action items: push/pop/rewrite vlan header,
    rewrite IP addresses in outermost IPv4/IPv6 header,
    rewrite port numbers in outermost TCP/UDP header,
    swap MAC addresses in outermost Ethernet header.

* **Added a devarg to use the latest supported vector path in i40e.**

  A new devarg ``use-latest-supported-vec`` was introduced to allow users to
  choose the latest vector path that the platform supported. For example, users
  can use AVX2 vector path on BDW/HSW to get better performance.

* **Added support for SR-IOV in netvsc PMD.**

  The ``netvsc`` poll mode driver now supports the Accelerated Networking
  SR-IOV option in Hyper-V and Azure. This is an alternative to the previous
  vdev_netvsc, tap, and failsafe drivers combination.

* **Added a new net driver for Marvell Armada 3k device.**

  Added the new ``mvneta`` net driver for Marvell Armada 3k device. See the
  :doc:`../nics/mvneta` NIC guide for more details on this new driver.

* **Added NXP ENETC PMD.**

  Added the new enetc driver for the NXP enetc platform. See the
  :doc:`../nics/enetc` NIC driver guide for more details on this new driver.

* **Added Ethernet poll mode driver for Aquantia aQtion family of 10G devices.**

  Added the new ``atlantic`` ethernet poll mode driver for Aquantia XGBE devices.
  See the :doc:`../nics/atlantic` NIC driver guide for more details on this
  driver.

* **Updated mlx5 driver.**

  Updated the mlx5 driver including the following changes:

  * Improved security of PMD to prevent the NIC from getting stuck when
    the application misbehaves.
  * Reworked flow engine to supported e-switch flow rules (transfer attribute).
  * Added support for header re-write(L2-L4), VXLAN encap/decap, count, match
    on TCP flags and multiple flow groups with e-switch flow rules.
  * Added support for match on metadata, VXLAN and MPLS encap/decap with flow
    rules.
  * Added support for ``RTE_ETH_DEV_CLOSE_REMOVE`` flag to provide better
    support for representors.
  * Added support for meson build.
  * Fixed build issue with PPC.
  * Added support for BlueField VF.
  * Added support for externally allocated static memory for DMA.

* **Updated Solarflare network PMD.**

  Updated the sfc_efx driver including the following changes:

  * Added support for Rx scatter in EF10 datapath implementation.
  * Added support for Rx descriptor status API in EF10 datapath implementation.
  * Added support for TSO in EF10 datapath implementation.
  * Added support for Tx descriptor status API in EF10 (ef10 and ef10_simple)
    datapaths implementation.

* **Updated the enic driver.**

  * Added AVX2-based vectorized Rx handler.
  * Added VLAN and checksum offloads to the simple Tx handler.
  * Added the "count" flow action.
  * Enabled the virtual address IOVA mode.

* **Updated the failsafe driver.**

  Updated the failsafe driver including the following changes:

  * Added support for Rx and Tx queues start and stop.
  * Added support for Rx and Tx queues deferred start.
  * Added support for runtime Rx and Tx queues setup.
  * Added support multicast MAC address set.

* **Added a devarg to use a PCAP interface physical MAC address.**

  A new devarg ``phy_mac`` was introduced to allow users to use the physical
  MAC address of the selected PCAP interface.

* **Added TAP Rx/Tx queues sharing with a secondary process.**

  Added support to allow a secondary process to attach a TAP device created
  in the primary process, probe the queues, and process Rx/Tx in a secondary
  process.

* **Added classification and metering support to SoftNIC PMD.**

  Added support for flow classification (rte_flow API), and metering and
  policing (rte_mtr API) to the SoftNIC PMD.

* **Added Crypto support to the Softnic PMD.**

  The Softnic is now capable of processing symmetric crypto workloads such
  as cipher, cipher-authentication chaining, and AEAD encryption and
  decryption. This is achieved by calling DPDK Cryptodev APIs.

* **Added cryptodev port to port library.**

  Cryptodev port is a shim layer in the port library that interacts with DPDK
  Cryptodev PMDs including burst enqueuing and dequeuing crypto operations.

* **Added symmetric cryptographic actions to the pipeline library.**

  In the pipeline library support was added for symmetric crypto action
  parsing and an action handler was implemented. The action allows automatic
  preparation of the crypto operation with the rules specified such as
  algorithm, key, and IV, etc. for the cryptodev port to process.

* **Updated the AESNI MB PMD.**

  The AESNI MB PMD has been updated with additional support for the AES-GCM
  algorithm.

* **Added NXP CAAM JR PMD.**

  Added the new caam job ring driver for NXP platforms. See the
  :doc:`../cryptodevs/caam_jr` guide for more details on this new driver.

* **Added support for GEN3 devices to Intel QAT driver.**

  Added support for the third generation of Intel QuickAssist devices.

* **Updated the QAT PMD.**

  The QAT PMD was updated with additional support for:

  * The AES-CMAC algorithm.

* **Added support for Dynamic Huffman Encoding to Intel QAT comp PMD.**

  The Intel QuickAssist (QAT) compression PMD has been updated with support
  for Dynamic Huffman Encoding for the Deflate algorithm.

* **Added Event Ethernet Tx Adapter.**

  Added event ethernet Tx adapter library that provides configuration and
  data path APIs for the ethernet transmit stage of an event driven packet
  processing application. These APIs abstract the implementation of the
  transmit stage and allow the application to use eventdev PMD support or
  a common implementation.

* **Added Distributed Software Eventdev PMD.**

  Added the new Distributed Software Event Device (DSW), which is a
  pure-software eventdev driver distributing the work of scheduling
  among all eventdev ports and the lcores using them. DSW, compared to
  the SW eventdev PMD, sacrifices load balancing performance to
  gain better event scheduling throughput and scalability.

* **Added extendable bucket feature to hash library (rte_hash).**

  This new "extendable bucket" feature provides 100% insertion guarantee to
  the capacity specified by the user by extending hash table with extra
  buckets when needed to accommodate the unlikely event of intensive hash
  collisions. In addition, the internal hashing algorithm was changed to use
  partial-key hashing to improve memory efficiency and lookup performance.

* **Added lock free reader/writer concurrency to hash library (rte_hash).**

  Lock free reader/writer concurrency prevents the readers from getting
  blocked due to a preempted writer thread. This allows the hash library
  to be used in scenarios where the writer thread runs on the control plane.

* **Added Traffic Pattern Aware Power Control Library.**

  Added an experimental library that extends the Power Library and provides
  empty_poll APIs. This feature measures how many times empty_polls are
  executed per core and uses the number of empty polls as a hint for system
  power management.

  See the :doc:`../prog_guide/power_man` section of the DPDK Programmers
  Guide document for more information.

* **Added JSON power policy interface for containers.**

  Extended the Power Library and vm_power_manager sample app to allow power
  policies to be submitted via a FIFO using JSON formatted strings. Previously
  limited to Virtual Machines, this feature extends power policy functionality
  to containers and host applications that need to have their cores frequency
  controlled based on the rules contained in the policy.

* **Added Telemetry API.**

  Added a new telemetry API which allows applications to transparently expose
  their telemetry in JSON via a UNIX socket. The JSON can be consumed by any
  Service Assurance agent, such as CollectD.

* **Updated KNI kernel module, rte_kni library, and KNI sample application.**

  Updated the KNI kernel module with a new kernel module parameter,
  ``carrier=[on|off]`` to allow the user to control the default carrier
  state of the KNI kernel network interfaces. The default carrier state
  is now set to ``off``, so the interfaces cannot be used until the
  carrier state is set to ``on`` via ``rte_kni_update_link`` or
  by writing ``1`` to ``/sys/devices/virtual/net/<iface>/carrier``.
  In previous versions the default carrier state was left undefined.
  See :doc:`../prog_guide/kernel_nic_interface` for more information.

  Also added the new API function ``rte_kni_update_link()`` to allow the user
  to set the carrier state of the KNI kernel network interface.

  Also added a new command line flag ``-m`` to the KNI sample application to
  monitor and automatically reflect the physical NIC carrier state to the
  KNI kernel network interface with the new ``rte_kni_update_link()`` API.
  See :doc:`../sample_app_ug/kernel_nic_interface` for more information.

* **Added ability to switch queue deferred start flag on testpmd app.**

  Added a console command to testpmd app, giving ability to switch
  ``rx_deferred_start`` or ``tx_deferred_start`` flag of the specified queue of
  the specified port. The port must be stopped before the command call in order
  to reconfigure queues.

* **Add a new sample application for vDPA.**

  The vdpa sample application creates vhost-user sockets by using the
  vDPA backend. vDPA stands for vhost Data Path Acceleration which utilizes
  virtio ring compatible devices to serve virtio driver directly to enable
  datapath acceleration. As vDPA driver can help to set up vhost datapath,
  this application doesn't need to launch dedicated worker threads for vhost
  enqueue/dequeue operations.

* **Added cryptodev FIPS validation example application.**

  Added an example application to parse and perform symmetric cryptography
  computation to the NIST Cryptographic Algorithm Validation Program (CAVP)
  test vectors.

* **Allow unit test binary to take parameters from the environment.**

  The unit test "test", or "dpdk-test", binary is often called from scripts,
  which can make passing additional parameters, such as a coremask,
  difficult. Support has been added to the application to allow it to take
  additional command-line parameter values from the ``DPDK_TEST_PARAMS``
  environment variable to make this application easier to use.


API Changes
-----------

.. This section should contain API changes. Sample format:

   * Add a short 1-2 sentence description of the API change.
     Use fixed width quotes for ``function_names`` or ``struct_names``.
     Use the past tense.

   This section is a comment. Do not overwrite or remove it.
   Also, make sure to start the actual text at the margin.
   =========================================================

* eal: ``rte_memseg_list`` structure now has an additional flag indicating
  whether the memseg list is externally allocated. This will have implications
  for any users of memseg-walk-related functions, as they will now have to skip
  externally allocated segments in most cases if the intent is to only iterate
  over internal DPDK memory.

  In addition the ``socket_id`` parameter across the entire DPDK has gained
  additional meaning, as some socket ID's will now be representing externally
  allocated memory. No changes will be required for existing code as backwards
  compatibility will be kept, and those who do not use this feature will not
  see these extra socket ID's. Any new API's must not check socket ID
  parameters themselves, and must instead leave it to the memory subsystem to
  decide whether socket ID is a valid one.

* eal: The following devargs functions, which were deprecated in 18.05,
  were removed in 18.11:
  ``rte_eal_parse_devargs_str()``, ``rte_eal_devargs_add()``,
  ``rte_eal_devargs_type_count()``, and ``rte_eal_devargs_dump()``.

* eal: The parameters of the function ``rte_devargs_remove()`` have changed
  from bus and device names to ``struct rte_devargs``.

* eal: The deprecated functions attach/detach were removed in 18.11.
  ``rte_eal_dev_attach`` can be replaced by
  ``rte_dev_probe`` or ``rte_eal_hotplug_add``.
  ``rte_eal_dev_detach`` can be replaced by
  ``rte_dev_remove`` or ``rte_eal_hotplug_remove``.

* eal: The scope of ``rte_eal_hotplug_add()``/``rte_dev_probe()``
  and ``rte_eal_hotplug_remove()``/``rte_dev_remove()`` has been extended.
  In the multi-process model, they will guarantee that the device is
  attached or detached on all processes.

* mbuf: The ``__rte_mbuf_raw_free()`` and ``__rte_pktmbuf_prefree_seg()``
  functions were deprecated since 17.05 and are replaced by
  ``rte_mbuf_raw_free()`` and ``rte_pktmbuf_prefree_seg()``.

* ethdev: The deprecated functions attach/detach were removed in 18.11.
  ``rte_eth_dev_attach()`` can be replaced by ``RTE_ETH_FOREACH_MATCHING_DEV``
  and ``rte_dev_probe()`` or ``rte_eal_hotplug_add()``.
  ``rte_eth_dev_detach()`` can be replaced by
  ``rte_dev_remove()`` or ``rte_eal_hotplug_remove()``.

* ethdev: A call to ``rte_eth_dev_release_port()`` has been added in
  ``rte_eth_dev_close()``. As a consequence, a closed port is freed
  and seen as invalid because of its state ``RTE_ETH_DEV_UNUSED``.
  This new behavior is enabled per driver for a migration period.

* A new device flag, ``RTE_ETH_DEV_NOLIVE_MAC_ADDR``, changes the order of
  actions inside ``rte_eth_dev_start()`` regarding MAC set. Some NICs do not
  support MAC changes once the port has started and with this new device
  flag the MAC can be properly configured in any case. This is particularly
  important for bonding.

* The default behavior of CRC strip offload has changed in this
  release. Without any specific Rx offload flag, default behavior by a PMD is
  now to strip CRC. ``DEV_RX_OFFLOAD_CRC_STRIP`` offload flag has been removed.
  To request keeping CRC, application should set ``DEV_RX_OFFLOAD_KEEP_CRC``
  Rx offload.

* eventdev: The type of the second parameter to
  ``rte_event_eth_rx_adapter_caps_get()`` has been changed from uint8_t to
  uint16_t.

* kni: By default, interface carrier status is ``off`` which means there won't
  be any traffic. It can be set to ``on`` via ``rte_kni_update_link()`` API or
  via ``sysfs`` interface: ``echo 1 > /sys/class/net/vEth0/carrier``.

  Note interface should be ``up`` to be able to read/write sysfs interface.
  When KNI sample application is used, ``-m`` parameter can be used to
  automatically update the carrier status for the interface.

* kni: When ethtool support is enabled (``CONFIG_RTE_KNI_KMOD_ETHTOOL=y``)
  ethtool commands ``ETHTOOL_GSET & ETHTOOL_SSET`` are no longer supported for
  kernels that have ``ETHTOOL_GLINKSETTINGS & ETHTOOL_SLINKSETTINGS`` support.
  This means ``ethtool "-a|--show-pause", "-s|--change"`` won't work, and
  ``ethtool <iface>`` output will have less information.


ABI Changes
-----------

.. This section should contain ABI changes. Sample format:

   * Add a short 1-2 sentence description of the ABI change
     that was announced in the previous releases and made in this release.
     Use fixed width quotes for ``function_names`` or ``struct_names``.
     Use the past tense.

   This section is a comment. Do not overwrite or remove it.
   Also, make sure to start the actual text at the margin.
   =========================================================

* eal: added ``legacy_mem`` and ``single_file_segments`` values to
  ``rte_config`` structure on account of improving DPDK usability when
  using either ``--legacy-mem`` or ``--single-file-segments`` flags.

* eal: EAL library ABI version was changed due to previously announced work on
  supporting external memory in DPDK:

  - Structure ``rte_memseg_list`` now has a new field indicating length
    of memory addressed by the segment list
  - Structure ``rte_memseg_list`` now has a new flag indicating whether
    the memseg list refers to external memory
  - Structure ``rte_malloc_heap`` now has a new field indicating socket
    ID the malloc heap belongs to
  - Structure ``rte_mem_config`` has had its ``malloc_heaps`` array
    resized from ``RTE_MAX_NUMA_NODES`` to ``RTE_MAX_HEAPS`` value
  - Structure ``rte_malloc_heap`` now has a ``heap_name`` member
  - Structure ``rte_eal_memconfig`` has been extended to contain next
    socket ID for externally allocated segments

* eal: Added ``dma_maskbits`` to ``rte_mem_config`` for keeping the most
  restrictive DMA mask based on the devices addressing limitations.

* eal: The structure ``rte_device`` has a new field to reference a
  ``rte_bus``.  It thus changes the size of the ``struct rte_device`` and the
  inherited device structures of all buses.


Shared Library Versions
-----------------------

.. Update any library version updated in this release
   and prepend with a ``+`` sign, like this:

     librte_acl.so.2
   + librte_cfgfile.so.2
     librte_cmdline.so.2

   This section is a comment. Do not overwrite or remove it.
   =========================================================

The libraries prepended with a plus sign were incremented in this version.

.. code-block:: diff

     librte_acl.so.2
     librte_bbdev.so.1
     librte_bitratestats.so.2
     librte_bpf.so.1
   + librte_bus_dpaa.so.2
   + librte_bus_fslmc.so.2
   + librte_bus_ifpga.so.2
   + librte_bus_pci.so.2
   + librte_bus_vdev.so.2
   + librte_bus_vmbus.so.2
     librte_cfgfile.so.2
     librte_cmdline.so.2
     librte_compressdev.so.1
     librte_cryptodev.so.5
     librte_distributor.so.1
   + librte_eal.so.9
     librte_efd.so.1
   + librte_ethdev.so.11
   + librte_eventdev.so.6
     librte_flow_classify.so.1
     librte_gro.so.1
     librte_gso.so.1
     librte_hash.so.2
     librte_ip_frag.so.1
     librte_jobstats.so.1
     librte_kni.so.2
     librte_kvargs.so.1
     librte_latencystats.so.1
     librte_lpm.so.2
     librte_mbuf.so.4
     librte_member.so.1
     librte_mempool.so.5
     librte_meter.so.2
     librte_metrics.so.1
     librte_net.so.1
     librte_pci.so.1
     librte_pdump.so.2
     librte_pipeline.so.3
     librte_pmd_bnxt.so.2
     librte_pmd_bond.so.2
     librte_pmd_i40e.so.2
     librte_pmd_ixgbe.so.2
     librte_pmd_dpaa2_qdma.so.1
     librte_pmd_ring.so.2
     librte_pmd_softnic.so.1
     librte_pmd_vhost.so.2
     librte_port.so.3
     librte_power.so.1
     librte_rawdev.so.1
     librte_reorder.so.1
     librte_ring.so.2
     librte_sched.so.1
     librte_security.so.1
     librte_table.so.3
     librte_timer.so.1
     librte_vhost.so.4


Known Issues
------------

.. This section should contain new known issues in this release. Sample format:

   * **Add title in present tense with full stop.**

     Add a short 1-2 sentence description of the known issue
     in the present tense. Add information on any known workarounds.

   This section is a comment. Do not overwrite or remove it.
   Also, make sure to start the actual text at the margin.
   =========================================================

* When using SR-IOV (VF) support with netvsc PMD and the Mellanox mlx5
  bifurcated driver the Linux netvsc device must be brought up before the
  netvsc device is unbound and passed to the DPDK.

* IBM Power8 is not supported in this release of DPDK. IBM Power9 is
  supported.

* ``AVX-512`` support has been disabled for ``GCC`` builds [1] because of a
  crash [2]. This can affect ``native`` machine type build targets on the
  platforms that support ``AVX512F`` like ``Intel Skylake`` processors, and
  can cause a possible performance drop. The immediate workaround is to use
  ``clang`` compiler on these platforms. The issue has been identified as a
  GCC defect and reported to the GCC community [3]. Further actions will be
  taken based on the GCC defect result.

  - [1]: Commit 8d07c82b239f ("mk: disable gcc AVX512F support")
  - [2]: https://bugs.dpdk.org/show_bug.cgi?id=97
  - [3]: https://gcc.gnu.org/bugzilla/show_bug.cgi?id=88096


Tested Platforms
----------------

.. This section should contain a list of platforms that were tested
   with this release.

   The format is:

   * <vendor> platform with <vendor> <type of devices> combinations

     * List of CPU
     * List of OS
     * List of devices
     * Other relevant details...

   This section is a comment. Do not overwrite or remove it.
   Also, make sure to start the actual text at the margin.
   =========================================================

* Intel(R) platforms with Intel(R) NICs combinations

   * CPU

     * Intel(R) Atom(TM) CPU C3758 @ 2.20GHz
     * Intel(R) Xeon(R) CPU D-1541 @ 2.10GHz
     * Intel(R) Xeon(R) CPU E5-2680 v2 @ 2.80GHz
     * Intel(R) Xeon(R) CPU E5-2699 v3 @ 2.30GHz
     * Intel(R) Xeon(R) CPU E5-2699 v4 @ 2.20GHz
     * Intel(R) Xeon(R) Platinum 8180 CPU @ 2.50GHz

   * OS:

     * CentOS 7.5
     * Fedora 25
     * Fedora 28
     * FreeBSD 11.2
     * Red Hat Enterprise Linux Server release 7.5
     * Open SUSE 15
     * Wind River Linux 8
     * Ubuntu 14.04
     * Ubuntu 16.04
     * Ubuntu 16.10
     * Ubuntu 17.10
     * Ubuntu 18.04

   * NICs:

     * Intel(R) 82599ES 10 Gigabit Ethernet Controller

       * Firmware version: 0x61bf0001
       * Device id (pf/vf): 8086:10fb / 8086:10ed
       * Driver version: 5.2.3 (ixgbe)

     * Intel(R) Corporation Ethernet Connection X552/X557-AT 10GBASE-T

       * Firmware version: 0x800003e7
       * Device id (pf/vf): 8086:15ad / 8086:15a8
       * Driver version: 4.4.6 (ixgbe)

     * Intel(R) Ethernet Converged Network Adapter X710-DA4 (4x10G)

       * Firmware version: 6.01 0x80003221
       * Device id (pf/vf): 8086:1572 / 8086:154c
       * Driver version: 2.4.6 (i40e)

     * Intel(R) Corporation Ethernet Connection X722 for 10GbE SFP+ (4x10G)

       * Firmware version: 3.33 0x80000fd5 0.0.0
       * Device id (pf/vf): 8086:37d0 / 8086:37cd
       * Driver version: 2.4.6 (i40e)

     * Intel(R) Ethernet Converged Network Adapter XXV710-DA2 (2x25G)

       * Firmware version: 6.01 0x80003221
       * Device id (pf/vf): 8086:158b / 8086:154c
       * Driver version: 2.4.6 (i40e)

     * Intel(R) Ethernet Converged Network Adapter XL710-QDA2 (2X40G)

       * Firmware version: 6.01 0x8000321c
       * Device id (pf/vf): 8086:1583 / 8086:154c
       * Driver version: 2.4.6 (i40e)

     * Intel(R) Corporation I350 Gigabit Network Connection

       * Firmware version: 1.63, 0x80000dda
       * Device id (pf/vf): 8086:1521 / 8086:1520
       * Driver version: 5.4.0-k (igb)

* Intel(R) platforms with Mellanox(R) NICs combinations

   * CPU:

     * Intel(R) Xeon(R) Gold 6154 CPU @ 3.00GHz
     * Intel(R) Xeon(R) CPU E5-2697A v4 @ 2.60GHz
     * Intel(R) Xeon(R) CPU E5-2697 v3 @ 2.60GHz
     * Intel(R) Xeon(R) CPU E5-2680 v2 @ 2.80GHz
     * Intel(R) Xeon(R) CPU E5-2650 v4 @ 2.20GHz
     * Intel(R) Xeon(R) CPU E5-2640 @ 2.50GHz
     * Intel(R) Xeon(R) CPU E5-2620 v4 @ 2.10GHz

   * OS:

     * Red Hat Enterprise Linux Server release 7.6 (Maipo)
     * Red Hat Enterprise Linux Server release 7.5 (Maipo)
     * Red Hat Enterprise Linux Server release 7.4 (Maipo)
     * Red Hat Enterprise Linux Server release 7.3 (Maipo)
     * Red Hat Enterprise Linux Server release 7.2 (Maipo)
     * Ubuntu 18.10
     * Ubuntu 18.04
     * Ubuntu 17.10
     * Ubuntu 16.04
     * SUSE Linux Enterprise Server 15

   * MLNX_OFED: 4.4-2.0.1.0
   * MLNX_OFED: 4.5-0.3.1.0

   * NICs:

     * Mellanox(R) ConnectX(R)-3 Pro 40G MCX354A-FCC_Ax (2x40G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1007
       * Firmware version: 2.42.5000

     * Mellanox(R) ConnectX(R)-4 10G MCX4111A-XCAT (1x10G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 10G MCX4121A-XCAT (2x10G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 25G MCX4111A-ACAT (1x25G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 25G MCX4121A-ACAT (2x25G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 40G MCX4131A-BCAT/MCX413A-BCAT (1x40G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 40G MCX415A-BCAT (1x40G)

       * Host interface: PCI Express 3.0 x16
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 50G MCX4131A-GCAT/MCX413A-GCAT (1x50G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 50G MCX414A-BCAT (2x50G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 50G MCX415A-GCAT/MCX416A-BCAT/MCX416A-GCAT (2x50G)

       * Host interface: PCI Express 3.0 x16
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 50G MCX415A-CCAT (1x100G)

       * Host interface: PCI Express 3.0 x16
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 100G MCX416A-CCAT (2x100G)

       * Host interface: PCI Express 3.0 x16
       * Device ID: 15b3:1013
       * Firmware version: 12.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 Lx 10G MCX4121A-XCAT (2x10G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1015
       * Firmware version: 14.23.8022 and above

     * Mellanox(R) ConnectX(R)-4 Lx 25G MCX4121A-ACAT (2x25G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1015
       * Firmware version: 14.23.8022 and above

     * Mellanox(R) ConnectX(R)-5 100G MCX556A-ECAT (2x100G)

       * Host interface: PCI Express 3.0 x16
       * Device ID: 15b3:1017
       * Firmware version: 16.23.8022 and above

     * Mellanox(R) ConnectX(R)-5 Ex EN 100G MCX516A-CDAT (2x100G)

       * Host interface: PCI Express 4.0 x16
       * Device ID: 15b3:1019
       * Firmware version: 16.23.8022 and above

* ARM platforms with Mellanox(R) NICs combinations

   * CPU:

     * Qualcomm ARM 1.1 2500MHz

   * OS:

     * Red Hat Enterprise Linux Server release 7.5 (Maipo)

   * NICs:

     * Mellanox(R) ConnectX(R)-4 Lx 25G MCX4121A-ACAT (2x25G)

       * Host interface: PCI Express 3.0 x8
       * Device ID: 15b3:1015
       * Firmware version: 14.24.0220

     * Mellanox(R) ConnectX(R)-5 100G MCX556A-ECAT (2x100G)

       * Host interface: PCI Express 3.0 x16
       * Device ID: 15b3:1017
       * Firmware version: 16.24.0220

* Mellanox(R) BlueField SmartNIC

   * Mellanox(R) BlueField SmartNIC MT416842 (2x25G)

       * Host interface: PCI Express 3.0 x16
       * Device ID: 15b3:a2d2
       * Firmware version: 18.24.0246

   * SoC ARM cores running OS:

     * CentOS Linux release 7.4.1708 (AltArch)
     * MLNX_OFED 4.4-2.5.3.0

  * DPDK application running on ARM cores inside SmartNIC

* ARM SoC combinations from NXP (with integrated NICs)

   * SoC:

     * NXP/Freescale QorIQ LS1046A with ARM Cortex A72
     * NXP/Freescale QorIQ LS2088A with ARM Cortex A72

   * OS:

     * Ubuntu 18.04.1 LTS with NXP QorIQ LSDK 1809 support packages
     * Ubuntu 16.04.3 LTS with NXP QorIQ LSDK 1803 support packages

18.11.1  Release Notes
----------------------

18.11.1 Fixes
~~~~~~~~~~~~~

* app/bbdev: fix return value check
* app/eventdev: detect deadlock for timer event producer
* app/pdump: fix vdev cleanup
* app/testpmd: expand RED queue thresholds to 64 bits
* app/testpmd: fix MPLS BoS bit default value
* app/testpmd: fix MPLSoGRE encapsulation
* app/testpmd: fix MPLSoUDP encapsulation
* app/testpmd: fix quit to stop all ports before close
* app/testpmd: fix Tx metadata show command
* bb/turbo_sw: fix dynamic linking
* build: fix meson check for binutils 2.30
* build: fix variable name in dependency error message
* build: mention -march in pkg-config description
* build: use static deps for pkg-config libs.private
* bus/dpaa: do nothing if bus not present
* bus/dpaa: fix logical to physical core affine logic
* bus/fslmc: fix parse method for bus devices
* bus/fslmc: fix ring mode to use correct cache settings
* bus/fslmc: fix to convert error msg to warning
* bus/fslmc: fix to reset portal memory before use
* bus/fslmc: fix to use correct physical core for logical core
* bus/ifpga: fix AFU probe failure handler
* bus/ifpga: fix build for cpp applications
* bus/ifpga: fix forcing optional devargs
* bus/vmbus: fix race in subchannel creation
* common/qat: remove check of valid firmware response
* compressdev: fix structure comment
* compress/qat: fix dequeue error counter
* compress/qat: fix returned status on overflow
* compress/qat: fix return on building request error
* config: enable C11 memory model for armv8 with meson
* crypto/dpaa2_sec: fix FLC address for physical mode
* crypto/qat: fix block size error handling
* crypto/qat: fix digest in wireless auth case
* crypto/qat: fix message for CCM when setting unused counter
* crypto/qat: fix message for NULL algo setting unused counter
* devtools: fix build check for whether meson has run
* devtools: fix return of forbidden addition checks
* devtools: fix symbol check when adding experimental section
* devtools: fix wrong headline lowercase for arm
* doc: add dependency for PDF in contributing guide
* doc: add GCM AAD limitation in qat guide
* doc: add GRO limitations in programmers guide
* doc: add missing loopback option in testpmd guide
* doc: clarify libnuma requirement for NUMA systems
* doc: fix AESNI_MB guide
* doc: fix a parameter name in testpmd guide
* doc: fix a typo in power management guide
* doc: fix a typo in testpmd guide
* doc: fix a typo in testpmd guide
* doc: fix flow action command names in testpmd guide
* doc: fix garbage text in generated HTML guides
* doc: fix ifc naming
* doc: fix MAC address rewrite actions in prog guide
* doc: fix references in power management guide
* doc: remove note on memory mode limitation in multi-process
* drivers/crypto: fix PMDs memory leak
* drivers: fix sprintf with snprintf
* drivers/net: fix several Tx prepare functions
* eal/bsd: remove clean up of files at startup
* eal: check string parameter lengths
* eal: clean up unused files on initialization
* eal: close multi-process socket during cleanup
* eal: fix build of external app with clang on armv8
* eal: fix clang build with intrinsics forced
* eal: fix core number validation
* eal: fix detection of duplicate option register
* eal: fix leak on multi-process request error
* eal: fix log level of error in option register
* eal: fix missing newline in a log
* eal: fix out of bound access when no CPU available
* eal: fix strdup usages in internal config
* eal/linux: fix parsing zero socket memory and limits
* efd: fix tail queue leak
* ethdev: declare Tx prepare API as not experimental
* ethdev: fix errno to have positive value
* ethdev: fix typo in queue setup error log
* eventdev: fix error log in eth Rx adapter
* eventdev: fix eth Tx adapter queue count checks
* eventdev: fix xstats documentation typo
* eventdev: remove redundant timer adapter function prototypes
* examples/bond: fix crash when there is no active slave
* examples/bond: fix initialization order
* examples/flow_filtering: fix example documentation
* examples/ipsec-secgw: fix crypto-op might never get dequeued
* examples/ipsec-secgw: fix inbound SA checking
* examples/ipsec-secgw: fix outbound codepath for single SA
* examples/ipsec-secgw: make local variables static
* examples/kni: fix crash while handling userspace request
* examples/tep_term: remove unused constant
* examples/vhost_crypto: fix bracket
* examples/vhost: fix a typo
* examples/vhost: fix path allocation failure handling
* gro: check invalid TCP header length
* gro: fix overflow of payload length calculation
* gso: fix VxLAN/GRE tunnel checks
* hash: fix out-of-bound write while freeing key slot
* hash: fix return of bulk lookup
* ip_frag: fix IPv6 when MTU sizes not aligned to 8 bytes
* kni: fix build for dev_open in Linux 5.0
* kni: fix build for igb_ndo_bridge_setlink in Linux 5.0
* kni: fix build on RHEL 8
* kni: fix build on RHEL8 for arm and Power9
* malloc: fix deadlock when reading stats
* malloc: fix duplicate mem event notification
* malloc: fix finding maximum contiguous IOVA size
* malloc: make alignment requirements more stringent
* malloc: notify primary process about hotplug in secondary
* mem: check for memfd support in segment fd API
* mem: fix segment fd API error code for external segment
* mem: fix storing old policy
* mem: fix variable shadowing
* memzone: fix unlock on initialization failure
* mk: do not install meson.build in usertools
* mk: fix scope of disabling AVX512F support
* net/af_packet: fix setting MTU decrements sockaddr twice
* net/avf/base: fix comment referencing internal data
* net/bnx2x: cleanup info logs
* net/bnx2x: fix segfaults due to stale interrupt status
* net/bonding: fix possible null pointer reference
* net/bonding: fix reset active slave
* net/cxgbe: fix control queue mbuf pool naming convention
* net/cxgbe: fix overlapping regions in TID table
* net/cxgbe: skip parsing match items with no spec
* net/dpaa2: fix bad check for not-null
* net/dpaa2: fix device init for secondary process
* net/dpaa: fix secondary process
* net/ena: add reset reason in Rx error
* net/ena: add supported RSS offloads types
* net/ena: destroy queues if start failed
* net/ena: do not reconfigure queues on reset
* net/ena: fix cleanup for out of order packets
* net/ena: fix dev init with multi-process
* net/ena: fix errno to positive value
* net/ena: fix invalid reference to variable in union
* net/ena: skip packet with wrong request id
* net/ena: update completion queue after cleanup
* net/enic: remove useless include
* net: fix underflow for checksum of invalid IPv4 packets
* net/fm10k: fix internal switch initial status
* net/i40e: clear VF reset flags after reset
* net/i40e: fix config name in comment
* net/i40e: fix get RSS conf
* net/i40e: fix getting RSS configuration
* net/i40e: fix overwriting RSS RETA
* net/i40e: fix port close
* net/i40e: fix queue region DCB configure
* net/i40e: fix statistics
* net/i40e: fix statistics inconsistency
* net/i40e: fix using recovery mode firmware
* net/i40e: fix VF overwrite PF RSS LUT for X722
* net/i40e: perform basic validation on VF messages
* net/i40e: remove redundant reset of queue number
* net/i40e: revert fix offload not supported mask
* net/ifc: store only registered device instance
* net/ifcvf: fix typo on struct name
* net/igb: fix LSC interrupt when using MSI-X
* net/ixgbe/base: add LHA ID
* net/ixgbe: fix crash on remove
* net/ixgbe: fix over using multicast table for VF
* net/ixgbe: fix overwriting RSS RETA
* net/ixgbe: fix Rx LRO capability offload for x550
* net/mlx5: fix build for armv8
* net/mlx5: fix deprecated library API for Rx padding
* net/mlx5: fix function documentation
* net/mlx5: fix instruction hotspot on replenishing Rx buffer
* net/mlx5: fix Multi-Packet RQ mempool free
* net/mlx5: fix Rx packet padding
* net/mlx5: fix shared counter allocation logic
* net/mlx5: fix TC rule handle assignment
* net/mlx5: fix typos and code style
* net/mlx5: fix validation of Rx queue number
* net/mlx5: fix VXLAN port registration race condition
* net/mlx5: fix VXLAN without decap action for E-Switch
* net/mlx5: remove checks for outer tunnel items on E-Switch
* net/mlx5: support ethernet type for tunnels on E-Switch
* net/mlx5: support tunnel inner items on E-Switch
* net/mlx5: validate ethernet type on E-Switch
* net/mlx5: validate tunnel inner items on E-Switch
* net/netvsc: disable multi-queue on older servers
* net/netvsc: enable SR-IOV
* net/netvsc: fix probe when VF not found
* net/netvsc: fix transmit descriptor pool cleanup
* net/qede: fix performance bottleneck in Rx path
* net/qede: remove prefetch in Tx path
* net/sfc: add missing header guard to TSO header file
* net/sfc/base: fix Tx descriptor max number check
* net/sfc: discard last seen VLAN TCI if Tx packet is dropped
* net/sfc: fix crash in EF10 TSO if no payload
* net/sfc: fix datapath name references in logs
* net/sfc: fix port ID log
* net/sfc: fix Rx packets counter
* net/sfc: fix typo in preprocessor check
* net/sfc: fix VF error/missed stats mapping
* net/sfc: pass HW Tx queue index on creation
* net/tap: add buffer overflow checks before checksum
* net/tap: allow full length names
* net/tap: fix possible uninitialized variable access
* net/tap: let kernel choose tun device name
* net/vhost: fix double free of MAC address
* net/virtio: add barrier before reading the flags
* net/virtio-user: fix used ring in cvq handling
* raw/ifpga: fix memory leak
* Revert "net/mlx5: fix instruction hotspot on replenishing Rx buffer"
* sched: fix memory leak on init failure
* telemetry: fix using ports of different types
* test: check zero socket memory as valid
* test/crypto: fix misleading trace message
* test/fbarray: add to meson
* test/hash: fix perf result
* test/mem: add external mem autotest to meson
* test/metrics: fix a negative case
* timer: fix race condition
* version: 18.11.1-rc1
* version: 18.11.1-rc2
* vfio: allow secondary process to query IOMMU type
* vfio: do not unregister callback in secondary process
* vfio: fix error message
* vhost/crypto: fix possible dead loop
* vhost/crypto: fix possible out of bound access
* vhost: enforce avail index and desc read ordering
* vhost: enforce desc flags and content read ordering
* vhost: ensure event idx is mapped when negotiated
* vhost: fix access for indirect descriptors
* vhost: fix crash after mmap failure
* vhost: fix deadlock in driver unregister
* vhost: fix double read of descriptor flags
* vhost: fix memory leak on realloc failure
* vhost: fix possible dead loop in vector filling
* vhost: fix possible out of bound access in vector filling
* vhost: fix race condition when adding fd in the fdset

18.11.1 Validation
~~~~~~~~~~~~~~~~~~

* Intel(R) Testing

   * 18.11.1 LTS release passed the basic Intel(R) NIC(ixgbe and i40e) testing

   * cryptodev
   * virtio and Intel NIC/virtio performance
   * vlan
   * vxlan
   * Jumbo frames
   * Generic filter
   * Flow director
   * PF and VF

* Mellanox(R) Testing

   * Basic functionality

      * Send and receive multiple types of traffic
      * testpmd xstats counter test
      * testpmd timestamp test

   * Changing/checking link status through testpmd

      * RTE flow and flow_director tests
      * Some RSS tests
      * VLAN stripping and insertion tests
      * Checksum and TSO tests
      * ptype tests
      * Port interrupt testing
      * Multi-process testing

   * Drivers tested

      * MLNX_OFED_LINUX-4.5-1.0.1.0
      * MLNX_OFED_LINUX-4.4-2.0.1.0
      * rdma-core upstream commit 0ea43f6

   * Tested NICs

      * ConnectX-4 Lx (fw 14.24.1000)
      * ConnectX-5 (fw 16.24.1000)

   * OSes tested

      * RHEL7.4 (kernel 5.0.0)
      * RHEL7.4 (kernel 3.10.0-693.el7.x86_64)


* OVS Testing Intel(R)

   * OVS testing against head OVS Master and OVS 2.11.0 with VSPERF
   * Tested with i40e (X710), ixgbe (82599ES) and igb(I350) devices

      * PVP
      * P2P
      * Multiqueue
      * Vhostuserclient reconnect
      * Vhost cross-NUMA awareness
      * Jumbo frames
      * Rate limiting
      * QoS policer

* Microsoft(R) Azure Testing

   * SRIOV/Failsafe
   * DPDK-OVS

* Red Hat(R) Virtualization Testing

   * PF
   * VF
   * vhost single/multi queues and cross-NUMA
   * vhostclient reconnect
   * vhost live migration with single/multi queues and cross-NUMA

18.11.2  Release Notes
----------------------

18.11.2 Fixes
~~~~~~~~~~~~~

* acl: fix compiler flags with meson and AVX2 runtime
* app/bbdev: replace sprintf with snprintf or strlcpy
* app/crypto-perf: check range of socket id
* app/pdump: remove only created vdevs
* app/test: fix build with musl libc
* app/test: fix flags with meson
* app/test: fix sprintf with strlcat
* app/testpmd: add missing newline when showing statistics
* app/testpmd: extend forwarding statistics to 64 bits
* app/testpmd: fix a typo in log message
* app/testpmd: fix help info for interactive commands
* app/testpmd: fix hex string parser support for flow API
* app/testpmd: fix mempool free on exit
* app/testpmd: fix offload flags after port config
* app/testpmd: fix return value check
* app/testpmd: fix stdout flush after printing stats
* app/testpmd: fix Tx QinQ set
* app/testpmd: fix Tx VLAN and QinQ dependency
* app/testpmd: fix typo in comment
* app/testpmd: fix unintentional integer overflow
* app/testpmd: fix variable use before null check
* app/testpmd: remove unused field from port struct
* app/testpmd: remove useless casts on statistics
* bitrate: fix unchecked return value
* build: fix crash by disabling AVX512 with binutils 2.31
* build: fix meson binutils workaround
* build: fix ninja install on FreeBSD
* build: remove meson warning for Arm
* build: use default flags for default Arm machine
* bus/dpaa: fix Rx discard register mask
* bus/fslmc: decrease log level for unsupported devices
* bus/fslmc: fix build with musl libc
* bus/fslmc: fix warning with GCC 9
* bus/fslmc: fix warning with GCC 9
* bus/fslmc: remove unused include of error.h
* bus/vdev: fix debug message on probing
* bus/vdev: fix hotplug twice
* bus/vmbus: fix check for mmap failure
* bus/vmbus: fix resource leak on error
* bus/vmbus: fix secondary process setup
* bus/vmbus: map ring in secondary process
* bus/vmbus: stop mapping if empty resource found
* cfgfile: replace strcat with strlcat
* ci: add a distinguisher to the extra Travis builds
* ci: enable ccache in Travis
* ci: introduce Travis builds for GitHub repositories
* common/cpt: fix null auth only
* compress/isal: fix compression stream initialization
* compress/isal: fix getting information about CPU
* compress/qat: fix setup inter buffers
* crypto/caam_jr: fix memory leak and illegal access
* crypto/caam_jr: fix shared descriptor endianness
* crypto/caam_jr: fix total length in auth only s/g
* cryptodev: fix driver name comparison
* crypto/dpaa2_sec: fix offset calculation for GCM
* crypto/dpaa2_sec: fix session clearing
* crypto/dpaa: fix session destroy
* crypto/kasumi: fix dependency check
* crypto/openssl: fix big numbers after computations
* crypto/openssl: fix modexp
* crypto/qat: fix null cipher algo for non 8-byte multiple
* crypto/snow3g: add to meson build
* crypto/virtio: fix IV offset
* crypto/virtio: use local log type
* crypto/zuc: fix dependency check
* devtools: accept experimental symbol promotion
* devtools: add libelf dependency to build test
* devtools: fix build test on FreeBSD
* devtools: fix check of symbol added as stable API
* devtools: fix result of svg include check
* devtools: fix symbol name in check log
* devtools: fix test of some build options
* devtools: skip meson build for missing compilers
* devtools: support older compilers with meson test
* devtools: test build of zlib PMD
* doc: add flow API to qede NIC features
* doc: add missing algorithms for AESNI-MB PMD
* doc: fix ABI check script examples
* doc: fix a minor typo in testpmd guide
* doc: fix broken link in LPM guide
* doc: fix examples in bonding guide
* doc: fix formatting in testpmd guide
* doc: fix heading levels in bbdev test guide
* doc: fix interactive commands in testpmd guide
* doc: fix JSON interface for power sample
* doc: fix link in Linux getting started guide
* doc: fix links to doxygen and sphinx sites
* doc: fix missing asymmetric crypto table
* doc: fix PCI whitelist typo in prog guide
* doc: fix spelling in testpmd guide
* doc: fix spelling reported by aspell in comments
* doc: fix spelling reported by aspell in guides
* doc: fix tag for inner RSS feature
* doc: fix two typos in contributing guide
* doc: fix typo in IPC guide
* doc: fix typo in mlx5 guide
* doc: fix typos in mlx5 guide
* doc: fix typos in testpmd user guide
* doc: remove reference to rte.doc.mk in programmers guide
* doc: update cross Arm toolchain in Linux guide
* drivers/event: disable OcteonTx for buggy Arm compilers
* drivers: fix SPDX license id consistency
* drivers/net: fix possible overflow using strlcat
* drivers/net: fix shifting 32-bit signed variable 31 times
* drivers/qat: fix queue pair NUMA node
* eal: fix check when retrieving current CPU affinity
* eal: fix cleanup in no-shconf mode
* eal: fix control threads pinnning
* eal: fix core list validation with disabled cores
* eal: fix formatting of hotplug error message
* eal: fix typo in comment of vector function
* eal: initialize alarms early
* eal/linux: fix log levels for pagemap reading failure
* eal/linux: remove thread ID from debug message
* eal/ppc: fix global memory barrier
* eal: remove dead code in core list parsing
* eal: restrict control threads to startup CPU affinity
* eal: support strlcat function
* eal: tighten permissions on shared memory files
* ethdev: fix a typo
* ethdev: fix method name in doxygen comment
* ethdev: fix typo in error messages
* ethdev: remove unused variable
* eventdev: fix crypto adapter
* eventdev: fix Rx adapter event flush
* eventdev: update references to removed function
* event/dsw: fix capability flags
* event/dsw: ignore scheduling type for single-link queues
* event/opdl: replace sprintf with snprintf
* event/sw: fix enqueue checks in self-test
* examples/ethtool: fix two typos
* examples/fips_validation: fix CMAC test
* examples/ip_pipeline: disable build when no epoll
* examples/ipsec-secgw: fix AES-CTR block size
* examples/ipsec-secgw: fix build error log
* examples/ipsec-secgw: fix debug logs
* examples/ipsec-secgw: fix SPD no-match case
* examples/l2fwd-cat: fix build on FreeBSD
* examples/multi_process: fix buffer underrun
* examples/power: fix buffer overrun
* examples/power: fix build with some disabled PMDs
* examples/power: fix json null termination
* examples/power: fix overflowed value
* examples/power: fix resource leak
* examples/power: fix string null termination
* examples/power: fix string overflow
* examples/power: fix unreachable VF MAC init
* examples/vhost_crypto: fix dependency on vhost library
* examples/vhost_scsi: fix null-check for parameter
* hash: fix doc about thread/process safety
* hash: fix position returned in free slots
* hash: fix total entries count
* ipc: add warnings about correct API usage
* ipc: add warnings about not using IPC with memory API
* ipc: fix memory leak on request failure
* ipc: fix send error handling
* ipc: handle more invalid parameter cases
* ipc: harden message receive
* ipc: unlock on failure
* kni: fix build with Linux 5.1
* kni: fix type for MAC address
* maintainers: update for IBM POWER
* malloc: fix documentation of realloc function
* malloc: fix IPC message initialization
* mbuf: fix a typo
* mbuf: update Tx VLAN and QinQ flags documentation
* mem: limit use of address hint
* mempool/dpaa2: fix continuous print on empty pool
* mem: warn user when running without NUMA support
* mk: disable packed member pointer warning for telemetry
* mk: disable warning for packed member pointer
* mk: fix AVX512 disabled warning on non x86
* mk: fix build of shared library with libbsd
* net/atlantic: bad logic with offsets talking with firmware
* net/atlantic: eeprom get/set should consider offset
* net/atlantic: eliminate excessive log levels on Rx/Tx
* net/atlantic: enable broadcast traffic
* net/atlantic: error handling for mailbox access
* net/atlantic: extra line at eof
* net/atlantic: fix buffer overflow
* net/atlantic: fix EEPROM get for small and uneven lengths
* net/atlantic: fix link configuration
* net/atlantic: fix max eeprom size
* net/atlantic: fix missing VLAN filter offload
* net/atlantic: fix negative error codes
* net/atlantic: fix xstats return
* net/atlantic: flow control settings synchronization on rx
* net/atlantic: remove extra checks for error codes
* net/atlantic: remove unused variable
* net/atlantic: use capability bits to detect eeprom access
* net/atlantic: validity check for eeprom dev address
* net/avf: fix admin queue interrupt for ICE
* net/bnx2x: fix DMAE timeout
* net/bnx2x: fix memory leak
* net/bnx2x: fix MTU for jumbo frame
* net/bnx2x: fix optic module verification
* net/bnx2x: fix race for periodic flags
* net/bnx2x: fix ramrod timeout
* net/bnxt: fix big endian build
* net/bnxt: fix Rx VLAN offload flags
* net/bnxt: silence IOVA warnings
* net/bnxt: support IOVA VA mode
* net/bnxt: suppress spurious error log
* net/bonding: avoid warning for invalid port
* net/bonding: fix buffer length when printing strings
* net/bonding: fix LACP negotiation
* net/bonding: fix link status
* net/bonding: fix packet count type for LACP
* net/bonding: fix port id types
* net/bonding: fix queue index types
* net/bonding: fix slave id types
* net/bonding: fix slave Tx burst for mode 4
* net/bonding: fix Tx in 802.3ad mode
* net/bonding: fix values of descriptor limits
* net/cxgbe: fix colliding function names
* net/cxgbe: fix missing checksum flags and packet type
* net/cxgbe: update Chelsio T5/T6 NIC device ids
* net/enetc: fix big endian build and buffer allocation
* net/enetc: fix crash at high speed traffic
* net/enetc: fix SMMU unhandled context fault
* net/enic: allow flow mark ID 0
* net/enic: check for unsupported flow item types
* net/enic: fix endianness in VLAN match
* net/enic: fix flow director SCTP matching
* net/enic: fix inner packet matching
* net/enic: fix max MTU calculation
* net/enic: fix SCTP match for flow API
* net/enic: fix VLAN inner type matching for old hardware
* net/enic: fix VXLAN match
* net/enic: move arguments into struct
* net/enic: reset VXLAN port regardless of overlay offload
* net: fix Tx VLAN flag for offload emulation
* net/fm10k: fix VLAN strip offload flag
* net/i40e: fix dereference before check when getting EEPROM
* net/i40e: fix dereference before null check in mbuf release
* net/i40e: fix link speed for X722
* net/i40e: fix logging on VF close
* net/i40e: fix queue number check
* net/i40e: fix scattered Rx enabling
* net/i40e: fix time sync for 25G
* net/i40e: forbid two RSS flow rules
* net/i40e: log when provided RSS key is not valid
* net/iavf: fix info get
* net/ixgbe: fix warning with GCC 9
* net/ixgbe: restore VLAN filter for VF
* net/kni: fix return value check
* net/mlx4: change device reference for secondary process
* net/mlx4: fix memory region cleanup
* net/mlx5: check Tx queue size overflow
* net/mlx5: fix comments mixing Rx and Tx
* net/mlx5: fix errno typos in comments
* net/mlx5: fix external memory registration
* net/mlx5: fix flow priorities probing error path
* net/mlx5: fix hex dump of error completion
* net/mlx5: fix instruction hotspot on replenishing Rx buffer
* net/mlx5: fix max number of queues for NEON Tx
* net/mlx5: fix memory event on secondary process
* net/mlx5: fix memory region cleanup
* net/mlx5: fix Multi-Packet RQ mempool name
* net/mlx5: fix packet inline on Tx queue wraparound
* net/mlx5: fix release of Rx queue object
* net/mlx5: fix RSS validation function
* net/mlx5: fix sync when handling Tx completions
* net/mlx5: fix Tx metadata for multi-segment packet
* net/mlx: prefix private structure
* net/mlx: remove debug messages on datapath
* net/netvsc: fix include of fcntl.h
* net/netvsc: fix VF support with secondary process
* net/netvsc: remove useless condition
* net/netvsc: reset mbuf port on VF Rx
* net/nfp: check return value
* net/nfp: fix build with musl libc
* net/nfp: fix file descriptor check
* net/nfp: fix memory leak
* net/nfp: fix possible buffer overflow
* net/nfp: fix potential integer overflow
* net/nfp: fix RSS query
* net/nfp: fix setting MAC address
* net/octeontx: fix vdev name
* net/pcap: fix memory leak
* net/qede: fix Rx packet drop
* net/qede: fix Tx packet prepare for tunnel packets
* net/qede: support IOVA VA mode
* net/ring: avoid hard-coded length
* net/ring: check length of ring name
* net/ring: fix coding style
* net/ring: fix return value check
* net/ring: use calloc style where appropriate
* net/sfc: fix logging from secondary process
* net/sfc: fix MTU change to check Rx scatter consistency
* net/sfc: fix speed capabilities reported in device info
* net/sfc: improve TSO header length check in EF10 datapath
* net/sfc: improve TSO header length check in EFX datapath
* net/sfc: log port ID as 16-bit unsigned integer on panic
* net/sfc: remove control path logging from Rx queue count
* net/softnic: fix possible buffer overflow
* net/tap: fix getting max iovec
* net/tap: fix memory leak on IPC request
* net/tap: fix multi process reply buffer
* net/tap: fix multi-process request
* net/tap: fix potential IPC buffer overrun
* net/vdev_netvsc: fix device cast
* net/virtio: add barrier in interrupt enable
* net/virtio: add barriers for extra descriptors on Rx split
* net/virtio: fix buffer leak on VLAN insert
* net/virtio: fix dangling pointer on failure
* net/virtio: fix duplicate naming of include guard
* net/virtio: fix in-order Tx path for split ring
* net/virtio: remove forward declaration
* net/virtio: remove useless condition
* net/virtio: set offload flag for jumbo frames
* net/virtio-user: fix multi-process support
* net/virtio-user: fix multiqueue with vhost kernel
* net/virtio-user: fix return value check
* net/vmxnet3: add VLAN filter capability
* power: fix cache line alignment
* power: fix governor storage to trim newlines
* power: fix thread-safety environment modification
* power: remove unused variable
* raw/dpaa2_cmdif: fix warnings with GCC 9
* raw/dpaa2_qdma: fix spin lock release
* raw/dpaa2_qdma: fix to support multiprocess execution
* raw/ifpga: fix file descriptor leak in error path
* raw/ifpga: modify log output
* raw/skeleton: fix warnings with GCC 9
* Revert "app/testpmd: fix offload flags after port config"
* ring: enforce reading tail before slots
* ring: fix an error message
* ring: fix namesize macro documentation block
* rwlock: reimplement with atomic builtins
* spinlock: reimplement with atomic one-way barrier
* table: fix arm64 hash function selection
* telemetry: fix mapping of statistics
* test/barrier: fix allocation check
* test/barrier: fix for Power CPUs
* test/barrier: fix typo in log
* test/bonding: fix MAC assignment for re-run
* test: clean remaining trace of devargs autotest
* test/compress: fix missing header include
* test/crypto: fix duplicate id used by CCP device
* test/crypto: fix possible overflow using strlcat
* test/distributor: replace sprintf with strlcpy
* test/event: replace sprintf with snprintf
* test/hash: replace sprintf with snprintf
* test/pmd_perf: fix the way to drain the port
* test/spinlock: amortize the cost of getting time
* test/spinlock: remove delay for correct benchmarking
* version: 18.11.2-rc1
* vfio: document multiprocess limitation for container API
* vhost/crypto: fix parens
* vhost: fix device leak on connection add failure
* vhost: fix interrupt suppression for the split ring
* vhost: fix null pointer checking
* vhost: fix passing destroyed device to destroy callback
* vhost: fix potential use-after-free for memory region
* vhost: fix potential use-after-free for zero copy mbuf
* vhost: fix silent queue enabling with legacy guests
* vhost: fix sprintf with snprintf
* vhost: prevent disabled rings to be processed with zero-copy
* vhost: restore mbuf first when freeing zmbuf

18.11.2 Validation
~~~~~~~~~~~~~~~~~~

* IBM(R) Testing

   * Tests run

      * Single port stability test using l3fwd (16 cpus) and TRex
      * 64 and 1500 byte packets at a 0.0% drop rate for 4 hours each

   * System tested

      * IBM Power9 Model 8335-101 CPU: 2.3 (pvr 004e 1203)

   * Tested NICs

      * ConnectX-5 (fw 16.23.1020).

   * OS Tested

      * Ubuntu 18.04.2 LTS (kernel 4.15.0-47-generic)

* Intel(R) Openvswitch Testing

   * OVS testing against head OVS Master and OVS 2.11.0 with VSPERF

   * Tested NICs

      * i40e (X710) and i40eVF
      * ixgbe (82599ES) and ixgbeVF
      * igb (I350) and igbVF

   * Functionality

      * P2P
      * PVP
      * PVVP
      * PV-PV in parallel
      * Hotplug
      * Multiqueue
      * Vhostuserclient reconnect
      * Vhost cross-NUMA awareness
      * Jumbo frames
      * Rate limiting
      * QoS policer

* Mellanox(R) Testing

   * Basic functionality

      * Send and receive multiple types of traffic
      * testpmd xstats counter test
      * testpmd timestamp test
      * Changing/checking link status through testpmd
      * RTE flow and flow_director tests
      * Some RSS tests
      * VLAN stripping and insertion tests
      * Checksum and TSO tests
      * ptype tests
      * Port interrupt testing
      * Multi-process testing

   * OFED versions tested

      * MLNX_OFED_LINUX-4.5-1.0.1.0
      * MLNX_OFED_LINUX-4.6-1.0.1.1

   * Tested NICs

      * ConnectX-4 Lx (fw 14.25.1010).
      * ConnectX-5 (fw 16.25.1010).

   * OS tested

   * RHEL7.4 (kernel 3.10.0-693.el7.x86_64).

* Microsoft(R) Azure Testing

   * Images

      * Canonical UbuntuServer 16.04-LTS latest
      * Canonical UbuntuServer 18.04-DAILY-LTS latest
      * RedHat RHEL 7-RAW latest
      * RedHat RHEL 7.5 latest
      * Openlogic CentOS 7.5 latest
      * SUSE SLES 15 latest

   * Drivers

      * Mellanox and netvsc poll-mode drivers

   * Functionality

      * VM to VM traffic
      * SRIOV/Failsafe
      * Single and multicore performance

* Red Hat(R) Testing

   * RHEL 7 and RHEL 8
   * Functionality

      * PF
      * VF
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

* Intel(R) Testing

   * Basic Intel(R) NIC(ixgbe and i40e) testing

      * vlan
      * vxlan
      * Jumbo frames
      * Generic filter
      * Flow director
      * PF and VF
      * Intel NIC single core/NIC performance

   * Basic cryptodev and virtio testing

      * cryptodev
      * vhost/virtio basic loopback, PVP and performance test

18.11.2 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.2 contains fixes up to DPDK v19.05. Issues identified/fixed in DPDK master branch after DPDK v19.05 may be present in DPDK 18.11.2
* testpmd: queue specific offloads may be over-written by the default config. This is not a regression from earlier DPDK 18.11 releases.

Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f net/iavf: fix queue interrupt for ice (18.02)
* 281bd1aa3 net/iavf: fix stats reset (18.02)
* fe252fb69 test/rwlock: benchmark on all available cores (1.2.3r0)
* 6fef1ae4f test/rwlock: amortize the cost of getting time (1.2.3r0)

18.11.3 Release Notes
---------------------

18.11.3 Fixes
~~~~~~~~~~~~~

* acl: fix build with some arm64 compiler
* acl: fix undefined behavior of bit shifts
* app/crypto-perf: check lcore job failure
* app/crypto-perf: fix CSV format
* app/crypto-perf: fix display once detection
* app/eventdev: fix order test port creation
* app/testpmd: fix eth packet dump for small buffers
* app/testpmd: fix latency stats deinit on signal
* app/testpmd: fix MPLS IPv4 encapsulation fields
* app/testpmd: fix offloads config
* app/testpmd: fix parsing RSS queue rule
* app/testpmd: fix queue offload configuration
* app/testpmd: fix show port info routine
* app/testpmd: rename ambiguous VF config variable
* bpf: fix check array size
* bpf: fix pseudo calls for program loaded from ELF
* bpf: fix validate for function return value
* build: add libatomic dependency for 32-bit clang
* build: enable BSD features visibility for FreeBSD
* build: enable large file support on 32-bit
* build: remove unnecessary large file support defines
* build: set RTE_ARCH_64 based on pointer size
* bus/fslmc: fix build with 0 headroom
* bus/pci: fix TOCTOU for sysfs access
* bus/pci: remove unused x86 Linux constant
* bus/vmbus: skip non-network devices
* compress/isal: fix use after free
* compress/zlib: fix error handling
* config: disable armv8 crypto extension
* cryptodev: fix typo in comment
* crypto/dpaa2_sec: fix handling of session init failure
* crypto/mvsam: fix typo in comment
* crypto/openssl: fix free of asymmetric crypto keys
* crypto/openssl: fix usage of non constant time memcmp
* crypto/openssl: remove useless check before freeing
* crypto/qat: set message field to zero in sym SGL case
* crypto/virtio: check PCI config read
* devtools: fix building kernel component tags
* distributor: fix check of workers number
* distributor: fix livelock on flush
* doc: add a note for multi-process in mempool guide
* doc: add co-existence consideration for bnx2x
* doc: add co-existence consideration for qede
* doc: clarify data plane error handling in compressdev
* doc: cleanup test removal in armv8 and openssl guides
* doc: fix a grammar mistake in rawdev guide
* doc: fix build with latest meson
* doc: fix ethernet addresses in flow API guide
* doc: fix grammar in prog guides
* doc: fix link about bifurcated model in Linux guide
* doc: fix Linux guide for arm64 cross-compilation
* doc: fix PDF build
* doc: fix PDF with greek letter
* doc: fix triplicated typo in prog guides
* doc: fix typo in EAL guide
* doc: fix typos in flow API guide
* doc: remove useless Rx configuration in l2fwd guide
* doc: robustify PDF build
* doc: update features supported by mlx
* drivers: fix typo in NXP comments
* drivers/net: fix double free on init failure
* eal: correct log for alarm error
* eal: fix control thread affinity with --lcores
* eal: fix positive error codes from probe/remove
* eal: fix typo in comments
* eal/freebsd: fix config creation
* eal/freebsd: fix init completion
* eal: hide internal function
* eal: hide internal hotplug function
* eal: increase maximum different hugepage sizes on Arm
* eal/linux: fix return after alarm registration failure
* ethdev: avoid error on PCI unplug of closed port
* ethdev: avoid getting uninitialized info for bad port
* ethdev: fix endian annotation for SPI item
* ethdev: fix Tx prepare documentation to use positive errno
* eventdev: fix doxygen comment
* eventdev: fix error sign
* event/dpaa2: fix timeout ticks
* event/opdl: fix error sign
* event/sw: fix error sign
* examples/bpf: fix build
* examples: fix make clean when using pkg-config
* examples: fix pkg-config detection with older make
* examples: fix use of ethdev internal device array
* examples/ip_frag: fix stale content of ethdev info
* examples/ip_frag: fix unknown ethernet type
* examples/ip_frag: fix use of ethdev internal device array
* examples/ip_fragmentation: fix Tx queues init
* examples/ip_frag: remove Tx fast free offload flag
* examples/ipsec-secgw: fix error sign
* examples/ipsec-secgw: fix inline modes
* examples/ipsec-secgw: fix use of ethdev internal struct
* examples/l3fwd: fix unaligned memory access on x86
* examples/l3fwd-vf: remove unused Rx/Tx configuration
* examples/multi_process: do not dereference global config struct
* examples/multi_process: fix FreeBSD build
* examples/performance-thread: init timer subsystem
* examples/power: fix FreeBSD meson lib dependency
* examples/power: fix strcpy buffer overrun
* examples/ptpclient: fix delay request message
* examples/qos_sched: do not dereference global config struct
* examples/tep_term: remove duplicate definitions
* examples/vdpa: remove trace of legacy linuxapp
* examples/vhost_crypto: remove unused function
* fix off-by-one errors in snprintf
* flow_classify: fix out-of-bounds access
* hash: use ordered loads only if signature matches
* igb_uio: fix build on Linux 5.3 for fall through
* ip_frag: fix IPv6 fragment size calculation
* kernel/freebsd: fix module build on latest head
* kernel/linux: fix modules install path
* kni: abort when IOVA is not PA
* kni: fix build on RHEL8
* kni: fix copy_from_user failure handling
* kni: fix kernel 5.4 build - merged pci_aspm.h
* kni: fix kernel 5.4 build - num_online_cpus
* kni: fix kernel 5.4 build - skb_frag_t to bio_vec
* kni: fix kernel crash with multi-segments
* kni: fix segmented mbuf data overflow
* kni: fix style
* mem: ease init in a docker container
* mem: fix typo in API description
* mem: mark unused function in 32-bit builds
* mem: remove incorrect experimental tag on static symbol
* mk: fix custom kernel directory name
* net: adjust L2 length on soft VLAN insertion
* net/af_packet: remove redundant declaration
* net/ark: fix queue packet replacement
* net/ark: remove unnecessary cast
* net/atlantic: fix Tx prepare to set positive rte_errno
* net/atlantic: remove unnecessary cast
* net/avf: fix address of first segment
* net/avf: fix driver crash when enable TSO
* net/avf: fix endless loop
* net/avf: fix Rx bytes stats
* net/axgbe: remove unnecessary cast
* net/bnx2x: fix fastpath SB allocation for SRIOV
* net/bnx2x: fix interrupt flood
* net/bnx2x: fix invalid free on unplug
* net/bnx2x: fix link events polling for SRIOV
* net/bnx2x: fix link state
* net/bnx2x: fix memory leak
* net/bnx2x: fix packet drop
* net/bnx2x: fix reading VF id
* net/bnx2x: fix supported max Rx/Tx descriptor count
* net/bnx2x: fix warnings from invalid assert
* net/bnxt: check for error conditions in Tx path
* net/bnxt: check for null completion ring doorbell
* net/bnxt: check invalid VNIC id for firmware
* net/bnxt: check invalid VNIC in cleanup path
* net/bnxt: fix adding MAC address
* net/bnxt: fix checking result of HWRM command
* net/bnxt: fix check of address mapping
* net/bnxt: fix compiler warning
* net/bnxt: fix crash on probe failure
* net/bnxt: fix device init error path
* net/bnxt: fix enabling/disabling interrupts
* net/bnxt: fix endianness in ring macros
* net/bnxt: fix error handling in port start
* net/bnxt: fix extended port counter statistics
* net/bnxt: fix getting statistics
* net/bnxt: fix icc build
* net/bnxt: fix interrupt rearm logic
* net/bnxt: fix interrupt vector initialization
* net/bnxt: fix L4 checksum error indication in Rx
* net/bnxt: fix lock release on getting NVM info
* net/bnxt: fix return values to standard error codes
* net/bnxt: fix ring type macro name
* net/bnxt: fix RSS RETA indirection table ops
* net/bnxt: fix Rx interrupt vector
* net/bnxt: fix RxQ count if ntuple filtering is disabled
* net/bnxt: fix setting primary MAC address
* net/bnxt: fix TSO
* net/bnxt: fix Tx batching
* net/bnxt: fix Tx hang after port stop/start
* net/bnxt: fix unconditional wait in link update
* net/bnxt: fix variable width in endian conversion
* net/bnxt: fix xstats
* net/bnxt: optimize Tx batching
* net/bnxt: reduce verbosity of a message
* net/bnxt: remove unnecessary cast
* net/bnxt: remove unnecessary interrupt disable
* net/bnxt: reset filters before registering interrupts
* net/bnxt: retry IRQ callback deregistration
* net/bnxt: save the number of EM flow count
* net/bonding: remove unnecessary cast
* net/cxgbe: do not dereference global config struct
* net/cxgbe: remove unnecessary cast
* net: define IPv4 IHL and VHL
* net/dpaa2: fix multi-segment Tx
* net/dpaa: check multi-segment external buffers
* net/dpaa: fix build with 0 headroom
* net/e1000: fix buffer overrun while i219 processing DMA
* net/e1000: fix Tx prepare to set positive rte_errno
* net/e1000: remove unnecessary cast
* net/ena: fix admin CQ polling for 32-bit
* net/ena: fix assigning NUMA node to IO queue
* net/ena: fix L4 checksum Tx offload
* net/ena: fix Rx checksum errors statistics
* net/ena: remove unnecessary cast
* net/enic: fix Tx prepare to set positive rte_errno
* net/enic: remove flow count action support
* net/enic: remove flow locks
* net/enic: remove unnecessary cast
* net/failsafe: fix reported device info
* net: fix definition of IPv6 traffic class mask
* net: fix encapsulation markers for inner L3 offset
* net: fix how L4 checksum choice is tested
* net/fm10k: advertise supported RSS hash function
* net/fm10k: fix address of first segment
* net/fm10k: fix descriptor filling in vector Tx
* net/fm10k: fix stats crash in multi-process
* net/fm10k: fix Tx prepare to set positive rte_errno
* net/i40e: fix address of first segment
* net/i40e: fix crash when TxQ/RxQ set to 0 in VF
* net/i40e: fix dropped packets statistics name
* net/i40e: fix ethernet flow rule
* net/i40e: fix flow director rule destroy
* net/i40e: fix MAC removal check
* net/i40e: fix RSS hash update for X722 VF
* net/i40e: fix SFP X722 with FW4.16
* net/i40e: fix Tx prepare to set positive rte_errno
* net/i40e: fix Tx threshold setup
* net/i40e: fix unexpected skip FDIR setup
* net/i40e: remove empty queue stats mapping set devops
* net/i40e: remove unnecessary cast
* net/iavf: fix Tx prepare to set positive rte_errno
* net/ixgbe/base: fix product version check
* net/ixgbe: fix address of first segment
* net/ixgbe: fix IP type for crypto session
* net/ixgbe: fix RETA size for VF
* net/ixgbe: fix Tx prepare to set positive rte_errno
* net/ixgbe: fix Tx threshold setup
* net/ixgbe: fix unexpected link handler
* net/ixgbe: remove unnecessary cast
* net/ixgbevf: add full link status check option
* net/mlx4: fix crash on info query in secondary process
* net/mlx5: check memory allocation in flow creation
* net/mlx5: fix 32-bit build
* net/mlx5: fix condition for link update fallback
* net/mlx5: fix crash for empty raw encap data
* net/mlx5: fix crash on null operation
* net/mlx5: fix description of return value
* net/mlx5: fix device arguments error detection
* net/mlx5: fix link speed info when link is down
* net/mlx5: fix memory free on queue create error
* net/mlx5: fix missing validation of null pointer
* net/mlx5: fix order of items in NEON scatter
* net/mlx5: fix typos in comments
* net/mlx5: fix validation of VLAN PCP item
* net/mlx5: fix VLAN inner type matching on DR/DV
* net/mlx5: remove redundant item from union
* net/mlx5: remove unnecessary cast
* net/mlx5: report imissed statistics
* net/mvneta: fix ierror statistics
* net/netvsc: fix definition of offload values
* net/netvsc: fix RSS offload settings
* net/netvsc: fix xstats for VF device
* net/netvsc: fix xstats id
* net/netvsc: initialize VF spinlock
* net/nfp: disable for 32-bit meson builds
* net/null: remove redundant declaration
* net/pcap: fix concurrent multiseg Tx
* net/pcap: fix possible mbuf double freeing
* net/pcap: fix Rx with small buffers
* net/pcap: fix Tx return count in error conditions
* net/pcap: remove redundant declaration
* net/qede: fix Tx prepare to set positive rte_errno
* net/qede: fix warnings from invalid assert
* net/ring: remove redundant declaration
* net/sfc/base: enable chained multicast on all EF10 cards
* net/sfc/base: fix shift by more bits than field width
* net/sfc/base: fix signed/unsigned mismatch
* net/sfc: ensure that device is closed on removal
* net/sfc: fix align to power of 2 when align has smaller type
* net/sfc: fix power of 2 round up when align has smaller type
* net/sfc: unify power of 2 alignment check macro
* net/tap: remove redundant declarations
* net/thunderx: fix crash on detach
* net/vhost: remove redundant declaration
* net/virtio: add Tx preparation
* net/virtio: fix build
* net/virtio: fix build with 0 headroom
* net/virtio: fix in-order Rx with segmented packet
* net/virtio: fix memory leak in in-order Rx
* net/virtio: fix queue memory leak on error
* net/virtio: move VLAN tag insertion to Tx prepare
* net/virtio: remove useless check on mempool
* net/virtio: unmap device on initialization error
* net/virtio: unmap port IO for legacy device
* net/virtio_user: remove redundant declaration
* net/vmxnet3: fix Tx prepare to set positive rte_errno
* net/vmxnet3: fix uninitialized variable
* raw/dpaa2_cmdif: remove redundant declaration
* raw/ifpga/base: fix physical address info
* raw/ifpga/base: fix use of untrusted scalar value
* raw/skeleton: fix test of attribute set/get
* raw/skeleton: remove redundant declaration
* security: remove duplicated symbols from map file
* table: fix crash in LPM IPv6
* telemetry: add missing header include
* telemetry: fix build
* telemetry: fix build warnings seen when using gcc 9
* telemetry: fix memory leak
* test: add rawdev autotest to meson
* test/distributor: fix flush with worker shutdown
* test/eal: fix --socket-mem option
* test: enable installing app with meson
* test/eventdev: fix producer core validity checks
* test: fix autotest crash
* test/flow_classify: fix undefined behavior
* test/hash: fix data reset on new run
* test/hash: fix off-by-one check on core count
* test/hash: rectify slave id to point to valid cores
* test/hash: use existing lcore API
* test: remove link to ixgbe/i40e with meson
* test/rwlock: amortize the cost of getting time
* test/rwlock: benchmark on all available cores
* usertools: fix input handling in telemetry script
* usertools: fix refresh binding infos
* usertools: replace unsafe input function
* version: 18.11.3-rc1
* version: 18.11.3-rc2
* vfio: remove incorrect experimental tag
* vfio: use contiguous mapping for IOVA as VA mode
* vhost/crypto: fix inferred misuse of enum
* vhost/crypto: fix logically dead code
* vhost: fix missing include

18.11.3 Validation
~~~~~~~~~~~~~~~~~~

* Intel(R) Testing

   * Basic Intel(R) NIC(ixgbe and i40e) testing

      * PF (i40e)
      * PF (ixgbe)
      * VF
      * Compile Testing
      * Intel NIC single core/NIC performance

   * Basic cryptodev and virtio testing

      * cryptodev
      * vhost/virtio basic loopback, PVP and performance test

* Red Hat(R) Testing

   * RHEL 8
   * Functionality

      * PF
      * VF
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

* IBM(R) - DPDK on Power result

   * Basic PF on Mellanox: No new errors or regressions seen
   * Performance: no degradation compared to 18.11.2

   * System tested

      * IBM Power9 Model 8335-101 CPU: 2.3 (pvr 004e 1203)

   * Tested NICs

      * Mellanox Technologies MT28800 Family [ConnectX-5 Ex]
      * firmware version: 16.26.292
      * MLNX_OFED_LINUX-4.7-1.0.0.1

* Intel(R) Openvswitch Testing (Ian Stokes)

   * OVS testing against head OVS Master, 2.12.2 and 2.11.3 with VSPERF

   * Tested NICs

      * i40e (X710) and i40eVF
      * ixgbe (82599ES) and ixgbeVF
      * igb (I350) and igbVF

   * Functionality

      * P2P
      * PVP
      * PVVP
      * PV-PV in parallel
      * Hotplug
      * Multiqueue
      * Vhostuserclient reconnect
      * Vhost cross-NUMA awareness
      * Jumbo frames
      * Rate limiting
      * QoS policer

* Mellanox(R) Testing

   * Basic functionality with testpmd

      * ConnectX-5

         * RHEL 7.4
         * Kernel 5.3.0
         * Driver rdma-core v25.1
         * fw 16.26.1040

      * ConnectX-4 Lx

         * RHEL 7.4
         * Kernel 5.3.0
         * Driver rdma-core v25.1
         * fw 14.26.1040

      * ConnectX-5

         * RHEL 7.4
         * Kernel 3.10.0-693.el7.x86_64
         * Driver MLNX_OFED_LINUX-4.7-1.0.0.1
         * fw 16.26.1040

      * ConnectX-4 Lx

         * RHEL 7.4
         * Kernel 3.10.0-693.el7.x86_64
         * Driver MLNX_OFED_LINUX-4.7-1.0.0.1
         * fw 14.26.1040

* Microsoft(R) Azure Testing

   * Images

      * Canonical UbuntuServer 16.04-LTS latest
      * Canonical UbuntuServer 18.04-DAILY-LTS latest
      * RedHat RHEL 7-RAW latest
      * RedHat RHEL 7.5 latest
      * Openlogic CentOS 7.5 latest
      * SUSE SLES 15 latest

   * Drivers

      * Mellanox and netvsc poll-mode drivers

   * Functionality

      * VM to VM traffic
      * SRIOV/Failsafe
      * Single core performance
      * Multicore performance - see known issues below

18.11.3 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.3 contains fixes up to DPDK v19.08. Issues identified/fixed in DPDK master branch after DPDK v19.08 may be present in DPDK 18.11.3
* Microsoft validation team testing saw a performance drop with some multicore tests compared to previous testing at time of DPDK 18.11.2 release. A re-run of DPDK 18.11.2 showed a similar drop, so it is not believed that there is any regression in DPDK.

18.11.3 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f net/iavf: fix queue interrupt for ice
* 281bd1aa3 net/iavf: fix stats reset
* b149a7064 eal/freebsd: add config reattach in secondary process
* 6f43682e0 test/hash: init parameters in the correct function
* a135e050a examples/ipsec-secgw: fix packet length
* b0437f8b0 hash: load value after full key compare
* 9d10f53e4 test/metrics: fix second run
* 2967612f4 test/crypto: fix session init failure for wireless case
* 96b0931d5 net/bnxt: fix extended port counter statistics
* 72aaa312e net/bnxt: fix VF probe when MAC address is zero
* fe7848521 net/bnxt: fix doorbell register offset for Tx ring
* 1f3cea004 net/bnxt: fix check of address mapping
* 324c56551 net/bnxt: fix error checking of FW commands
* b4e190d55 net/bnxt: fix MAC/VLAN filter allocation
* ea81c1b81 net/mlx5: fix NVGRE matching
* dbda2092d net/i40e: fix request queue in VF
* 721c95301 net/mlx5: fix Rx scatter mode validation
* aa2c00702 net/bnxt: fix traffic stall on Rx queue stop/start

18.11.4 Release Notes
---------------------

18.11.4 Fixes
~~~~~~~~~~~~~

* vhost: fix possible denial of service by leaking FDs
* vhost: fix possible denial of service on SET_VRING_NUM

18.11.4 Validation
~~~~~~~~~~~~~~~~~~

* Security patches tested for CVE-2019-14818

18.11.4 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.4 contains fixes up to DPDK v19.08 plus some security fixes. Issues identified/fixed in DPDK master branch after DPDK v19.08 may be present in DPDK 18.11.4

18.11.4 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f net/iavf: fix queue interrupt for ice
* 281bd1aa3 net/iavf: fix stats reset
* b149a7064 eal/freebsd: add config reattach in secondary process
* 6f43682e0 test/hash: init parameters in the correct function
* a135e050a examples/ipsec-secgw: fix packet length
* b0437f8b0 hash: load value after full key compare
* 9d10f53e4 test/metrics: fix second run
* 2967612f4 test/crypto: fix session init failure for wireless case
* 96b0931d5 net/bnxt: fix extended port counter statistics
* 72aaa312e net/bnxt: fix VF probe when MAC address is zero
* fe7848521 net/bnxt: fix doorbell register offset for Tx ring
* 1f3cea004 net/bnxt: fix check of address mapping
* 324c56551 net/bnxt: fix error checking of FW commands
* b4e190d55 net/bnxt: fix MAC/VLAN filter allocation
* ea81c1b81 net/mlx5: fix NVGRE matching
* dbda2092d net/i40e: fix request queue in VF
* 721c95301 net/mlx5: fix Rx scatter mode validation
* aa2c00702 net/bnxt: fix traffic stall on Rx queue stop/start

18.11.5 Release Notes
---------------------

18.11.5 Fixes
~~~~~~~~~~~~~

* vhost: fix vring requests validation broken if no FD

18.11.5 Validation
~~~~~~~~~~~~~~~~~~

* The fix was tested by Intel via the virtio/vhost regression tests

   * http://doc.dpdk.org/dts/test_plans/virtio_pvp_regression_test_plan.html
   * http://doc.dpdk.org/dts/test_plans/vhost_dequeue_zero_copy_test_plan.html
   * http://doc.dpdk.org/dts/test_plans/vm2vm_virtio_pmd_test_plan.html

18.11.5 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.5 contains fixes up to DPDK v19.08 plus a security fix for CVE-2019-14818. Issues identified/fixed in DPDK master branch after DPDK v19.08 may be present in DPDK 18.11.5

18.11.5 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f net/iavf: fix queue interrupt for ice
* 281bd1aa3 net/iavf: fix stats reset
* b149a7064 eal/freebsd: add config reattach in secondary process
* 6f43682e0 test/hash: init parameters in the correct function
* a135e050a examples/ipsec-secgw: fix packet length
* b0437f8b0 hash: load value after full key compare
* 9d10f53e4 test/metrics: fix second run
* 2967612f4 test/crypto: fix session init failure for wireless case
* 96b0931d5 net/bnxt: fix extended port counter statistics
* 72aaa312e net/bnxt: fix VF probe when MAC address is zero
* fe7848521 net/bnxt: fix doorbell register offset for Tx ring
* 1f3cea004 net/bnxt: fix check of address mapping
* 324c56551 net/bnxt: fix error checking of FW commands
* b4e190d55 net/bnxt: fix MAC/VLAN filter allocation
* ea81c1b81 net/mlx5: fix NVGRE matching
* dbda2092d net/i40e: fix request queue in VF
* 721c95301 net/mlx5: fix Rx scatter mode validation
* aa2c00702 net/bnxt: fix traffic stall on Rx queue stop/start

18.11.6 Release Notes
---------------------

18.11.6 Fixes
~~~~~~~~~~~~~

* app/crypto-perf: fix input of AEAD decrypt
* app/eventdev: check function errors
* app/eventdev: fix divide by zero
* app/procinfo: use strlcpy for copying string
* app/testpmd: block xstats for hidden ports
* app/testpmd: fix crash on port reset
* app/testpmd: fix CRC strip command
* app/testpmd: fix help for loop topology option
* app/testpmd: fix invalid port detaching
* app/testpmd: fix Tx checksum when TSO enabled
* app/testpmd: remove duplicated Rx offload commands
* app/testpmd: report invalid command line parameter
* build: avoid overlinking
* build: remove redundant libs from pkgconfig
* build: remove unneeded meson option
* buildtools: fix build with coverage
* bus/dpaa: fix dpaa_sec blacklist
* bus/fslmc: fix global variable multiple definitions
* bus/pci: align next mapping address on page boundary
* bus/pci: fix Intel IOMMU sysfs access check
* bus/pci: remove useless link dependency on ethdev
* ci: add missing dependencies for documentation
* ci: use meson 0.47.1
* common/cpt: fix possible null dereference
* common/dpaax: fallback to check separate memory node for VM
* common/octeontx: add missing public symbol
* compress/octeontx: fix global variable multiple definitions
* compress/octeontx: remove commented out code
* crypto/ccp: fix CPU authentication crash
* crypto/ccp: fix digest size capabilities
* crypto/ccp: fix maximum queues and burst size
* crypto/ccp: fix scheduling of burst
* cryptodev: fix checks related to device id
* cryptodev: fix initialization on multi-process
* crypto/dpaa2_sec: fix length retrieved from hardware
* crypto/dpaa_sec: fix auth-cipher check for AEAD
* crypto/dpaa_sec: fix IOVA table
* crypto/null: fix global variable multiple definitions
* crypto/octeontx: enable unbinding
* crypto/openssl: use local copy for session contexts
* crypto/qat: fix AES CMAC mininum digest size
* crypto/qat: fix digest length in XCBC capability
* crypto/qat: fix null auth when using VFIO
* crypto/virtio: fix global variable multiple definitions
* devtools: fix cleanup of checkpatch temporary file
* doc: fix a common typo in NIC guides
* doc: fix AESNI-GCM limitations in crypto guide
* doc: fix description of links to EAL options pages
* doc: fix description of versioning macros
* doc: fix format in virtio guide
* doc: fix internal links for older releases
* doc: fix l2fwd-crypto usage in CCP guide
* doc: fix link to AESNI mb external library
* doc: fix tap guide
* doc: fix typo in l2fwd-crypto guide
* doc: fix typo in virtio in-order Rx function name
* doc/guides: clean repeated words
* doc: update arm64 cross build tool version
* drivers/crypto: remove some invalid comments
* eal: add ack interrupt API
* eal: drop unused macros for primary process check
* eal: fix header file install with meson
* eal/linux: restore specific hugepage ordering for ppc
* eal/ppc: fix 64-bit atomic exchange operation
* eal: remove dead code on NUMA node detection
* eal: remove legacy PMD log macro
* eal/x86: force inlining of all memcpy and mov helpers
* ethdev: avoid undefined behaviour on configuration copy
* ethdev: fix doc reference to FDIR disabled mode
* ethdev: fix expand RSS flows
* ethdev: fix include of ethernet header file
* ethdev: fix item expansion for RSS flow
* ethdev: fix last item detection on RSS flow expand
* ethdev: fix typos for ENOTSUP
* ethdev: limit maximum number of queues
* ethdev: remove redundant device info cleanup before get
* eventdev: fix possible use of uninitialized var
* eventdev: use same log macro for all unsupported calls
* event/dpaa2: fix default queue configuration
* event/dpaa: fix number of supported atomic flows
* event/octeontx: fix partial Rx packet handling
* event/opdl: remove commented out code
* event/sw: fix xstats reset value
* examples/fips_validation: fix auth verify
* examples/fips_validation: fix null dereferences
* examples: hide error for missing pkg-config path flag
* examples/ipsec-secgw: fix access to freed packet
* examples/ipsec-secgw: fix default configuration
* examples/ipsec-secgw: fix GCM IV length
* examples/ipsec-secgw: fix SHA256-HMAC digest length
* examples/ipsec-secgw: fix unchecked return value
* examples/l3fwd-power: fix Rx interrupt disabling
* examples/multi_process: check server port validity
* examples/multi_process: fix client crash with sparse ports
* examples/vm_power: fix build without i40e
* examples/vm_power: fix OOB frequency oscillations
* examples/vm_power: fix type of cmdline token in cli
* kni: fix ethtool build for gcc 9
* lib/distributor: fix deadlock on aarch64
* lib: fix doxygen typos
* lib: fix log typos
* malloc: fix memory element size in case of padding
* malloc: fix realloc copy size
* malloc: fix realloc padded element size
* mempool: use actual IOVA addresses when populating
* mk: fix build on arm64
* mk: remove library search path from binary
* net/af_packet: fix stale sockets
* net/af_packet: improve Tx statistics accuracy
* net/atlantic: add FW mailbox guard mutex
* net/atlantic: remove double function declaration
* net/axgbe: fix double unlock
* net/bnxt: advertise scatter Rx offload capability
* net/bnxt: cleanup comments
* net/bnxt: enforce IO barrier for doorbell command
* net/bnxt: expose some missing counters in port stats
* net/bnxt: fix accessing variable before null check
* net/bnxt: fix async link handling and update
* net/bnxt: fix check of address mapping
* net/bnxt: fix coding style
* net/bnxt: fix crash after removing and adding slaves
* net/bnxt: fix crash in xstats get
* net/bnxt: fix debug log level
* net/bnxt: fix deferred start of Tx queues
* net/bnxt: fix dereference before null check
* net/bnxt: fix doorbell register offset for Tx ring
* net/bnxt: fix error checking of FW commands
* net/bnxt: fix extended port counter statistics
* net/bnxt: fix flow flush handling
* net/bnxt: fix flow steering
* net/bnxt: fix forwarding with higher mbuf size
* net/bnxt: fix IP checksum error indication
* net/bnxt: fix L4 checksum indication in non-vector Rx
* net/bnxt: fix MAC/VLAN filter allocation
* net/bnxt: fix mbuf free when clearing Tx queue
* net/bnxt: fix multicast filter programming
* net/bnxt: fix ping with MTU change
* net/bnxt: fix resource qcaps with older FW
* net/bnxt: fix Rx queue count
* net/bnxt: fix rx queue start/stop
* net/bnxt: fix setting default MAC address
* net/bnxt: fix setting max RSS contexts
* net/bnxt: fix stats errors handling
* net/bnxt: fix VF probe when MAC address is zero
* net/bnxt: fix writing MTU to FW
* net/bnxt: get default HWRM command timeout from FW
* net/bnxt: move macro definitions to header file
* net/bnxt: remove commented out code
* net/bnxt: remove duplicate barrier
* net/bnxt: remove unnecessary variable assignment
* net/bnxt: replace memory barrier for doorbell response
* net/bnxt: return error if setting link up fails
* net/bnxt: update trusted VF status only when it changes
* net/bonding: fix LACP fast queue Rx handler
* net/bonding: fix link speed update in broadcast mode
* net/bonding: fix OOB access in other aggregator modes
* net/bonding: fix out of bound access in LACP mode
* net/bonding: fix port ID check
* net/bonding: fix selection logic
* net/bonding: fix slave id types
* net/bonding: fix unicast packets filtering
* net/bonding: use non deprecated PCI API
* net/cxgbe: add prefix to global functions
* net/cxgbe: avoid polling link status before device start
* net/cxgbe: fix null access when allocating CLIP entry
* net/cxgbe: fix parsing VLAN ID rewrite action
* net/cxgbe: fix prefetch for non-coalesced Tx packets
* net/cxgbe: fix races on flow API operations
* net/cxgbe: fix slot allocation for IPv6 flows
* net/dpaa2: add retry and timeout in packet enqueue API
* net/dpaa2: fix possible use of uninitialized vars
* net/dpaa2: fix Rx offload flags on jumbo MTU set
* net/dpaa2: set port in mbuf
* net/e1000: fix i219 hang on reset/close
* net/e1000: fix link status
* net/e1000: fix link status update
* net/e1000: fix memory barrier usage in Tx
* net/e1000: fix zeroing of RSS config
* net/enetc: fix BD ring alignment
* net/enic: fix crash in secondary process
* net/enic: fix probe for secondary process
* net/enic: re-enable link status change interrupt
* net/fm10k: fix mbuf free in vector Rx
* net/i40e: downgrade error log
* net/i40e: fix address of first segment
* net/i40e: fix exception with multi-driver
* net/i40e: fix integer overflow
* net/i40e: fix VF runtime queues RSS config
* net/i40e: remove compiler barrier from NEON Rx
* net/i40e: remove memory barrier from NEON Rx
* net/i40e: set speed to undefined for default case
* net/ifc: check VFIO query error
* net/igb: fix global variable multiple definitions
* net/igb: fix PHY status if PHY reset is not blocked
* net/ixgbe: enable new PF host mbox version
* net/ixgbe: fix address of first segment
* net/ixgbe: fix link status
* net/ixgbe: fix link status
* net/ixgbe: fix MACsec setting
* net/ixgbe: fix performance drop caused by MACsec
* net/ixgbe: fix queue interrupt for X552/557
* net/ixgbe: fix VF RSS offloads configuration
* net/ixgbe: fix X553 speed capability
* net/ixgbe: fix zeroing of RSS config
* net/ixgbe: remove memory barrier from NEON Rx
* net/ixgbe: remove redundant assignment
* net/ixgbe: support packet type with NEON
* net/mlx4: fix build on ppc64
* net/mlx4: remove dependency on libmnl in meson
* net/mlx5: allow pattern start from IP
* net/mlx5: block RSS action without Rx queue
* net/mlx5: fix BlueField VF type recognition
* net/mlx5: fix check of RSS queue index
* net/mlx5: fix crash on hash Rx queue handling for drop
* net/mlx5: fix Rx CQ doorbell synchronization on aarch64
* net/mlx5: improve flow item IP validation
* net/mlx5: validate flow rule item order
* net/mlx: allow build only on Linux
* net/mlx: fix build with clang 9
* net/mlx: fix build with make and recent gcc
* net/mlx: fix debug build with icc
* net/mlx: fix meson build with custom dependency path
* net/null: fix multi-process Rx and Tx
* net/qede/base: fix page index for PBL chains
* net/qede: fix odd number of queues usage in 100G mode
* net/qede: fix RSS configuration as per new allocation method
* net/qede: fix setting MTU
* net/qede: fix setting VLAN strip mode
* net/qede: fix stats flow as per new allocation method
* net/qede: limit Rx ring index read for debug
* net/qede: refactor Rx and Tx queue setup
* net/qede: use ack in interrupt handlers
* net/sfc: fix adapter lock usage on rule creation
* net/sfc: fix missing notification on link status change
* net/szedata2: fix dependency check
* net/tap: fix blocked Rx packets
* net/vhost: fix redundant queue state event
* net/virtio: fix Rx stats with vectorized functions
* net/virtio: get all pending Rx packets in vectorized paths
* net/virtio: reject deferred Rx start
* net/virtio: reject deferred Tx start
* net/virtio: reject unsupported Rx multi-queue modes
* net/virtio: reject unsupported Tx multi-queue modes
* net/virtio: remove remaining simple Tx related stuff
* net/virtio-user: drop attribute unused for memory callback
* net/virtio-user: fix setting filters
* net/vmxnet3: remove IP checksum from capabilities
* pmdinfogen: fix freebsd build
* port: fix pcap support with meson
* power: fix socket indicator value
* power: handle frequency increase with turbo disabled
* raw/dpaa2_cmdif: add retry and timeout in packet enqueue API
* reciprocal: fix off-by-one with 32-bit divisor
* Revert "eal/freebsd: fix config creation"
* security: fix doxygen fields
* service: use log for error messages
* test/bonding: fix LSC related cases
* test/bonding: fix LSC timeout unit
* test/crypto: fix checks for null digest in null auth
* test/crypto: fix session init failure for wireless case
* test/distributor: fix spurious failure
* test: fix global variable multiple definitions
* test/interrupt: account for race with callback
* test/lpm: fix measured cycles for delete
* test/mbuf: fix forged mbuf in clone test
* test: optimise fd closing in forks
* test/service: fix wait for service core
* usertools: fix pmdinfo with python 3 and pyelftools>=0.24
* usertools: fix telemetry client with python 3
* usertools: fix typo in SPDX tag of telemetry script
* version: 18.11.6-rc1
* version: 18.11.6-rc2
* vfio: fix leak with multiprocess
* vfio: fix truncated BAR offset for 32-bit
* vhost: batch used descs chains write-back with packed ring
* vhost: convert buffer addresses to GPA for logging
* vhost: do not inline packed and split functions
* vhost: do not inline unlikely fragmented buffers code
* vhost: do not limit packed ring size
* vhost: fix build dependency on hash lib
* vhost: fix build on RHEL 7.6 for Power
* vhost: fix IPv4 checksum
* vhost: fix slave request fd leak
* vhost: fix virtqueue not accessible
* vhost: fix vring address handling during live migration
* vhost: fix vring memory partially mapped
* vhost: forbid reallocation when running
* vhost: prevent zero copy mode if IOMMU is on
* vhost: protect vring access done by application
* vhost: simplify descriptor buffer prefetching
* vhost: translate incoming log address to GPA
* vhost: un-inline dirty pages logging functions

18.11.6 Validation
~~~~~~~~~~~~~~~~~~

* Intel(R) Testing

   * Basic Intel(R) NIC(ixgbe and i40e) testing

      * PF (i40e)
      * PF (ixgbe)
      * VF
      * Compile Testing
      * Intel NIC single core/NIC performance

   * Basic cryptodev and virtio testing

      * cryptodev
      * vhost/virtio basic loopback, PVP and performance test

* Red Hat(R) Testing

   * RHEL 7.8
   * Functionality

      * PF assignment
      * VF assignment
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

* Mellanox(R) Testing

   * Basic functionality with testpmd

      * Tx/Rx
      * xstats
      * Timestamps
      * Link status
      * RTE flow and flow_director
      * RSS
      * VLAN stripping and insertion
      * Checksum/TSO
      * ptype
      * Multi-process

   * ConnectX-5

      * RHEL 7.4
      * Kernel 3.10.0-693.el7.x86_64
      * Driver MLNX_OFED_LINUX-4.7-3.2.9.0
      * fw 16.26.4012

   * ConnectX-4 Lx

      * RHEL 7.4
      * Kernel 3.10.0-693.el7.x86_64
      * Driver MLNX_OFED_LINUX-4.7-3.2.9.0
      * fw 14.26.4012

* Microsoft(R) Azure Testing

   * Images

      * Canonical UbuntuServer 16.04-LTS
      * Canonical UbuntuServer 18.04-DAILY-LTS
      * Canonical UbuntuServer 19.10
      * Red Hat RHEL 7-RAW
      * SUSE SLES 15
      * RedHat RHEL 7.5
      * CentOS 7.5
      * Red Hat RHEL 8
      * CentOS 8

   * Drivers

      * Mellanox and netvsc poll-mode drivers

   * Functionality

      * VM to VM traffic
      * SRIOV/Failsafe
      * Single core performance
      * Multicore performance - one multicore test borderline. Not a regression.

* Intel(R) Testing with Open vSwitch

   * OVS testing with OVS branches 2.12 and 2.11 with VSPERF

   * Tested NICs

      * i40e (X710)
      * ixgbe (82599ES)

   * Functionality

      * P2P
      * PVP
      * Hotplug
      * Multiqueue
      * Vhostuserclient reconnect
      * Vhost cross-NUMA awareness
      * Jumbo frames
      * Rate limiting
      * QoS policer

18.11.6 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.6 contains fixes up to DPDK v19.11. Issues identified/fixed in DPDK master branch after DPDK v19.11 may be present in DPDK 18.11.6

18.11.6 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* 21b1f1abe4  net/bnxt: fix COS queue mapping
* 6d3f9917ff  eal: fix memory config allocation for multi-process
* b149a70642  eal/freebsd: add config reattach in secondary process
* 6080796f65  mem: make base address hint OS specific
* be048a1aaa  net/virtio: fix descriptor addressed in Tx
* 1526dd0532  net/virtio: fix Tx checksum offloads
* ea81c1b816  net/mlx5: fix NVGRE matching
* 9d10f53e40  test/metrics: fix second run
* 05817057fa  net/ena: fix indication of bad L4 Rx checksums
* e0d865357e  net/bnxt: enable interrupts according interface status
* 9bc556e3ec  net/bnxt: fix error handling in xstats
* dc177579d6  net/bnxt: fix L2 context calculation for Thor
* f5019a53d7  net/bnxt: fix memory leak
* a135e050ad  examples/ipsec-secgw: fix packet length
* e76097f848  net/virtio: cleanup on demand when in-order Tx
* 1ae55ad38e  net/virtio: fix mbuf data and packet length mismatch
* 721c953018  net/mlx5: fix Rx scatter mode validation
* 9e0d81c1a1  net/mlx5: fix selection between encap and decap
* 7392ad06f5  app/testpmd: use better randomness for Tx split
* f0617163b8  mempool/dpaa2: report error on endless loop in mbuf release
* f35eaaca5f  net/bnxt: fix crash in secondary process
* dcfbc594f7  net/iavf: fix queue interrupt for ice
* 6977f14517  app/testpmd: fix missing GENEVE item in raw encap
* e6d3c0928   eal/freebsd: fix config creation

18.11.7 Release Notes
---------------------

18.11.7 Fixes
~~~~~~~~~~~~~

* acl: fix 32-bit match for range field
* app/eventdev: fix pipeline test with meson build
* app/pdump: fix build with clang
* app/testpmd: call cleanup on exit
* app/testpmd: fix device mcast list error handling
* app/testpmd: fix hot-unplug detaching
* app/testpmd: fix identifier size for port attach
* app/testpmd: fix initial value when setting PFC
* app/testpmd: fix uninitialized members of MPLS
* app/testpmd: fix uninitialized members when setting PFC
* bpf: fix headers install with meson
* build: explicitly enable sse4 for meson
* bus/fslmc: remove conflicting memory barrier macro
* cfgfile: fix symbols map
* common/cpt: check cipher and auth keys are set
* common/cpt: fix component for empty IOV buffer
* common/cpt: remove redundant code in datapath
* crypto/armv8: fix clang build
* crypto/ccp: fix queue alignment
* doc: fix build with python 3.8
* doc: fix multi-producer enqueue figure in ring guide
* doc: fix warning with meson
* doc: reduce indentation in meson build file
* doc: reduce whitespace in meson build file
* doc: remove temporary files and directories
* drivers/crypto: fix session-less mode
* eal/linux: fix build error on RHEL 7.6
* eal/linux: fix uninitialized data valgrind warning
* ethdev: fix callback unregister with wildcard argument list
* ethdev: fix secondary process memory overwrite
* ethdev: fix switching domain allocation
* event/dsw: avoid credit leak on oversized enqueue bursts
* event/dsw: flush buffers immediately on zero-sized enqueue
* examples/ethtool: fix unchecked return value
* examples/fips_validation: fix AES-GCM cipher length parsing
* examples/fips_validation: fix cipher length for AES-GCM
* examples/fips_validation: fix string token for CT length
* examples/ipsec-secgw: fix crash on unsupported algo
* examples/l3fwd-power: fix a typo
* examples/l3fwd-power: fix interrupt disable
* examples/tep_term: remove redundant info get
* fix Mellanox copyright and SPDX tag
* kni: fix build with Linux 5.6
* kni: fix ethtool build for kernel 5.5
* kni: fix meson warning about console keyword
* kni: fix not contiguous FIFO
* kni: rename variable with namespace prefix
* latency: fix calculation for multi-thread
* lib: fix unnecessary double negation
* maintainers: resign from flow API maintenance
* maintainers: update for failsafe and PCI library
* maintainers: update for stable branches
* mem: fix munmap in error unwind
* mempool: fix anonymous populate
* mk: avoid combining -r and -export-dynamic linker options
* net/bnx2x: fix reset of scan FP flag
* net/bnx2x: fix to sync fastpath Rx queue access
* net/bnx2x: fix VLAN stripped flag
* net/bnxt: do not log error if stats queried before start
* net/bnxt: fix alloc filter to use a common routine
* net/bnxt: fix buffer allocation reattempt
* net/bnxt: fix crash in secondary process
* net/bnxt: fix default timeout for getting FW version
* net/bnxt: fix flow creation
* net/bnxt: fix IOVA mapping
* net/bnxt: fix link during port toggle
* net/bnxt: fix probe in FreeBSD
* net/bnxt: remove redundant if statement
* net/bnxt: remove unnecessary memset
* net/bnxt: use macro for PCI log format
* net/cxgbe: announce Tx multi-segments offload
* net/dpaa: fix Rx offload flags on jumbo MTU set
* net/failsafe: fix reported hash key size in device info
* net/fm10k: fix descriptor VLAN field filling in Tx
* net/fm10k: fix non-x86 build
* net/i40e/base: fix buffer address
* net/i40e/base: fix error message
* net/i40e/base: fix retrying logic
* net/i40e/base: fix Tx descriptors number
* net/i40e: fix Tx when TSO is enabled
* net/i40e: fix unchecked Tx cleanup error
* net/iavf: add TSO offload use basic path
* net/iavf/base: fix adminq return
* net/iavf/base: fix command buffer memory leak
* net/iavf: fix Rx total stats
* net/iavf: fix virtual channel return
* net/ixgbe: check for illegal Tx packets
* net/ixgbe: fix flow control mode setting
* net/ixgbe: fix link status
* net/ixgbe: fix link up in FreeBSD
* net/ixgbe: remove dead code
* net/ixgbe: remove duplicate function declaration
* net/mlx5: cache associated network device index
* net/mlx5: fix L3 VXLAN RSS expansion
* net/mlx5: fix match on ethertype and CVLAN tag
* net/mlx5: fix VLAN match for DV mode
* net/mlx5: fix VXLAN-GPE item translation
* net/netvsc: fix crash in secondary process
* net/netvsc: initialize link state
* net/octeontx: fix memory leak of MAC address table
* net/qede: do not stop vport if not started
* net/qede: fix VF reload
* net/sfc: fix log format specifiers
* net/tap: fix memory leak when unregister intr handler
* net/vhost: allocate interface name from heap
* net/vhost: check creation failure
* net/vhost: delay driver setup
* net/vhost: fix probing in secondary process
* net/vhost: fix setup error path
* net/vhost: prevent multiple setups on reconfiguration
* net/virtio: cleanup on demand when in-order Tx
* net/virtio: fix mbuf data and packet length mismatch
* net/virtio-user: check file descriptor before closing
* net/virtio-user: check tap offload setting failure
* net/virtio-user: do not close tap when disabling queue pairs
* Revert "common/octeontx: add missing public symbol"
* Revert "net/mlx5: fix VXLAN-GPE item translation"
* Revert "net/vhost: delay driver setup"
* Revert "net/vhost: fix setup error path"
* Revert "net/vhost: prevent multiple setups on reconfiguration"
* service: avoid false sharing on core state
* service: don't walk out of bounds when checking services
* test/compress: replace test vector
* test/crypto: fix missing operation status check
* usertools: fix syntax warning in python 3.8
* usertools: fix telemetry client with python 3
* version: 18.11.7-rc1
* vhost: catch overflow causing mmap of size 0
* vhost: check message header size read
* vhost/crypto: fix fetch size
* vhost: do not treat empty socket message as error
* vhost: fix crash on port deletion
* vhost: fix deadlock on port deletion
* vhost: fix packed virtqueue ready condition
* vhost: fix socket initial value
* vhost: protect log address translation in IOTLB update

18.11.7 Validation
~~~~~~~~~~~~~~~~~~

* Red Hat(R) Testing

   * RHEL 8
   * Functionality

      * PF assignment
      * VF assignment
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

* Intel(R) Testing

   * Basic Intel(R) NIC(ixgbe and i40e) testing

      * PF (i40e)
      * PF (ixgbe)
      * VF
      * Compile Testing
      * Intel NIC single core/NIC performance

   * Basic cryptodev and virtio testing

      * cryptodev
      * vhost/virtio basic loopback, PVP and performance test

* Mellanox(R) Testing

   * Basic functionality with testpmd

      * Tx/Rx
      * xstats
      * Timestamps
      * Link status
      * RTE flow and flow_director
      * RSS
      * VLAN stripping and insertion
      * Checksum/TSO
      * ptype
      * Multi-process

   * ConnectX-5

      * RHEL 7.4
      * Kernel 3.10.0-693.el7.x86_64
      * Driver MLNX_OFED_LINUX-5.0-1.0.0.0
      * fw 16.27.1016

   * ConnectX-4 Lx

      * RHEL 7.4
      * Kernel 3.10.0-693.el7.x86_64
      * Driver MLNX_OFED_LINUX-5.0-1.0.0.0
      * fw 14.27.1016

* Intel(R) Testing with Open vSwitch

   * OVS testing with OVS branches 2.12 and 2.11 with VSPERF

   * Tested NICs

      * i40e (X710)
      * ixgbe (82599ES)

   * Functionality

      * P2P
      * PVP
      * Hotplug
      * Multiqueue
      * Vhostuserclient reconnect
      * Vhost cross-NUMA awareness
      * Jumbo frames
      * Flow Control

18.11.7 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.7 contains fixes up to DPDK v20.02. Issues identified/fixed in DPDK master branch after DPDK v20.02 may be present in DPDK 18.11.7

18.11.7 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f  net/iavf: fix queue interrupt for ice
* b149a7064  eal/freebsd: add config reattach in secondary process
* a135e050a  examples/ipsec-secgw: fix packet length
* 9d10f53e4  test/metrics: fix second run
* ea81c1b81  net/mlx5: fix NVGRE matching
* 721c95301  net/mlx5: fix Rx scatter mode validation
* be048a1aa  net/virtio: fix descriptor addressed in Tx
* 6080796f6  mem: make base address hint OS specific
* 6d3f9917f  eal: fix memory config allocation for multi-process
* 1526dd053  net/virtio: fix Tx checksum offloads
* f0617163b  mempool/dpaa2: report error on endless loop in mbuf release
* 05817057f  net/ena: fix indication of bad L4 Rx checksums
* 9e0d81c1a  net/mlx5: fix selection between encap and decap
* 7392ad06f  app/testpmd: use better randomness for Tx split
* e6d3c0928  eal/freebsd: fix config creation
* dcd05da0a  app/testpmd: fix GENEVE flow item
* 2e02a2aff  ethdev: fix VLAN offloads set if no driver callback
* ec8615607  crypto/dpaa_sec: fix IOVA conversions
* 06387be8e  net/mlx5: fix encap/decap validation
* 7593cf1d3  net/mlx5: fix legacy multi-packet write session
* e21492a51  net/mlx: fix overlinking with meson and glue dlopen
* 150c9ac2d  app/testpmd: update Rx offload after setting MTU
* 207b1c813  test: fix build without ring PMD
* 819d0d1d5  net/ixgbe: fix blocking system events
* 050bfe033  net/mlx5: fix tunnel flow priority

18.11.8 Release Notes
---------------------

18.11.8 Fixes
~~~~~~~~~~~~~

* vhost: check log mmap offset and size overflow
* vhost/crypto: validate keys lengths
* vhost: fix vring index check

18.11.8 Validation
~~~~~~~~~~~~~~~~~~

* Red Hat(R) Testing

   * RHEL 8
   * Functionality

      * PF assignment
      * VF assignment
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

* Intel(R) Testing

   * Virtio features

      * vhost/virtio loopback test with virtio user as server mode
      * loopback multi queues
      * loopback multi paths port restart
      * vhost/virtio pvp multi-paths performance
      * pvp multi-queues and port restart
      * vhost dequeue zero copy
      * pvp share lib
      * pvp vhost user reconnect
      * pvp test with 4k pages
      * pvp test with 2M hugepages
      * pvp virtio bonding
      * pvp test with diff qemu version
      * vhost enqueue interrupt
      * vhost event idx interrupt
      * vhost virtio pmd interrupt
      * vhost virtio user interrupt
      * virtio event idx interrupt
      * virtio user for container networking
      * virtio user as exceptional path
      * vhost xstats
      * virtio-pmd multi-process
      * vm2vm virtio pmd
      * vm2vm virtio-net iperf
      * vm2vm virtio-user
      * vhost user live migration

18.11.8 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.8 contains fixes up to DPDK v20.02 and fixes for CVE-2020-10722, CVE-2020-10723 and CVE-2020-10724
* Issues identified/fixed in DPDK master branch after DPDK v20.02 may be present in DPDK 18.11.8

18.11.8 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f  net/iavf: fix queue interrupt for ice
* b149a7064  eal/freebsd: add config reattach in secondary process
* a135e050a  examples/ipsec-secgw: fix packet length
* 9d10f53e4  test/metrics: fix second run
* ea81c1b81  net/mlx5: fix NVGRE matching
* 721c95301  net/mlx5: fix Rx scatter mode validation
* be048a1aa  net/virtio: fix descriptor addressed in Tx
* 6080796f6  mem: make base address hint OS specific
* 6d3f9917f  eal: fix memory config allocation for multi-process
* 1526dd053  net/virtio: fix Tx checksum offloads
* f0617163b  mempool/dpaa2: report error on endless loop in mbuf release
* 05817057f  net/ena: fix indication of bad L4 Rx checksums
* 9e0d81c1a  net/mlx5: fix selection between encap and decap
* 7392ad06f  app/testpmd: use better randomness for Tx split
* dcd05da0a  app/testpmd: fix GENEVE flow item
* 2e02a2aff  ethdev: fix VLAN offloads set if no driver callback
* ec8615607  crypto/dpaa_sec: fix IOVA conversions
* 06387be8e  net/mlx5: fix encap/decap validation
* 7593cf1d3  net/mlx5: fix legacy multi-packet write session
* e21492a51  net/mlx: fix overlinking with meson and glue dlopen
* 150c9ac2d  app/testpmd: update Rx offload after setting MTU
* 207b1c813  test: fix build without ring PMD
* 819d0d1d5  net/ixgbe: fix blocking system events
* 050bfe033  net/mlx5: fix tunnel flow priority

18.11.9 Release Notes
---------------------

18.11.9 Fixes
~~~~~~~~~~~~~

* app/crypto-perf: fix display of sample test vector
* app/eventdev: check Tx adapter service ID
* app: fix usage help of options separated by dashes
* app/pipeline: fix build with gcc 10
* app: remove extra new line after link duplex
* app/testpmd: add parsing for QinQ VLAN headers
* app/testpmd: disable gcc 10 -fno-common build errors
* app/testpmd: fix DCB set
* app/testpmd: fix memory failure handling for i40e DDP
* app/testpmd: fix statistics after reset
* bbdev: fix doxygen comments
* build: disable gcc 10 zero-length-bounds warning
* bus/fslmc: fix dereferencing null pointer
* bus/fslmc: fix size of qman fq descriptor
* bus/pci: fix devargs on probing again
* bus/pci: fix UIO resource access from secondary process
* bus/pci: fix VF memory access
* bus/vmbus: fix comment spelling
* contigmem: cleanup properly when load fails
* crypto/caam_jr: fix check of file descriptors
* crypto/caam_jr: fix IRQ functions return type
* crypto/ccp: fix fd leak on probe failure
* cryptodev: fix SHA-1 digest enum comment
* crypto/kasumi: fix extern declaration
* crypto/octeontx: fix build with gcc 10
* crypto/octeontx: fix gcc 10 -fno-common build errors
* crypto/openssl: fix out-of-place encryption
* crypto/qat: fix cipher descriptor for ZUC and SNOW
* devtools: fix symbol map change check
* doc: add i40e limitation for flow director
* doc: add NASM installation steps
* doc: fix API index
* doc: fix build with doxygen 1.8.18
* doc: fix log level example in Linux guide
* doc: fix matrix CSS for recent sphinx
* doc: fix multicast filter feature announcement
* doc: fix number of failsafe sub-devices
* doc: fix sphinx compatibility
* doc: fix typo in contributors guide
* doc: fix typo in contributors guide
* doc: prefer https when pointing to dpdk.org
* drivers: add crypto as dependency for event drivers
* drivers/crypto: disable gcc 10 fno-common errors
* drivers/crypto: fix log type variables for -fno-common
* drivers: fix log type variables for -fno-common
* eal/arm64: fix precise TSC
* eal: fix C++17 compilation
* eal: fix comments spelling
* eal: fix log message print for regex
* eal: fix typo in endian conversion macros
* eal/ppc: fix build with gcc 9.3
* eal/x86: ignore gcc 10 stringop-overflow warnings
* ethdev: fix build when vtune profiling is on
* ethdev: fix spelling
* eventdev: fix probe and remove for secondary process
* event/dsw: avoid reusing previously recorded events
* event/dsw: fix enqueue burst return value
* event/dsw: remove redundant control ring poll
* event/dsw: remove unnecessary read barrier
* examples/eventdev: fix build with gcc 10
* examples/eventdev: fix crash on exit
* examples/ip_pipeline: remove check of null response
* examples/kni: fix crash during MTU set
* examples/kni: fix MTU change to setup Tx queue
* examples/l2fwd-keepalive: fix mbuf pool size
* examples: remove extra new line after link duplex
* examples/vmdq: fix output of pools/queues
* examples/vmdq: fix RSS configuration
* examples/vm_power: drop Unix path limit redefinition
* examples/vm_power: fix build because of missing include
* examples/vm_power: fix build with -fno-common
* fix same typo in multiple places
* fix various typos found by Lintian
* kni: fix ethtool build with kernel 5.6
* kni: fix ethtool dev_open build error
* kni: fix ethtool maybe-uninitialized warnings
* kni: fix ethtool pointer type build error
* kni: fix gcc 10 ethtool build error
* kvargs: fix buffer overflow when parsing list
* kvargs: fix invalid token parsing on FreeBSD
* kvargs: fix strcmp helper documentation
* log: fix level picked with globbing on type register
* lpm6: fix comments spelling
* lpm6: fix size of tbl8 group
* mem: fix overflow on allocation
* mem: mark pages as not accessed when freeing memory
* mem: mark pages as not accessed when reserving VA
* mempool/dpaa2: install missing header with meson
* mlx5: fix build with -fno-common
* net/avp: fix gcc 10 maybe-uninitialized warning
* net/bnxt: do not use PMD log type
* net/bnxt: fix error log for command timeout
* net/bnxt: fix FW version query
* net/bnxt: fix using RSS config struct
* net/bnxt: fix VLAN add when port is stopped
* net/dpaa2: fix 10G port negotiation
* net/dpaa: use dynamic log type
* net/e1000: fix port hotplug for multi-process
* net/ena/base: fix documentation of functions
* net/ena/base: fix indentation in CQ polling
* net/ena/base: fix indentation of multiple defines
* net/ena/base: fix testing for supported hash function
* net/ena/base: make allocation macros thread-safe
* net/ena/base: prevent allocation of zero sized memory
* net/ena: set IO ring size to valid value
* net/failsafe: fix fd leak
* net/i40e/base: update copyright
* net/i40e: fix flow director for ARP packets
* net/i40e: fix flush of flow director filter
* net/i40e: fix queue related exception handling
* net/i40e: fix setting L2TAG
* net/i40e: fix wild pointer
* net/i40e: fix X722 performance
* net/i40e: relax barrier in Tx for NEON
* net/iavf: fix setting L2TAG
* net/iavf: fix stats query error code
* net/ixgbe/base: update copyright
* net/ixgbe: check driver type in MACsec API
* net/ixgbe: fix link status synchronization on BSD
* net/ixgbe: fix statistics in flow control mode
* net/mlx4: fix build with -fno-common
* net/mlx4: fix drop queue error handling
* net/mlx4: remove device register remap
* net/mlx5: fix CVLAN tag set in IP item translation
* net/mlx5: fix mask used for IPv6 item validation
* net/mlx5: fix matching for UDP tunnels with Verbs
* net/mlx5: fix mlx5 devices port naming
* net/mlx5: fix recursive inclusion of header file
* net/mlx5: fix RSS enablement
* net/mlx5: fix Tx queue release debug log timing
* net/mlx5: fix validation of VXLAN/VXLAN-GPE specs
* net/mlx5: remove device register remap
* net/mvneta: do not use PMD log type
* net/mvpp2: fix build with gcc 10
* net/netvsc: avoid possible live lock
* net/netvsc: fix comment spelling
* net/netvsc: fix memory free on device close
* net/netvsc: handle Rx packets during multi-channel setup
* net/netvsc: handle Tx completions based on burst size
* net/netvsc: propagate descriptor limits from VF
* net/netvsc: remove process event optimization
* net/netvsc: split send buffers from Tx descriptors
* net/null: fix secondary burst function selection
* net/null: remove redundant check
* net/octeontx: fix dangling pointer on init failure
* net/octeontx: fix meson build for disabled drivers
* net/qede: fix link state configuration
* net/qede: fix port reconfiguration
* net/ring: fix device pointer on allocation
* net/sfc/base: fix manual filter delete in EF10
* net/sfc/base: handle manual and auto filter clashes in EF10
* net/sfc/base: reduce filter priorities to implemented only
* net/sfc/base: refactor filter lookup loop in EF10
* net/sfc/base: reject automatic filter creation by users
* net/sfc: fix initialization error path
* net/sfc: fix reported promiscuous/multicast mode
* net/sfc: fix Rx queue start failure path
* net/softnic: fix memory leak for thread
* net/softnic: fix resource leak for pipeline
* net/tap: do not use PMD log type
* net/tap: fix check for mbuf number of segment
* net/tap: fix crash in flow destroy
* net/tap: fix fd leak on creation failure
* net/tap: fix file close on remove
* net/tap: fix mbuf and mem leak during queue release
* net/tap: fix mbuf double free when writev fails
* net/tap: fix queues fd check before close
* net/tap: fix unexpected link handler
* net/tap: remove unused assert
* net/thunderx: use dynamic log type
* net/vhost: fix potential memory leak on close
* net/virtio: do not use PMD log type
* net/virtio: fix outdated comment
* net/virtio-user: fix devargs parsing
* net/vmxnet3: handle bad host framing
* pci: accept 32-bit domain numbers
* pci: fix build on ppc
* pci: reject negative values in PCI id
* remove references to private PCI probe function
* security: fix crash at accessing non-implemented ops
* security: fix return types in documentation
* security: fix session counter
* security: fix verification of parameters
* service: fix crash on exit
* service: fix identification of service running on other lcore
* service: fix race condition for MT unsafe service
* service: remove rte prefix from static functions
* test/crypto: fix flag check
* test/flow_classify: enable multi-sockets system
* test/kvargs: fix invalid cases check
* test/kvargs: fix to consider empty elements as valid
* test: remove redundant macro
* test: skip some subtests in no-huge mode
* usertools: check for pci.ids in /usr/share/misc
* version: 18.11.9-rc1
* version: 18.11.9-rc2
* vfio: fix race condition with sysfs
* vfio: fix use after free with multiprocess
* vhost/crypto: add missing user protocol flag
* vhost: fix peer close check
* vhost: fix zero-copy server mode
* vhost: make IOTLB cache name unique among processes
* vhost: prevent zero-copy with incompatible client mode
* vhost: remove unused variable
* vhost: remove zero-copy and client mode restriction

18.11.9 Validation
~~~~~~~~~~~~~~~~~~

* Intel(R) Testing

   * Basic Intel(R) NIC(ixgbe and i40e) testing

      * PF (i40e)
      * PF (ixgbe)
      * VF (i40e)
      * Compile Testing
      * Intel NIC single core/NIC performance

   * Basic cryptodev and virtio testing

      * vhost/virtio basic loopback, PVP and performance test
      * cryptodev function
      * cryptodev performance

* Red Hat(R) Testing

   * RHEL 8
   * QEMU 5.0
   * Functionality

      * PF assignment
      * VF assignment
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

* Mellanox(R) Testing

   * Basic functionality with testpmd

      * Tx/Rx
      * xstats
      * Timestamps
      * Link status
      * RTE flow and flow_director
      * RSS
      * VLAN stripping and insertion
      * Checksum/TSO
      * ptype

   * l3fwd-power example app tests
   * Multi-process

   * ConnectX-4 Lx

      * RHEL 7.4
      * Driver MLNX_OFED_LINUX-5.0-2.1.8.0
      * fw 14.27.1016

   * ConnectX-5

      * RHEL 7.4
      * Driver MLNX_OFED_LINUX-5.0-1.0.0.0
      * fw 16.27.2008

* Intel(R) Testing with Open vSwitch

   * OVS testing with OVS branches 2.12 and 2.11 with VSPERF

   * Tested NICs

      * i40e (X710)
      * ixgbe (82599ES)

   * Functionality

      * P2P throughput
      * P2P multiqueue
      * PVP RSS
      * Vhostuserclient reconnect
      * Flow Control

18.11.9 Known Issues
~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.9 contains fixes up to DPDK 20.05
* Issues identified/fixed in DPDK master branch after DPDK 20.05 may be present in DPDK 18.11.9

18.11.9 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f  net/iavf: fix queue interrupt for ice
* b149a7064  eal/freebsd: add config reattach in secondary process
* a135e050a  examples/ipsec-secgw: fix packet length
* 9d10f53e4  test/metrics: fix second run 
* ea81c1b81  net/mlx5: fix NVGRE matching
* 721c95301  net/mlx5: fix Rx scatter mode validation
* be048a1aa  net/virtio: fix descriptor addressed in Tx
* 6080796f6  mem: make base address hint OS specific
* 6d3f9917f  eal: fix memory config allocation for multi-process
* 1526dd053  net/virtio: fix Tx checksum offloads
* f0617163b  mempool/dpaa2: report error on endless loop in mbuf release
* 05817057f  net/ena: fix indication of bad L4 Rx checksums
* 9e0d81c1a  net/mlx5: fix selection between encap and decap
* 7392ad06f  app/testpmd: use better randomness for Tx split
* dcd05da0a  app/testpmd: fix GENEVE flow item
* 2e02a2aff  (Xavier)  ethdev: fix VLAN offloads set if no driver callback
* ec8615607  crypto/dpaa_sec: fix IOVA conversions
* 06387be8e  net/mlx5: fix encap/decap validation
* 7593cf1d3  net/mlx5: fix legacy multi-packet write session
* e21492a51  net/mlx: fix overlinking with meson and glue dlopen
* 150c9ac2d  (Xavier)  app/testpmd: update Rx offload after setting MTU
* 207b1c813  test: fix build without ring PMD
* 819d0d1d5  net/ixgbe: fix blocking system events
* 050bfe033  net/mlx5: fix tunnel flow priority
* bade47a75  net/i40e: relax barrier in Tx
* 48f9faddc  net/bnxt: fix MAC address setting when port is stopped
* f6752f660  net/sfc: set priority of created filters to manual
* 27fb5dd28  test: skip some subtests in no-huge mode
* 4236694f0  mem: preallocate VA space in no-huge mode
* 4448a202b  eal: remove useless makefiles
* efa8c72f1  net/ixgbe: fix link status inconsistencies
* 9c4971e52  net/mlx5: update VLAN and encap actions validation
* 00437823c  net/mlx5: use open/read/close for ib stats query
* 24cb500c1  net/tap: fix mbuf double free when writev fails
* ae08c73e6  net/i40e: fix flow director initialisation
* d68ab7a9f  net/ixgbe: fix resource leak after thread exits normally
* 036d82365  mempool: remove inline functions from export list
* d256c73c1  net/bnxt: fix memory leak during queue restart
* bc75bdb60  net/bnxt: fix VNIC Rx queue count on VNIC free
* 90ecace4f  examples/fips_validation: fix parsing of algorithms
* d70a869db  net/ixgbe: fix link state timing on fiber ports
* 92818d839  net/mlx5: fix match on empty VLAN item in DV mode
* 29fdc5bf4  test/crypto: fix statistics case
* 3ae4beb07  vhost: check log mmap offset and size overflow
* ff55182ce  net/mlx5: fix VLAN flow action with wildcard VLAN item
* a60704d1a  net/qede: fix assignment of Rx/Tx handlers
* 205b74295  net/bnxt: fix allocation of LED config info
* 96477b5dd  net/bnxt: fix allocation of COS queue info
* 986fa3ba8  net/bnxt: fix allocation of link info struct
* e8fe0e067  net/bnxt: fix allocation of PF info struct
* 86421846c  net/bnxt: fix storing MAC address twice
* da7018ec2  net/i40e: fix queue region in RSS flow
* 3acf10719  common/mlx5: fix netlink buffer allocation from stack
* 20cb28a0e  net/bnxt: fix Rx ring producer index
* 24cb500c1  net/tap: fix mbuf double free when writev fails
* be4ef2e0d  net/i40e: fix flow director enabling
* f6e63e59e  app/testpmd: fix global variable multiple definitions

18.11.10 Release Notes
----------------------

18.11.10 Fixes
~~~~~~~~~~~~~~

* app/eventdev: fix capability check in pipeline ATQ test
* app/testpmd: fix burst percentage calculation
* app/testpmd: fix memory leak on error path
* app/testpmd: fix stats error message
* app/testpmd: fix xstats clear
* app/testpmd: use clock time in throughput calculation
* bpf: fix add/sub min/max estimations
* bus/fslmc: fix getting FD error
* bus/vdev: fix a typo in doxygen comment
* bus/vmbus: fix ring buffer mapping
* cfgfile: fix stack buffer underflow
* common/cpt: fix encryption offset
* common/qat: fix uninitialized variable
* crypto/armv8: remove debug option
* crypto/armv8: use dedicated log type
* crypto/dpaax_sec: fix inline query for descriptors
* devtools: fix path in forbidden token check
* doc: fix ethtool app path
* doc: fix reference to master process
* doc: fix some typos in Linux guide
* doc: fix typo in bbdev test guide
* drivers/crypto: add missing OOP feature flag
* drivers/net: fix exposing internal headers
* eal/arm: add vcopyq intrinsic for aarch32
* eal/armv8: fix timer frequency calibration with PMU
* eal: fix parentheses in alignment macros
* eal: fix uuid header dependencies
* ethdev: fix data room size verification in Rx queue setup
* ethdev: fix log type for some error messages
* event/dpaa2: add all-types queue capability flag
* event/dpaa: remove dead code
* examples: add flush after stats printing
* examples/eventdev: fix 32-bit coremask
* examples/fips_validation: fix count overwrite for TDES
* examples/fips_validation: fix parsing of TDES vectors
* examples/packet_ordering: use proper exit method
* hash: fix out-of-memory handling in hash creation
* kni: fix build error on openSUSE 15.2 - pci clearing
* kni: fix build error on openSUSE 15.2 - skb_frag_t to bio_vec
* kni: fix ethtool build error on kernel 5.7
* kni: fix reference to master/slave process
* lib: remind experimental status in headers
* mem: fix 32-bit init config with meson
* net/af_packet: fix check of file descriptors
* net/af_packet: fix memory leak on init failure
* net/af_packet: fix munmap on init failure
* net/bnxt: remove unused enum declaration
* net/bonding: delete redundant code
* net/bonding: fix dead loop on RSS RETA update
* net/bonding: fix error code on device creation
* net/bonding: fix LACP negotiation
* net/bonding: fix MAC address when one port resets
* net/bonding: fix MAC address when switching active port
* net/bonding: fix socket ID check
* net/cxgbe: fix CLIP leak in filter error path
* net/cxgbe: fix double MPS alloc by flow validate and create
* net/cxgbe: fix L2T leak in filter error and free path
* net/dpaa: fix FD offset data type
* net/e1000: fix crash on Tx done clean up
* net/e1000: report VLAN extend capability
* net/failsafe: fix RSS RETA size info
* net: fix checksum on big endian CPUs
* net: fix IPv4 checksum
* net: fix pedantic build
* net: fix unneeded replacement of TCP checksum 0
* net/i40e: enable NEON Rx/Tx in meson
* net/i40e: fix binding interrupt without MSI-X vector
* net/i40e: fix flow director MSI-X resource allocation
* net/i40e: fix flow director Rx writeback packet
* net/i40e: fix getting EEPROM information
* net/i40e: remove duplicate tunnel type check
* net/i40e: report VLAN filter capability
* net/iavf: fix uninitialized variable
* net/ixgbe/base: fix host interface shadow RAM read
* net/ixgbe/base: fix infinite recursion on PCIe link down
* net/ixgbe/base: fix x550em 10G NIC link status
* net/ixgbe/base: remove dead code
* net/ixgbe: fix flow control status
* net/ixgbe: fix include of vector header file
* net/ixgbe: fix MAC control frame forward
* net/ixgbe: report 10Mbps link speed for x553
* net/kni: set packet input port in Rx
* net/mlx4: optimize stack memory size in probe
* net/mlx5: fix crash in NVGRE item translation
* net/mlx5: fix flow items size calculation
* net/mlx5: fix iterator type in Rx queue management
* net/mlx5: fix unreachable MPLS error path
* net/mlx5: fix vectorized Rx burst termination
* net/mvpp2: fix non-EAL thread support
* net/netvsc: do not spin forever waiting for reply
* net/netvsc: fix chimney index
* net/netvsc: fix crash during Tx
* net/netvsc: fix rndis packet addresses
* net/netvsc: fix underflow when Rx external mbuf
* net/nfp: fix RSS hash configuration reporting
* net/qede: fix multicast drop in promiscuous mode
* net/qede: remove dead code
* net/virtio-user: check tap system call setting
* net/virtio-user: fix status management
* pci: fix address domain format size
* rawdev: allow getting info for unknown device
* rawdev: export dump function in map file
* rawdev: fill NUMA socket ID in info
* rawdev: remove remaining experimental tags
* sched: fix port time rounding
* service: fix C++ linkage
* service: fix core mapping reset
* test/crypto: fix asymmetric session mempool creation
* test/cycles: restore default delay callback
* test: fix rpath for drivers with meson
* version: 18.11.10-rc1
* vfio: map contiguous areas in one go
* vfio: remove unused variable
* vhost/crypto: fix data length check
* vhost/crypto: fix incorrect descriptor deduction
* vhost/crypto: fix incorrect write back source
* vhost/crypto: fix missed request check for copy mode
* vhost/crypto: fix pool allocation
* vhost/crypto: fix possible TOCTOU attack
* vhost: fix double-free with zero-copy
* vhost: fix features definition location
* vhost: fix virtio ready flag check

18.11.10 Validation
~~~~~~~~~~~~~~~~~~~

* Intel(R) Testing with Open vSwitch

   * OVS testing with OVS branches 2.12 and 2.11 with VSPERF

   * Tested NICs

      * i40e (X710)
      * ixgbe (82599ES)

   * Functionality

      * RFC2544 throughput
      * RXQ and RSS validation
      * Dynamic/static flow control for ixgbe devices
      * RFC2544 for vhost interfaces
      * vhost re-connect
      * vhost numa
      * vhost multi queue (testpmd and Linux network stack tested within VM)

* Intel(R) Testing

   * Basic Intel(R) NIC(ixgbe and i40e)

      * PF (i40e)
      * PF (ixgbe)
      * VF (i40e)
      * Compile Testing
      * Intel NIC single core/NIC performance

   * Basic cryptodev and virtio

      * vhost/virtio basic loopback, PVP and performance
      * cryptodev function
      * cryptodev performance
      * vhost_crypto unit test and function/performance test

* Mellanox(R) Testing

   * testpmd send and receive multiple types of traffic
   * testpmd xstats counter
   * testpmd timestamp
   * Changing/checking link status through testpmd
   * RTE flow and flow_director

      *  items: eth / vlan / ipv4 / ipv6 / tcp / udp / gre
      *  actions: drop / queue / rss / mark / flag

   * RSS
   * VLAN stripping and insertion
   * checksum and TSO
   * ptype
   * l3fwd-power example application
   * multi-process example applications

   * ConnectX-4 Lx

      * RHEL 7.4

      * driver MLNX_OFED_LINUX-5.1-0.6.6.0
      * fw 14.28.1002

      * driver MLNX_OFED_LINUX-5.1-2.3.7.1
      * fw 14.28.2006

   * ConnectX-5

      * RHEL 7.4

      * driver MLNX_OFED_LINUX-5.1-0.6.6.0
      * fw 16.28.1002

      * driver MLNX_OFED_LINUX-5.1-2.3.7.1
      * fw 16.28.2006

* Red Hat(R) Testing

   * RHEL 8
   * QEMU 5.1
   * Functionality

      * PF assignment
      * VF assignment
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

18.11.10 Known Issues
~~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.10 contains fixes up to DPDK v20.08 and fixes for the following CVEs: CVE-2020-14374 CVE-2020-14375 CVE-2020-14376 CVE-2020-14377 CVE-2020-14378.
* Issues identified/fixed in DPDK main branch after DPDK v20.08 may be present in DPDK 18.11.10

18.11.10 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* dcfbc594f  net/iavf: fix queue interrupt for ice
* b149a7064  eal/freebsd: add config reattach in secondary process
* a135e050a  examples/ipsec-secgw: fix packet length
* 9d10f53e4  test/metrics: fix second run
* ea81c1b81  net/mlx5: fix NVGRE matching
* 721c95301  net/mlx5: fix Rx scatter mode validation
* be048a1aa  net/virtio: fix descriptor addressed in Tx
* 6080796f6  mem: make base address hint OS specific
* 6d3f9917f  eal: fix memory config allocation for multi-process
* 1526dd053  net/virtio: fix Tx checksum offloads
* f0617163b  mempool/dpaa2: report error on endless loop in mbuf release
* 05817057f  net/ena: fix indication of bad L4 Rx checksums
* 9e0d81c1a  net/mlx5: fix selection between encap and decap
* 7392ad06f  app/testpmd: use better randomness for Tx split
* dcd05da0a  app/testpmd: fix GENEVE flow item
* 2e02a2aff  ethdev: fix VLAN offloads set if no driver callback
* ec8615607  crypto/dpaa_sec: fix IOVA conversions
* 06387be8e  net/mlx5: fix encap/decap validation
* 7593cf1d3  net/mlx5: fix legacy multi-packet write session
* e21492a51  net/mlx: fix overlinking with meson and glue dlopen
* 150c9ac2d  app/testpmd: update Rx offload after setting MTU
* 207b1c813  test: fix build without ring PMD
* 819d0d1d5  net/ixgbe: fix blocking system events
* 050bfe033  net/mlx5: fix tunnel flow priority
* 48f9faddc6  net/bnxt: fix MAC address setting when port is stopped
* f6752f660f  net/sfc: set priority of created filters to manual
* 4236694f0a  mem: preallocate VA space in no-huge mode
* 4448a202b6  eal: remove useless makefiles
* efa8c72f1e  net/ixgbe: fix link status inconsistencies
* 9c4971e523  net/mlx5: update VLAN and encap actions validation
* 00437823cb  net/mlx5: use open/read/close for ib stats query
* ae08c73e6f  net/i40e: fix flow director initialisation
* d68ab7a9f9  net/ixgbe: fix resource leak after thread exits normally
* 036d82365e  mempool: remove inline functions from export list
* d256c73c11  net/bnxt: fix memory leak during queue restart
* bc75bdb60f  net/bnxt: fix VNIC Rx queue count on VNIC free
* 90ecace4f6  examples/fips_validation: fix parsing of algorithms
* d70a869db2  net/ixgbe: fix link state timing on fiber ports
* 92818d839e  net/mlx5: fix match on empty VLAN item in DV mode
* 29fdc5bf45  test/crypto: fix statistics case
* 3ae4beb079  vhost: check log mmap offset and size overflow
* ff55182ce3  net/mlx5: fix VLAN flow action with wildcard VLAN item
* a60704d1ac  net/qede: fix assignment of Rx/Tx handlers
* 205b742952  net/bnxt: fix allocation of LED config info
* 96477b5dd5  net/bnxt: fix allocation of COS queue info
* 986fa3ba83  net/bnxt: fix allocation of link info struct
* e8fe0e067b  net/bnxt: fix allocation of PF info struct
* 86421846cc  net/bnxt: fix storing MAC address twice
* da7018ec29  net/i40e: fix queue region in RSS flow
* 3acf107195  common/mlx5: fix netlink buffer allocation from stack
* 20cb28a0ec  net/bnxt: fix Rx ring producer index
* 24cb500c17  net/tap: fix mbuf double free when writev fails
* be4ef2e0d0  net/i40e: fix flow director enabling
* f6e63e59e7  app/testpmd: fix global variable multiple definitions
* c52ff36686  net/i40e: enable QinQ stripping
* 0466d286cd  net/bnxt: fix setting link speed
* c6854a4126  net/netvsc: fix warning when VF is removed
* a4f53bec7c  net/netvsc: do not query VF link state
* 2786b7bf90  net/mlx5: fix secondary process resources release
* 3b931ddb5e  app/testpmd: fix error detection in MTU command
* cb4261e0bf  event/octeontx2: improve datapath memory locality
* 91d581dc1b  crypto/dpaa2_sec: fix HFN override
* 1028d63eb2  eventdev: use C11 atomics for lcore timer armed flag
* e84d9c62c6  eventdev: remove redundant reset on timer cancel
* 030c216411  eventdev: relax SMP barriers with C11 atomics
* 1feb8e3f4e  net/iavf: fix RSS RETA after restart
* d4d5a04114  net/bnxt: fix unnecessary memory allocation
* 87aefef133  net/bnxt: fix flow error on filter creation
* 87520e59fb  net/bnxt: fix freeing filters on flow creation failure
* 11b58ac709  net/i40e: fix filter pctype
* 750ff30a8f  net/mlx5: fix tunnel flow priority

18.11.11 Release Notes
----------------------

18.11.11 Fixes
~~~~~~~~~~~~~~

* app/eventdev: check timer adadpters number
* app: fix ethdev port id size
* app: fix missing dependencies
* app/testpmd: do not allow dynamic change of core number
* app/testpmd: fix bonding xmit balance policy command
* app/testpmd: fix build with gcc 11
* app/testpmd: fix descriptor id check
* app/testpmd: fix displaying Rx/Tx queues information
* app/testpmd: fix port id check in Tx VLAN command
* app/testpmd: fix RSS key for flow API RSS rule
* app/testpmd: fix VLAN configuration on failure
* app/testpmd: remove restriction on Tx segments set
* baseband/turbo_sw: fix memory leak in error path
* build: skip detecting libpcap via pcap-config
* bus/fslmc: fix dpio close
* bus/pci: fix leak on VFIO mapping error
* bus/pci: fix memory leak when unmapping VFIO resource
* bus/pci: remove duplicate declaration
* bus/pci: remove unused scan by address
* common/qat: add missing kmod dependency info
* compress/isal: check allocation in queue setup
* config: enable packet prefetching with Meson
* cryptodev: fix parameter parsing
* crypto/dpaa2_sec: fix stats query without queue pair
* crypto/dpaa2_sec: remove dead code
* crypto/octeontx: fix out-of-place support
* crypto/scheduler: fix header install with meson
* devtools: fix directory filter in forbidden token check
* distributor: fix API documentation
* distributor: fix buffer use after free
* distributor: fix clearing returns buffer
* distributor: fix flushing in flight packets
* distributor: fix handshake deadlock
* distributor: fix handshake synchronization
* distributor: fix return pkt calls in single mode
* distributor: fix scalar matching
* distributor: handle worker shutdown in burst mode
* doc: add SPDX license tag header to Intel performance guide
* doc: add SPDX license tag header to meson guide
* doc: clarify instructions on running as non-root
* doc: fix diagram in dpaa2 guide
* doc: fix EF10 Rx mode name in sfc guide
* doc: fix ethdev port id size
* doc: fix formatting of notes in meson guide
* doc: fix grammar
* doc: fix missing classify methods in ACL guide
* doc: fix rule file parameters in l3fwd-acl guide
* doc: fix typo in KNI guide
* doc: fix typo in pcap guide
* doc: improve multiport PF in nfp guide
* doc: update information on using hugepages
* eal: fix doxygen for EAL cleanup
* eal: fix leak on device event callback unregister
* eal: fix parallel build
* eal/linux: fix memory leak in uevent handling
* eal: remove useless makefiles
* eal/x86: fix memcpy AVX-512 enablement
* efd: fix tailq entry leak in error path
* ethdev: fix data type for port id
* ethdev: fix memory ordering for callback functions
* ethdev: move non-offload capabilities
* ethdev: remove redundant license text
* eventdev: check allocation in Tx adapter
* eventdev: fix adapter leak in error path
* event/dpaa2: fix dereference before null check
* examples/fips_validation: fix buffer overflow
* examples/fips_validation: fix missed version line
* examples/kni: fix build with pkg-config
* examples/l2fwd-crypto: fix build with pkg-config
* examples/l2fwd-crypto: fix missing dependency
* examples/l2fwd-keepalive: skip meson build if no librt
* examples/l3fwd-power: check packet types after start
* examples/multi_process: fix build on Ubuntu 20.04
* examples/multi_process: fix compilation
* examples/performance-thread: fix build with low core count
* examples/performance-thread: fix build with pkg-config
* examples/qos_sched: fix usage string
* examples/vhost_crypto: add new line character in usage
* examples/vm_power: fix build on Ubuntu 20.04
* fix spellings that Lintian complains about
* gso: fix payload unit size for UDP
* kni: fix build on RHEL 8.3
* kni: fix ethtool build error on kernel 5.9
* license: add licenses for exception cases
* maintainers: update maintainer names and emails
* malloc: fix style in free list index computation
* mem: fix allocation failure on non-NUMA kernel
* mem: fix allocation in container with SELinux
* mem: fix config name in error logs
* mempool/octeontx: fix aura to pool mapping
* net/avf: fix command after PF reset
* net/avf: fix flow flush after PF reset
* net/avf: fix iterator for RSS LUT
* net/avf: fix performance drop after port reset
* net/avf: fix releasing mbufs
* net/avf: fix RSS RETA settings invalid
* net/avf: fix scattered Rx enabling
* net/avf: fix vector Rx
* net/bnx2x: add QLogic vendor id for BCM57840
* net/bnxt: add memory allocation check in VF info init
* net/bnxt: fix boolean operator usage
* net/bnxt: fix checking VNIC in shutdown path
* net/bnxt: fix drop enable in get Rx queue info
* net/bnxt: fix endianness while setting L4 destination port
* net/bnxt: fix flow error on filter creation
* net/bnxt: fix for memleak during queue restart
* net/bnxt: fix memory leak when freeing VF info
* net/bnxt: fix queue get info
* net/bnxt: fix queue release
* net/bnxt: fix resetting mbuf data offset
* net/bnxt: fix shift operation
* net/bnxt: fix structure variable initialization
* net/bnxt: fix to advance producer index
* net/bnxt: fix UDP tunnel port removal
* net/bnxt: increase size of Rx CQ
* net/bonding: fix possible unbalanced packet receiving
* net/bonding: fix Rx queue conversion
* net: check segment pointer in raw checksum processing
* net/cxgbe: fix duplicate MAC addresses in MPS TCAM
* net/cxgbe: fix queue DMA ring leaks during port close
* net/dpaa2: fix misuse of interface index
* net/ena/base: fix release of wait event
* net/ena/base: specify delay operations
* net/ena/base: use min/max macros with type conversion
* net/ena: remove unused macro
* net/failsafe: fix double space in warning log
* net/failsafe: fix state synchro cleanup
* net/fm10k: fix memory leak when thresh check fails
* net/fm10k: fix memory leak when Tx thresh check fails
* net/fm10k: fix vector Rx
* net/i40e: add C++ include guard
* net/i40e/base: fix function header arguments
* net/i40e/base: fix Rx only for unicast promisc on VLAN
* net/i40e: fix build for log format specifier
* net/i40e: fix byte counters
* net/i40e: fix filter pctype
* net/i40e: fix flow director for eth + VLAN pattern
* net/i40e: fix flow director initialisation
* net/i40e: fix link status
* net/i40e: fix QinQ flow pattern to allow non full mask
* net/i40e: fix queue region in RSS flow
* net/i40e: fix recreating flexible flow director rule
* net/i40e: fix vector Rx
* net/i40e: fix virtual channel conflict
* net/i40e: support aarch32
* net/iavf: downgrade error log
* net/ixgbe: check switch domain allocation result
* net/ixgbe: fix vector Rx
* net/ixgbe: fix VF reset HW error handling
* net/ixgbe: remove redundant MAC flag check
* net/mlx5: fix doorbell register mmap offset
* net/mlx5: fix match on empty VLAN item in DV mode
* net/mlx5: fix netlink buffer allocation from stack
* net/mlx5: fix PCI address lookup
* net/mlx5: fix representor interrupts handler
* net/mlx5: fix Rx descriptor status
* net/mlx5: fix Rx queue count calculation
* net/mlx5: fix Rx queue count calculation
* net/mlx5: fix secondary process resources release
* net/mlx5: fix selection between encap and decap
* net/mlx5: fix tunnel flow prioriity
* net/mlx5: fix UAR remap initialization for 32-bit systems
* net/mlx5: fix xstats reset reinitialization
* net/mlx5: remove unused log macros
* net/mlx5: remove unused variable in Tx queue creation
* net/mlx5: use open/read/close for ib stats query
* net/mlx: fix overlinking with meson and glue dlopen
* net/mvpp2: fix memory leak in error path
* net/netvsc: allocate contiguous physical memory for RNDIS
* net/netvsc: check for overflow on packet info from host
* net/netvsc: fix multiple channel Rx
* net/netvsc: fix stale value after free
* net/netvsc: fix Tx queue leak in error path
* net/netvsc: replace compiler builtin overflow check
* net/nfp: expand device info get
* net/qede: fix dereference before null check
* net/qede: fix getting link details
* net/qede: fix milliseconds sleep macro
* net/ring: check internal arguments
* net/ring: fix typo in log message
* net/sfc/base: fix tunnel configuration
* net/tap: free mempool when closing
* net/thunderx: fix memory leak on rbdr desc ring failure
* net/vdev_netvsc: fix device probing error flow
* net/vhost: fix xstats after clearing stats
* net/virtio: check raw checksum failure
* pmdinfogen: fix build with gcc 11
* port: remove useless assignment
* raw/dpaa2_qdma: fix reset
* raw/skeleton: allow closing already closed device
* raw/skeleton: reset test statistics
* Revert "app/testpmd: fix descriptor id check"
* Revert "app/testpmd: remove restriction on Tx segments set"
* table: fix hash for 32-bit
* test/crypto: fix device number
* test/crypto: fix stats test
* test/distributor: collect return mbufs
* test/distributor: fix freeing mbufs
* test/distributor: fix lcores statistics
* test/distributor: fix mbuf leak on failure
* test/distributor: fix quitting workers in burst mode
* test/distributor: fix race conditions on shutdown
* test/distributor: fix shutdown of busy worker
* test/event: fix function arguments for crypto adapter
* timer: add limitation note for sync stop and reset
* usertools: fix CPU layout script to be PEP8 compliant
* usertools: fix pmdinfo parsing
* vdpa/ifc: fix build with recent kernels
* version: 18.11.11-rc1
* vfio: fix group descriptor check
* vhost/crypto: fix feature negotiation
* vhost: fix error path when setting memory tables
* vhost: fix fd leak in dirty logging setup
* vhost: fix fd leak in kick setup
* vhost: fix IOTLB mempool single-consumer flag
* vhost: fix virtio-net header length with packed ring
* vhost: fix virtqueue initialization
* vhost: fix virtqueues metadata allocation
* vhost: validate index in available entries API
* vhost: validate index in guest notification API

18.11.11 Validation
~~~~~~~~~~~~~~~~~~~

* Red Hat(R) Testing

   * RHEL 8
   * QEMU 5.2
   * Functionality

      * PF assignment
      * VF assignment
      * vhost single/multi queues and cross-NUMA
      * vhostclient reconnect
      * vhost live migration with single/multi queues and cross-NUMA
      * OVS PVP

   * Tested NICs

      * X540-AT2 NIC(ixgbe, 10G)

* Intel(R) Testing

   * Basic Intel(R) NIC(ixgbe and i40e)

      * PF (i40e)
      * PF (ixgbe)
      * VF (i40e)
      * VF (ixgbe)
      * Compile Testing
      * Intel NIC single core/NIC performance

   * Basic cryptodev and virtio

      * vhost/virtio basic loopback, PVP and performance

         * See known issues

      * cryptodev function
      * cryptodev performance

* Intel(R) Testing with Open vSwitch

   * OVS testing with OVS branches 2.12 and 2.11 with VSPERF

   * i40e

      * RFC2544 and throughput
      * RSS validation
      * Jumbo Frames
      * Flow Control

   * i40evf

      * Basic functionality
      * Jumbo Frames

   * ixgbe

      * RFC2544 and throughput
      * RSS validation
      * Jumbo Frames
      * Flow Control
      * RSS

   * vhostuserclient

      * Jumbo Frames
      * Reconnect
      * NUMA node

* Mellanox(R) Testing

   * testpmd send and receive multiple types of traffic
   * testpmd xstats counter
   * testpmd timestamp
   * Changing/checking link status through testpmd
   * RTE flow and flow_director
   * RSS
   * VLAN stripping and insertion
   * Checksum and TSO
   * ptype
   * l3fwd-power example application
   * multi-process example applications

   * ConnectX-4 Lx

      * RHEL 7.4
      * driver MLNX_OFED_LINUX-5.2-1.0.4.0
      * fw 14.29.1016

   * ConnectX-5

      * RHEL 7.4
      * driver MLNX_OFED_LINUX-5.2-1.0.4.0
      * fw 16.29.1016

18.11.11 Known Issues
~~~~~~~~~~~~~~~~~~~~~

* DPDK 18.11.11 contains fixes up to DPDK v20.11
* Issues identified/fixed in DPDK main branch after DPDK v20.11 may be present in DPDK 18.11.11
* rss_to_rte_flow/set_key_keylen: create rule failed: Invalid argument

   * https://bugs.dpdk.org/show_bug.cgi?id=573

* The UDP fragmentation offload feature of Virtio-net device can not be turned on in the VM

   * https://bugzilla.kernel.org/show_bug.cgi?id=207075

18.11.11 Fixes skipped and status unresolved
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* b149a7064  eal/freebsd: add config reattach in secondary process
* a135e050a  examples/ipsec-secgw: fix packet length
* 9d10f53e4  test/metrics: fix second run
* 6080796f6  mem: make base address hint OS specific
* 6d3f9917f  eal: fix memory config allocation for multi-process
* f0617163b  mempool/dpaa2: report error on endless loop in mbuf release
* 7392ad06f  app/testpmd: use better randomness for Tx split
* ec8615607  crypto/dpaa_sec: fix IOVA conversions
* 207b1c813  test: fix build without ring PMD
* f6752f660  net/sfc: set priority of created filters to manual
* 4236694f0  mem: preallocate VA space in no-huge mode
* 036d82365  mempool: remove inline functions from export list
* be4ef2e0d  net/i40e: fix flow director enabling
* f6e63e59e  app/testpmd: fix global variable multiple definitions
* c6854a412  net/netvsc: fix warning when VF is removed
* a4f53bec7  net/netvsc: do not query VF link state
* cb4261e0b  event/octeontx2: improve datapath memory locality
* 91d581dc1  crypto/dpaa2_sec: fix HFN override
* 7838d3a6a  net/netvsc: check for overflow on packet info from host
* b253a6bbf  app/testpmd: fix packet header in txonly mode
* 07bfb9047  crypto/aesni_mb: fix CCM digest size check
* c6887eca5  crypto/caam_jr: fix device tree parsing for SEC_ERA
* ead06572b  net/iavf: fix performance with writeback policy
* 856edce2b  net/ena: fix setting Rx checksum flags in mbuf
* ceccbcd73  raw/ifpga: use trusted buffer to free
* fe5d0e85b  net/i40e: fix flow director flex configuration
