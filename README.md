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

#### WPM_HTTP_RetInit
```c

```

Compiles 7 regex pattern one after the other. If any fails then it returns 1.
   
REGEX 1: HTTP Request Line
- Format: "METHOD URI HTTP_VERSION"
- Example: "GET /default.html HTTP/1.1"

REGEX 2: Web Proxy URL Path
- Format: "/webproxy/NUM/NUM/NUM/PROTO/HOST,PARAMS"
- Example: "/webproxy/123/456/789/https/example.com,timeout=30"

REGEX 3: Host:Port
- Format: "HOST:PORT"
- Example: "example.com:8080"

REGEX 4: HTTP Header
- Format: "HEADER: VALUE"
- Example: "Content-Type: text/html"

REGEX 5: Cookie (SVNWebProxyCookie)
- Format: "SVNWebProxyCookie=value"
- Example: "SVNWebProxyCookie=123"

REGEX 6: HTTP Response
- Format: "HTTP_VERSION CODE MESSAGE"
- Example: "HTTP/1.1 200 OK"

REGEX 7: Key-Value
- Format: "KEY = VALUE"
- Example: "timeout=30"

---
Cookie: no length limit. This cookie is also used by the `WPM_HTTP_GetTokenId` function

#### WPM_HTTP_GetTokenId
```c
INT32 WPM_HTTP_GetTokenId(UCHAR *pucCookie,UCHAR *pucTokenId)
```

Check SVNWebProxyCookie cookie.
Assume something like SVNWebProxyCookie=<token>
Skip "SVNWebProxyCookie", copy token until delimiter (;, ' ' or '\0')

Copies characters from `cookie_ptr + 18` ("SVNWebProxyCookie=") into pucTokenId until it hits ; or \0 or a space - basically, the token

# vrp

`ELF 32-bit MSB shared object, MIPS, N32 MIPS64 rel2 version 1 (SYSV), dynamically linked, interpreter /lib32/ld.so.1, for GNU/Linux 2.6.34, with debug_info, not stripped`

> VRP is Huawei’s network operating system that runs on network devices such as routers and switches.

## CLI Console

The `WCLI_*` functions are handling the different features of the CLI Console.