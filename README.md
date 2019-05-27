![pic of dog doesn't know what its doing](https://i0.kym-cdn.com/entries/icons/original/000/008/342/ihave.jpg "Ruff!")

So, java stinks basically. It's like there's these crazy kids with an apocalyptic, world ending, huge nerf war going on, shooting darts (memory) around like crazy. Every so often the parents (java) comes downstairs, sees the mess, and forces the kids (processors) to stop what they're doing to clean up. Time spent sitting around cleaning up means less fun time.



But...

What if we had a really big basement to generate a GIANT mess (lots of RAM) that we baricaded everyone out of and wouldn't share with anyone else?

What if we only cleaned up when we were drowning in darts (memory very full)?

What if we formed a union, and told the parents we'd only clean up at most 1% of the time?

Moreover, what if we said we'd clean up for no more than 25ms at a time?

What if we had people SPECIFICLY HIRED TO CLEAN UP OUR MESS AND KEEP TRACK OF WHAT TO CLEAN WHILE WE'RE PLAYING (GC threads)

WHAT IF SAFETY WASN'T A CONCERN?

WHAT IF WE TREATED BIGGER MESSES LIKE NO BIG DEAL? (Large Heap region size to make Humongous objects look less humongous)

WHAT IF WE SAID MESSES THAT ARE STILL AROUND AFTER A COUPLE TIMES CLEANING UP ARE "NOT IMPORTANT TO CLEAN UP"? (I find it interesting that java uses the word "tenured" here to describe these messes)

As you can see, this would be rediculous, no one would ever do such a thing...


https://www.oracle.com/technetwork/articles/java/g1gc-1984535.html

https://docs.oracle.com/javase/9/gctuning/garbage-first-garbage-collector-tuning.htm#GUID-8D9B2530-E370-4B8B-8ADD-A43674FC6658

https://stackoverflow.com/questions/36345409/why-does-g1gc-shrink-the-young-generation-before-starting-mixed-collections#36428963



# Opts

-Xms10G -Xmx10G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:GCTimeRatio=99 -XX:MaxTenuringThreshold=4 -XX:G1NewSizePercent=60 -XX:InitiatingHeapOccupancyPercent=90 -XX:G1MixedGCLiveThresholdPercent=95 -XX:G1ReservePercent=7 -XX:G1MixedGCCountTarget=8 -XX:G1OldCSetRegionThresholdPercent=5 -XX:MaxGCPauseMillis=25 -XX:G1HeapRegionSize=32M -XX:G1ConcRefinementThreads=2 -XX:ParallelGCThreads=4 -XX:G1HeapWastePercent=70 -XX:+ParallelRefProcEnabled -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+AggressiveOpts

# Tuning:
Use as much RAM as humanly (or, uh, otherwise) possible. As of writing this, you can buy 32GB of ECC DDR3 ram for 100$ on amazon. combine that with a dual core or quad core computer from a dumpster somewhere and you've got a great server put together.

Set -Xms and -Xmx to nG where n is the number of gigabytes of memory you are willing to sacrifice
Set ParallelGCThreads to the number of cores on your system.
Set G1ConcRefinementThreads to anywhere between n/4 to n/2 where n is the number of cores on your system.

# Use Cases:

Game Client: Run a minecraft client with a lot of RAM, no stuttering, only a small "blip" every few minutes, very good for competitive (e.g. multiplayer) etc.

Game Server: Run a minecraft server with almost no lag or "rubber-banding". Brief blips occasionally, but the more memory that is allocated the less often these small blips happen.

Real time video: Streaming images, video, etc. Where extremely small blips are preferable to small pauses

Other: IDK, maybe some kind of backend infrastructure where response time to a user is more important than using ungodly amounts of RAM. This conifg isn't desined for "throughput" in any traditional sense, but instead is focused on insanely lazy garbage collection: Cleaning up only when we are absolutely forced to, cleaning up for an extremely short period of time, and being really smart/lazy about what we choose to clean up when we do it.

# Improvement:
Honestly, the biggest bottlenecks here is probably -XX:G1NewSizePercent=60. This value is way higher than it is normally, and for some reason 60 is the highest value I've been able to run on the OpenJDK JVM without crashing. Pushing this value higher to 80 or 90 would allow these brief "blips" to only happen at 90% used RAM instead of 60, allowing almost 50% longer periods between blips for a given amount of memory. 

It is possible that this advanced config with the newer G1GC is outperformed by older collecters, but IDK. 

# Thoughts:
The real takeaway here is that cleanup is no fun, and the only way around it is to be as messy as possible.


