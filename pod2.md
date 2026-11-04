---
title: POD2
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) POD2 [POD6](pod6.md) | [**POD3**](pod3.md) [POD4](pod4.md) [POD5](pod5.md) [HOME](index.md)

### Games
- Nocturne
- Blair Witch
- 4x4 Evo
- 4x4 Evo 2 

```cpp
struct header
{
    c8<4>  ident; /* "POD2" */
    u32<1> checksum; /* covering data starting after header->checksum */
    c8<80> comment;
    u32<1> entry_count;
    u32<1> audit_count;
};
struct entry
{
    u32<1> path_offset; /* relative to end of file->entries */
    u32<1> size;
    u32<1> offset; /* relative to start of file */
    t32<1> timestamp;
    u32<1> checksum;
};
struct file
{
    struct header    header;
    struct entry     entries[header.entry_count];
    str              path_data;
    buf              entry_data;
    tr::archive<pod2>::audit audits[header.audit_count];
};
```
