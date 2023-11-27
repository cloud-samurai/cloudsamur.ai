---
layout: post
date: 2023-11-16
categories: [kotlin]
description: "Setup a ramdisk on MacOS X for fast gradle builds"
tags: 
  - kotlin 
  - gradle
image: '/images/gears.jpg'
author: david_maple
---

## Set up Gradle to build on ramdisk (Mac OSX)

### setup the ramdisk with a `gramdisk` command
```bash
sudo touch /usr/local/bin/gramdisk
sudo chmod +x /usr/local/bin/gramdisk
sudo tee -a /usr/local/bin/gramdisk > /dev/null <<EOT
#!/usr/bin/env bash
diskutil erasevolume HFS+ 'gramdisk' `hdiutil attach -nobrowse -nomount ram://16777216`
EOT
```

### Create ~/.gradle/init.gradle
```bash
gradle.projectsLoaded {
    rootProject.allprojects {
        buildDir =
"/Volumes/gramdisk/${rootProject.name}/${project.name}"
    }
}
```
You're ready to blaze through code with superfast in-memory builds!

