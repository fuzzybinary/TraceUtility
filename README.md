# TraceUtility

*Only tested with Instruments.app bundled in Xcode 8.3*

*[Blog post](https://qusic.me/post/extract-data-from-trace-documents/) (in Chinese 中文)*

A proof of concept on how to extract data from .trace documents generated by Instruments, using the undocumented frameworks shipped with Instruments.

We need to link against these frameworks:

* `/Applications/Xcode.app/Contents/SharedFrameworks/DVTFoundation.framework`
* `/Applications/Xcode.app/Contents/SharedFrameworks/DVTInstrumentsFoundation.framework`
* `/Applications/Xcode.app/Contents/Applications/Instruments.app/Contents/Frameworks/InstrumentsPlugIn.framework`
* `/Applications/Xcode.app/Contents/Applications/Instruments.app/Contents/Frameworks/InstrumentsKit.framework`

Instrument templates used by the app are packages in `/Applications/Xcode.app/Contents/Applications/Instruments.app/Contents/Packages` and plugins in `/Applications/Xcode.app/Contents/Applications/Instruments.app/Contents/PlugIns`.

It seems Apple is migrating from `PlugIns` to `Packages`. For example, the instrument `Time Profile` in `PlugIns/SamplerPlugin.xrplugin` is deprecated in Xcode 8 and there is a new version in `Packages/Sampling.instrdst`.

This project contains these example routines:

* **Time Profiler**: print out all functions in descending order of self execution time.
* **Allocations**: print out the memory allocated during each second in descending order of the size.
* **Core Animation**: print out all FPS data samples.
* **Connections**: print out all connections.
* **Energy Usage Log**: print out all energy usage level data.

They are not all of the templates included in Instruments. However, it is well enough to demonstrate how to work with the data in several common structures, such as call trees, tables and streams.

Here is a sample output.

```
Trace: /Users/qusic/Downloads/Instruments.trace
Device: Q's (10.3.1) (iPhone9,1 10.3.1 14E304)
Process: Playground (me.qusic.Playground)

Instrument: Time Profiler (com.apple.xray.instrument-type.coresampler2)
Run #2: Run 2
libobjc.A.dylib flushCaches(objc_class*) 25 ms
libobjc.A.dylib cache_erase_nolock 18 ms
libobjc.A.dylib std::__1::__function::__func<void (objc_class*) block_pointer, std::__1::allocator<void (objc_class*) block_pointer>, void (objc_class*)>::operator()(objc_class*&&) 7 ms
dyld __open 4 ms
dyld __mmap 3 ms
libsystem_platform.dylib OSAtomicDequeue 3 ms
libobjc.A.dylib std::__1::__function::__func<foreach_realized_class_and_metaclass(std::__1::function<void (objc_class*)>)::'lambda'(objc_class*), std::__1::allocator<foreach_realized_class_and_metaclass(std::__1::function<void (objc_class*)>)::'lambda'(objc_class*)>, bool (objc_class*)>::operator()(objc_class*&&) 3 ms
dyld getattrlist 2 ms
dyld stat64 2 ms
libobjc.A.dylib realizeClass(objc_class*) 2 ms
libobjc.A.dylib ___ZL11flushCachesP10objc_class_block_invoke_2 2 ms
dyld ImageLoaderMachOCompressed::eachBind(ImageLoader::LinkContext const&, unsigned long (ImageLoaderMachOCompressed::*)(ImageLoader::LinkContext const&, unsigned long, unsigned char, char const*, unsigned char, long, long, char const*, ImageLoaderMachOCompressed::LastLookup*, bool)) 2 ms
dyld ImageLoaderMachOCompressed::interposeAt(ImageLoader::LinkContext const&, unsigned long, unsigned char, char const*, unsigned char, long, long, char const*, ImageLoaderMachOCompressed::LastLookup*, bool) 2 ms
liboainject.dylib 0x10009806c 1 ms
libsystem_c.dylib _thread_stack_pcs 1 ms
libsystem_kernel.dylib __ulock_wake 1 ms
libsystem_platform.dylib _platform_strcmp 1 ms
libsystem_kernel.dylib __open 1 ms
libsystem_kernel.dylib stat 1 ms
libobjc.A.dylib rwlock_tt<false>::read() 1 ms
libsystem_platform.dylib _platform_memmove 1 ms
libsystem_kernel.dylib __ulock_wait 1 ms
libsystem_platform.dylib os_unfair_lock_lock 1 ms
libsystem_kernel.dylib _kernelrpc_mach_port_mod_refs_trap 1 ms
libsystem_platform.dylib _platform_memmove 1 ms
libsystem_pthread.dylib pthread_rwlock_unlock 1 ms
libxpc.dylib xpc_pipe_routine 1 ms
libsystem_c.dylib _thread_stack_pcs 1 ms
libsystem_platform.dylib OSAtomicDequeue 1 ms
libobjc.A.dylib search_method_list(method_list_t const*, objc_selector*) 1 ms
liboainject.dylib 0x10009b064 1 ms
// Truncated for brevity

Instrument: Allocations (com.apple.xray.instrument-type.oa)
Run #2: Run 2
#1 2.8 MB
#6 1.5 MB
#36 768 KB
#31 385 KB
#33 384 KB
#25 214 KB
#27 209 KB
#26 105 KB
#2 2 KB
#28 400 bytes

Instrument: Core Animation (com.apple.xray.instrument-type.coreanimation)
Run #2: Run 2
#0 0 FPS
#1 41 FPS
#2 59 FPS
#3 60 FPS
#4 59 FPS
#5 60 FPS
#6 59 FPS
#7 59 FPS
#8 60 FPS
#9 59 FPS
#10 60 FPS
#11 59 FPS
#12 60 FPS
#13 59 FPS
#14 59 FPS
#15 60 FPS
#16 59 FPS
#17 59 FPS
#18 60 FPS
#19 59 FPS
#20 59 FPS
#21 60 FPS
#22 59 FPS
#23 60 FPS
#24 59 FPS
#25 47 FPS
#26 0 FPS
#27 43 FPS
#28 2 FPS
#29 0 FPS
#30 0 FPS
#31 0 FPS
#32 0 FPS
#33 0 FPS
#34 0 FPS
#35 0 FPS
#36 0 FPS
#37 0 FPS
#38 0 FPS
#39 0 FPS
#40 0 FPS
#41 0 FPS
#42 0 FPS

Instrument: Connections (com.apple.xray.instrument-type.networking)
Run #2: Run 2
ipv4 *:5353 -> ipv4 *:*: 4.8 MB received, 2.7 MB sent
ipv6 ::.5353 -> ipv6 ::.*: 3.4 MB received, 2.9 MB sent
ipv6 fe80::101c:769f:fb99:73e7.59261 -> ipv6 fe80::10ed:2a48:98c1:bb89.52619: 120 KB received, 68 KB sent
ipv6 2409:8800:9209:40b8:18f6:72a:79f9:43af.64342 -> ipv6 ::.*: 111 KB received, Zero KB sent
ipv4 192.168.1.111:59324 -> ipv4 17.252.156.68:443: 37 KB received, 39 KB sent
ipv4 240.0.0.1:59323 -> ipv4 17.252.156.68:443: 37 KB received, 38 KB sent
ipv4 192.168.1.111:60519 -> ipv4 17.132.92.5:443: 6 KB received, 6 KB sent
ipv4 240.0.0.1:60518 -> ipv4 17.132.92.5:443: 6 KB received, 5 KB sent
ipv4 240.0.0.1:60544 -> ipv4 240.0.0.3:6152: 5 KB received, 3 KB sent
ipv4 192.168.1.111:60545 -> ipv4 104.199.205.33:63389: 5 KB received, 2 KB sent
ipv6 fe80::101c:769f:fb99:73e7.59282 -> ipv6 fe80::10ed:2a48:98c1:bb89.52624: 3 KB received, 3 KB sent
ipv6 fe80::101c:769f:fb99:73e7.59283 -> ipv6 fe80::10ed:2a48:98c1:bb89.52625: 2 KB received, 2 KB sent
ipv6 fe80::101c:769f:fb99:73e7.59277 -> ipv6 fe80::10ed:2a48:98c1:bb89.52623: 2 KB received, 2 KB sent
ipv6 fe80::101c:769f:fb99:73e7.59552 -> ipv6 fe80::10ed:2a48:98c1:bb89.53346: 2 KB received, 885 bytes sent
ipv4 240.0.0.1:60526 -> ipv4 240.0.0.3:6152: 680 bytes received, 820 bytes sent
ipv4 240.0.0.1:60527 -> ipv4 240.0.0.3:6152: 310 bytes received, 876 bytes sent
??? -> ???: Zero KB received, Zero KB sent
ipv6 2409:8800:9209:40b8:18f6:72a:79f9:43af.64342 -> ipv6 ::.*: Zero KB received, Zero KB sent
ipv6 2409:8800:9209:40b8:18f6:72a:79f9:43af.5060 -> ipv6 ::.*: Zero KB received, Zero KB sent
ipv6 2409:8800:9209:40b8:18f6:72a:79f9:43af.64341 -> ipv6 ::.*: Zero KB received, 80 KB sent
ipv6 2409:8800:9209:40b8:18f6:72a:79f9:43af.5060 -> ipv6 ::.*: Zero KB received, Zero KB sent

Instrument: Energy Usage Log (com.apple.xray.power.mobile.energy)
Run #2: Run 2
0.559495-1.559495001 s: Energy Usage Level 1
1.559495-2.559495001 s: Energy Usage Level 1
2.559495-3.559495001 s: Energy Usage Level 1
3.559495-4.559495001 s: Energy Usage Level 1
4.559495-5.559495001 s: Energy Usage Level 1
5.559495-6.559495001 s: Energy Usage Level 1
6.559495-7.559495001 s: Energy Usage Level 1
7.559495-8.559495001 s: Energy Usage Level 1
8.559495-9.559495001 s: Energy Usage Level 1
9.559495-10.559495001 s: Energy Usage Level 1
10.559495-11.559495001 s: Energy Usage Level 1
11.559495-12.559495001 s: Energy Usage Level 1
12.559495-13.559495001 s: Energy Usage Level 1
13.559495-14.559495001 s: Energy Usage Level 1
14.559495-15.559495001 s: Energy Usage Level 1
15.559495-16.559495 s: Energy Usage Level 1
16.559494999-17.559495001 s: Energy Usage Level 1
17.559495-18.559495001 s: Energy Usage Level 1
18.559495-19.559495001 s: Energy Usage Level 1
19.559495-20.559495001 s: Energy Usage Level 1
20.559495-21.559495001 s: Energy Usage Level 1
21.559495-22.559495001 s: Energy Usage Level 1
22.559495-23.559495001 s: Energy Usage Level 1
23.559495-24.559495001 s: Energy Usage Level 1
24.559495-25.559495001 s: Energy Usage Level 1
25.559495-25.559495 s: Energy Usage Level 1
```
