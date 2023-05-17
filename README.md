# How to build a 'virus proof' laptop for grandma

- The goal of todays repo (and youtube video) is to explain the thought process when creating a safe, secure, and easy to use desktop evironment for those less familiar with modern online threats like malware, scammers, and ransomware. 
	- Note that this is a `quick and easy guide`, designed to be followed by the one person in the family who became designated as "the tech savvy one" because they learned what those yellow, red, and white cables in the back of the VCR player did. 
	- This is only the `tip of the iceburg` when it comes to online safety and computer security
	- The files contained in this repository are more than likely `not` going to receive future updates. There will be documentation links for future-dwellers to compare against the official docs and make adjustments as necessary 
		- (ex: if the chrome options change, as they do every third update....)
- **Legalese disclaimer:** always consult a professional, this comes as is and I am abolutely not responsible for anything that happens, now, later, or ever to any of the electons flowing through your device, as a result of the guidance here. 

### How we'll turn a laptop into a fortress
- `a strategic choice of operating system` (ub22)
- `automated updates` (cron)
- `enterprise browser policies` (chrome, firefox)
- `wide spectrum malware and content blocking` (ublock origin)
- `privledge restrictions` (sudo)
- `secure, easy remote assistance` (rustdesk?)

<!-- Background Knowledge -->
## Background Knowledge

#### Scams
- Typically, scam based exploits are entirely user facing - vulnerable users invite them in to remotely access the system with programs like teamviewer, anydesk, and similar. 
- Jim Browning's youtube channel has a lot of good examples of scam based attacks [Jim Browning - YouTube](https://www.youtube.com/@JimBrowning) 

#### Viruses and Malware
- For the typical user, viruses and malware end up on a system from the user saying 'yes' to a prompt somwhere along the line. 
	- By restricting admin privledges (on any OS) the majority of these programs are rendered signficantly less effective
- One of the most prominent methods malicious programs get introduced is through the web browser, mainly through adverts. 

#### Ransomware
- The most common entry methods for ransomware are the two categories listed above. They can be executed by a user, or installed by a scammer with remote access. 

## Choosing an operating system

[Desktop Operating System Market Share Worldwide | Statcounter Global Stats](https://gs.statcounter.com/os-market-share/desktop/worldwide)

![operating system breakdown](https://github.com/qkNorris/virus-free-grandma/blob/2e5f88a74c97d2fd99e3bf9d5d40f569a3a27a72/snapshot-os_combined-04-2023-desktop-00.png)

- Due to the market share of operating systems, most scams, and malicious software is typically coded for the two largest Operating Systems - Windows and MacOS. 
- For this reason, one of the best ways a vulnerable user can avoid these issues is to simply run a less popular OS. Scam based call centers generally train their employees on how to exploit vulernable users on Windows and MacOS. Running a different operating system can prevent their script from going as planned, exposing the scam, or prevent access entirely if they don't have a binary for the target system. 
	
*Given the information we know about Operating System selection, I see two obvious choices*
- `Linux`
- `ChromeOS`

*For this tutorial we'll be targeting Linux, due to my personal familiarity over chromeOS*
- `Ubuntu 22.04 Cinnamon to be precise.`

<!-- Prerequisites -->
## Prerequisites

Heres my current setup, in case you want to mirror it. I'll provide alternatives too.
- an already setup windows computer
- a 8GB usb flash drive
- a computer you want to wipe, and setup for Grandma

<!-- GETTING STARTED -->
## Getting Started

https://ubuntucinnamon.org/ 
![cinnamon banner](https://github.com/qkNorris/virus-free-grandma/blob/a90227aabd98cded49d066509181fd83f0648179/Pasted%20image%2020230515174731.png)

<!-- Creating an ubuntu flash drive -->
### Creating an ubuntu flash drive

1.  On an existing windows computer, `navigate to` [ubuntucinnamon.org](https://ubuntucinnamon.org)
2. `click download`. The version at the time of writing is 22.04.2 LTS. 
	- LTS versions are always reccommended for Ubuntu. 

3. while that file is downloading, go ahead and `grab a flash drive and plug it in`. Your flash drive will need to be `8 GB` or larger. Files on flash drive will be wiped, so *make sure theres nothing already on it.* 

4. Once the file is done downloading, we'll use `Rufus` to burn the ubuntu iso to the flash drive. 
	- `Navigate to` [Rufus - Create bootable USB drives the easy way](https://rufus.ie/en/) 
	- Head down to the downloads section, and select the download for
		- `Standard Windows x64`. 
		- If you somehow have a 32 bit system, click the x86 standard, but you probably don't in 2023. 
5. Open rufus, by clicking on the downloaded file. 
	- At the top of rufus, you'll have a selection called "device"
		- Make sure this is your flash drive. In my case it says `PNY [8 GB]`
		- Double check the size to make sure its your flash drive. 
	- Under boot selection, select `Disk or ISO image`
	- To the right of that, click `select`
	- Now, navigate to your `Downloads` folder and select the file we just downloaded
		- click on `ubuntucinnamon-2X.XX-amd64` and click `open`
	- under `ISOIMAGE` give the flash drive a good name, I went with `ubuntuinstaller` 
	- Then click start. This will erase the current files on your flash drive, and turn it into a bootable ubuntu installer. 
		- Select `ISO mode (reccommended)` and agree to the prompts. 
	- You should now see a green progress bar as rufus creates our installer. 

<!-- Booting from the flash drive -->
### Booting from the flash drive

1. go to google, and search for `hp elitebook boot menu key` , replacing hp elitebook with the model of Grandmas laptop. 
    -  eg. `dell xps boot menu key`
2. Make sure the laptop is powered off, then press the power button
3. While the laptop is turning on, `spam the boot menu key repeatdly` until the menu appears
    -  in my case, i had to press `f2` repeatedly during boot
4. From the boot menu, select your flash drive using the arrow keys and press enter its. `UEFI PNY ubuntuinstaller` in my case. 

<!-- installing ubuntu -->
### installing ubuntu

-  select `install ubuntu`
-  select `erase disk and install ubuntu`
-  select `use LVM`
-  

<!-- Booting from the flash drive -->
### Booting from the flash drive
```
sudo apt update && sudo apt upgrade -y && sudo reboot
```
#### sudo 
cd /etc/sudoers.d/
touch standardusersudo
id grandma
nano standusersudo

```
%grandma ALL=(ALL) /sbin/reboot
%grandma ALL=(ALL) /sbin/shutdown
%grandma ALL=(ALL) /usr/bin/apt update
%grandma ALL=(ALL) /usr/bin/apt upgrade -y
%grandma ALL=(ALL) /usr/bin/dpkg --configure -a
```  

#### browser policies for Firefox

```
sudo snap refresh
```

```
mkdir -p /etc/firefox/policies/
cd /etc/firefox/policies/
touch policies.json
  
nano policies.json  
```

```
{
  "policies": {
    "DisablePocket": true,
    "DisableFirefoxAccounts": true,
    "DisableSafeMode": true,
    "DisableTelemetry": true,
    "OfferToSaveLoginsDefault": true,
    "SearchEngines": {
      "PreventInstalls": true
    },
    "SSLVersionMin": "tls1.1",
    "ExtensionSettings": {
      "*": {
        "blocked_install_message": "User can not",
        "install_sources": [
          "about:addons",
          "https://addons.mozilla.org/"
        ],
        "installation_mode": "blocked",
        "allowed_types": [
          "extension"
        ]
      },
      "uBlock0@raymondhill.net": {
        "installation_mode": "force_installed",
        "install_url": "https://addons.mozilla.org/firefox/downloads/latest/ublock/origin/latest.xpi"
      }
    }
  }
}
```

#### Policies for chrome
/etc/opt/chrome/policies/managed/test_policy.json  
/etc/chromium-browser/policies/managed  
  
note: chromium and chrome have different policy paths.  
  
Note: extension ID's are in the chrome app store's url  
  
this code block is deprecated - linux browser policies are now deployed by puppet  
  
mkdir -p /etc/chromium-browser/policies/managed/ 
cd /etc/chromium-browser/policies/managed/ 
touch chomium_policy.json 
nano chomium_policy.json 


```
{
	"AllowOutdatedPlugins": 0,
	"AutofillAddressEnabled": false,
	"AutofillCreditCardEnabled": false,
	"BackgroundModeEnabled": false,
	"BrowserGuestModeEnabled": false,
	"BrowserSignin": 0,
	"ChromeCleanupEnabled": false,
	"ChromeCleanupReportingEnabled": false,
	"DefaultSearchProviderEnabled": true,
	"DefaultSearchProviderName": "Google Encrypted",
	"DefaultSearchProviderSearchURL": "https://www.google.com/search?q={searchTerms}",
	"DownloadRestrictions": 1,
	"ImportAutofillFormData": false,
	"ImportSavedPasswords": false,
	"MetricsReportingEnabled": false,
	"NetworkPredictionOptions": 2,
	"PromptForDownloadLocation": true,
	"RemoteAccessHostFirewallTraversal": false,
	"SSLVersionMin": "tls1.1",
	"SafeBrowsingExtendedReportingEnabled": false,
	"SafeBrowsingProtectionLevel": 1,
	"SyncDisabled": true,
	"URLBlocklist": [
		"javascript://*"
	],
	"UrlKeyedAnonymizedDataCollectionEnabled": false,
	"WebRtcEventLogCollectionAllowed": false,
	"ExtensionSettings": {
		"*": {
			"blocked_install_message": "User does not have permission to install extensions",
			"install_sources": [
				"chrome://extensions/",
				"https://chrome.google.com/"
			],
			"installation_mode": "blocked",
			"allowed_types": [
				"extension"
			]
		},
		"cjpalhdlnbpafiamejdnhcphjbkeiagm": {
			"toolbar_pin": "force_pinned",
			"installation_mode": "force_installed",
			"update_url": "https://clients2.google.com/service/update2/crx"
		}
	}
}
```
