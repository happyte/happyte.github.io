---
layout: post
title:  "objective-c多种排序算法图形化演示"
date:   2016-12-02 10:30:00 +0800
categories: jekyll update
tags: [objective-c] 
excerpt: objective-c图形化演示多种常见排序算法 
---

前几天在码农网看到了一篇文章，关于讲objective-c的几种排序算法的图形化操作方式，自己也写了一份代码温习下排序算法。

## 代码链接

先添上我的代码仓库链接[https://github.com/happyte/sort](https://github.com/happyte/sort)

## 选择排序

- 1.排序效果演示如下

![这里写图片描述](http://img.blog.csdn.net/20161201200602007)

- 2.选择排序的原理

 * 2.1 假定第一个元素是"最小值"，往后遍历，发现比"最小值"要小的元素就两者进行交换,即最终要把找到的最小值放在第一个元素位置上。
 * 2.2 上面保证第一个元素肯定是最小值，接着缩小范围从第二个元素开始且默认为最小值，不断往后遍历交换最小值。
 * 2.3 选择排序保障每次都能找到一个最小元素放在指定位置


- 3.选择排序代码实现如下

```objc
    - (void)zs_selectSort:(ZSCompare)compare withCallback:(ZSExchange)exchange{
    if (self.count == 0) {
        return;
    }
    for (int i = 0; i < self.count; i++) {
        for (int j = i+1; j < self.count; j++) {
            if (compare(self[i],self[j])==NSOrderedDescending) {
                [self zs_exchangeWithIndexA:i indexB:j Change:exchange];
            }
        }
    }
}
```

## 冒泡排序

- 1.冒泡排序效果演示

![这里写图片描述](http://img.blog.csdn.net/20161201202354079)

- 2.冒泡排序原理

 *  2.1 从第一个元素开始，比较相邻两个元素，如果后面一个元素比前面一个元素小，则两者进行交换（即小的在前，大的在后）。大的元素不断往后移动，最终最大的元素沉到最后一个位置。
 *  2.2 去掉最后一个位置，从第一个元素开始继续执行上面的操作，不断循环直至完成。
 *  2.3 冒泡排序保障每一次操作要把最大值放在最后。

- 3.冒泡排序代码实现

```objc
- (void)zs_bubbleSort:(ZSCompare)compare withCallback:(ZSExchange)exchange{
    if (self.count == 0) {
        return;
    }
    for (NSInteger i = self.count-1; i > 0; i --) {
        for (NSInteger j = 0; j < i; j++) {
            if (compare(self[j],self[j+1])==NSOrderedDescending) {
                [self zs_exchangeWithIndexA:j indexB:j+1 Change:exchange];
            }
        }
    }
}
```


## 插入排序

- 1.插入排序效果演示

![这里写图片描述](http://img.blog.csdn.net/20161201203337593)

- 2.插入排序原理

 * 2.1 插入排序用一个数组实现的话，把数组分为无序区和有序区两个区间，开始有序区只有第一个元素，从第二个元素到最后一个元素都为乱序区。
 * 2.2 从乱序区取出第一个元素，与有序区的最后一个元素（即乱序区的前一个元素比较），下面有3种情况。
     * 2.2.1 如果比前一个元素要大，则有序区范围增加1，乱序区范围减去1。
     * 2.2.2 如果与前一个元素相等，则可以继续与前二个元素比较，大的话就插入在该元素后面，相等继续与前三个元素比较吧，不可能比前二个元素小，因为每次排序都是保证有序区是按顺序排列的。其实相等可以不继续向前比较，直接插入在有序区末位。此时有序区增加1，无序区减去1。
     *  2.2.3 如果比前一个元素要小，则先交换位置，继续比较取出的元素与前一个元素，如果还要小继续交换位置，相等则继续往前比较。把乱序区的元素插入到有序区的正确位置。
  * 2.3 经过上面的过程，已经把乱序区的第一个元素插入到有序区，且有序区长度增加1，乱序区长度减去1，取出缩小范围后乱序区的第一个元素继续上面操作，直至范围不能缩小。

- 3.插入排序代码实现

```objc
   - (void)zs_insertSort:(ZSCompare)compare withCallback:(ZSExchange)exchange{
    if (self.count == 0) {
        return;
    }
    for (NSInteger i = 0; i < self.count-1; i++) {
        for (NSInteger j = i+1; j > 0; j--) {
            if (compare(self[j],self[j-1])==NSOrderedAscending) {
                [self zs_exchangeWithIndexA:j indexB:j-1 Change:exchange];
            }
            else{
                break;
            }
        }
    }
}
```

## 快速排序

- 1.快速排序图形化演示

![这里写图片描述](http://img.blog.csdn.net/20161201210247422)

- 2.这里讲解下最原始的快速排序方法，先介绍下三个概念
 * 2.1 中间值(pivot)，第一次快速排序默认第一个值为中间值
 * 2.2 左游标(i)，排序开始时第一个元素的位置为左游标，左游标不断向右移动
 * 2.3 右游标(j)，排序开始时最后一个元素的位置为右游标，右游标不断向左移动
 * 2.4 当左右游标重合的时候。
- 3.下面解释下元素是如何进行交换的,例如给出一个数组[37,28,5,14,75,25,89,22,11]
 * 3.1 从右边游标(j)开始(即11元素所在位置)，向前找比中间值(37)小的元素，此时找到的数就是11,交换中间值(pivot)和右游标j对应的元素。交换完成后，数组变为[11,28,5,14,75,25,89,22,37]。此时左游标指向的元素变为11,11肯定比中间值(pivot)即37要小，所以左游标可以直接+1往后移动一位，左游标指向28，右游标指向37。
 * 3.2 从左边游标(i)开始(即现在的28元素所在位置)，向后找比中间值(37)大的元素，找到的数是75，交换中间值(pivot)和左游标i对应的元素。交换完成后，数组变为[11,28,5,14,37,25,89,22,75]。左游标i此时指向中间值37，此时右游标指向的元素变为75,75肯定比中间值(pivot)即37要大，所以右游标可以直接-1往前移动一位,右游标指向22。
 * 3.3 上面两步的结论就是从右游标j开始找比中间值(pivot)小的，交换完成后，左游标i向后移动一位(i++)。从左游标i开始找比中间值(pivot)大的，交换完成后，右游标j向前移动一位(j--)。
 * 3.4 经过上面的排序，数组排序成为[11,28,5,14,37,25,89,22,75],左游标为中间值37的位置，右游标为22指向的位置。再次开始从右游标开始找比中间值37小的元素，找到的就是22。交换22与37，数组成为[11,28,5,14,22,25,89,37,75],左游标+1来到25元素位置,右游标指向37元素所在位置。再次从左游标开始向后找大的，找到的数为89，交换左游标位置的89和中间值37。数组成为[11,28,5,14,22,25,37,89,75]，右游标减1来到37位置。左游标也在37位置，此时左右游标重合第一次快速排序结束。
 * 3.5 通过第一次快速排序使得，比中间值37小的数都在37元素的前面。比37大的数都在37元素的后面。第一次快速排序获得了中间值37元素的位置，第二次快速排序从第一个元素到37开始进行排序，即[11,28,5,14,22,25,37]进行排序，同样取第一个元素11为中间值，左游标为11所在位置，右游标为37所在位置。
 * 快速排序是个递归的过程，上述的[11,28,5,14,22,25,37]全部排序完成后，会执行第一次快速排序得到的后面几个元素，即[89,75]数组进行快速排序。

- 4.快速排序代码实现如下:

```objc
   - (void)zs_quickSort:(ZSCompare)compare withCallback:(ZSExchange)exchange{
    if (self.count == 0) {
        return;
    }
    [self zs_quickSortWithLow:0 High:self.count-1 Compare:compare Callback:exchange];
}

//递归函数,必须有返回
- (void)zs_quickSortWithLow:(NSInteger)low High:(NSInteger)high Compare:(ZSCompare)compare Callback:(ZSExchange)exchange{
    if (low >= high) {
        return;
    }
    NSInteger pivotIndex = [self zs_pivotIndexWithLeft:low Right:high Compare:compare Callback:exchange];
    [self zs_quickSortWithLow:low High:pivotIndex-1 Compare:compare Callback:exchange];
    [self zs_quickSortWithLow:pivotIndex+1 High:high Compare:compare Callback:exchange];
}

- (NSInteger)zs_pivotIndexWithLeft:(NSInteger)left Right:(NSInteger)right Compare:(ZSCompare)compare Callback:(ZSExchange)exchange{
    NSInteger i = left;
    NSInteger j = right;
    id pivot = self[left];
    while (i < j) {
        //从右边找小的,如果右边大于等于pivot，则右游标向左移动
        while (i < j && compare(pivot,self[j])!= NSOrderedDescending) {
            j--;
        }
        //右边的值小于pivot
        if (i < j) {
            [self zs_exchangeWithIndexA:i indexB:j Change:exchange];
            i++;
        }
        //从左边找大的，如果左边小于等于pivot,则左游标向右移动
        while (i < j && compare(self[i],pivot)!=NSOrderedDescending) {
            i++;
        }
        //左边的值大于pivot
        if (i < j) {
            [self zs_exchangeWithIndexA:i indexB:j Change:exchange];
            j--;
        }
    
    }
    return i;
}
```
 
## 堆排序

- 1.堆排序图形化演示

![这里写图片描述](http://img.blog.csdn.net/20161201220847233)

- 2.首先可以把数组画成一个二叉树的形式，还是用[12,28,5,14,75,25,89,22,11]数组进行讲解。把数组画成二叉树，如下所示

![这里写图片描述](http://img.blog.csdn.net/20161202160550177)

- 3.堆排序首先要初始化一个大顶堆或小顶堆，大顶堆即父结点比子左结点和子右结点大，反之则为小顶堆。假设数组下标从0开始，父结点下标为i，子左结点下标为2i+1,子右结点下标为2i+2。我在写程序的时候给数组下标为0的位置添加了一个空值，那么父结点下标为i,子左结点下标为2i,子右结点下标为2i+1。

- 4.下面解释下如何构建大顶堆
  * 4.1 从最后一个非叶子结点开始，即14开始，如果父结点(14)比子左右结点(22和11)中的最大值(22)小，那么父结点(14)与子左右结点的最大值(22)交换，交换结果如下：

  ![这里写图片描述](http://img.blog.csdn.net/20161202162346590)

  * 4.2 如果交换后的14比它的子左右结点还要小的话，要继续沉底(交换后一定要满足大顶堆的特点)，可是这里它已经没有子左右结点。因此从倒数第二个非叶子结点(5)开始，5与子左右结点最大值(89)交换，交换结果如下:

  ![这里写图片描述](http://img.blog.csdn.net/20161202162757727)

  * 4.3 接下来从非子结点28开始，与子左右结点最大值(75)交换

  ![这里写图片描述](http://img.blog.csdn.net/20161202162916280)

  * 4.4 接下来从非子结点12开始，与子左右结点最大值(89)交换

  ![这里写图片描述](http://img.blog.csdn.net/20161202163029731)

  * 4.5 交换后的12比子左右结点的最大值(25)要小，不满足大顶堆特点，需要继续沉底

  ![这里写图片描述](http://img.blog.csdn.net/20161202163252609)

 * 4.6 经过上面几步，大顶堆已经构造出来了，最大的那个元素(89)在最顶端，交换第一个元素(89)和最后一个元素(11)，交换效果如下,交换后把89从这个二叉树中移除:

 ![这里写图片描述](http://img.blog.csdn.net/20161202163908587)

 * 4.7 现在的二叉树显然不满足大顶堆特点，则第一个元素11需要继续沉底，与子左右结点的最大值(75)交换，结果如下:

 ![这里写图片描述](http://img.blog.csdn.net/20161202164230004)

 * 4.8 交换后的11仍然需要继续沉底，与28交换

 ![这里写图片描述](http://img.blog.csdn.net/20161202164534425)
 
 * 4.9 现在二叉树又满足大顶堆的特点了，交换第一个元素(75)与最后一个元素(14),交换后把最后一个元素从二叉树中删除。

 ![这里写图片描述](http://img.blog.csdn.net/20161202164707067)

 * 4.10 再继续沉底第一个元素，直至范围不能缩小，就能得到正确的排序。

- 5.堆排序代码实现如下:

```objc
   - (void)zs_heapSort:(ZSCompare)compare withCallback:(ZSExchange)exchange{
    if (self.count == 0) {
        return;
    }
    [self insertObject:[[NSNull alloc]init] atIndex:0];
    //初始化最大堆排序
    for (NSInteger index = (self.count-1)/2; index > 0; index--) {
        [self sinkIndex:index bottomIndex:self.count-1 compare:compare Callback:exchange];
    }
    //第一次交换根结点与最后一个元素,然后不断沉底第一个元素
    for (NSInteger index = self.count-1; index > 1; index--) {
        [self zs_exchangeWithIndexA:1 indexB:index Change:exchange];
        [self sinkIndex:1 bottomIndex:index-1 compare:compare Callback:exchange];
    }
    [self removeObjectAtIndex:0];
}

//第一个参数是需要沉底的元素索引值，第二个元素是能允许沉底的最大索引值
- (void)sinkIndex:(NSInteger)currentIndex bottomIndex:(NSInteger)bottomIndex compare:(ZSCompare)compare Callback:(ZSExchange)exchange{
    //数组第一个数为空，就是为了子左结点刚好是2倍，子右节点是2倍+1
    for (NSInteger maxIndex = 2*currentIndex; maxIndex <= bottomIndex ; maxIndex *= 2) {
        //找到子左右节点的最大值，父节点与这个值比较,首先必须保证右节点要存在
        if ((maxIndex+1)<=bottomIndex && compare(self[maxIndex],self[maxIndex+1])==NSOrderedAscending) {
            ++ maxIndex;
        }
        //比较父结点与子左右节点的最大值，如果小于，则交换，否则break跳出
        if (compare(self[currentIndex],self[maxIndex])==NSOrderedAscending) {
            [self zs_exchangeWithIndexA:currentIndex indexB:maxIndex Change:exchange];
        }
        else{
            break;
        }
        //将当前需要改变的节点位置currentIndex变成maxIndex,这样就可以继续沉底
        currentIndex = maxIndex;
    }
}
```