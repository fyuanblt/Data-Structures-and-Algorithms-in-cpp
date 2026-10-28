### 内排序

|      Part I       |     Part II     |    Part III     |     Part IV     |       Part V        |
| :---------------: | :-------------: | :-------------: | :-------------: | :-----------------: |
| [插入排序](#p1) | [选择排序](#p2) | [交换排序](#p3) | [归并排序](#p4) | [基于分配的排序](#p5) |

<span id="p1">**==1. 插入排序**</span>
~~~cpp
1.1直接插入排序
for(int i=1;i<n;i++){
int key=arr[i];
int j=i-1;
while(j>=0 && arr[j]>key){
arr[j+1]=arr[j];
j--;}
arr[j+1]=key;
}
~~~
~~~cpp
1.2折半插入排序
for(int i=1;i<n;i++){
int key=arr[i];
int left=0,right=i-1;
while(left<=right){
int mid=(left+right)/2;
if(arr[mid]>key){
right=mid-1;
}
else{
left=mid+1;
}
for(int j=i-1;j>=left;j--){
arr[j+1]=arr[j];
arr[left]=key;
}
}
}
~~~
~~~cpp
1.3希尔排序
for(int gap=n/2;gap>0;gap/=2){
for(int i=gap;i<n;i++){
int key=arr[i];
int j=i-gap;
while(j>=0 && arr[j]>key){
arr[j+gap]=arr[j];
j=j-gap;
}
arr[j+gap]=key;
}
}
~~~
<span id="p2">**==2. 选择排序**</span>
~~~cpp
2.1简单选择排序
for(int i=0;i<n-1;i++){
int min=i;
for(int j=i+1;j<n;j++){
if(arr[j]<arr[min]){
min=j;}
}
if(min!=i){
swap(arr[i],arr[min]);
}
}
~~~
~~~cpp
2.2堆排序
~~~
<span id="p3">**==3. 交换排序**</span>
~~~cpp
3.1冒泡排序
for (int i = 0; i < n - 1; ++i) {            // ① 最多 n-1 趟
        bool swapped = false;                    // ② 本趟是否发生交换
        for (int j = 0; j < n - 1 - i; ++j) {    // ③ 每趟比较到 n-1-i
            if (arr[j] > arr[j + 1]) {           // ④ 前面比后面大就交换
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped) break;                     // ⑤ 本趟没交换 → 已有序，提前结束
    }
~~~
~~~cpp
3.2快速排序
int partition(vector<int>& arr, int lo, int hi) {
    int pivot = arr[lo];          // ① 挖出 arr[lo] 当基准，lo 成为"坑"
    int i = lo, j = hi;
    while (i < j) {
        while (i < j && arr[j] >= pivot) --j;   // ② 右指针左移，找 < pivot 的
        arr[i] = arr[j];                        //    填到左边坑，j 变新坑
        while (i < j && arr[i] <= pivot) ++i;   // ③ 左指针右移，找 > pivot 的
        arr[j] = arr[i];                        //    填到右边坑，i 变新坑
    }
    arr[i] = pivot;               // ④ i == j，枢轴归位
    return i;                     //    返回枢轴位置
}

// 快速排序：对 arr[lo..hi] 排序
void quickSort(vector<int>& arr, int lo, int hi) {
    if (lo >= hi) return;         // ⑤ 终止条件：区间长度 ≤ 1
    int p = partition(arr, lo, hi);   // ⑥ 分区，p 是枢轴归位后的下标
    quickSort(arr, lo, p - 1);        // ⑦ 递归排左半区
    quickSort(arr, p + 1, hi);        // ⑧ 递归排右半区
}
~~~
<span id="p4">**==4. 归并排序**</span>
~~~cpp
4.1归并排序
4.1.1自顶向下
void merge(std::vector<int>& arr, int lo, int mid, int hi) {
    std::vector<int> tmp(hi - lo + 1);
    int i = lo, j = mid + 1, k = 0;
    while (i <= mid && j <= hi) {
        if (arr[i] <= arr[j]) tmp[k++] = arr[i++];  // <= 保证稳定
        else tmp[k++] = arr[j++];
    }
    while (i <= mid) tmp[k++] = arr[i++];
    while (j <= hi) tmp[k++] = arr[j++];
    for (int t = 0; t < k; ++t) arr[lo + t] = tmp[t];
}

void mergeSort(std::vector<int>& arr, int lo, int hi) {
    if (lo >= hi) return;
    int mid = lo + (hi - lo) / 2;
    mergeSort(arr, lo, mid);
    mergeSort(arr, mid + 1, hi);
    merge(arr, lo, mid, hi);
}
4.1.2自底向上
void merge(std::vector<int>& arr, int lo, int mid, int hi) {
    std::vector<int> tmp(hi - lo + 1);
    int i = lo, j = mid + 1, k = 0;
    while (i <= mid && j <= hi) {
        if (arr[i] <= arr[j]) tmp[k++] = arr[i++];  // <= 保证稳定
        else tmp[k++] = arr[j++];
    }
    while (i <= mid) tmp[k++] = arr[i++];
    while (j <= hi) tmp[k++] = arr[j++];
    for (int t = 0; t < k; ++t) arr[lo + t] = tmp[t];
}
void mergeSortBottomUp(vector<int>& arr) {
    int n = arr.size();
    // width 表示"当前每个子段的长度"，从 1 开始翻倍
    for (int width = 1; width < n; width *= 2) {
        for (int lo = 0; lo < n; lo += 2 * width) {
            int mid = min(lo + width - 1, n - 1);        // 左段末尾
            int hi  = min(lo + 2 * width - 1, n - 1);    // 右段末尾
            if (mid < hi) merge(arr, lo, mid, hi);       // 两段都存在才合
        }
    }
}
~~~













































































































 
