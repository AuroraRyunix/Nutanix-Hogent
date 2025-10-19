# Nutanix-Hogent
Full Nutanix stack, AHV+CVM+PC. Optional extra tasks like network segmentation and overlay networking using the Network Controller.
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

## Flashing the ISO
- Think this would be easy? Think again. It is easy, IF you read the docs; #rtfm am i right?
- There's currently a bug in Rufus above 3.21, so use an older rufus, or unetbooting/win32diskimager. [source](https://portal.nutanix.com/page/documents/details?targetId=Nutanix-Community-Edition-Getting-Started-v2_1:top-installing-ce-t.html)
- Make sure to disable Secure boot, and actually use the old legacy boot this time, works better for Nutanix.

# Installing AHV
- Here comes the fun part, waiting... tududud
- when the waiting is done you'll get a disk selection screen, do as i say, don't do as i do!
- Use the smallest ssd as boot/hypervisor (H)
- Use the bigger ssd as CVM (C)
- Use the other hdds as data (D)
- DO NOT CLICK NEXT (lol)
- Double check everything, assign 2 ips that'll be accesible on vlan0 (untagged); one for the CVM and one for AHV.
- Personally i always use 10.10.0.1/24 (yes CIDR my beloved, google that if you don't know what it is!)
- So i set AHV to 10.10.0.4, and CVM to 10.10.0.5, PC will later be set to 10.10.0.8.
- STILL DO NOT CLICK NEXT. 
- have your config double checked by the person responsible, the supervisor. Not to be confused with hypervisor. ^_^
- Hit install, and go take a 1 hour break, yeah that's how long it takes, maybe even more.

![Alt text](assets/hinstall.png)
- Oh yay it's done! I hope it worked as well as it did for me, if not, uuuhm. Yikes, call support.

![Alt text](assets/hinstallcomp.png)
- Notice: But won't hdds be slow? Yes, BUT, nutanix CE uses the spare CVM space for caching, and that works wonders for what we're doing (:
- Notice: if you have a boot ssd, it can go a lot faster, i've tested most stuff on an kioxia CM6, which took about 25 min for the full install.
- After around 20-30 min you'll see "INFO Hypervisor Installation in progress". That's the CVM being deployed

## Starting the cluster.
- ssh into AHV (10.10.0.4 for me), use the root user. change the password of root, nutanix, and admin. The nutanix user will eventually be deprecated.
- do the same for CVM (10.10.0.5). Make sure to remember them, they can be the same.
- DO THIS BEFORE PROCEEDING, MODIFYING THE PASSWORDS AFTERWARDS REQUIRE A MORE COMPLEX APPROACH AS THEY ARE LINKED IN DATABASES ELSEWHERE
- YOU HAVE BEEN WARNED
- exit the CVM, and ssh into it again, with the admin user.
- run the magic command: cluster -s CVM_IP --redundancy_factor=2 create (where CVM_IP is.... The bloody ip of your one cvm! So in my case 10.10.0.5)
- Doing so should give a line about Cluster:XXXX Will seed prism with password hash....

![Alt text](assets/clustercreate.png)
- This took about 100 sec~ on my NVMe system. Shouldn't take much longer, after a while you'll get a list like this:

![Alt text](assets/zeusstart.png)
- This basically means it's waiting for all the newly set up services to start, so sit back and enjoy some more coffee, or tea (you know who you are!)
- This can take another few minutes, took mine about 15 of them! But do not worry, we're almost there!





# And when you're finished?
question yourself are you truly finished? Because i see a lot of big errors in our nutanix environement, let's fix those!
The main one is about unsupported disks, well, cause, you know. Our disks aren't supported. But we know, and we don't care, so let's properly get rid of this error.
I would personally recommend to do your own research first. But if you can't find it, have a look under ~patches/hcl.json, i've given an example that works, certified by yours truly. ^_^
