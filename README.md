# Another-AD-Lab
A shrimple guide on how to host Active Directory on a Windows 2019 Server using VirtualBox, and how to attack AD using Vulnerable-AD-Plus and Atomic Red Team!

> [!IMPORTANT]
> You can use VMware to set up this entire lab. I would actually recommend it if you have access to the software since VirtualBox Shared Clipboard doesn't work even with Guest Additions installed, from my experience at least. 

# Some Tips Before Diving In
Please check out the <strong>[Sources section](#sources)</strong> of this guide for any important documentation as needed. 

# What You Will Learn!
By the end of this guide, <strong>you are going to know a lot of stuff</strong>; you'll:
- Know how to use VirtualBox to create VMs.
- Know how to setup Active Directory on a Windows Server 2019 machine.
- Establish simulated attacks in a controlled environment by using Atomic Red Team (ART).
- Gain an understanding into how Active Directory can be attacked w/ tools like Mimikatz.
- Gain an understanding as to how Atomic Red Team tests can be used for defensive purposes to enhance security postures.
- Understand [MITRE ATT&CK](https://attack.mitre.org/) and how it's a great tool for understanding different attacks. 

# Getting Started
> [!IMPORTANT]
> If you're coming from my Elastic-ART guide, then let it be known that you can 100% connect your Windows Server to your SIEM lab by just installing Winlogbeat and Sysmon, just like in the guide! It will allow you to gain a refined view of the attacks that we'll be doing, but it is **not required!**

Check out my [other guide for how to install VirtualBox](https://github.com/nubbsterr/Elastic-ART/blob/main/README.md#virtualbox-installation) for your system. I'm doing this on Arch, so you can just:

```bash
sudo pacman -S virtualbox virtualbox-host-dkms
```

And we're finished. **Do note that I am using the Zen Linux kernel, so the kernel modules you download may be different! [Consult the Arch Wiki as needed.](https://wiki.archlinux.org/title/VirtualBox)**

Our next step is to go grab a [Server 2019 ISO from here](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022) (website is lying to you, it is in fact Server 2019) and open up VirtualBox. We'll create our VM with the following specs:

- 4GB of RAM
- 2vCPUs
- ~40GB of Storage

> [!IMPORTANT]
> The Evaluation Center ISO will last only ~180 days. If you don't plan on using this lab in the future, you have nothing to worry about. However, if you plan to continue using it, you should make a VM snapshot to prevent the VM from effectively expiring.

Start up the VM and get started with installation! Select the Standard Evaluation (Desktop Experience) option when prompted for what OS you wish to install; we will want a GUI for later when we get to some fun stuff. Select Custom Install and select our empty VM drive and start the actual OS installation. 

Once the installation finishes, the VM will reboot, make sure to exit from the installation menu by clicking the red 'X'. When prompted, give a password for the Administrator user then let the installation continue.

When prompted, you'll need to press Crtl+Alt+Del to unlock the VM and authenticate as the Administrator user. You can do this by using the Input menu in VirtualBox by going to Input --> Keyboard --> Insert Crtl+Alt+Del.

Now that you're on the desktop, there are a few things for you to do before moving on, which shouldn't require much explanation:

- Update your system
- Disable Windows Defender (assuming you're continuing with the guide in the same VM session, if not, then remember to disable it when you're continuing through the guide)

# Installing Active Directory and Vulnerable-AD-Plus
[Vulnerable-AD-Plus Repo](https://github.com/WaterExecution/vulnerable-AD-plus)

To be continued!

# Mock Incident 1: DCSync
[Atomic Red Team Test](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1003.006/T1003.006.md#atomic-test-1---dcsync-active-directory)

To be continued!

# Mock Incident 2: Kerberoast w/ Rubeus
[Atomic Red Team Test](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1558.003/T1558.003.md#atomic-test-2---rubeus-kerberoast)

To be continued!

# Mock Incident 3: C2 Data Exfiltration 
[Atomic Red Team Test](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1041/T1041.md#atomic-test-1---c2-data-exfiltration)

To be continued!

# Sources
- [Windows Server 2019 ISOs](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022)
- [Sources from my Elastic-ART Guide!](https://github.com/nubbsterr/Elastic-ART?tab=readme-ov-file#sources)
