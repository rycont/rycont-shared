```c
#pragma warning(disable:4996)
#include <stdio.h>
#include <stdlib.h>
#define swap(type, x, y) do { type t = x; x = y; y = t;} while (0)

void main() {
   int i, nx;
   int* x;

   printf("정렬\n");
   printf("요소 개수 : ");
   scanf("%d", &nx);

   x = calloc(nx, sizeof(int));

   for (i = 0; i < nx; i++) {
      printf("x[%d] : ", i);
      scanf("%d", &x[i]);
   }

   // 함수 호출

   printf("오름 차순으로 정렬하였습니다.\n");
   for (i = 0; i < nx; i++)
      printf("x[%d] = %d\n", i, x[i]);

   free(x);

}

void selection(int a[], int n) {
   int i, j;

   for (i = 0; i < n - 1; i++) {
      int min = i;
      for (j = i + 1; j < n; j++)
         if (a[j] < a[min])
            min = j;
      swap(int, a[i], a[min]);
   }
}

void bubble(int a[], int n) {
   int k = 0;
   while (k < n - 1) {
      int j;
      int last = n - 1;
      for(j=n-1; j>k; j--)
         if (a[j - 1] > a[j]) {
            swap(int, a[j - 1], a[j]);
            last = j;
         }
      k = last;
   }
}

void insertion(int a[], int n) {
   int i, j;

   for (i = 1; i < n; i++) {
      int tmp = a[i];
      for (j = i; j > 0 && a[j - 1] > tmp; j--)
         a[j] = a[j - 1];
      a[j] = tmp;
   }
}

void shell(int a[], int n) {
   int i, j, h;
   for(h=n/2; h >0; h/= 2)
      for (i = h; i < n; i++) {
         int tmp = a[i];
         for (j = i - h; j >= 0 && a[j] > tmp; j -= h)
            a[j + h] = a[j];
         a[j + h] = tmp;
      }
}

static int* buff;
static void __mergeSort(int a[], int left, int right) {
   if (left < right) {
      int center = (left + right) / 2;
      int p = 0;
      int i;
      int j = 0;
      int k = left;
      __mergeSort(a, left, center);
      __mergeSort(a, center + 1, right);
      for (i = left; i <= center; i++)
         buff[p++] = a[i];
      while (i <= right && j < p)
         a[k++] = (buff[j] <= a[i]) ? buff[j++] : a[i++];
      while (j < p)
         a[k++] = buff[j++];
   }
}
int mergesort(int a[], int n) {
   if ((buff = calloc(n, sizeof(int))) == NULL)
      return -1;
   __mergeSort(a, 0, n - 1);
   free(buff);
   return 0;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTU4MTMzMzAyOF19
-->