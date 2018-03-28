### 14.04.5.11 ISO image built on 2018/03/28

**Please note!** This ISO image includes the **EXPERIMENTAL** Elastic stack!

The Elastic components are included in the ISO image and Setup gives you an option of Stable Setup (ELSA) or Experimental Setup (Elastic).  If you do not want to try the new Elastic stack, you can choose Stable Setup.  

If you **do** want to proceed with the new 14.04.5.11 ISO image, please skip down to the "Download and Verify" section.  If you would prefer an ISO image with no Elastic components at all, you have a few options:

- Use the older Security Onion 14.04.5.2 ISO image (and then run `sudo soup`):<br>
https://github.com/Security-Onion-Solutions/security-onion/blob/master/old/Verify_ISO_14.04.5.2.md

OR 

- Install your preferred flavor of Ubuntu 14.04 and then add our PPA and packages:<br>
https://github.com/Security-Onion-Solutions/security-onion/wiki/InstallingOnUbuntu

### Download and Verify

14.04.5.11 ISO image:  
https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.5.11_20180328/securityonion-14.04.5.11.iso

Signature for ISO image:  
https://github.com/Security-Onion-Solutions/security-onion/raw/master/sigs/securityonion-14.04.5.11.iso.sig  

Signing key:  
https://raw.githubusercontent.com/Security-Onion-Solutions/security-onion/master/KEYS  

For example, here are the steps you can use on most Linux distributions to download and verify our Security Onion ISO image.

Download the signing key:  
```
wget https://raw.githubusercontent.com/Security-Onion-Solutions/security-onion/master/KEYS
```

Import the signing key:  
```
gpg --import KEYS
```

Download the signature file for the ISO:  
```
wget https://github.com/Security-Onion-Solutions/security-onion/raw/master/sigs/securityonion-14.04.5.11.iso.sig
```

Download the ISO image:  
```
wget https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.5.11_20180328/securityonion-14.04.5.11.iso
```

Verify the downloaded ISO image using the signature file:  
```
gpg --verify securityonion-14.04.5.11.iso.sig securityonion-14.04.5.11.iso
```

The output should show "Good signature" and the Primary key fingerprint should match what's shown below:
```
gpg: Signature made Wed 28 Mar 2018 04:09:51 PM EDT using RSA key ID ED6CF680
gpg: Good signature from "Doug Burks <doug.burks@gmail.com>"
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: BD56 2813 E345 A068 5FBB  91D3 788F 62F8 ED6C F680
```

Once you've verified the ISO image, you're ready to proceed to our Installation guide:  
https://github.com/Security-Onion-Solutions/security-onion/wiki/Installation
