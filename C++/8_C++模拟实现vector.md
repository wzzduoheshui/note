# 模拟实现vector

---

## 数据类型

```cpp
namespace myvector
{
	template <typename T>
	class myvct
	{
	public:

		typedef T value_type;
		typedef value_type* iterator;
		typedef const value_type* const_iterator;
	private:
		iterator _start;
		iterator _finish;
		iterator _end_of_storage;
	};
}
```

---

## 构造

```cpp
//构造
myvct()
	:_start(nullptr)
	,_finish(nullptr)
	,_end_of_storage(nullptr)
{}
```

---

```cpp
myvct(size_t num, const value_type& _vec = value_type())
    :_start(nullptr)
    ,_finish(nullptr)
    ,_end_of_storage(nullptr)
{
    for (int i = 0; i <= num; i++)
    {
        push_back(_vec);
    }
}
```



---

## 析构

```cpp
~myvct()
{
	delete[] _start;
	_finish = _end_of_storage = nullptr;
}
```

---

## 拷贝构造

 ```cpp
 myvct(const myvct<value_type>& vec)
 {
     iterator _new_start = new value_type[vec.getcapacity()];//开辟空间
     memcpy(_new_start, vec._start, sizeof(value_type) * vec.getsize());//拷贝数据
     _start = _new_start;
     _finish = _start + vec.getsize();
     _end_of_storage = _start + vec.getcapacity();
 }
 ```

---

## size_t getcapacity()const

```cpp
size_t getcapacity()const
{
    return _end_of_storage - _start;
}
```

* **为了避免更改设为const函数**

---

## size_t getsize()const

```cpp
size_t getsize()const
{
    return _finish - _start;
}
```

* **为了避免更改设为const函数**

---

## 迭代器

```cpp
//迭代器
iterator begin()
{
	return _start;
}
iterator end()
{
	return _finish;
}
const_iterator const_begin()const
{
	return _start;
}
const_iterator const_end()const
{
	return _finish;
}
```

---

## void reserve(size_t n)

```cpp
void reserve(size_t n)
{
    //assert(n <= npos);//开辟空间不能超过最大空间
    if (n > this->getcapacity())//需要开辟空间
    {
        iterator _new = new value_type[n];//定义临时变量
        memcpy(_new, _start, sizeof(value_type)* getsize());//拷贝数据
        //拷贝数据的时候记得大小要乘数据类型的大小
        this->_finish = _new + this->getsize();//改变指针
        this->_end_of_storage = _new + n;
        this->_start = _new;
	}
}
```

* **memcpy拷贝是按照字节拷贝**
* **memcpy拷贝数据的时候记得大小要乘数据的类型**
* ![image-20231026182702623](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202310261827709.png)

---

## void push_back(const value_type& x)

```cpp
void push_back(const value_type& x)
{
	if (_finish + 1 >= _end_of_storage)//检查空间
	{
		reserve(getcapacity() == 0 ? 4 : getcapacity() * 2);//扩容
	}
		*_finish = x;//添加数据
		_finish++;
}
```

---

## value_type& operator\[](size_t n)

```cpp
value_type& operator[](size_t n)
{
    if (n <= getsize())
    {
        return _start[n];
    }
}
```

---

##  const value_type& operator\[](size_t n)const

```cpp
const value_type& operator[](size_t n)const
{
    if (n <= getsize())
    {
        return _start[n];
    }
}
```

---

##  void pop_back()

```cpp
void pop_back()
{
    if (_finish - 1 >= _start)
    {
        _finish--;
    }
}
```

---

##  void insert(const size_t n, const value_type x)

```cpp
void insert(const size_t n, const value_type x)
{
    //检查空间
    if (_finish + 1 >= _end_of_storage)
    {
        reserve(getcapacity() == 0 ? 4 : getcapacity() * 2);
    }

    //挪动数据
    iterator _end = _finish;
    while (_end >= _start + n)
    {
        *(_end) = *(_end - 1);
        _end--;
    }
    _finish++;
            
    //插入数据
    *(_start + n) = x;
}
```

---

##  void insert(iterator _pos, const value_type x)

```cpp
void insert(iterator _pos, const value_type x)
{
    if (_pos >= _start && _pos <= _finish)
    {
        //检查空间
        if (_finish + 1 >= _end_of_storage)
        {
            //因为涉及到扩容_start,_finish的位置都会发生改变但是pos不变
            //会影响到挪动数据，所以需要记录pos的相对位置（pos距离_start的距离）
            size_t len = _pos - _start;
            reserve(getcapacity() == 0 ? 4 : getcapacity() * 2);
            _pos = _start + len;
        }

        //挪动数据
        iterator _end = _finish;
        while (_end >= _pos)
        {
            *(_end) = *(_end - 1);
            _end--;
        }

        *(_pos) = x;
        _finish++;
    }
}
```



---

## iterator erase(iterator _pos)

```cpp
iterator erase(iterator _pos)
{
    if (_pos >= _start && _pos <= _finish)
    {
        iterator _begin = _pos;
        while (_begin <= _finish)
        {
            *(_begin) = *(_begin + 1);
            _begin++;
        }
        _finish--;
        return _pos;
    }
}
```

* **STL规定返回一个迭代器，指向被删除位置的下一个位置迭代器**
* ![image-20231026191644692](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202310261916737.png)

---

##