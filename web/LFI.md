# [LFI](#lfi)

# LFI
Refs:
* <https://www.acunetix.com/blog/articles/local-file-inclusion-lfi/>
## Fuzz URI parameters
* <https://github.com/jai-the-seeker/CTF-OSCP/blob/master/web/web_recon.md#fuzzing-parameter-for-lfi>
## Check for LFI
In burpsuite, send the intercepted request to repeater mode. Then temper with the arguments to the parameter, to find the LFI vulnerability. In case the vulnerability exists, the contents of `/etc/passwd`, will show up in the response. The parameter `file` has been found out through fuzzing in the previous step.
```
GET /manage.php?file=../../../../../../../../../../../etc/passwd HTTP/1.1
```

