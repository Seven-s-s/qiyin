---
title: 排序算法
sticky: true
date: 2020/12/16
tags: 算法
---

### 冒泡排序

1. 算法思路

   > 比较相邻的元素。如果第一个比第二个大，就交换他们两个。
   > 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。
   > 针对所有的元素重复以上的步骤，除了最后一个。
   > 持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。

2. 动图演示

   ![bubbleSort](/assets/algorithm.assets/bubbleSort.gif)

3. java代码

   ```java
   public int[] sort(int array[]){
   int length = array.length;
       int[] arr = Arrays.copyOf(array, length);
       for (int i = 1; i < length; i++) {
          //定一个标记，若为true，说明此次循环没有进行交换，则说明已经有序，排序已完成
          boolean flag = true;
          for (int j = 0; j < length-i; j++) {
              if (arr[j] > arr[j+1]){
                  int temp = arr[j+1];
                  arr[j+1] = arr[j];
                  arr[j] = temp;
                  flag = false;
              }
          }
          if (flag){
              break;
          }
       }
       return arr;
   } 
   ```

### 选择排序

1. 算法思路

   > 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。
   > 再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
   > 重复第二步，直到所有元素均排序完毕。


2. 动图演示

   ![selectionSort](/assets/algorithm.assets/selectionSort.gif)

3. 代码实现

   ```java
   public int[] sort(int[] array){
       int length = array.length;
       int arr[] = Arrays.copyOf(array,length);
       //经过n-1轮比较
       for (int i = 0; i < length - 1; i++) {
          int min = i;
          //每轮需要比较n-i次
          for (int j = i + 1; j < length; j++) {
              if (arr[j] < arr[min]) {
                  //记录目前能找到最小值元素的下标
                  min = j;
              }
          }
          //找到最小值和i的位置进行交换
          if (i != min) {
              int temp = arr[i];
              arr[i] = arr[min];
              arr[min] = temp;
          }
       }
       return arr;
   }
   ```

### 插入排序

1. 算法思路

   > 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。
   >
   > 从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）

2. 动图演示

   ![insertionSort](/assets/algorithm.assets/insertionSort.gif)

3. 代码实现

   ```java
   public int[] sort(int[] array){
       int length = array.length;
       int[] arr = Arrays.copyOf(array,length);
       // 从下标为1的元素开始选择合适的位置插入，因为下标为0的只有一个元素，默认是有序的
       for (int i = 1;i < length ;i++){
           // 记录要插入的数据
           int temp = arr[i];
           // 从已经排序的序列最右边的开始比较，找到比其小的数
           int j = i;
           while (j > 0 && arr[j - 1] > temp){
               arr[j] = arr[j - 1];
               j--;
           }
           // 存在比其小的数，插入
           if ( j != i){
               arr[j] = temp;
           }
       }
       return arr;
   }
   ```

### 希尔排序

1. 算法思路

   > 希尔排序（Shell Sort）又叫做**缩小增量排序**（diminishing increment sort），是直接插入排序算法的一种更高效的改进版本。希尔排序是非稳定排序算法。希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，当增量减至1时，整个文件恰被分成一组，算法便终止。

2. 动图演示

   ![shellSort](/assets/algorithm.assets/shellSort.gif)

3. 代码实现

   ```java
   public static int[] sort(int[] array){
       int length = array.length;
       int[] arr = Arrays.copyOf(array,length);
       int temp;
       for (int step = length / 2;step >= 1;step /= 2){
          for (int i = step;i < length;i++){
              temp = arr[i];
              int j = i - step;
              while (j >= 0 && arr[j] > temp){
                  arr[j+step] = arr[j];
                  j -= step;
              }
              arr[j+step] = temp;
          }
       }
       return arr;
   }
   ```

### 归并算法

1. 算法思路

   > 1. 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；
   > 2. 设定两个指针，最初位置分别为两个已经排序序列的起始位置；
   > 3. 比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；
   > 4. 重复步骤 3 直到某一指针达到序列尾；
   > 5. 将另一序列剩下的所有元素直接复制到合并序列尾。

2. 动图演示

   ![mergeSort](/assets/algorithm.assets/mergeSort.gif)

3. 代码实现

   ```java
   public int[] sort(int[] array){
      int length = array.length;
      int[] arr = Arrays.copyOf(array,length);
      if (arr.length < 2){
          return arr;
      }
      int middle = (int) Math.floor(length / 2);
      int left[] = Arrays.copyOfRange(arr,0,middle);
      int right[] = Arrays.copyOfRange(arr,middle,length);
      return merge(sort(left),sort(right));
   }
   
   private int[] merge(int[] left, int[] right) {
      int[] arr = new int[left.length+right.length];
      int i = 0;
      while (left.length > 0 && right.length > 0){
          if (left[0] <= right[0]){
              arr[i++] = left[0];
              left = Arrays.copyOfRange(left,1,left.length);
          }else {
              arr[i++] = right[0];
              right = Arrays.copyOfRange(right,1,right.length);
          }
      }
      while (left.length > 0){
          arr[i++] = left[0];
          left = Arrays.copyOfRange(left,1,left.length);
      }
      while (right.length > 0){
          arr[i++] = right[0];
          right = Arrays.copyOfRange(right,1,right.length);
      }
      return arr;
   }
   ```

### 快速排序

1. 算法思路

   > 1. 从数列中挑出一个元素，称为 "基准"（pivot）;
   > 2. 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
   > 3. 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；

   2. 动图演示

2. ![quickSort](/assets/algorithm.assets/quickSort.gif)

3. 代码实现

   ```java
   public int[] sort(int[] array){
       int[] arr = Arrays.copyOf(array,array.length);
       return quickSort(arr,0,arr.length -1);
   }
   
   private int[] quickSort(int[] arr, int left, int right) {
       if (left < right){
          int partitionIndex = partition(arr,left,right);
          quickSort(arr,left,partitionIndex - 1);
          quickSort(arr,partitionIndex + 1,right);
       }
       return arr;
   }
   
   private int partition(int[] arr, int left, int right) {
       //设定基准值pivot
       int pivot = left;
       int index = pivot + 1;
       for (int i = index;i <= right;i++){
          if (arr[i] < arr[pivot]) {
              swap(arr,i,index);
              index++;
          }
       }
       swap(arr,pivot,index - 1);
       return index - 1;
   }
   
   private void swap(int[] arr, int i, int j) {
       int temp = arr[i];
       arr[i] = arr[j];
       arr[j] = temp;
   }
   ```

### 堆排序

1. 算法思路

   > 1. 创建一个堆 H[0……n-1]；
   > 2. 把堆首（最大值）和堆尾互换；
   > 3. 把堆的尺寸缩小 1，并调用 shift_down(0)，目的是把新的数组顶端数据调整到相应位置；
   > 4. 重复步骤 2，直到堆的尺寸为 1。

2. 动图演示

   ![heapSort](/assets/algorithm.assets/heapSort.gif)

   ![Sorting_heapsort_anim](/assets/algorithm.assets/Sorting_heapsort_anim.gif)

3. 代码实现

   ```java
   public int[] sort(int[] array){
      int length = array.length;
      int[] arr = Arrays.copyOf(array,length);
      buildMaxHeap(arr,length);
      for (int i = length - 1;i > 0;i--){
          swap(arr,0,i);
          length--;
          heapify(arr,0,length);
      }
      return arr;
   }
   
   private void buildMaxHeap(int[] arr, int length) {
      for (int i = (int) Math.floor(length / 2);i >= 0 ; i--) {
          heapify(arr,i,length);
      }
   }
   
   private void heapify(int[] arr, int i, int length) {
      int left = 2 * i + 1;
      int right = 2 * i + 2;
      int largest = i;
   
      if (left < length && arr[left] > arr[largest]){
          largest = left;
      }
   
      if (right < length && arr[right] > arr[largest]){
          largest = right;
      }
   
      if (largest != i){
          swap(arr,i,largest);
          heapify(arr,largest,length);
      }
   }
   
   private void swap(int[] arr, int i, int j) {
      int temp = arr[i];
      arr[i] = arr[j];
      arr[j] = temp;
   }
   ```

### 计数排序

1. 算法思路

   > 1. 找出待排序的数组中最大和最小的元素
   >
   > 2. 统计数组中每个值为i的元素出现的次数，存入数组C的第i项
   >
   > 3. 对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）
   >
   > 4. 反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1

2. 动图演示

   ![countingSort](/assets/algorithm.assets/countingSort.gif)

3. 代码实现

   ```java
   public int[] sort(int[] array){
    int[] arr = Arrays.copyOf(array,array.length);
    int maxLength  = getMaxLength(arr);
    return countingSort(arr,maxLength);
   }
   
   private int[] countingSort(int[] arr, int maxLength) {
    int count[] = new int[maxLength+1];
    for (int value : arr){
        count[value]++;
    }
    int index = 0;
    for (int i = 0; i <= maxLength;i++){
        while (count[i] > 0){
            arr[index++] = i;
            count[i]--;
        }
    }
    return arr;
   }
   
   private int getMaxLength(int[] arr) {
    int max = arr[0];
    for (int value : arr){
        if (value > max){
            max = value;
        }
    }
    return max;
   }
   ```

   

### 桶排序

1. 算法思路

   > 1. 在额外空间充足的情况下，尽量增大桶的数量
   > 2. 使用的映射函数能够将输入的 N 个数据均匀的分配到 K 个桶中

2. 动图演示

   > 将元素分布于桶中

   ![Bucket_sort_1.svg_](/assets/algorithm.assets/Bucket_sort_1.svg.png)

   > 在桶内进行排序

   ![Bucket_sort_2.svg_](/assets/algorithm.assets/Bucket_sort_2.svg.png)

3. 代码实现

   ```java
   import java.util.Arrays;
   
   public class BucketSort {
    private static InsertionSort insertSort = new InsertionSort();
   
    public int[] sort(int[] array){
        int[] arr = Arrays.copyOf(array,array.length);
        return bucketSort(arr,5);
    }
   
    private int[] bucketSort(int[] arr, int backetSize) {
        if(arr.length == 0){
            return arr;
        }
        int max = arr[0];
        int min = arr[0];
        for (int value : arr){
            if (value > max){
                max = value;
            }
            else if (value < min){
                min = value;
            }
        }
        int bucketCount = (int) Math.floor((max - min) / backetSize) + 1;
        int[][] buckets = new int[bucketCount][0];
   
        //利用映射将数据分配到各个桶中
        for (int i = 0;i < arr.length;i++){
            int index = (int) Math.floor((arr[i] - min) /backetSize);
            buckets[index] = arrAppend(buckets[index],arr[i]);
        }
        int arrIndex = 0;
        for (int[] bakcet : buckets){
            if (bakcet.length <= 0){
                continue;
            }
            //对每个桶进行排序，这里采用了插入排序
            bakcet = insertSort.sort(bakcet);
            for (int value : bakcet){
                arr[arrIndex++] = value;
            }
        }
        return arr;
    }
   
    /**
        * 自动扩容，并保存数据
        * @param arr
        * @param value
        * @return
        */
       private int[] arrAppend(int[] arr, int value) {
           arr = Arrays.copyOf(arr,arr.length+1);
           arr[arr.length - 1] = value;
           return arr;
       }
   }
   ```

### 基数排序

1. 算法思路

   根据键值的每位数字来分配桶；

2. 动图演示

   ![radixSort](/assets/algorithm.assets/radixSort.gif)

3. 代码实现

   ```java
   import java.util.Arrays;
   
   public class RadixSort {
        public int[] sort(int[] array){
            int[] arr = Arrays.copyOf(array,array.length);
            int maxDigit = getMaxDigit(arr);
            return radixSort(arr,maxDigit);
        }
   
        /**
            * 获得最高位
            * @param arr
            * @return
        */
       private int getMaxDigit(int[] arr) {
           int maxValue = getMaxValue(arr);
           return getNumLength(maxValue);
       }
   
       private int getMaxValue(int[] arr) {
           int maxValue = arr[0];
           for (int value : arr){
               if (maxValue < value){
                   maxValue = value;
               }
           }
           return maxValue;
       }
       private int getNumLength(long maxValue) {
           if (maxValue == 0){
               return 1;
           }
           int length = 0;
           for (long temp = maxValue;temp != 0;temp /= 10){
               length++;
           }
           return length;
       }
   
       private int[] radixSort(int[] arr, int maxDigit) {
           int mod = 10;
           int dev = 1;
           for (int i = 0; i < maxDigit;i++,dev *= 10,mod *= 10){
               // 考虑负数的情况，这里扩展一倍队列数，其中 [0-9]对应负数，[10-19]对应正数 (bucket + 10)
               int[][] counter = new int[mod * 2][0];
   
               for (int j = 0;j < arr.length;j++) {
                   int bucket = ((arr[j] % mod) / dev) + mod;
                   counter[bucket] = arrAppend(counter[bucket], arr[j]);
               }
               int pos = 0;
               for (int[] bucket : counter){
                   for (int value : bucket){
                       arr[pos++] = value;
                   }
               }
           }
           return arr;
       }
   
       /**
        * 自动扩容，并保存数据
        *
        * @param arr
        * @param value
        * @return
        */
       private int[] arrAppend(int[] arr, int value) {
           arr = Arrays.copyOf(arr,arr.length+1);
           arr[arr.length - 1] = value;
           return arr;
       }
   
   }
   ```

   