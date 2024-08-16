# JVM---GraalVM

`
java -server --add-modules=jdk.incubator.vector -Xms8G -Xmx14G -XX:AllocatePrefetchStyle=3 -XX:+UseG1GC -XX:MaxGCPauseMillis=130 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=28 -XX:G1HeapRegionSize=16M -XX:G1ReservePercent=20 -XX:G1MixedGCCountTarget=3 -XX:InitiatingHeapOccupancyPercent=10 -XX:G1MixedGCLiveThresholdPercent=90 -XX:SurvivorRatio=32 -XX:MaxTenuringThreshold=1 -XX:+PerfDisableSharedMem -XX:G1SATBBufferEnqueueingThresholdPercent=30 -XX:G1ConcMarkStepDurationMillis=5 -XX:G1RSetUpdatingPauseTimePercent=0 -XX:G1HeapWastePercent=18 -XX:GCTimeRatio=99 -XX:+AlwaysActAsServerClassMachine -XX:NmethodSweepActivity=1 -XX:ThreadPriorityPolicy=1 -XX:ReservedCodeCacheSize=400M -XX:NonNMethodCodeHeapSize=12M -XX:ProfiledCodeHeapSize=194M -XX:NonProfiledCodeHeapSize=194M -XX:-DontCompileHugeMethods -XX:MaxNodeLimit=240000 -XX:NodeLimitFudgeFactor=8000 -XX:+UseVectorCmov -XX:+UseFastUnorderedTimeStamps -XX:+UseCriticalJavaThreadPriority -XX:+UseLargePages -XX:+UseHugeTLBFS -XX:+UseTransparentHugePages -XX:LargePageSizeInBytes=2M -XX:+OptimizeStringConcat -XX:+DisableAttachMechanism -XX:+EagerJVMCI -Dgraal.TuneInlinerExploration=1 -Dgraal.LoopRotation=true -Dgraal.OptWriteMotion=true -Dgraal.CompilerConfiguration=enterprise -XX:+PrintCommandLineFlags -jar server.jar nogui
`


```
-server：选择服务器模式的 JVM，会进行更多的优化，可能包括更大的内存分配、更积极的编译等，通常用于服务器环境以获得更好的性能和伸缩性。
--add-modules=jdk.incubator.vector：添加特定的实验性模块，可能启用一些新的特性或功能，但这依赖于该模块的具体内容和应用场景。
-Xms8G：设置初始堆内存大小为 8GB，这决定了应用启动时分配的堆内存量，较大的初始堆可能适合内存需求较大的应用，但也可能导致启动时间较长和一定的内存浪费，如果系统内存不足可能无法设置这么大的值。
-Xmx14G：设置最大堆内存为 14GB，限制了堆内存的最大可用量，确保不会无限制地消耗系统内存，同样需要系统有足够的物理内存支持。
-XX:AllocatePrefetchStyle=3：控制预取的方式，可能影响内存访问性能，但具体效果因应用特性而异。
-XX:+UseG1GC：使用 G1 垃圾回收器，它是一种较新的垃圾回收策略，旨在提供更好的垃圾回收性能和可预测性。
-XX:MaxGCPauseMillis=130：期望的最大垃圾回收暂停时间，但实际可能无法完全保证。
-XX:+UnlockExperimentalVMOptions：解锁实验性的虚拟机选项，允许使用一些可能不稳定或未完全成熟的特性。
-XX:+UnlockDiagnosticVMOptions：解锁诊断性的虚拟机选项，方便进行一些调试和监测。
-XX:+DisableExplicitGC：禁止应用程序进行显式的垃圾回收调用，防止不恰当的手动回收影响性能。
-XX:+AlwaysPreTouch：在启动时预触摸所有内存页面，可减少运行时的页面错误，但会增加启动时间和一定的开销。
-XX:G1NewSizePercent=28：设置年轻代（新生成区）初始占整个堆的比例。
-XX:G1HeapRegionSize=16M：规定了 G1 回收器中堆区域的大小，影响垃圾回收的粒度。
-XX:G1ReservePercent=20：预留一定比例的堆空间作为备用。
-XX:G1MixedGCCountTarget=3：混合垃圾回收的目标次数。
-XX:InitiatingHeapOccupancyPercent=10：达到这个堆占用比例时触发并发垃圾回收。
-XX:G1MixedGCLiveThresholdPercent=90：在混合回收中存活对象的比例阈值。
-XX:SurvivorRatio=32：设置伊甸区和幸存者区的比例关系。
-XX:MaxTenuringThreshold=1：对象晋升到老年代的最大年龄。
-XX:+PerfDisableSharedMem：可能对共享内存的使用进行限制，具体影响需根据应用情况判断。
-XX:G1SATBBufferEnqueueingThresholdPercent=30：与 SATB 缓冲区的入队阈值相关。
-XX:G1ConcMarkStepDurationMillis=5：并发标记阶段的步长时间。
-XX:G1RSetUpdatingPauseTimePercent=0：RSet 更新暂停时间占比。
-XX:G1HeapWastePercent=18：堆空间浪费的比例阈值。
-XX:GCTimeRatio=99：设置垃圾回收时间占比的目标。
-XX:+AlwaysActAsServerClassMachine：始终以服务器类机器的模式运行。
-XX:NmethodSweepActivity=1：与方法清理活动相关。
-XX:ThreadPriorityPolicy=1：线程优先级策略。
-XX:ReservedCodeCacheSize=400M：预留的代码缓存大小。
-XX:NonNMethodCodeHeapSize=12M：非方法代码堆的大小。
-XX:ProfiledCodeHeapSize=194M：已分析代码堆的大小。
-XX:NonProfiledCodeHeapSize=194M：未分析代码堆的大小。
-XX:-DontCompileHugeMethods：不编译巨大方法。
-XX:MaxNodeLimit=240000：节点数量限制。
-XX:NodeLimitFudgeFactor=8000：节点限制调整因子。
-XX:+UseVectorCmov：启用向量相关的特定操作。
-XX:+UseFastUnorderedTimeStamps：使用快速无序时间戳。
-XX:+UseCriticalJavaThreadPriority：使用关键 Java 线程优先级。
-XX:+UseLargePages：启用大页面，可能提高内存性能，但需要系统支持，且可能有配置要求。
-XX:+UseTransparentHugePages
-XX:LargePageSizeInBytes=2M：大页面的大小。
-XX:+OptimizeStringConcat：优化字符串拼接操作。
-XX:+DisableAttachMechanism：禁用附加机制。
-XX:+EagerJVMCI：积极启用 JVM 编译器接口相关功能。
-Dgraal.TuneInlinerExploration=1：与 Graal 内联器探索相关。
-Dgraal.LoopRotation=true：启用循环旋转。
-Dgraal.OptWriteMotion=true：优化写入移动。
-Dgraal.CompilerConfiguration=enterprise：设置 Graal 编译器配置为企业版。
-XX:+PrintCommandLineFlags：打印命令行标志。
```

