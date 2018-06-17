---
layout: post
current: post
cover: 'assets/images/lock_and_chain.jpg'
navigation: True
title: 'Password Managers - KeePass Master Class'
date: 2018-05-31 14:00:00
tags: security
class: post-template
subclass: 'post'
author: nvhaver
---
Ever had the problem of having to remember too many passwords? Or did you every reuse a password? You know it is bad practice to do so, but what else can you do when every service you use needs a separate account with separate credentials?

That's where a password manager comes in. Basically a password manager is a secure management system to store your passwords. They come in many different form but the two most prevalent formats are online services, such as [LastPass](https://www.lastpass.com), and offline databases, like [KeePass](https://keepass.info). Both provide encrypted and secure storage of your passwords but differ in the trust model and ease of use.

## Types of Password Managers

Online password managers seem to entail that you need to trust the company hosting the online service. However, as been [proven in the past](https://lifehacker.com/lastpass-hacked-time-to-change-your-master-password-1711463571), as long as the provider properly implements the encryption used and does not implement any backdoors, your passwords are completely safe. Neither the provider, nor the government can decrypt your data as you should be the only one with access to the passwords. That being said, if you lose your master password, nobody will be able to help you out. Most often, a browser plugin is available to easily use your password database.

My preference, however, is the offline model. As there is no external server, there is no other party on which you depend. There is no other player who you need to be concerned about. There is only your encrypted storage and your (compound) master key. Again, this means that nobody will be there to save you when you would forget your password. An added complexity in this case is that you are also responsible for distributing your password database across all the devices where you need your passwords. In the case of KeePass, there are many projects that port KeePass to other operating systems, such as [KeePass2Android](https://play.google.com/store/apps/details?id=keepass2android.keepass2android).

## KeePass

So now that we covered what a password manager is, we'll dive deeper into working with KeePass, my go-to tool for all needs password-ish.

### Basic Usage

First things first, getting KeePass and setting up your database. So head over to the [KeePass download page](https://keepass.info/download.html) and grab yourself an installer. The installation process per platform is explained briefly there as well.

Now open KeePass. It  

TODO: What is the default when no database is present.

### Distributing your Database

In order to get your database on all your devices, you can use a cloud storage service like [Dropbox](https://www.dropbox.com). Why do I recommend these services? Why not simply copy your password database to all your devices manually? Well, there are some requirements to suffice when it comes to passwords. One is that your password storage should be highly available. Using the services of these large companies ensures that this is the case. Moreover, there is no danger of leaking your passwords because the entire database is encrypted.

A second reason is synchronisation. KeePass does include a very handy merge feature, however, this feature is used to merge local password databases, meaning that you would still have to merge all databases on all your devices manually. To prevent this, it is easier to let the online storage handle this synchronisation. In case there would be changes on multiple devices, or instance when you create accounts on different devices, the merge functionality can be used to merge conflicting copies in the online storage. Being able to go back in time and retrieve an older version of your password database may also be a blessing. This feature is also offered by most cloud storage services.

The service I recommend for the use case of password management is Dropbox. I tried working with Google Drive, but I quickly encountered some issues. A first issue is that Google is not clear on what happens in case of conflicting versions of the same file. We don't want the service to prefer one version over another as this might mean that newly created passwords may be overwitten or discarded by another conflicting copy of the database. A second and even more pressing issue is how Google Drive works on the different platforms. On Linux for example, the Google Drive folder is not a physical folder which can be navigated to on the disk. Instead it is a virtual folder which is only available from the Nautilus file browser. The folder represents the connection to the linked Google account and as such requires an active internet connection to function. This is definitely a no-go.

### Caveats

Note that you still need to remember a few passwords. These are:

- The password to boot your pc
- The password to log into your account
- The password to unlock your password database
- The password to access your online version of the password database

Note that the last one is crucial the first time you set up your keepass solution. Imagine that you are confident you have all your passwords with you whereever you go to then realize that you cannot access your online storage and thus your online passwords. One way to prevent this from happening, is to have another device which contains a copy of your password database (either online or offline). A smartphone would do the trick.

The password used to unlock your password database determines the security for all your stored passwords. It is the key to your kingdom. As such, you should make sure that that password is highly resistant to brute-force attacks and guessing attacks. You can use a compound key by using a file as part of the login procedure. In that case you require something you know (password) and something you have (key file) to unlock your database. However this seems to be the way to go, bear in mind it is up to you to get the key file everywhere you need to access your database.

Alternatively, a secure token, such as a [Yubikey](https://www.yubico.com/) could be used. Again, you need (to make sure this token is compatible with the devices on which you need to access your password database.

To go one step further in securing your password database, you can opt to change the encryption algorithm used from AES to something more exotic (e.g. [ChaCha20](https://tools.ietf.org/html/rfc7539)). Note however that not all ports of KeePass support these algorithms and the lastest format of KeePass databases. In addition to that you can also change the key derivation algorithm and fine-tune its settings. These steps are not required as KeePass already uses highly secure standards, but can be interesting to tweak.

The KeePass default security settings, however, do need some tweaking. In order not to leave your safe unlocked the whole day, we can play with them in the "Tools > Options" menu. In the "Security" tab, you should reconfigure the following: 

- Lock workspace after global user inactivity (seconds): 240
- Lock workspace when locking the computer or switching the user
- Lock workspace when the remote control mode changes
- Clear clipboard when closing KeePass
- Use native library for faster key transformations
- Clear master key command line parameters after using them once

This way your passwords are again a bit safer.

### Plug-ins

KeePass on its own already is very powerful, but combined with the following plug-ins, it really becomes a complete toolset for working with passwords and even keypairs. An overview of all availanle plug-ins can be found on the [KeePass plugn page](https://keepass.info/plugins.html).

#### Installing a plug-in

A plug-in for KeePass usually consists of a .plgx file. This file should be placed in the KeePass plug-in directory (for Windows, by default this is: `C:\Program Files(x86)\KeePass Password Safe 2\Plugins`). To verify the plug-in was loaded correctly, you can have a look at the plug-ins window within KeePass (Tools > Plugins). Note that plug-ins are loaded at the start of KeePass, as such newly installed plug-ins only show up after restarting KeePass.

#### KeeOTP
[KeeOTP](https://bitbucket.org/devinmartin/keeotp/downloads) provides KeePass with the ability to generate it's own TOTP tokens. By doing so it can replace the need for a Google Authenticator app or hardware token. The initial set up goes as follows: 

- Right-click your KeePass entry and select "Timed One Time Password".
- A dialog window pops up where you can enter the secret (this is the value that's behind the QR code). This secret can be revealed on the website of your service, most likely this is under "advanced settings". 
- Next, press "CTRL + T" on your entry and the TOTP is copied into your clipboard.

TODO: A placeholder is also exposed, which I utilized in previous use-cases (such as KeeFox, Auto-Type, ...).

#### KPEnhancedEntryView
[KPEnhancedEntryView](https://sourceforge.net/projects/kpenhentryview/): This plug-in improves the UI of KeePass by changing some functionality on the front-end, an example is that all fields (even those in the "Advanced" tab) are now accessible from the main window. You no longer need to open the entries to view notes and custom fields.

#### KeeAgent
[KeeAgent](http://lechnology.com/software/keeagent/) provides functionality similar to Pageant, which exposes private keys securely to Putty and other clients such as WinSCP. Using this plug-in your private keys are kept secure in the KeePass safe, and still have the ability to be used easily.

TODO: more info needed

#### KeeAutoExec
[KeeAutoExec](http://keepass.info/plugins.html#keeautoexec) enables you to automatically open additional KeePass safes after you loaded your initial safe, this can potentially save a lot of time if you have to open the same safes multiple times a day. 

#### QualityHighlighter
[QualityHighlighter](https://github.com/sdrichter/QualityHighlighter/releases), as the name indicates, highlights the quality of your passwords and points your attention to weak passwords. Once installed, the plug-in will indicate the password strength of every entry in the database using a colour gradient ranging from red to green, based on the password entropy.

#### AutoTypeSearch
[AutoTypeSearch](https://sourceforge.net/projects/autotypesearch/) provides a quick searching capability as an enhancement to the global auto-type system. If a global auto-type is requested, but no matching entry for the active window is found, this plugin will show a quick as-you-type search window which lets you to easily pick the entry to auto-type.

This plug-in works hand in hand with the global auto-type feature. This feature will look at its context, i.e. the currently focused application, and will look for an entry to fill in. The AutoTypeSearch plug-in, however, kicks in when this lookup fails. From the [documentation included with the plug-in file]():

AutoTypeSearch is initially configured to automatically appear after an unsuccessful global auto-type. However, this can be changed in the KeePass Options window (an AutoTypeShow tab has been added). Here, a system-wide hot key can be configured to show the AutoTypeSearch window immediately. It is also possible to show the window by running KeePass.exe passing "/e:AutoTypeSearch" as a command line parameter. Once the window is shown, usage is extremely simple. Just start typing, and AutoTypeSearch will search your database for matching entries. By default, the Title, Url, Notes, Tags, and Custom Fields will be searched, but this can be configured in the AutoTypeShow tab of the KeePass Options window. Protected fields (like Password) will not be searched. The arrow keys can be used to move the selection in the list of results, then press Enter to auto-type the selected entry. Alternatively, press Shift+Enter to open the entry instead of auto-typing it. (These actions can also be customised in the Options window.) Clicking and Shift-Clicking an entry will also perform those actions.

#### QuickSearch
[QuickSearch](https://github.com/iamkarlson/keepass-quicksearch/releases) is a plug-in providing enhanced search capabilities. It can be used as a replacement for the built-in QuickFind toolbar control. This plug-in enables searching while you type in the KeePass database. It does not replace the default search bar but adds a second one. 

#### KeePassQRCodeView
[KeePassQRCodeView](https://github.com/JanisEst/KeePassQRCodeView/releases): Allows generating QR codes for fields of an entry so that information can be easily transfered to a mobile device. 

## Shout-outs

In particular, I'd like to thank Dries Eestermans for showing me new ways to use KeyPass and for introducing me to the available plugins.

## References

- [LastPass homepage](https://www.lastpass.com)
- [KeePass homepage](https://keepass.info/)
- [LastPass hack article on LifeHacker](https://lifehacker.com/lastpass-hacked-time-to-change-your-master-password-1711463571)
- [KeePass plugins page](https://keepass.info/plugins.html)
