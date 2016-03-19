ISO image:  
https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.4.1/securityonion-14.04.4.1.iso  

Signature for current ISO image:  
https://github.com/Security-Onion-Solutions/security-onion/raw/master/securityonion-14.04.4.1.iso.sig  

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
wget https://github.com/Security-Onion-Solutions/security-onion/raw/master/securityonion-14.04.4.1.iso.sig
```

Download the ISO image:  
```
wget https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.4.1/securityonion-14.04.4.1.iso
```

Verify the downloaded ISO image using the signature file:  
```
gpg --verify securityonion-14.04.4.1.iso.sig securityonion-14.04.4.1.iso
```

The output should show "Good signature" and the Primary key fingerprint should match what's shown below:
```
gpg: Signature made Sat 19 Mar 2016 12:02:50 PM UTC using RSA key ID ED6CF680
gpg: Good signature from "Doug Burks <doug.burks@gmail.com>"
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: BD56 2813 E345 A068 5FBB  91D3 788F 62F8 ED6C F680
```

Once you've verified the ISO image, you're ready to proceed to our Installation guide:  
https://github.com/Security-Onion-Solutions/security-onion/wiki/Installation
