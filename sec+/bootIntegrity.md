# Boot Integrity

1. Malware wants to stay persistent on machines
1. Boot process is perfect infection point
1. Need to protect boot process

## Trusted platform module

1. TPM helps with cryptographic functions
 1. Random number generator
 1. Key generator
1. Persistent memory:
 1. Unique keys that is burned onto module during production
1. All of it is password protected

## UEFI BIOS

1. Software security for boot sequence
1. Secure boot is part of uefi spec
1. Bios protections:
 1. Bios includes manufacturer's public key
 1. Digital signature checked during bios update
 1. Bios prevents unauthorized writes to flash
1. Secure boot verifies bootloader:
 1. Signature is not modified
 1. Signed by certificate
 1. Manually approved by digital signature

## Trusted Boot

1. Bootloader verifies digital signature of os kernel
1. If kernel is modified then this process will stop and halt boot
1. Kernel verifies:
 1. Boot drivers
 1. Startup files
1. Right before loading drivers, start ELAM:
 1. Early launch anti malware
 1. Which checks drivers for malware

## Measured boot

1. Remte attestation:
 1. Report with all contenst encrypted by tpm keys sent to server
    which verifis everything. No changes have been made, soft/hard-ware
