---
title: POD4
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) [POD6](pod6.md) | [**POD3**](pod3.md) POD4 [POD5](pod5.md) | [**HOME**](index.md)

### Overview
- [String Helpers](utils.md)
- [Checksum Algorithm](checksum.md)
- [Compression Algorithm](compress.md)
- [Dependency Records](depends.md)
- [Audit Trail](audits.md)

### Games
- The King of Fighters Collection: The Orochi Saga
- Samurai Shodown Anthology

```cpp
using header = struct pod3::header;
using extra_header = struct pod3::extra_header;

struct entry
{
        u32<1> path_offset; /* (entry_offset + entry_count * sizeof(entry)) based */
        u32<1> size;
        u32<1> offset;
        u32<1> uncompressed_size;
        u32<1> compression_level;
        t32<1> timestamp;
        u32<1> checksum;
};
```
