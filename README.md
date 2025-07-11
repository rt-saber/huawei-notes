# huawei-notes
Notes from my reverse engineering of the Huawei USG6000 firewall firmware

## Functions

- `VOS_StrStr` is a wrapper around `strstr()`, it does NULL checks.