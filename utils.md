---
title: String Helpers
toc: no
---

[**POD1**](pod1.md) [EPD](epd.md) [POD2](pod2.md) [POD6](pod6.md) | [**POD3**](pod3.md) [POD4](pod4.md) [POD5](pod5.md) | [**HOME**](index.md)

- String Helpers
- [Checksum Algorithm](checksum.md)
- [Compression Algorithm](compress.md)
- [Dependency Records](depends.md)
- [Audit Trail](audits.md)
- [Version History](history.md)
- [Related Software](links.md)

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
