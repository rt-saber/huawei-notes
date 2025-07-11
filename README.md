# huawei-notes
Notes from my reverse engineering of the Huawei USG6000 firewall firmware. This repository was not meant to be public, only reason I haven't switched it to private is because hopefully it can one day help someone else doing reverse engineering on this device firmwares.

---

Most binaries extracted from the firmware have their debug symbols.

# svn.out

SVN in Huawei context can have several meanings, here I think it is related to the VPN client. 

```
ELF 64-bit MSB shared object, MIPS, MIPS64 rel2 version 1 (SYSV), dynamically linked, interpreter /lib64/ld.so.1, for GNU/Linux 2.6.34, with debug_info, not stripped
```

## Functions

- `VOS_StrStr` is a wrapper around `strstr()`, it does NULL checks.
