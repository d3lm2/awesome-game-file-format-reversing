---
title: Audit Trail
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) [POD6](pod6.md) | [**POD3**](pod3.md) [POD4](pod4.md) [POD5](pod5.md) | **HOME**

### Overview
- [String Helpers](utils.md)
- [Checksum Algorithm](checksum.md)
- [Compression Algorithm](compress.md)
- [Dependency Records](depends.md)
- Audit Trail
```cpp
namespace audit
{
enum action : u32
{
	add = 0,
	rem = 1,
	chg = 2,
};
struct entry
{
	c8  user[32];
	i32 timestamp;
	enum action action;
	c8  path[256];
	i32 old_timestamp;
	u32 old_size;
	i32 new_timestamp;
	u32 new_size;
};
};
```
