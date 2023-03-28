# Evaluate performance of merge algorithm in Linux kernel's list_sort

This repository contains code to test performance of merge algorithm.
The result is that the performance of original merge algoirhm is better than the new one.

This is my experiment environment:

```bash
$ gcc --version
gcc (Ubuntu 11.3.0-1ubuntu1~22.04) 11.3.0

$ lscpu
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         39 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  8
  On-line CPU(s) list:   0-3
  Off-line CPU(s) list:  4-7
Vendor ID:               GenuineIntel
  Model name:            Intel(R) Core(TM) i5-8250U CPU @ 1.60GHz
    CPU family:          6
    Model:               142
    Thread(s) per core:  1
    Core(s) per socket:  4
    Socket(s):           1
    Stepping:            10
    CPU max MHz:         3400.0000
    CPU min MHz:         400.0000
    BogoMIPS:            3600.00
    Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mc
                         a cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss 
                         ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc art
                          arch_perfmon pebs bts rep_good nopl xtopology nonstop_
                         tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cp
                         l vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4_1
                          sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsav
                         e avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault
                          epb invpcid_single pti ssbd ibrs ibpb stibp tpr_shadow
                          vnmi flexpriority ept vpid ept_ad fsgsbase tsc_adjust 
                         sgx bmi1 avx2 smep bmi2 erms invpcid mpx rdseed adx sma
                         p clflushopt intel_pt xsaveopt xsavec xgetbv1 xsaves dt
                         herm ida arat pln pts hwp hwp_notify hwp_act_window hwp
                         _epp md_clear flush_l1d arch_capabilities
Virtualization features: 
  Virtualization:        VT-x
Caches (sum of all):     
  L1d:                   128 KiB (4 instances)
  L1i:                   128 KiB (4 instances)
  L2:                    1 MiB (4 instances)
  L3:                    6 MiB (1 instance)
NUMA:                    
  NUMA node(s):          1
  NUMA node0 CPU(s):     0-3

```

The result is:

```
==== Testing list_sort_old ====
  Elapsed time:   748371
  Comparisons:    18686654
  List is sorted
==== Testing list_sort_new ====
  Elapsed time:   779118
  Comparisons:    18686654
  List is sorted
```

It is tested with Turbo Boost and SMT disabled.


