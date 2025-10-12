# Another-AD-Lab
A shrimple guide on how to host Active Directory on a Windows 2019 Server using VirtualBox, and how to attack AD using Atomic Red Team

> [!IMPORTANT]
> You can use VMware to set up this entire lab. I would actually recommend it if you have access to the software since VirtualBox Shared Clipboard doesn't work even with Guest Additions installed, from my experience at least. 

# Some Tips Before Diving In
Please check out the <strong>[Sources section](#sources)</strong> of this guide for any important documentation as needed. 

# What You Will Learn!
By the end of this guide, <strong>you are going to know a lot of stuff</strong>; you'll:
- Know how to use VirtualBox to create VMs.
- Know how to setup Active Directory on a Windows Server 2019 machine!

**If you choose to continue past the setup provided by this guide, you'll:**
- Establish simulated attacks in a controlled environment by using Atomic Red Team (ART).
- Gain an understanding into how Active Directory can be attacked w/ tools like Mimikatz.
- Gain an understanding as to how Atomic Red Team tests can be used for defensive purposes to enhance security postures.

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
> The Evaluation Center ISO has a time limit of 180 days on it. You're advised to take a snapshot of your VM when comfortable so that you can always revert back a few days to keep your VM fresh. Of course, if you don't plan on keeping this lab forever, then there is no need. 

Start up the VM and get started with installation! Select the Standard Evaluation (Desktop Experience) option when prompted for what OS you wish to install; we will want a GUI for later when we get to some fun stuff. Select Custom Install and select our empty VM drive and start the actual OS installation. 

Once you're finished installation, log in and update your system! Once all the updates install though, we're gonna want to update and shutdown. Once the VM does shutdown, we need to remove our installation ISO from the VM's SATA ports (pretty easy, just go to the VM settings and you can hit 'Remove Attachment' on the ISO file that you used).

Now once you start the VM again, you'll notice that you need to unlock the machine w/ Crtl+Alt+Del. You can of course enter these keys, or go to the Insert menu in VirtualBox and hit Insert Crtl+Alt+Del and get the same effect.

# Installing Active Directory and Vulnerable-AD-Plus
Simple stuff from here, since it's just copy/paste commands from the [GitHub page](https://github.com/WaterExecution/vulnerable-AD-plus). Firstly, let's install ADDS:

```
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
Import-Module ADDSDeployment
Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath "C:\\Windows\\NTDS" -DomainMode "7" -DomainName "lab.local" -DomainNetbiosName "lab" -ForestMode "7" -InstallDns:$true -LogPath "C:\\Windows\\NTDS" -NoRebootOnCompletion:$false -SysvolPath "C:\\Windows\\SYSVOL" -Force:$true
```

Your VM will restart after AD is installed. Once your system is back up and running, we can continue and install Vulnerable-AD-Plus:

```
IEX((new-object net.webclient).downloadstring("https://raw.githubusercontent.com/WaterExecution/vulnerable-AD-plus/master/vulnadplus.ps1"));
Invoke-VulnAD -UsersLimit 10 -DomainName "lab.local"
```

> [!NOTE]
> You can actually modify the parameters inside the PS1 script that we're using an instead execute it on our local system by downloading it with curl/wget/IWR. This can allow for some funny usernames, passwords, etc. For this guide though, it isn't needed, but for you, it may be a welcome addition to your own lab.

Unfortunately for me, I got an error which basically invalidated the options I gave to vulnerable-AD-plus and it just went ahead and made 100 users in the environment lol. Regardless, you'll be prompted to restart again. Once you reboot, you're officially ready to begin the mock incidents below.

# What's Next?
I would continue this guide, and show you guys how to do a bunch of the Atomic Red Team tests, but it requires a lot of extra work with getting executables and feeding options to all the tests. 

All tests I was planning to do are linked below for you to give a go! Feel free to read the docs for them and get everything setup yourself; primarily downloading Mimikatz and Rubeus for the DCSync and Kerberoast tests, which you can do through the [-GetPrereqs](https://github.com/redcanaryco/invoke-atomicredteam/wiki/Check-or-Get-Prerequisites-for-Atomic-Tests#get-prerequisites) option when running your tests.

* [Atomic Red Team Test - DCSync](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1003.006/T1003.006.md#atomic-test-1---dcsync-active-directory)

* [Atomic Red Team Test - Kerberoast](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1558.003/T1558.003.md#atomic-test-2---rubeus-kerberoast)
* [Atomic Red Team Test - C2 Exfiltration](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1041/T1041.md#atomic-test-1---c2-data-exfiltration)

# Installing Atomic Red Team
```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -getAtomics
cd C:\AtomicRedTeam\invoke-atomicredteam
Import-Module .\Invoke-AtomicRedTeam.psd1
```

# Sources
- [Windows Server 2019 ISOs](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022)
