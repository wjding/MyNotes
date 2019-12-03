1. What is Linux capabilities?
Capabilities breaks down the control of privileges with distinct units.
Capabilities is introduced from Linux kernel 2.2.

1. What you should know to capabilities?
   1. Capabilities can be an attribute assigned to a process/thread. It 
includes 3 different attributes:
      1. Permitted
      2. Inheritable
      3. Effective
   1. Capabilities can also be an attribute assigned to an executable. It has
the same 3 attributes as that of process/thread. This impacts the process
behavior. The capabilities of an executable can be operated by getcap, setcap.
   1. In order to make capabilities work for a normal user, the executable
must have corresponding capabilities bit set.
