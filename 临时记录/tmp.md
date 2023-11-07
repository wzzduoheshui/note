# 函数名后跟const

![image-20231025165937759](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202310251659842.png)

---

# string中迭代器end指向的是字符串末尾的'\0'

![image-20231025172610209](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202310251726252.png)

---

# assert是不满足的情况下抛异常

![image-20231025180825177](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202310251808213.png)

---

# npos表示的是无穷大的数

npos转化为int后是-1

![image-20231025205743155](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202310252057179.png)

---

## .和->的区别

![image-20231101175208401](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202311011752465.png)

---

## realloc

realloc开辟空间是以字节为单位需要乘sizeof

realloc的返回值是一个void*指针，需要强制转化为我们需要的指针类型

---

## 开辟动态数组

开辟动态数组的时候要先开辟到临时变量内，避免因为开辟失败造成原数据丢失

```cpp
int newcapacity = php->capacity == 0 ? 4 : php->capacity * 2;
HPDataType* tmp = (HPDataType*)realloc(php>a,sizeof(HPDataType)*newcapacity);
if (tmp == NULL)
{
    printf("realloc fail\n");
    exit(-1);
}

php->a = tmp;
php->capacity = newcapacity;
```

不要写成：

```cpp
hp->_capacity = hp->_capacity == 0 ? 4 : hp->_capacity * 2;
hp->_heap = (heapDateType*)realloc(hp->_heap,hp->_capacity * sizeof(heapDateType));//开辟空间
```

---

