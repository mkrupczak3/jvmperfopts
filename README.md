So, java stinks basically. It's like there's these crazy kids with an apocalyptic, world ending, huge nerf war going on, shooting darts (memory) around like crazy. Every so often the parents (garbage collector) comes downstairs, sees the mess, and forces the kids (processors) to stop what they're doing to clean up. Time spent sitting around cleaning up means less fun time.

But...

What if we had a really big basement to generate a GIANT mess (lots of RAM) that we baricaded everyone out of and wouldn't share with anyone else?

What if we only cleaned up when we were drowning in darts (memory very full)?

What if we formed a union, and told the parents we'd only clean up at most 1% of the time?

Moreover, what if we said we'd clean up for no more than 25ms at a time?

What if we had people SPECIFICLALY HIRED TO CLEAN UP OUR MESS AND KEEP TRACK OF WHAT TO CLEAN WHILE WE'RE PLAYING (GC threads)

WHAT IF SAFETY WASN'T A CONCERN?

WHAT IF WE TREATED BIGGER MESSES LIKE NO BIG DEAL? (Large Heap region size to make Humongous objects look less humongous)

WHAT IF WE SAID MESSES THAT ARE STILL AROUND AFTER A COUPLE TIMES CLEANING UP ARE "NOT IMPORTANT TO CLEAN UP"? (I find it interesting that java uses the word "tenured" here to describe these messes)

As you can see, this would be rediculous, no one would ever do such a thing...


https://www.oracle.com/technetwork/articles/java/g1gc-1984535.html

https://docs.oracle.com/javase/9/gctuning/garbage-first-garbage-collector-tuning.htm#GUID-8D9B2530-E370-4B8B-8ADD-A43674FC6658

https://stackoverflow.com/questions/36345409/why-does-g1gc-shrink-the-young-generation-before-starting-mixed-collections#36428963


-Xms12G -Xmx12G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:GCTimeRatio=99 -XX:MaxTenuringThreshold=4 -XX:G1NewSizePercent=60 -XX:InitiatingHeapOccupancyPercent=90 -XX:G1MixedGCLiveThresholdPercent=95 -XX:G1ReservePercent=5 -XX:G1MixedGCCountTarget=30 -XX:G1OldCSetRegionThresholdPercent=5 -XX:MaxGCPauseMillis=40 -XX:G1HeapRegionSize=32M -XX:G1ConcRefinementThreads=2 -XX:ParallelGCThreads=4 -XX:G1HeapWastePercent=70 -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+ParallelRefProcEnabled -XX:G1UseAdaptiveIHOP -XX:+AggressiveOpts


-Xms10G -Xmx10G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:GCTimeRatio=99 -XX:MaxTenuringThreshold=4 -XX:G1NewSizePercent=60 -XX:InitiatingHeapOccupancyPercent=90 -XX:G1MixedGCLiveThresholdPercent=95 -XX:G1ReservePercent=7 -XX:G1MixedGCCountTarget=8 -XX:G1OldCSetRegionThresholdPercent=5 -XX:MaxGCPauseMillis=40 -XX:G1HeapRegionSize=32M -XX:G1ConcRefinementThreads=2 -XX:ParallelGCThreads=4 -XX:G1HeapWastePercent=70 -XX:+ParallelRefProcEnabled -XX:+DisableExplicitGC -XX:+AggressiveOpts

-Xms10G -Xmx10G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:GCTimeRatio=99 -XX:MaxTenuringThreshold=4 -XX:G1NewSizePercent=60 -XX:InitiatingHeapOccupancyPercent=90 -XX:G1MixedGCLiveThresholdPercent=95 -XX:G1ReservePercent=7 -XX:G1MixedGCCountTarget=8 -XX:G1OldCSetRegionThresholdPercent=5 -XX:MaxGCPauseMillis=40 -XX:G1HeapRegionSize=32M -XX:G1ConcRefinementThreads=2 -XX:ParallelGCThreads=4 -XX:G1HeapWastePercent=70 -XX:+ParallelRefProcEnabled -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+AggressiveOpts

-Xms10G -Xmx10G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:GCTimeRatio=99 -XX:MaxTenuringThreshold=4 -XX:G1NewSizePercent=60 -XX:InitiatingHeapOccupancyPercent=90 -XX:G1MixedGCLiveThresholdPercent=95 -XX:G1ReservePercent=7 -XX:G1MixedGCCountTarget=8 -XX:G1OldCSetRegionThresholdPercent=5 -XX:MaxGCPauseMillis=25 -XX:G1HeapRegionSize=32M -XX:G1ConcRefinementThreads=2 -XX:ParallelGCThreads=4 -XX:G1HeapWastePercent=70 -XX:+ParallelRefProcEnabled -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+AggressiveOpts