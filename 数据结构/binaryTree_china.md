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

