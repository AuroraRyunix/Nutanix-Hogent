# Nutanix-Hogent
Introducing mayor whack + absolute shenanigans, with a tiny bit of hope~~
Human readable format: Full Nutanix stack, AHV+CVM+PC. Optional extra tasks like network segmentation and overlay networking using the Network Controller.
(Those that know me well enough know i shove some NSX overlay networking in everything)

## Notice
- I will use the terms AHV,CVM,and PC during this guide. The first 2 you should know, PC is just Prism Central.

## What do you need?
- courage
- no fear
- any dell, or hpe server with xeon e5 v3s or e5 v4s.
- Nutanix CE account and iso's, more on this later.
- one ssd bigger then 128gb (AHV)
- one ssd bigger then 512gb (CVM+PC)

## Nutanix Account
- notice: one per team is enough.
- head over to this [link](https://my.nutanix.com/page/signup)
- create a new account, once confirmed by mail, head to [link](https://next.nutanix.com/discussion-forum-14/download-community-edition-38417)
- download the installer iso and VirtIO for Windows ISO, keep VirtIO for later.
- Make sure to remember your account as we need it later.


# And when you're finished?
question yourself are you truly finished? Because i see a lot of big errors in our nutanix environement, let's fix those!
The main one is about unsupported disks, well, cause, you know. Our disks aren't supported. But we know, and we don't care, so let's properly get rid of this error.
I would personally recommend to do your own research first. But if you can't find it, have a look under ~patches/hcl.json, i've given an example that works, certified by yours truly. ^_^
