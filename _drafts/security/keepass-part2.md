---
layout: post
current: post
cover: 'assets/images/lock_and_chain.jpg'
navigation: True
title: 'Password Managers - Part 2 - KeePass Master Class'
date: 2018-05-31 14:00:00
tags: security
class: post-template
subclass: 'post'
author: nvhaver
---
In the previous post in this series, we've learned how to use a single KeePass database. This time, we set our focus on a more elaborate scenario in which multiple databases and different platforms are combined.

### Distributing your Database

In the previous post we have set up our first password database. However we only have it in one place... In order to get your database on all your devices, you can use a cloud storage service like [Dropbox](https://www.dropbox.com). Why would I, the tin foil guy in the room, recommend these kinds of services? Why not simply copy your password database to all your devices manually? Or host our own storage solution? Well, there are some requirements to suffice when it comes to our passwords. One is that your password storage should be highly available. Using the services of these large companies ensures that this is the case. Moreover, there is no danger of leaking your passwords because the entire database is encrypted.

A second reason is synchronisation. KeePass does include a very handy merge feature, however, this feature is used to merge local password databases, meaning that you would still have to merge all databases on all your devices manually. To prevent this, it is easier to let the online storage handle this synchronisation. In case there would be changes on multiple devices, or instance when you create accounts on different devices, the merge functionality can be used to merge conflicting copies in the online storage. Being able to go back in time and retrieve an older version of your password database may also be a blessing. This feature is also offered by most cloud storage services. In the case of Dropbox, this can be done through the website.

The service I recommend for the use case of password management is Dropbox. I tried working with Google Drive, but I quickly encountered some issues. A first issue is that Google is not clear on what happens in case of conflicting versions of the same file. We don't want the service to prefer one version over another as this might mean that newly created passwords may be overwitten or discarded by another conflicting copy of the database. A second and even more pressing issue is how Google Drive works on the different platforms. On Linux for example, the Google Drive folder is not a physical folder which can be navigated to on the disk. Instead it is a virtual folder which is only available from the Nautilus file browser. The folder represents the connection to the linked Google account and as such requires an active internet connection to function. This is definitely a no-go.

Something important I want to point out here is that, when using a key file next to your password to unlock the database, you should copy this key file over to your devices in an out-of-band way. So if your KeePass database is being transfered using Dropbox, the key file should not be shared this way. Instead, you should copy the file over using a USB cable. If your Dropbox account would be compromised, an attacker does not have your key file, only your encrypted database. [Passwords can be bruteforced](), but to bruteforce a file, now that's something else entirely. 

### Mobile Clients

Having your KeePass database on all your devices is nice, but it is even better to be able to open it and actually use it. Therefore we need an app per platform.

#### Android

An open source app is available for Android called [KeePass2Android](). The app offers a special integration with Dropbox (the "KP2A" method), but then you are required to put your database in a special location (Dropbox/Apps/KeePass2Android).

#### iOS

For iOS, [KyPass3]() is the recommended app, even though it is not free.

#### Windows Phone

Well, if you still have one of these, I recommend getting some new device... That being said, even though [Microsoft has abandoned you](), Windows Phone 8 users can still use KeePass using the free app [CodeSafe Pro](). For older phones (Windows Phone 7) there is a paid app called [PassKeeper]().

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

### Caveats

On top of the caveats listed in the previous post, some ne ones are to be added to the list due to the nature of working with different KeePass databases.

#### Synchronisation Conflicts

When working with services like Dropbox we need to be careful not to create any conflicting copies of the password database. Storage providers will often not care about conflict resolution and only retain the latest version of your password database. As such, when the database is updated offline in two places, only one of these files will be retained. There is no guarantee on what happens to the other version of the database. In my experiments, using my desktop and phone to deliberately create a conflict, the database on the phone was overwritten without any way of recovering the entries which were created on the phone offline. Therefore, I recommend to avoid this scenario if possible and reopening the database whenever you updated it. Having the discipline to ensure everything is synced after modifying the database is a must.

#### Updates

Next to KeePass itself, the plugins also need to be kept up to date. Again I strongly urge you to update your plugins to prevent any known issues to criple your security.

## Shout-outs

In particular, I'd like to thank Dries Eestermans and [IS4U]() for showing me new ways to use KeePass and for introducing me to the available plugins.

## References

- [KeePass homepage](https://keepass.info/)
- [LastPass hack article on LifeHacker](https://lifehacker.com/lastpass-hacked-time-to-change-your-master-password-1711463571)
- [KeePass plugins page](https://keepass.info/plugins.html)
- [cf zero's post on KeePass mobile sync](https://cfzero.wordpress.com/2014/06/07/sync-keepass-mobile-dropbox-security/)
