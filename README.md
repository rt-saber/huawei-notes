# huawei-notes
Notes from my reverse engineering of the Huawei USG6000 firewall firmware

---

Most binaries extracted from the firmware have their debug symbols.

# svn.out

```
ELF 64-bit MSB shared object, MIPS, MIPS64 rel2 version 1 (SYSV), dynamically linked, interpreter /lib64/ld.so.1, for GNU/Linux 2.6.34, with debug_info, not stripped
```

## Functions

- `VOS_StrStr` is a wrapper around `strstr()`, it does NULL checks.
