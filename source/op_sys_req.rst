System requirements
====================

To install NextGIS Web On-Premise you need a physical or virtual server based on x86_64 (amd64) with the following parameters:

======= =========== =============
\       Minimum     Recommended
======= =========== =============
CPU     4-core      8-core
RAM     8 GiB       16 GiB
Storage 250 GB SSD  500 GB SSD
======= =========== =============

The amount of storage space required depends primarily on the estimated volume of data that will be stored in the system. We recommend using SSD or NVMe drives for better performance. The processor clock speed is not of fundamental importance, but using a CPU with a higher clock speed has a positive effect on system performance, as the most resource-intensive tasks in NextGIS Web are related to image compression when viewing maps.  

The server must have a Linux-based operational system allowing to install Docker Engine 23 and up, for example:

-  Ubuntu LTS 24.04, 22.04
-  Debian 12, 11
-  CentOS Stream 11, 10

If you use a different Linux distribution, make sure that the Linux kernel version on the server is 5.10 or above, which corresponds to the current LTS version.

To avoid conflicts the server must not be used for other purposes: as a file or mail server, PostGIS server etc.

To follow the steps described in this manual you'll need access as a ``root`` user. Internet access is not required for installation or updates, but it makes the process much easier.
