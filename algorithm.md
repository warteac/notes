## sorting
- 分为5类：交换排序、插入排序、

### 交换排序
```

	void swap (int * a, int * b) {
		int tmp = *a;
		*a = *b;
		*b = tmp;
	}
	
	void swapsort (int arr[], int n) {
		for (int i = 0; i < n-1; i++) {
			for (int j = i+1; j < n; j++) {
				if (arr[i] < arr[j]) {
					swap (&arr[i],&arr[j]);
				}
			}
		}
	}
```

#### 1.冒泡排序
- 两两相邻比较


```

	void bubblesort (int arr[], int n) {
		for (int i = 0; i < n-1; i++) {        // n-1轮循环
			for (int j = 0; j < n-i-1; j++) {  // n-i-1
				if (arr[j] < arr[j+1]) {
					swap (&arr[j],&arr[j+1]);  // 两两相邻比较
				}
			}
		}	
	}
```

- 冒泡排序优化
	- 设置一个标志位，如果有一轮循环中没有出现交换，表示数组已经有序，就退出循环。
#### 2.快速排序
- 选序列的第一个为标准，一次遍历之后可以分为左右两个子序列，左边都比标准小，右边都大
- 递归调用，将左右两个子序列再快排，直至完全排好序

```

	void quicksort (int arr[], int left, int right) {
		if (left >= right) return;
		int i = left, j = right;
		int key = arr[i];
		int empty = i;
		while (i < j) {
			if (empty == i){
				if (arr[j] < key){
					arr[empty] = arr[j];
					empty = j; i++;
				}else{
					j--;				
				}
			} else {
				if (arr[i] > key) {
					arr[empty] = arr[i];
					empty = i; j--;
				}else{
					i++;				
				}
			}
		}
		arr[i] = key;
		quicksort(arr,left,empty-1);
		quicksort(arr,empty+1,right);
	}
```

```

	void quicksort (int arr[], int left, int right) {		
		if (left >= right) return;
		int i = left, j = right;
		int key = arr[i];
		while (i < j) {
			while ((i < j)&&(arr[j] >= key)) {
				j--;
			}
			arr[i] = arr[j];
			while ((i < j)&&(arr[i] <= key)) {
				i++;
			}
			arr[j] = arr[i];
		}
		arr[i] = key;
		quicksort(arr,left,i-1);
		quicksort(arr,i+1,right);
	}
```

- 时间复杂度：平均 nlog(n)

- 空间复杂度：nlog(n)，递归调用需要栈空间


- 不稳定的排序算法

#### 补充：递归
- Three laws of recursion

1 A recursive algorithm must have a base case. (出口)

2 A recursive algorithm must change its state and move towards the base case. （子问题）

3 A recursive algorithm must call itself, recursively. （自调用）
[http://www.interactivepython.org/runestone/static/pythonds/Recursion/TheThreeLawsofRecursion.html](http://www.interactivepython.org/runestone/static/pythonds/Recursion/TheThreeLawsofRecursion.html)

### 插入排序
- 插入排序可视为两步操作：一步是插入，一步是排序。
- 基本思想是:将一条记录插入到一组有序的序列中，得到一个有序的、数据个数加1的新序列。

#### 1. 直接插入排序
```

	void insertsort(int arr[], int n) {
		for (int i = 1; i < n; i++) {
			int j = i;
			while (j > 0) {
				if (arr[j-1] > arr[j]){
					swap(&arr[j-1],&arr[j]);
					j--;
				}else {
					break;
				}
			}
		}
	}

```
```

	void insertsort(int arr[], int n) {
		int i,j;
		for (i = 1; i < n; i++){
			int tmp = arr[i];
			j = i;
			while ((j > 0) && (arr[j-1] > tmp)) {
				arr[j] = arr[j-1];
				j--;
			}
			arr[j] = tmp;
		}
	}
```

- 时间复杂度：olog(n)
- 稳定的

#### 2.折半插入排序
- 对直接插入排序的优化，在已经有序的序列中寻找合适的插入位置，使用折半查找办法

```

	int binarysearch (int arr[], int num, int left, int right) {
		if (left >= right) return left;
		int mid = (left + right) / 2;
		if (arr[mid] == num) {
			return mid;
		}else if (arr[mid] > num) {
			binarysearch(arr, num, left, mid-1);
		}else {
			binarysearch(arr, num, mid+1, right);
		}
	}

	void binaryinsertsort (int arr[], int n) {
		for (int i = 1; i < n; i++) {
			int pos = binarysearch(arr,arr[i],0,i-1);
			for(int j = i-1; j >= pos; j--) {
				arr[j+1] = arr[j];
			}
			arr[pos] = arr[i];
		}
	}
```


