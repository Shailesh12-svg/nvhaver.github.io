---
layout: post
current: post
cover: 'assets/images/lock_and_chain.jpg'
navigation: True
title: 'Password Managers - Part 1 - KeePass Basics'
date: 2018-05-31 14:00:00
tags: security
class: post-template
subclass: 'post'
author: nvhaver
---
Ever had the problem of having to remember too many passwords? Or did you every reuse a password? You know it is bad practice to do so, but what else can you do when every service you use needs a separate account with separate credentials? You want your passwords to be memorable, but this makes them easy to guess. And we all know [where](https://arstechnica.com/information-technology/2013/10/how-the-bible-and-youtube-are-fueling-the-next-frontier-of-password-cracking/) that [leads...](https://arstechnica.com/information-technology/2013/10/izmy-p55w0rd-saph/)

That's where a password manager comes in. Basically a password manager is a secure management system to store your passwords. They come in many different form but the two most prevalent formats are online services, such as [LastPass](https://www.lastpass.com), and offline databases, like [KeePass](https://keepass.info). Both provide encrypted and secure storage of your passwords but differ in the trust model and ease of use.

In this series of blog posts, we'll dive deeper into my favourite password manager, KeePass. In this post we'll cover how you can get started with KeePass and its basic usage. But first we'll have a quick look at the underlying architecture.

## Types of Password Managers

Online password managers seem to entail that you need to trust the company hosting the online service. However, as been [proven in the past](https://lifehacker.com/lastpass-hacked-time-to-change-your-master-password-1711463571), as long as the provider properly implements the encryption used and does not implement any backdoors, your passwords are completely safe. Neither the provider, nor the government can decrypt your data as you should be the only one with access to the passwords. That being said, if you lose your master password, nobody will be able to help you out. Most often, a browser plugin is available to easily use your password database.

My preference, however, is the offline model. As there is no external server, there is no other party on which you depend. There is no other player who you need to be concerned about. There is only your encrypted storage and your (compound) master key. Again, this means that nobody will be there to save you when you would forget your password. An added complexity in this case is that you are also responsible for distributing your password database across all the devices where you need your passwords. In the case of KeePass, there are many projects that port KeePass to other operating systems, such as [KeePass2Android](https://play.google.com/store/apps/details?id=keepass2android.keepass2android).

## KeePass

So now that we covered what a password manager is, we'll dive deeper into working with KeePass, my go-to tool for all needs password-ish.

### Basic Usage

First things first, getting KeePass and setting up your database. So head over to the [KeePass download page](https://keepass.info/download.html) and grab yourself an installer. The installation process per platform is explained briefly there as well.

Now open KeePass and create a new password database.
########################## TODO

To go one step further in securing your password database, you can opt to change the encryption algorithm used from AES to something more exotic (e.g. [ChaCha20](https://tools.ietf.org/html/rfc7539)). Note however that not all ports of KeePass support these algorithms and the lastest format of KeePass databases. In addition to that you can also change the key derivation algorithm and fine-tune its settings. These steps are not required as KeePass already uses highly secure standards, but can be interesting to tweak.

The KeePass default security settings, however, do need some tweaking. In order not to leave your safe unlocked the whole day, we can play with them in the "Tools > Options" menu. In the "Security" tab, you should reconfigure the following: 

- Lock workspace after global user inactivity (seconds): 240
- Lock workspace when locking the computer or switching the user
- Lock workspace when the remote control mode changes
- Clear clipboard when closing KeePass
- Use native library for faster key transformations
- Clear master key command line parameters after using them once

This way your passwords are again a bit safer.

### Database Creation

First things first, getting KeePass and setting up your database. So head over to the [KeePass download page](https://keepass.info/download.html) and grab yourself an installer. The installation process per platform is explained briefly there as well.

Now open KeePass and create a new password database. KeePass will prompt you to create a composite master key. This is the combination of factors you will need to unlock the KeePass database. I say factors, because next to a password, you can opt to use a key file or key provider next to your password. A Windows account can also be used, but as we want our database to be accessible across multiple platforms, we don't have any use for it.

For your password, please make sure that you select a sufficiently complex and long password. In a later blog post I will elaborate on strong passwords and password entropy, but for now just try to have a password that is rather long (16+ characters), does not contain any dictionary words and contains some letters (uppercase and lowercase), digits and special characters.

The key file can be any file. Alternatively, you can use KeePass to generate a key file. Personally, I prefer to use an existing file because the generated files tend to be quite low in entropy. Then again, I need to make sure that the file cannot be easily guessed or retrieved. The profile picture I use on social media would therefore be a rather poor choice. Also, the larger the file, the higher the probability of file corruption. If your key file goes corrupt, you will be locked out of your password database, so we need to avoid that. The trick is to strike a balance between a file which is quite random but not too long. Even better would be to use a key provider, although this needs to be compatible with your devices. In a later post I will look into how a [Yubikey]() would fit in this picture. 

Next you can configure your database settings. These settings can be changed later on through the "File > Database Settings" menu. In the first tab you can add some information to identify your database such as a title, description and colour. Newly created entries can also be given a custom default name. 

To go one step further in securing your password database, on the second tab, you can change the encryption algorithm used from AES to something more exotic (e.g. [ChaCha20](https://tools.ietf.org/html/rfc7539)). Note however that not all ports of KeePass support these algorithms and the lastest format of KeePass databases. In addition to that you can also change the key derivation algorithm and fine-tune its settings. These steps are not required as KeePass already uses highly secure standards, but can be interesting to tweak.

In the third tab you can enable or disable compression (enabled by default). The Recycle Bin tab, as the name indicates, allows you to enable a recycle bin in KeePass. This can be very useful when you need to recover a deleted password entry. Overwritten passwords can be retrieved from the entry's history tab, but when the entry itself is deleted, the recycle bin can really be a lifesaver.

The last tab contains advanced options related to templates, automatic history maintenance (maximum history size per entry in MB or number of items) and master key renewal (when recommend changing the database key, when to force it and whether to force it the next time the database is opened). By default, the master key renewal is disabled, bt I highly recommend to enable it. This is the key to your kingdom, so it should not stay the same forever. You can change your master key from the "File > Change Master Key" menu.

### Creating Entries and Groups

####### TODO

# Click the drop-down of the "Add Entry" toolbar button in the main window to create a new entry based on a template in the group above?



### More Settings

The KeePass default security settings, however, do need some tweaking. In order not to leave your safe unlocked the whole day, we can play with them in the "Tools > Options" menu. In the "Security" tab, you should reconfigure the following options to make them enabed: 

- Lock workspace after global user inactivity (seconds): 240
- Lock workspace when locking the computer or switching the user
- Lock workspace when the remote control mode changes
- Clear clipboard when closing KeePass
- Use native library for faster key transformations
- Clear master key command line parameters after using them once

This way your passwords are again a bit safer. You can also set a custom policy to prevent actions by disabling them in the second tab of the options screen. For instance, you can disable exporting from your database or disable the use of plugins. This can be especially interesting when using KeePass in the context of an enterprise.

The "Interface" tab provides settings regarding the behaviour of the user interface. The next tab, "Integration", handles how KeePass integrates with your system. From this screen you can also access the URL overrides. This are commands performed for entries with a special type of URL (e.g. ssh). KeePass will look at the protocol used in the URL field (e.g. starts with http) and will apply the first matching URL override when opening the URL from KeePass (Ctrl+U). This can be very usefull as the override allows any command to be used.

In the last tab you can change other settings related to the start and exit of KeePass, auto-type, file connections and others.

### Import/Export/Synchronisation

KeePass provides its own mechanism for importing, exporting and synchronising passwords and entire KeePass databases. This can be usefull when you have several versions of the same database or when you have used a new temporary database, which is to be merged into your main database. These features can be found under the "File" menu.

### Database Maintenance

After using KeePass a while, deleted entries may pile up, empty groups are left behind and whatnot. KeePass provides some features to clean up this mess under the "Tools > Database Tools" menu. Even more advanced cleanup tools can be found in the Database Maintenance item of that menu. These tools are more advanced in the sense that they also consider cleanup for things like plugins.

### Auto-type



### Triggers


### Caveats

Although this post proposes a fantastic way of managing your passwords, there are still some thing to keep in mind to ensure you do not get in trouble. 

#### Passwords you still need to remember
Note that you still need to remember a few passwords. These are:

- The password to boot your pc
- The password to log into your account
- The password to unlock your password database
- The password to access your online version of the password database

Note that the last one is crucial the first time you set up your keepass solution. Imagine that you are confident you have all your passwords with you whereever you go to then realize that you cannot access your online storage and thus your online passwords. One way to prevent this from happening, is to have another device which contains a copy of your password database (either online or offline). A smartphone would do the trick, but how to do this will be covered in a later post.

The password used to unlock your password database determines the security for all your stored passwords. It is the key to your kingdom. As such, you should make sure that that password is highly resistant to brute-force attacks and guessing attacks. You can use a compound key by using a file as part of the login procedure. In that case you require something you know (password) and something you have (key file) to unlock your database. However this seems to be the way to go, bear in mind it is up to you to get the key file everywhere you need to access your database.

Alternatively, a secure token, such as a [Yubikey](https://www.yubico.com/) could be used. Again, you need (to make sure this token is compatible with the devices on which you need to access your password database. Secure tokens will also be covered in a future post.

#### Updates

I specifically want to mention updates because it is a basic security principle to keep your systems and software patched and up to date. While zero-day exploits cannot be prevented, you can ensure that your not falling prey to known bugs that have been patched already. Updating KeePass is easy ("Help > Check for updates"), so you have no reason not to do so!

## Next up

Great, we have set up our first password database! On top of that, we also know how to use and maintain it.

Up until now, we've limited ourselves to the usage of a single KeePass database. In the next posts in this series, we'll get to the more advanced usages of KeePass with multiple databases and plugins.

## References

- [LastPass homepage](https://www.lastpass.com)
- [KeePass homepage](https://keepass.info/)
- [LastPass hack article on LifeHacker](https://lifehacker.com/lastpass-hacked-time-to-change-your-master-password-1711463571)
- [KeePass plugins page](https://keepass.info/plugins.html)
- [cf zero's post on KeePass mobile sync](https://cfzero.wordpress.com/2014/06/07/sync-keepass-mobile-dropbox-security/)
