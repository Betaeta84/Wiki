If you are going to write code either for your own purposes or for the community at large you need to understand the underlying architecture.

# Kernel

The Kernel serves as the central hub of communication between different aspects of the system. These are mapped to particular contexts that have path locations within the kernel. The contexts can have modules opened and modifiers applied to them.

* manages overall program [[lifecycle processes|Tech:-Lifecycle]].
* a central dictionary of [[registered objects|Tech:-Registered-Objects]].
* stores [[persistence values|Tech: Persistence Settings]] at a given [[contexts|Tech:-Contexts]].
* [[context path|Tech: Contexts]] locations for partitioning of information
* provides for [[signals, listeners|Tech: Signals]], partitioned at the context level
* manages [[thread interactions|Tech: Threading]] and [[shutdown|Tech:-Shutdown]]
* provides a [[scheduler, jobs for the scheduler|Tech:-Scheduler]]
* general data [[channels|Tech:-Channels]]
* a command [[console, registered commands|Tech:-Console]]
* execution and operation of [[modules|Tech:-Modules]], [[modifiers|Tech:-Modifiers]], [[plugins|Tech:-Plugins]]
* [[translation information and functionality|Tech:-Translations]]
* run_later operations for moving operations to a gui or other main thread
* [[plugins|Tech:-Plugins]] api
* hooks for [[MeerK40t Specific api elements|Tech:-MeerK40t-Specific-API]]

# Hardware
Understanding the hardware we are trying to drive is a very important step in understanding how we must do what we want to do.

## Lhystudios M2 Nano
* [[M2 Nano Communication Protocols|Tech:-Lhymicro-Control-Protocols]]
* [[M2 Nano Hardware Information|Tech:-Lhymicro-M2-Nano-Hardware]]