### 16.04.6.6 ISO image built on 2020/05/01

### Download and Verify

16.04.6.6 ISO image:  
https://download.securityonion.net/file/Security-Onion-16/securityonion-16.04.6.6.iso

Signature for ISO image:  
https://github.com/Security-Onion-Solutions/security-onion/raw/master/sigs/securityonion-16.04.6.6.iso.sig  

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
wget https://github.com/Security-Onion-Solutions/security-onion/raw/master/sigs/securityonion-16.04.6.6.iso.sig
```

Download the ISO image:  
```
wget https://download.securityonion.net/file/Security-Onion-16/securityonion-16.04.6.6.iso
```

Verify the downloaded ISO image using the signature file:  
```
gpg --verify securityonion-16.04.6.6.iso.sig securityonion-16.04.6.6.iso
```

The output should show "Good signature" and the Primary key fingerprint should match what's shown below:
```
gpg: Signature made Fri 01 May 2020 02:53:58 PM EDT using RSA key ID ED6CF680
gpg: Good signature from "Doug Burks <doug.burks@gmail.com>"
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: BD56 2813 E345 A068 5FBB  91D3 788F 62F8 ED6C F680
```

Once you've verified the ISO image, you're ready to proceed to our Installation guide:  
https://securityonion.net/docs/Installation
