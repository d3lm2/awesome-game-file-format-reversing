---
title: POD5
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) [POD6](pod6.md) | [**POD3**](pod3.md) [POD4](pod4.md) POD5 | [**HOME**](index.md)

- [String Helpers](utils.md)
- [Checksum Algorithm](checksum.md)
- [Compression Algorithm](compress.md)
- [Dependency Records](depends.md)
- [Audit Trail](audits.md)
- [Version History](history.md)
- [Related Software](links.md)

### Games
- Ghostbusters: The Video Game
- Country Dance 2
- Silent Hill: Book of Memories
- The Walking Dead: Survival Instinct

```cpp
struct header : pod3::header
{
/* 0x0120 */ u32<1> next_archive;
};

struct extra_header : struct header
{
/* 0x0124 */ u32<1> pad124;
}

using entry = pod4::entry;
```
