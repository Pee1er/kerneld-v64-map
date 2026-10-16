```text
Title: FinalWire AIDA64 kerneld-v64.sys arbitrary physical memory

Vendor: FinalWire Kft. (info@finalwire.com)
Product: AIDA64 Extreme
Component: kerneld-v64.sys
SHA-256: 27375351b4a723465f937866f0ffc86e8e612b093673ee25ccf0b7ea803f888f
Size: 34648 bytes
PE timestamp: 2015-07-25 13:05:29 UTC
Version resource: none
Signer: CN=FinalWire, thumbprint D511E00707858607EC0B8DDB8BD296AEE32E32BD
Device: \\.\AIDA64Driver

Description:
kerneld-v64.sys passes a caller-supplied physical address to MmMapIoSpace on IOCTLs 0x8011207c, 0x80112080, 0x80112088, 0x8011208c, 0x80112090, and 0x801120b4. After an administrator loads the driver, a local low-privileged user who can open \\.\AIDA64Driver can read and write physical memory through these codes.

Access:
Tested on 2026-10-15 on an isolated Windows 10 virtual machine. An administrator loaded the driver. Local account aida-low, which is not in Administrators, then opened \\.\AIDA64Driver and sent IOCTL 0x80112080 and IOCTL 0x8011207c. The device security descriptor grants Everyone (WD) access mask 0x1201bf:
O:BAG:SYD:(A;;0x1201bf;;;WD)(A;;FA;;;SY)(A;;FA;;;BA)(A;;0x1200a9;;;RC)S:AI(ML;;NW;;;LW)
Loading the driver and resolving a physical address still require an administrator. These IOCTLs do not add their own administrator check.

Read-back:
On 2026-10-15, local account aida-low, which is not a member of Administrators, wrote one byte at a physical address supplied by the caller and read it back: 0x5A, then 0xA5. An administrator process resolved that page and saw the same bytes (page[0]=0x22, page[1]=0x5A, then page[1]=0xA5). Sending these IOCTLs does not require an administrator. The driver does not check SeProfileSingleProcessPrivilege.

This note covers the file and six IOCTL codes above. IOCTL 0x80112084 on this file is CVE-2019-7244. CVE-2024-26507 identifies IOCTL 0x80112104 and SHA-256 7790ab2136def33afc6c86d64da81c2e1bc25d34e33282c987961cdaa87e4330.

The package version that shipped this exact file was not recorded. FinalWire has described kerneld-v64.sys as the driver used on 64-bit Windows Vista, and has said the file is deprecated and blocklisted on Windows 11:
https://forums.aida64.com/topic/11448-eset-detecting-kerneldv64/

Suggested fix: remove kerneld-v64.sys from current installers, and reject or delete these IOCTLs in any build that still loads this file.

Credit:
type finder, value: Xingkai Ding
type other, value: Jiaye Pan (academic advisor)
type other, value: Letian Sha (academic advisor)
```
