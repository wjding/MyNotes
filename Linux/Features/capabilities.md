# Capabilities
## What is Linux capabilities?

Capabilities breaks down the control of privileges with distinct units.
Capabilities is introduced from Linux kernel 2.2.

## What should you know to capabilities?
   1. Capabilities can be attributes assigned to a process/thread. They can be
of 3 types:
      1. Permitted
      1. Inheritable
      1. Effective
   1. Capabilities can also be attributes assigned to an executable. They have
the same 3 types as those of process/thread. This impacts the corresponding
process behavior, which is created by exec the executable. The capabilities of
an executable can be operated by getcap, setcap, etc. 
   1. In order to make capabilities work for a normal user, there are a few
criteria to meet:
      1. The executable must have corresponding capabilities bit set.
      1. The process that is forking the executable must have corresponding
      capabilities bit set with Permitted and Inheritable. 

   1. One can use below command: `grep Cap /proc/<pid>/status` to observe a
   running process' capabilities. 
      ```
      CapInh: 0000000000000000
      CapPrm: 0000000000000000
      CapEff: 0000000000000000
      CapBnd: 0000003fffffffff
      CapAmb: 0000000000000000
      ```
   Explanations as below:
      ```
       **CapInh**   bitmap of inheritable capabilities
       **CapPrm**   bitmap of permitted capabilities
       **CapEff**   bitmap of effective capabilities
       **CapBnd**   bitmap of capabilities bounding set
       **CapAmb**   bitmap of ambient capabilities
      ```

## Docker related operations
   1. Examples
   SYS_NICE is 0000000000800000. Capabilities of the current shell can be
   checked by `grep Cap /proc/$$/status`
      1. Container without capabilities:
         ```
         root@scg-test01:~# docker run -it -u 1000:1000 --cap-drop all  debian:wjdingtmp bash
         lss@b9fc7535a944:/$ grep Cap /proc/$$/status 
         CapInh: 0000000000000000
         CapPrm: 0000000000000000
         CapEff: 0000000000000000
         CapBnd: 0000000000000000
         CapAmb: 0000000000000000
         ```
      1. With SYS_NICE on:
         ```
         root@scg-test01:~# docker run -it -u 1000:1000 --cap-drop all --cap-add SYS_NICE debian:wjdingtmp bash
         lss@e035892778ca:/$ grep Cap /proc/$$/status 
         CapInh: 0000000000800000
         CapPrm: 0000000000000000
         CapEff: 0000000000000000
         CapBnd: 0000000000800000
         CapAmb: 0000000000000000
         ```
      Having the SYS_NICE capability does not mean a user can adjust NICE of a process to higher (lower value):
         ```
         lss@eff29e81a251:/$ grep Cap /proc/$$/status
         CapInh: 0000000000800000
         CapPrm: 0000000000000000
         CapEff: 0000000000000000
         CapBnd: 0000000000800000
         CapAmb: 0000000000000000
         lss@eff29e81a251:/$ nice -n -10 sleep 10 &
         [1] 11
         lss@eff29e81a251:/$ nice: cannot set niceness: Permission denied

         lss@eff29e81a251:/$ ps ax -o pid,ni,cmd
           PID  NI CMD
             1   0 bash
            11   **0** sleep 10
            12   0 ps ax -o pid,ni,cmd
         lss@eff29e81a251:/$ 
         lss@eff29e81a251:/$ nice -n 10 sleep 10 &
         [2] 13
         [1]   Done                    nice -n -10 sleep 10
         lss@eff29e81a251:/$ ps ax -o pid,ni,cmd
           PID  NI CMD
             1   0 bash
            13  **10** sleep 10
            14   0 ps ax -o pid,ni,cmd
         ```
      This is because the nice executable does not have a capability bit set:
         ```
         lss@eff29e81a251:/$ getcap /usr/bin/nice 
         lss@eff29e81a251:/$ 
         ```
      Now let's see another case, where the executable has the SYS_NICE
      capability bit set:
         ```
         lss@eff29e81a251:/$ getcap /usr/bin/renice
         /usr/bin/renice = cap_sys_nice+eip
         lss@eff29e81a251:/$ sleep 3600 &
         [1] 18
         lss@eff29e81a251:/$ ps ax -o pid,ni,cmd | grep -v grep | grep sleep
            18   **0** sleep 3600
         lss@eff29e81a251:/$ renice -n 10 -p 18
         18 (process ID) old priority 0, new priority 10
         lss@eff29e81a251:/$ ps ax -o pid,ni,cmd | grep -v grep | grep sleep
            18  **10** sleep 3600
         lss@eff29e81a251:/$ renice -n -10 -p 18
         18 (process ID) old priority 10, new priority -10
         lss@eff29e81a251:/$ ps ax -o pid,ni,cmd | grep -v grep | grep sleep
            18 **-10** sleep 3600
         ```


   1. Procedure to use capabilities in a least privileged container:
      1. Assign the needed capabilities to the executable. 
      1. Make sure the owner of the executable is root.
      1. Commit the docker image. 
      1. Start the container with the docker image, drop the root privilege,
      and assign the corresponding capabilities using below parameters:
      ```
      -u <uid>:<gid> --cap-drop all --cap-add SYS_NICE```
      1. Start the executable which has the capabilities set, and the
      process/thread now has the corresponidng capabilities. 
