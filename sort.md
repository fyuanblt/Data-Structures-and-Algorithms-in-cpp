### 内排序

|      Part I       |     Part II     |    Part III     |     Part IV     |       Part V        |
| :---------------: | :-------------: | :-------------: | :-------------: | :-----------------: |
| [插入排序](#p1) | [选择排序](#p2) | [交换排序](#p3) | [归并排序](#p4) | [基于分配的排序](#p5) |

<span id="p1">**==1. 顺序线性表==**</span>
1.1直接插入排序
for(int i=1;i<n;i++){
int key=arr[i];
int j=i-1;
while(j>=0 && arr[j]>key){
arr[j+1]=arr[j];
j--;}
arr[j+1]=key;
}

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
