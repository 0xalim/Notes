# Certificate Formats

## Certificate FIle Formats

1. x.509 digital certs:
 1. Standardized
1. Can convert certs, openssl can do this

## Distinguished Encoding Rules (DER)

1. Encode data, but good for x.509
1. Binary format
1. Commonly used with java

## Privacy Enhanced Mail

1. Sending email services will modify binary files, so encode with b64
1. Encode DER cert with b64
1. Provided by CA
1. Supported on many different platforms

## PKCS12

1. Public key Cryptography Standard 12
1. Developed by RSA security, now RFC standard
1. Container format:
 1. Can put many certs in there, .p21 or .pfx file
 1. Common to transfer public, private key
 1. Container can be password protected
1. Extended from Microsoft .pfx cert

## CER

1. Windows x.509 file extension
1. Usually contains only public key
1. Private keys stored in .pfx format
1. .cer file format

## PKCS7

1. Public key Cryptography Standard 7
1. Cryptographic message syntax standard, .p7b file
1. Stored in ascii
1. Contaisn certs and chain certs but not private keys
