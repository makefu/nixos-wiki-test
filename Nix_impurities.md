---
title: Nix impurities
permalink: /Nix_impurities/
---

What can theoretically cause different build results?

-   current date
-   cpu
-   RAM available
-   fileystem
-   kernel running the builds \[1\]
-   is a virtualization system being used ?
-   sensors (such as temperature)
-   timing behaviour of any component in use (fs, ..) (causing timeouts and such)

\[1\] quoting Lluis Batlle:

We have the kernelHeaders. Programs should respect kernelHeaders for the kernel API, and not talk to the running kernel.

-   gconf runs in daemon mode
-   qt -running qgit

` "Cannot mix incompatible Qt library (version 0x40800) with this library (version 0x40801)"`

runtime impurities
------------------

Stateful directories in NixOS: /etc, /var, /home can cause packages to do what they usually do. In case of trouble make a backup, delete and recreate those to trouble shoot a problem.