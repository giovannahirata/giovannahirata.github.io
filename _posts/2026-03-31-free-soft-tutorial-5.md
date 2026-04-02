---
title: "[Week 5] Tutorials 5 and 6"
date: 2026-03-25 19:02:00 -0300
last_modified_at: 2026-03-31 19:03:00 -0300
categories: [Free Software Development, Tutorials, Tutorials 5 and 6]
tags: [kernel, open source, free software development]
---

# Tutorial 5: sending patches via email with git

The Linux Kernel community operates through email mailing lists, unlike modern web development that relies on interfaces like GitHub or GitLab.

So, this [fifth tutorial](https://flusp.ime.usp.br/git/sending-patches-by-email-with-git/) teaches how to configure `git send-email` to submit contributions without corrupting code formatting. 

- Settup and installation:

Git does not support email by default on most distros. So, on Debian/Ubuntu, we run `sudo apt-get install git-email`:

Using a real name and consistent email is mandatory for the kernel's legal and technical tracking, so we run: `git config --global user.name "complete name"` and `git config --global user.email "your@email.com"`.

SMTP server:
```bash
git config --global sendemail.smtpserver smtp.gmail.com

git config --global sendemail.smtpuser "your@email.com"

git config --global sendemail.smtpencryption tls

git config --global sendemail.smtpserverport 587
```

- Development workflow and useful commands:

Involves commiting changes locally and the dispatching them to the mailing list (or ourself for testing).

| Command              | Description                                                                                   |
| git send-email -1    | prepares and sends the very last commit performed.                                            |
| --annotate           | opens the editor before sending for final review or to add notes.                             |
| --dry-run            | simulates the entire process without actually sending the email.   |
| --suppress-cc=all    | prevents the patch from being automatically sent to everyone mentioned in the code (avoids accidental spam). |
| --cover-letter       | creates an introductory email (0/N) to explain a series of patches.                           |
| -v2, -v3...          | adds a version prefix to the email subject (e.g., [PATCH v2]).                                |







# Tutorial 6

The [sixth tutorial]()



# My experience