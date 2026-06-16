---
title: "[Weeks 7-8] First Patch"
date: 2026-04-20 22:16:00 -0300
last_modified_at: 2026-04-20 22:16:00 -0300
categories: [Free Software Development, Linux kernel patch, First patch]
tags: [kernel, open source, free software development]
---

# My experience

In this first contribution patch to the Linux kernel, my partner Naili and I opted for the suggestion to replace calls to mutex_lock(&lock) and mutex_unlock(&lock) with guard(mutex)(&lock) for parts of the code that require synchronization when dealing with concurrent process streams accessing critical portions of code where there is shared data, race conditions, etc. 

To do this, we followed the tutorial explained in the pad: we cloned the kernel code, searched for files that used the approach with calls to mutex_lock(&lock) and mutex_unlock(&lock) to replace with guard(mutex)(&lock), and found a good candidate for beginners like us in the world of kernel contribution: the file drivers/iio/common/ms_sensors/ms_sensors_i2c.c. 

So we worked on it, making the replacements, with the purpose of better handling cleanup after calling mutex_lock(&lock), since, as David explained in class, some of the potential problems with using it are that the developer may forget to call mutex_unlock(&lock), or, depending on the conditional instructions in the critical section of the code, many calls to mutex_unlock(&lock) are necessary. Therefore, to avoid these problems and code complexity, we use guard(mutex)(&lock), which unlocks the mutex if and only if it leaves the scope of the function in which it is located, which can be in any of the return instructions.

After finishing the replacement, we sent the commit to the CI pipeline, where it went through all the tests, and then we sent it to the world, that is, to the maintainers. 

The feedback was relatively quick, and in it Andy asked for a 'slow down' on sending patches and that we pay attention to the comments of other patches sent before submitting more. Naili and I were confused since it was our first attempt at contributing, so we replied that we thought there might have been a misunderstanding as there were no previous patches under our names, but we appreciated the feedback. 

Then, quickly looking at the comments on patches that had similar approaches to guard(mutex)(&lock), I saw that some recommendations were to alphabetically order the #includes, so I also mentioned this in the email to Andy about a possible new patch version after we resolve the slowdown request, and for now we await a response.

Andy responded, admitting that he had confused our patch with someone else's due to the large and sudden influx of similar patches. He concluded with an observation that ideally, newcomers should review at least 10 other patches in the subsystem before submitting their first.