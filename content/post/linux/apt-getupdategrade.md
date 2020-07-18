---
title: "apt-get update和upgrade和dist-upgrade的区别"
date: 2019-12-07T22:37:54+08:00
categories:
- Linux
tags:
- apt-get
- debian
keywords:
- apt-get
#thumbnailImage: //example.com/image.jpg
---



```powershell
update
      update is used to resynchronize the package index files from their sources. The indexes of available packages are fetched from the location(s) specified in /etc/apt/sources.list. For example, when using a Debian archive, this command retrieves and scans the Packages.gz files, so that information about new and updated packages is available. An update should always be performed before an upgrade or dist-upgrade. Please be aware that the overall progress meter will be incorrect as the size of the package files cannot be known in advance.

upgrade
      upgrade is used to install the newest versions of all packages currently installed on the system from the sources enumerated in /etc/apt/sources.list. Packages currently installed with new versions available are retrieved and upgraded; under no circumstances are currently installed packages removed, or packages not already installed retrieved and installed. New versions of currently installed packages that cannot be upgraded without changing the install status of another package will be left at their current version. An update must be performed first so that apt-get knows that new versions of packages are available.

dist-upgrade
       dist-upgrade in addition to performing the function of upgrade,also intelligently handles changing dependencies with new versions of packages; apt-get has a "smart" conflict resolution system, and it will attempt to upgrade the most important packages at the expense of less important ones if necessary. The dist-upgrade command may therefore remove some packages. The /etc/apt/sources.list file contains a list of locations from which to retrieve desired package files. See also apt_preferences(5) for a mechanism for overriding the general settings for individual packages.
```

- update的作用是从/etc/apt/source.list文件中定义的源中去同步包的索引文件，

-  upgrade则是更据update命令同步好了的包的索引文件，去真正地更新软件。

- dist-upgrade则是更聪明的upgrade，man文档中说它以更聪明的方式来解决更新过程中出现的软件依赖问题，它也是从/etc/apt/source.list文件中获得地址，然后从这些地址中检索需要更新的包。


