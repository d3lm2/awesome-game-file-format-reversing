---
title: POD6
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) POD6 | [**POD3**](pod3.md) [POD4](pod4.md) [POD5](pod5.md) | [**HOME**](index.md)

### Games
- Ghostbusters: The Video Game Remastered

```cpp
struct header
{
        c8<4> ident;
        u32<1> entry_count;
        u32<1> version;
        u32<1> entry_offset;
        u32<1> names_count;
};
struct entry
{
        u32<1> names_offset;
        u32<1> size;
        u32<1> offset;
        u32<1> uncompressed;
        u32<1> compression_level; /* bitmask flag field defaulting to 8 */
        u32<1> zero;
};
struct file
{
     struct header header;
     void*         contents;
     struct entry  entries[header.entry_count];
     c8<1>         names[header.names_count]; /* end of entries + entries[i].names_offset */
};
```
