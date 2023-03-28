# Evaluate performance of merge algorithm in Linux kernel's list_sort

This repository contains code to test performance of merge algorithm.
The result is that the performance of original merge algoirhm is better than the new one purposed by me.

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

## Result

`list_sort_old` refered to the original merge algorithm in Linux kernel's list_sort.c
`list_sort_new` is the algorithm I proposed to use.

The elapsed time of `list_sort_new` is higher than the elapsed time of `list_sort_old`.
Both instruction counts and cycle counts of `list_sort_new` are also higher than
those of `list_sort_old`

Therefore, `list_sort_new` is not faster than `list_sort_old`.

It is performed with Turbo Boost and SMT disabled.
The PRNG seed, srand, is set to a fixed value `1050` to make sure the test is deterministic.

The result is:

```
==== Testing list_sort_old ====
  Elapsed time:   932879
  Comparisons:    18687074
  List is sorted
==== Testing list_sort_new ====
  Elapsed time:   968952
  Comparisons:    18687074
  List is sorted
```

The result of perf:

list_sort_old
```
==== Testing list_sort_old ====
  Elapsed time:   932879
  Comparisons:    18687074
  List is sorted

 Performance counter stats for './main':

          2,191.79 msec task-clock                #    0.999 CPUs utilized          
                26      context-switches          #   11.862 /sec                   
                 0      cpu-migrations            #    0.000 /sec                   
            17,640      page-faults               #    8.048 K/sec                  
     3,483,542,142      cycles                    #    1.589 GHz                      (49.75%)
     1,044,185,810      instructions              #    0.30  insn per cycle           (62.34%)
       290,589,632      branches                  #  132.581 M/sec                    (62.51%)
        57,638,386      branch-misses             #   19.83% of all branches          (62.71%)
       264,842,404      L1-dcache-loads           #  120.834 M/sec                    (62.80%)
        30,758,979      L1-dcache-load-misses     #   11.61% of all L1-dcache accesses  (62.66%)
        17,525,868      LLC-loads                 #    7.996 M/sec                    (49.88%)
        10,929,504      LLC-load-misses           #   62.36% of all LL-cache accesses  (49.70%)

       2.194075207 seconds time elapsed

       2.128773000 seconds user
       0.064143000 seconds sys
```

list_sort_new
```
==== Testing list_sort_new ====
  Elapsed time:   968952
  Comparisons:    18687074
  List is sorted

 Performance counter stats for './main':

          2,252.19 msec task-clock                #    0.999 CPUs utilized          
                24      context-switches          #   10.656 /sec                   
                 0      cpu-migrations            #    0.000 /sec                   
            17,642      page-faults               #    7.833 K/sec                  
     3,579,407,995      cycles                    #    1.589 GHz                      (50.00%)
     1,241,668,747      instructions              #    0.35  insn per cycle           (62.60%)
       280,393,124      branches                  #  124.498 M/sec                    (62.70%)
        58,405,660      branch-misses             #   20.83% of all branches          (62.71%)
       358,193,062      L1-dcache-loads           #  159.042 M/sec                    (62.72%)
        30,661,094      L1-dcache-load-misses     #    8.56% of all L1-dcache accesses  (62.43%)
        17,743,846      LLC-loads                 #    7.879 M/sec                    (49.72%)
        11,428,072      LLC-load-misses           #   64.41% of all LL-cache accesses  (49.71%)

       2.255113789 seconds time elapsed

       2.205093000 seconds user
       0.048198000 seconds sys
```
