## 堆的定义

```cpp
typedef int heapDateType;

typedef struct Heap
{
	heapDateType* _heap;
	size_t _size;
	size_t _capacity;
}Hp;
```

## 堆的删除

堆的删除不能使用挪动数据覆盖的方法，必须要将末尾数据和堆顶数据交换后再删除

否则一旦使用挪动数据覆盖的方法，堆中的父子关系全乱

## 创建堆的时候可以利用数组遍历

```cpp
heapDateType arr[] = {27,15,19,18,28,34,65,49,25,37};
for(int i = 0; i < 10; ++i)
{
    heapPush(&heap,arr[i]);
}
```

遍历数组的时候需要知道数组的元素多少可以采用sizeof求大小的方法

```cpp
heapDateType arr[] = {27,15,19,18,28,34,65,49,25,37};
for(int i = 0; i < sizeof(add) / sizeof(heapDateType); ++i)
{
    heapPush(&heap,arr[i]);
}
```

---

## 向下调整算法

前提：左右孩子均为堆

1、选出左右孩子中小的那个

2、小的孩子与父亲比较，如果比父亲大则和父亲交换，如果比父亲小则结束（大堆）

3、最多调整到叶子节点就结束（判断到叶子结点：当前结点的子结点超过数组大小即为叶子节点）

```c
void adjustDown(Hp* hp)
{
    int parent = 0;
    int leftChild = parent * 2 + 1;
    int rightChild = parent * 2 + 2;
    int maxChild;
    while(leftChild < hp->_size)
    {
        if(rightChild < hp->_size - 1)
        {
            maxChild = hp->_heap[leftChild] > hp->_heap[rightChild] ? leftChild : rightChild;
        }
        else
        {
            maxChild = leftChild;
        }
        if(hp->_heap[parent] < hp->_heap[maxChild])
        {
            heapDateSwap(hp, maxChild, parent);
            parent = maxChild;
            leftChild = parent * 2 + 1;
            rightChild = parent * 2 + 2;
        }
        else
        {
            break;
        }
    }
}
```

第二种实现方法

```c
void adjustDown(Hp* hp)
{
    int parent = 0;
    int child = parent * 2 + 1;
	while (child < hp->_size)
	{
		if (child + 1 < hp->_size && hp->_heap[child+1] > hp->_heap[child])
		{
			++child;
		}

		if (hp->_heap[child] > hp->_heap[parent])
		{
			heapDateSwap(hp, parent, child);
			parent = child;
			child = parent * 2 + 1;
		}
		else
		{
			break;
		}
	}	
}
```

---

## TOP-k问题

求数据集合中前k个最小或最大的元素，一般情况下数据量都比较大

> 1、排序O（NlogN）
>
> 2、N个数的大堆，top/pop个k次，时间复杂读是O（N+k*logN）
>
> 3、见下（可能空间效率更高）

排序最大的前K个数据

1、前K个数建立小堆

2、剩下的N-K个数如果比堆顶的数据大就替换堆顶的数据进堆

走完以后，堆里面的K个数就是最大的前K个

![TOP_k](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202311091652651.jpg)

* 时间复杂度O（K+（N-K）*logK）

> K个数建小堆
>
> N-K个数遍历并且伴随着K个数的堆向下调整
