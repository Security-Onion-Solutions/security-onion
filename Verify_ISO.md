Download the signing key:
```wget https://raw.githubusercontent.com/Security-Onion-Solutions/security-onion/master/KEYS```

Import the signing key:
```gpg --import KEYS```

Download the signature file for the ISO:
```wget https://github.com/Security-Onion-Solutions/security-onion/raw/master/securityonion-14.04.3.1.iso.sig```

Download the ISO image:
```wget https://github.com/Security-Onion-Solutions/security-onion/releases/download/v14.04.3.1/securityonion-14.04.3.1.iso```

Verify the downloaded ISO image using the signature file:
```gpg --verify securityonion-14.04.3.1.iso.sig securityonion-14.04.3.1.iso```
