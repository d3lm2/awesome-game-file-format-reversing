---
title: Version History
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) [POD6](pod6.md) | [**POD3**](pod3.md) [POD4](pod4.md) [POD5](pod5.md) | [**HOME**](index.md)

- [String Helpers](utils.md)
- [Checksum Algorithm](checksum.md)
- [Compression Algorithm](compress.md)
- [Dependency Records](depends.md)
- [Audit Trail](audits.md)
- Version History
- [Related Software](links.md)

### POD1 Style
- [POD1](pod1.md) header: entry count and comment; entry: name, size and offset
- [EPD](epd.md) magic ident, checksum and timestamp 
- [POD2](pod2.md) path name string table and audit logs
- [POD6](pod6.md) compression instead of checksum, no more audit logs

### POD3 Style
- [POD3](pod3.md) [EPD](epd.pod)/[POD2](pod2.pod) changes + dependencies
- [POD4](pod4.md) compression
- [POD5](pod5.md) linked archive list
