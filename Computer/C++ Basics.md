# C++基础

### 位域

```c++
struct Test
{
    unsigned int bValid : 1; //结构体大小为4个字节（不是8个字节），bValid只占第一个字节最右边的一位，值只有true/false
    unsigned int bBegin : 1; //bBegin为最右边第二位
    unsigned int Size : 6; //Size占六位，为第三到第八位。
};
```

### Union(共用体)

```c++
union Data
{
    int i;
    float f;
    char str[20]; //与结构体类似，但所有变量共用一块内存，大小为变量中最大的Size，此为20字节。任何时候都只能有一个变量有值可以访问。
};
```

