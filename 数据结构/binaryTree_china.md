# 链式二叉树

![image-20231109170950788](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202311091709827.png)

---

普通二叉树增删查改没有意义

目的是为了为搜索二叉树、红黑树、B树（多叉平衡搜索树）打基础

---

* **二叉树的遍历：**

1. 前序遍历(Preorder Traversal 亦称先序遍历)——访问根结点的操作发生在遍历其左右子树之前。
2. 中序遍历(Inorder Traversal)——访问根结点的操作发生在遍历其左右子树之中（间）。
3. 后序遍历(Postorder Traversal)——访问根结点的操作发生在遍历其左右子树之后。  

由于被访问的结点必是某子树的根，**所以**N(Node**）、**L(Left subtree**）和**R(Right subtree**）又可解释为根、根的左子树和根的右子树**。NLR、LNR和LRN分别又称为先根遍历、中根遍历和后根遍历。  

---

## 定义

```c
typedef int brinaryTreeType;
typedef struct myBrinaryTreeNode
{
    brinaryTreeType _data;
    struct myBrinaryTreeNode* _left;
    struct myBrinaryTreeNode* _right;
}mBTNode;
```

---

## 二叉树的遍历采取了分治的算法

* 分而治之

> 把大问题分成了类似规模的子问题

* 类似于现实问题

  > 假设校长统计全校在校人数：
  >
  > 安排：校长->院长->班长->舍长
  >
  > 上报：校长<-院长<-班长<-舍长

## 先序遍历

```c
void PreOrder(mBTNode* root)
{
    if(root == NULL)
    {
        printf("# ");
        return ;
    }

    printf("%d ",root->_data);
    PreOrder(root->_left);
    PreOrder(root->_right);
}
```

![二叉树先序遍历](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202311091843276.jpg)

![先序遍历](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/202311091909990.png)

---

## 中序遍历

```c
void InOrder(mBTNode* root)
{
    if(root == NULL)
    {
        printf("# ");
        return ;
    }

    PreOrder(root->_left);
    printf("%d ",root->_data);
    PreOrder(root->_right);
}
```

---

## 后序遍历

```c
void PostOrder(mBTNode* root)
{
    if(root == NULL)
    {
        printf("# ");
        return ;
    }

    PreOrder(root->_left);
    PreOrder(root->_right);
    printf("%d ",root->_data);
}
```

---

## 求二叉树中链表的结点个数

```c
int numBTNode(mBTNode* root)
{
    return root == NULL ? 0 : numBTNode(root->_left) + numBTNode(root->_right) + 1;
}
```

依旧是采取分治策略

---

## 求二叉树深度为K的结点的个数

```c
int numBTklevel(mBTNode* root,int k)
{
    if(root == NULL)
    {
        return 0;
    }
    return k == 1 ? 1 : numBTklevel(root->_left,k - 1) + numBTklevel(root->_right,k - 1);
}
```

---

## 查找二叉树值为x的结点

```c
mBTNode* findBTNode(mBTNode* root,int x)
{
    if(root == NULL)
    {
        return NULL;
    }
    if(root->_data == x)
    {
        return root;
    }

    mBTNode* left = findBTNode(root->_left, x);
    if(left)
        return left;

    mBTNode* right = findBTNode(root->_right, x);
    if(right)
        return right;
}
```

---

## 求二叉树的高度

```c
int numBTDepth(mBTNode* root)
{
    if(root == NULL)
    {
        return 0;
    }
    

    int leftDepth = numBTDepth(root->_left) + 1;
    int rightDepth = numBTDepth(root->_right) + 1;

    return leftDepth > rightDepth ? leftDepth : rightDepth; 
}
```

---

## 销毁二叉树

```c
void delBT(mBTNode* root)
{
    if(root == NULL)
    {
        return;
    }

    delBT(root->_left);
    root->_left = NULL;
    delBT(root->_right);
    root->_right = NULL;

    if(!root->_left && !root->_right)
    {
        free(root);
    }
}
```

---

## 二叉树的层序遍历

* 广度优先遍历

```c
mBTNode* arr[20] = {0};
void levelOrder(mBTNode* root)
{
    int head = 0;
    int tail = 0;
    if(root != NULL)
    {
        arr[tail] = root;
        tail++;
    }

    while(1)
    {
        if(arr[head]->_left)
        {
            arr[tail] = arr[head]->_left;
            ++tail;
        }
        if(arr[head]->_right)
        {
            arr[tail] = arr[head]->_right;
            tail++;
        }

        if(++head == tail)
        {
            break;
        }
    }

    for(int i = 0; i < tail; ++i)
    {
        printf("%d ",arr[i]->_data);
    }   
}
```

这里更适合利用栈，但是由于C语言并没有栈的库函数，我也没有自己实现，只好先用数组来代替

![二叉树层序遍历](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.jpg)

这里可以理解为tail在前面跑（负责插入新数据），head在后面追（负责遍历各个结点的子结点）

---

## 判断一棵树是不是完全二叉树

```c
bool completeBTree(mBTNode* root)
{   
    int parent = 0;
    int child = parent * 2 + 1;

    while(1)
    {
        if(arr[child] == NULL)
        {
            true;
        }
        if(arr[child + 1] == NULL)
        {
            return arr[parent]->_left == arr[child] ? true : false;
        }
        if(arr[parent]->_left != arr[child] || arr[parent]->_right != arr[child + 1])
        {
            return false;
        }
        
        ++parent;
        child = parent * 2 + 1;
    }
}
```

这里借用了上一步二叉树的层序遍历所得到得数组arr，理论上来说还是应该用堆或者是开辟动态数组来完成，但是大体的实现逻辑是这样的

![判断是否为完全二叉树](https://dhrs-oss.oss-cn-beijing.aliyuncs.com/img/%E5%88%A4%E6%96%AD%E6%98%AF%E5%90%A6%E6%98%AF%E5%AE%8C%E5%85%A8%E4%BA%8C%E5%8F%89%E6%A0%91.jpg)

这里我们也可以在层序遍历的时候直接进行判断

---

