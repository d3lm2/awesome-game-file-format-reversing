---
title: Checksum Algorithm
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) [POD6](pod6.md) | [**POD3**](pod3.md) [POD4](pod4.md) [POD5](pod5.md) | **HOME**

# **Overview**
- [String Helpers](utils.md)
- Checksum Algorithm
- [Compression Algorithm](compress.md)
- [Dependency Records](depends.md)
- [Audit Trail](audits.md)

### Variant
Checksum algorithm in use for [EPD](epd.md)/[POD2](pod2.md) and up is [CRC-32/MPEG-2](https://crccalc.com/?crc=123456789&method=CRC-32/MPEG-2&datatype=0&outtype=0) available in [crcle](https://sr.ht/~joneqdaniel/crcle)
```sh
poly    : 0x04C11DB7
ref_in  : false
ref_out : false
xor_in  : 0xFFFFFFFF
xor_out : 0
```

### Ranges
```c
/* checksum: crc32::mpeg2 */
POD2 : offset=sizeof(ident)+sizeof(checksum) count=sizeof(file  )-offset
POD3 : offset=sizeof(ident)+sizeof(checksum) count=sizeof(header)-offset
POD4 : offset=sizeof(ident)+sizeof(checksum) count=sizeof(header)-offset
POD5 : offset=sizeof(ident)+sizeof(checksum) count=sizeof(header)-offset
EPD  : offset=sizeof(header)                 count=sizeof(file  )-offset
/* checksum: none         */
POD6 : offset=sizeof(header)                 count=sizeof(file  )-offset
POD1 : offset=sizeof(header)=4+80            count=sizeof(file  )-offset
```

### Examples
```cpp

enum class section
{
     none   = 0,
     file   = 1,
     header = 2,
     extra  = 3,
     entry  = 4,
     depend = 5,
     audit  = 6,
};

const std::pair<u32<1>, enum section> range[last] =
{
     { 0u                                                                                 , section::none   },
     { sizeof(tr::archive<pod1>::header)                                                  , section::file   },
     { sizeof(tr::archive<pod2>::header::ident) + sizeof(tr::archive<2>::header::checksum), section::file   },
     { sizeof(tr::archive<pod3>::header::ident) + sizeof(tr::archive<3>::header::checksum), section::header },
     { sizeof(tr::archive<pod4>::header::ident) + sizeof(tr::archive<4>::header::checksum), section::header },
     { sizeof(tr::archive<pod5>::header::ident) + sizeof(tr::archive<5>::header::checksum), section::header },
     { sizeof(tr::archive<pod6>::header)                                                  , section::file   },
     { sizeof(tr::archive<epd1>::header)                                                  , section::file   },
     { sizeof(tr::archive<epd2>::header)                                                  , section::file   },
};

template<enum version version>
constexpr inline size_t section_size(size_t size)
{
     switch(range[version].second)
     {
           case section::entry:
           case section::file:
               return size;
           case section::header:
               return sizeof(tr::archive<version>::header);
           case section::none:
           default:
               return 0;
     }
}

template<enum version version>
constexpr inline size_t section_offset()
{
     switch(range[version].second)
     {
           case section::header:
           case section::file:
               return range[version].first;
           case section::entry:
           case section::none:
           default:
               return 0;
     }
}

template<enum version version>
constexpr inline u32<1> checksum(uint8_t* buf, size_t size)
{
     return crc32::mpeg2::compute(buf + section_offset<version>(), section_size<version>(size) - section_offset<version>());
}
```
