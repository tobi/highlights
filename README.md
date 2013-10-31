# Highlights

This serves a very simple purpose: Download all your kindle highlights for save keeping and then email it to you. I personally get this delivered every morning and it really helps with remembering the important parts of the books I read. 

## Options

set the following environment variables

* AMAZON_USER - Amazon login
* AMAZON_PASS - Amazon password
* STMP - your smtp server. you can get a free one from mailgun
* USER - SMTP login 
* PASS - SMTP password
* TO - Email address to send the highlight to, e.g. yours


## Commands

* rake download - Will download all your kindle highlights
* rake email - Will take a random highlight from your download and email it to someone (you)
* rake print - Will take a random highlight and print it on the shell

## Best setup

add to your crontab:

```bash
# every mornign at 9am send me a highlight
00 09 * * * * cd ~/src/highlights && rake download TO=... SMTP=... ...

# once a week, download
00 08 00 * * * cd ~/src/highlights && rake download AMAZON_USER=... AMAZON_PASS=...
```