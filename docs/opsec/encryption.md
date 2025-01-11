---
title: Encryption
hide:
---

# Encryption

## Encryption Overview

This is one of the most important sections for you to learn. Most services being offered these days are encrypted, though there is some level of decisions and knowledge that home users need to be aware of. This has become a critical point in keeping yourself safe and for businesses and governments to keep sensitive data protected. Encryption wasn’t really discussed or implemented much for the average user until the last ten to fifteen years. Businesses and governments have been using it for quite a long time, and there's really good reasons to.

Encryption is also not a singular approach to something. You should strive to have your operating system encrypted, as well as emails, messages, documents that are stored locally or on the cloud, etc. Every now and then, there are morally disingenuous arguments made against encryption by some jackwagon saying something like, "why should you want your stuff encrypted if you have nothing to hide?"

That's a stupid comment that’s meant to gaslight people that aren’t tech savvy. Many corporations and governments would like to have the ability to spy on people more than they already do. If you have someone make a comment like that to you, keep in mind that they don’t have your best interests in mind or they are grossly misinformed on the topic.

!!! note "Why encryption should matter to you"

    I see the word "encryption" getting thrown around a lot like it's a buzzword, but it's really important to pay attention to. It's how you keep your data private. Anything not encrypted can be easily stolen. Using encryption should be just as standard as locking your door.

As for how to go about this, that isn’t a simple answer. The field of cryptography is a deep specialization of its own. Each operating system has its own method of encryption, sometimes open source and sometimes closed source, some are more vulnerable than others to exploits, some apps have data that’s encrypted, some don’t, and so on.

MacOS, Windows, and Linux all have their own methods for encryption. The native approach is the one I would recommend, i.e. use BitLocker with Windows and use LUKS with Linux. Sometimes, the encryption is done automatically on the device, such as with Apple (as long as you use a passcode). There are also other options available, like using VeraCrypt which can be used for full disk encryption or even creating hidden encrypted drives if you need plausible deniability.

As for things like messages, emails, and cloud storage, that gets more complicated. Some apps and services that claim to be encrypted are actually massive data collection machines and some have been caught doing nefarious things with customer data. Whatever you use requires some careful searching and due diligence to figure out what’s good and what’s not. I cover some of these in the software considerations section.

Another important area to consider is how many Internet of Things (IoT) devices are being used now. Just about everything seems to have an internet connection these days, whether it's a thermostat, a microwave, or a washing machine. Putting the security risk of those devices aside, you really need to consider how you're securing your home network, i.e. are you using WPA3 with a strong password?

## Encryption In Detail

There's quite a lot to explain here, so I'll start with the overview first. There's a few different types of encryption. There's symmetric encryption which is when the same key is used to encrypt and decrypt info. One of the common algorithms that uses this is Advanced Encryption Standard (AES). Typically when you see this, you see something like AES-128 or AES-256. I'll explain more on the numbers later. This algorithm is used quite a lot for encrypting data that's "at rest," meaning it's stored on something like a hard drive. BitLocker for example uses AES.

Then there's asymmetric encryption. This is where a public and a private key are used. The public key is openly shared, but the private key stays with the device (or owner). When there's communication between two parties, the sender uses the receivers public key to encrypt the message. The receivers private key is then used to decrypt it. An common example of this is during a TLS handshake.

The other method is hybrid encryption. This is where asymmetric is used for the key exchange due to the security benefits, and symmetric encryption is used for the data transmission, due to the speed benefits. You'll usually see this with cloud storage like Dropbox and with some VPN providers.

Here's some of the encryption protocols that are used:

- SSL (Secure Socket Layer)/TLS (Transport Layer Security): You'll usually hear SSL being used (like someone referring to an SSL certificate). The last version of SSL came out in 1996 and was replaced with TLS. TLS 1.3 was released in 2018 and is in the process of being rolled out now to replace TLS 1.2.
- SSH (Secure Shell): This is for remote login to a server and to manage file transfers. SSH operates on port 22 (more on that in the firewalls section).
- HTTPS (Hyper Text Transport Protocol Secure): This is used to encrypt web traffic, i.e. a person visiting a merchant site. HTTPS uses port 443.
- PGP (Pretty Good Privacy): This is used for encrypting emails and requires setup from a person to be able to use. This can be used to enhance security as email is considered highly insecure.

I could list more, though this should give you a good starting point. There's plenty more to discuss on encryption anyway. Now, let's talk about hashes and salting.

A hash is used to take something like a password and turn it into a string of characters. If you were to take the same input, i.e. a password, it would produce the same hash. This is a secure way to store something like passwords for a place such as a website. For example, if you ever do a forgot password request to a website and they send you an email with your actual password in the email, that's a problem and signals they don't have proper security practices. Many hackers have discovered files on company devices where the file was a database of unhashed passwords, which then damages the reputation of the company for having garbage security practices.

Hashing is done by using an algorithm, similar to the encryption examples I have above. One of the most commonly used ones now is SHA-256. This is also used for things like data integrity and should be used in place of older algorithms like MD5. An example of this being used is sometimes when you download a file, the dev will say to verify the hash so you can verify the file wasn't tampered with. If the hash is different, then the file shouldn't be trusted. Even the tiniest change to the file will cause an entirely different hash to generate.

To use a website example, when you login to a site, the password you put in is hashed and compared to the hash stored on the website database. This way, hackers and people that work at that company can't see your password.

Let's say you use a password like "applejacks!00." The resulting hash would be a string of random characters and numbers such as:

dffd6021bb2bd5b0af676290809ec3a53191dd81c7f70a4b28688a362182986f

Now let's add to this. Hashes are able to be salted. A salt is a random string that gets added with the password when the hash is made. With just a hash, two people could use the same password and it would generate the same hash. You can probably see why this is a security issue, especially in the context of large platforms, like Amazon. When a salt is added, those same passwords will end up with completely different hashes. If you took the above example hash, it would look completely different when salted:

38638fc079df32cd598550f94cf65b7f138c68cf2dff3c7d74a94fe851759ba2

With a salt, that hash will look entirely different every time, even though the password is the same.

One last note is regarding the key length. I mentioned earlier about AES-256. In the context of AES as an example, you'll see 128 and 256 bits. This is the key length. The longer it is, the stronger the encryption is. This is why you'll see places like intelligence agencies recommend using 256 bit. The other important aspect is that we are on the brink of quantum computing. 256 bit is going to be a lot more resistant to being broken by a quantum computer than something that's 128 bit.

### What you'll see

Now that you've got a grasp of how this works, let's talk about what you'll see in a more practical way. When you see encryption mentioned, you'll sometimes come across some meaningless garbage like "military grade encryption." There's different ways encryption can be used and sometimes companies will use it to paint a different picture than what is actually being done.

First, "military grade encryption" is just how standard encryption should be done. For example, the NSA uses AES-256, but so does something like BitLocker when it encrypts the files of a home user (with a caveat discussed more in the BitLocker section). When you see those words thrown around, it was probably someone on the marketing team who thought that sounded cool, but it really doesn't mean much.

Then there's the issue of if you are the only one with the keys, or if someone else has keys as well. In the case of cloud storage, any files uploaded will be encrypted (at least with reputable providers), but this doesn't mean that only you can see them. The provider may have them encrypted on their servers, but if they hold the keys, they can (and many do) snoop through your files. If you don't believe this, take a look at the terms of service and privacy policies for the providers.

The ideal way of handling this is with something called zero knowledge encryption. This is where only you hold the keys. While this is great, the increased security does have a downside. You must make sure that you never forget your password, or whatever it is that's encrypted will be gone forever. One example that I'll cover later is Cryptomator. They allow you to encrypt your files before they ever go to a cloud provider, so no one can snoop through your files. That also means that you are entirely responsible for the password to decrypt them.

There are more and more options coming available for people to be able to use zero knowledge. Another example that's becoming well known is Apple's Advanced Data Protection. It's a way for someone to back up their Apple data but not allow Apple to have the keys anymore. If the password is lost, then their data is gone.

!!! warning

    If you're going to use zero knowledge encryption, you will be entirely responsible for your password! Whatever data was encrypted will be gone forever if you forget your password. Whoever was providing the encryption won't have a way for you to recover your password. Plan accordingly before using any zero knowledge methods!

Now, just because something isn't zero knowledge doesn't mean that you shouldn't use it. That decision is something you need to make, but there's plenty of services, apps, etc, that aren't zero knowledge that are great and I don't have an issue with using. A service could have encryption where they hold the keys but be something that's okay to use.

## Applying encryption to your digital life

Let's talk about working encryption into your digital life. If you are new to this, I recommend you start small and work up from there. For example, maybe your want to move to an encrypted messaging service like Signal, turn on BitLocker or LUKS, encrypt your cloud files, and so on.

As far as tools are concerned, there's plenty to choose from depending on what it is that you want to do. For example, if you just wanted to encrypt a couple files, you could use something like Cryptomator, but if you want to encrypt your entire hard drive, you would need to use something like BitLocker.

!!! warning "Limitations of encryption"

    Encryption is not the end-all be-all of keeping data safe. For example, if you get a virus on your computer, your private data is likely going to get exfiltrated off the computer. Also, you really need to take the password seriously. The best encryption algorithm in the world won't protect against a weak password. Using something like "password" will be broken almost instantly with a dictionary attack. You should use at least 12 characters with a mix of upper and lower case letters, numbers, and symbols. Also, make sure you remember your passwords, otherwise you can lose all your data!

### Tools

You'll need to pick the right tool, depending on what it is that you're trying to do. This is by no means an exhaustive list, but gives you some options and ideas of what can be used.

Full Disk Encryption (FDE):

- BitLocker: This is used to encrypt drives on Windows operating systems. The setup and configuration is quite detailed. Refer to the BitLocker section for more info.
- LUKS: This is used for full disk encryption for Linux distros. It's a PITA to do this after the distro is installed, so it's best to do it beforehand.
- VeraCrypt: This is an open source tool that replaced TrueCrypt. It can be used with Windows, Linux, and Mac.

Encryption For Cloud Providers, Files & Folders:

- Cryptomator: Many people use this to encrypt files and folders before they get uploaded to a cloud storage provider (i.e. OneDrive), but this is also an excellent option if you just wanted to encrypt something and keep it on your computer. There are other options available, but Cryptomator works very well and is open source.

!!! note "Note on cloud encryption"

    All reputable cloud storage providers should be using encryption, though not all of them have zero knowledge encryption. You need to consider the sensitivity of the files. If the provider were to be hacked and the private keys they held were compromised, your personal data could be seen. If you use something like Cryptomator, you're adding an extra layer of encryption to hedge against a scenario like that. You also have to consider the usability as well. It provides more security, but it will add more hassle every time you need to access or update files.
