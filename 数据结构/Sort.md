# Sort（排序）

---

## 插入排序

> 打扑克牌时，我们会对我们手中的牌按大小插入进行排序，插入排序的思想也是如此
>
> ![微信截图_20231111183558](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/insertsort1.png)

但是我们具体的实现时是从前往后比还是从后往前比这个是没有规定的

![微信截图_20231111183431](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/insertSort.png)

---

我们也可以理解为第一个数字有序插入第二个数字--》前两个数字有序插入第三个数据--》……--》前n-1个数字有序插入第n个数字

```c
void insertSort(int* a, int n)
{
    for(int i = 1; i < n; ++i)
    {
        int j = i;
        while(a[j] < a[j - 1])
        {
            int tmp = a[j];
            a[j] = a[j - 1];
            a[j - 1] = tmp;
            if(--j == 0)
            {
                break;
            }
        }
    }
}
```

这里是将下标为j的元素依次与前一个元素比较，如果满足条件则交换，但是交换的效率又不是很高所以第二种写法采用临时变量tmp

```c
void insertSort(int* a,int n)
{
    for(int i = 1; i < n; ++i)
    {
        int j = i;
        int tmp = a[j];
        while(j > 0)
        {
            if(a[j - 1] > tmp)
            {
                a[j] = a[j - 1];
            }
            else
            {
                break;
            }
            --j;
        }
        a[j] = tmp;
    }
}
```

---

| 时间复杂度 |                                  |
| ---------- | -------------------------------- |
| O（N^N）   | 逆序的情况下，是0+1+……+N等差数列 |
| O（N）     | 顺序的情况下，是遍历             |

---

## 希尔排序

1、分组预排序（接近有序）

2、直接插入排序

---

![shellsort1](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/shellsort1.png)

对数组元素进行分组，对每个组里的数据分别进行插入排序

这样相当于里面的数据不是一步一步挪动了，而是一大步一大步的挪动

```c
void shellSort(int* a,int n)
{
    int gap = 3;// 每间隔gap为一组元素
    for(int i = 0; i < gap; i++)// 相当于一共有gap组
    {
        int j = i + gap;// 默认组内第一个元素为有序
        int end = j;// 记录下排到当前组的哪一个元素了
        while(j < n)// 对当前元素进行插入排序
        {
            int tmp = a[j];
            while(j - gap >= 0)
            {
                if(a[j - gap] > tmp)
                {
                    a[j] = a[j - gap];
                }
                else
                {
                    break;
                }
                j -= gap;
            }
            a[j] = tmp;

            j = end + gap;// 排当前组的下一个元素
            end = j;// 刷新当前排序情况
        }
    }

    insertSort(a,n);// 分组排序完成后进行插入排序
}
```

除此之外我们也可以让gap组数据进行交替排序

gap越大，对应的数可以更快的走到对应的位置（即升序的话，大的数更快的走到后面），但是越不容易接近有序

所以我们可以进行多组gap的排序直到gap为1时就相当于是直接插入排序

```c
void shellSort(int* a,int n)
{
    int gap = n;
    while(gap > 1)
    {
        gap = gap / 2;
        for(int i = gap; i < n; ++i)
        {
            int end = i;
            int tmp = a[end];
            while(end - gap >= 0)
            {
                if(a[end - gap] >= tmp)
                {
                    a[end] = a[end - gap];
                    end -= gap;
                }
                else
                {
                    break;
                }
                
            }
            a[end] = tmp;
        }
    }
}
```

---

希尔排序的时间复杂度大概是在O（N^1.3）左右

> * gap的预排序阶段：
>
> 只看
>
> ```c
>     int gap = n;
>     while(gap > 1)
>     {
>         gap = gap / 2;
> ```
>
> 可以推算出
>
> > 1. $ gap/2/2/.../2 = 1 $ 
> > 2. $ 2 ^ x = gap $
> > 3. $ x = log _2 gap $
> > 4. $ x = log _2 N $
>
> * gap内的循环
>
> 看这部分
>
> ```c
> for(int i = gap; i < n; ++i)
> {
>     int end = i;
>     int tmp = a[end];
>     while(end - gap >= 0)
>     {
>         if(a[end - gap] >= tmp)
>         {
>             a[end] = a[end - gap];
>             end -= gap;
>         }
>         else
>         {
>             break;
>         }
> 
>     }
>     a[end] = tmp;
> ```
>
> 1. gap越大的时候，for循环大概是$ n /2 $
>
>    里面的while循环反而因为gap很大，只需要比较2次左右的样子
>
>    合起来的话大概就是N
>
> 2. gap很小的时候，for循环大概是1、2次
>
>    里面的while循环，由于继承了前面的排序结果，很容易就能跳出循环
>
>    大概总体上也是接近于N
>
> 3. 中间的一些情况，也是在于上述两个区间，近似于N
>
> * 总体看来，希尔排序的时间复杂度大概是$ O(Nlog _2 N) $

---

## 选择排序

选择排序是每一次从待排序的数据元素中选出最小（或最大）的一个元素，存放在序列的起始位置，直到全部待排序的
数据元素排完 。

```c

void selectSort(int* a,int n)
{
    for(int end = 0; end < n; ++end)
    {
        int min = end;
        for(int select = min; select < n; ++select)
        {
            if(a[min] > a[select])
            {
                min = select;
            }
        }
        int tmp = a[min];
        a[min] = a[end];
        a[end] = tmp;
    }
}
```

> 这个时间复杂度过大，很少在实际情况中去使用

---

以下是对选择排序进行的优化，在遍历选择最小的时候，同时选择出最大的来

```c
void Swap(int* p1, int* p2)
{
    int tmp = *p1;
    *p1 = *p2;
    *p2 = tmp;
}
void selectSort(int* a, int n)
{
    for(int head = 0, tail = n - 1; head < tail; ++head, --tail)
    {
        int min = head;
        int max = tail;
        for(int i = head; i <= tail; ++i)
        {
            if(a[i] < a[min])
            {
                min = i;
            }

            if(a[i] > a[max])
            {
                max = i;
            }
        }

        Swap(&a[head], &a[min]);
        if(head == max)
        {
            max = min;
        }
        Swap(&a[max] ,&a[tail]);
    }
}
```

这里需要考虑到以下三种情况

![111](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/selectsort1.svg)

![222](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/selectsort2.png)

![222](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/selectsort3.png)

![222](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/selectsort4.png)


---



