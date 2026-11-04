---
title: termpod wiki
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) [POD6](pod6.md) | [**POD3**](pod3.md) [POD4](pod4.md) [POD5](pod5.md) | **HOME**

### String Helpers
```cpp
constexpr inline u32<1> ceil2mpow2(u32<1> x, u32<1> pow2)
{
    return (x + (pow2 - 1)) & -pow2;
}

u32 ceil(u32 len)
{
        static const std::array<u32,12> sizes = { 4, 8, 12, 16, 32, 48, 64, 80, 96, 128, 256, 264 };
        std::array<u32,12>::const_iterator dst = sizes.begin();
        while(size > *dst) dst++;
        return *dst;
}

char* gets(char* buf)
{
	uint32_t size = strnlen(buf, 264);
	if(size == 0)
		return nullptr;

	char* dst = (char*)calloc(ceil(size), 1);

	for(uint32_t i = 0; i < size; i++)
	{
		dst[i] = buf[i];
		if(dst[i] == EOF)
			dst[i] = '\0';
		if(dst[i] == '\0')
			break;
		if(dst[i] == '\\')
			dst[i] = '/';
	}
	if(strlen(dst) == 0)
	{
		free(dst);
		dst = nullptr;
	}
	return dst;
}

c8* fgets(u32 size, FILE* stream)
{
   if(size == 0)
       return nullptr;
   
   c8* dst = (c8*)calloc(ceil(size), 1);

   for(u32 i = 0; i < size; i++)
   {
       dst[i] = fgetc(stream);
       if(dst[i] == EOF)
          dst[i] = '\0';
       if(dst[i] == '\0')
          break;
   }
   if(strlen(dst) == 0)
   {
       free(dst);
       dst = nullptr;
   }
   return dst;
}

c8* ctime(const t32* time) 
{ 
        time_t t = (time_t)*time; 
        c8* dst = std::ctime(&t); 
        dst[strcspn(dst, "\n")] = '\0';
        return dst; 
}

int32_t ftime(const char* filename)
{
	struct stat sb;
	return stat(filename, &sb) != -1 ? (int32_t)sb.st_mtime : -1;
}

```
### Checksum Algorithm

Checksum algorithm in use for [[EPD|EPD-Format-Reference]]/[[POD2|Pod 2 Format Reference]] and up is [CRC-32/MPEG-2](https://crccalc.com/?crc=123456789&method=CRC-32/MPEG-2&datatype=0&outtype=0) available in [crcle](https://github.com/jopadan/crcle)
```sh
poly    : 0x04C11DB7
ref_in  : false
ref_out : false
xor_in  : 0xFFFFFFFF
xor_out : 0
```
Checksum ranges
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
### Compression Algorithm 
Compression algorithm in use for [[POD4|Pod 4 Format Reference]] and up is ZIP-Deflate available in [libzippp](https://github.com/ctabin/libzippp) and [libdeflate](https://github.com/ebiggers/libdeflate)
### Dependency Records
```cpp
namespace depend
{
struct entry
{
        u8<264> unknown;
};
};

```
### Audit Trail
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

