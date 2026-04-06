---
title: "[Week 5] Tutorials 5 and 6"
date: 2026-03-25 19:02:00 -0300
last_modified_at: 2026-04-05 23:06:00 -0300
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

# My experience

Completing tutorial 5 involved configuring `git send-email` and the SMTP protocol via the terminal to enable sending patches for contribution within the development community.

Initially, I tried using my institutional email from USP (University of São Paulo), but I discovered that accounts managed by the organization generally block the creation of application passwords. The solution was to migrate to my personal Gmail account, where I could enable two-step verification and generate the 16-character password requested by Git.

Even with the password in hand, I discovered that Git was trying to send emails via localhost on port 25 (the system's default port). So I just ran the tutorial commands to adjust the local repository settings to use smtp.gmail.com on port 587, and after that, it worked, with the status "Result: 250" in the terminal confirming that the connection was successfully established.

As recommended by the tutorial, I used `--dry-run` and `--suppress-cc=all` to test if the `git send-email` configuration actually worked.

With tutorial 5, I was able to understand how the collaborative culture of a project works, which is done via email; I found that quite interesting.





# Tutorial 6

The [sixth tutorial](https://flusp.ime.usp.br/git/sending-patches-with-git-and-a-usp-email/) explained how to send patches with USP institutional email. 

To contribute to projects like the Linux Kernel, it's complicated to use an institutional email (@usp.br), as administrators disabled "Less Secure Applications" after security incidents in 2025. Since application passwords are also unavailable, the solution suggested in the tutorial was to use OAuth 2.0 through a local email proxy. For this, we used a Docker container running an OAuth2 email proxy, which, as I understand it, acts as a bridge for `git send-email` to communicate. Then, the proxy communicates with Google using OAuth tokens, and Google sends the email.

- Some commands of workflow

| Step                 | Command                                                                 |
| Start Container   | docker compose up --build                                               |
| Enter Container   | docker exec -it emailproxy-container-server-1 /bin/bash                 |
| Start Proxy       | emailproxy --no-gui --external-auth --config-file /app/emailproxy.config|
| Setup kw          | kw send-patch --setup --smtpserver '127.0.0.1' --smtpserverport '2587'  |
| Send/Test         | kw send-patch --send --private --to='email@example.com'               |


# My experience

I ended up doing tutorial 5 with my personal email account, which was good for learning, and then I did tutorial 6. The most notable obstacle was Error 403: access_denied. Even with the proxy running, Google blocked my login, but after talking to David, he removed and re-added my email address to the 'developer-approved testers' list. After that, I tried again and everything went as expected in tutorial 6. It may have been some problem with the Google API that didn't register my email. After that, the whole process went smoothly and I was able to send the patch to my email and the email for the free software development course.
Now I can contribute by sending patches through my USP email!