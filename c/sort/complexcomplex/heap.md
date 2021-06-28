## Heap Sort 힙소트
사실 힙소트쯤 되면 이해하기가 굉장히 난해해요. 게다가 호선쌤의 코드가 일반적이지 않아서 더 그럴수도 있어요. 코드를 완벽히 이해하기보다는 대략 어떤 느낌으로 정렬이 되는지만 이해하고 넘어가면 좋아요.
사실 나도 이해가 안되는 부분이 있어서.. 코드 주석을 좀 추상적이게 달아준 부분은 미안해요ㅜ 조금만 더 힘내봅시다!
[호선쌤의 코드와 비슷한 구현체. ](https://m.blog.naver.com/ndb796/221228342808)
[힙트리는 무엇인가? 정석적인 개념을 알고싶다면 이쪽을 참고하세요](https://gmlwjd9405.github.io/2018/05/10/algorithm-heap-sort.html)
```c
static void downheap(int a[], int left, int right) { 
    int temp = a[left]; // 루트노드의 값, 즉 현재범위 힙트리의 최댓값을 담고있다
    int child; // 값이 더 큰 자식의 인덱스를 담을 곳
    int parent; // 현재 정렬중인 노드의 인덱스
    for (parent = left; parent < (right + 1) / 2; parent = child) {
        // 범위의 루트노드부터 시작해서, 아래로 아래로 쭉쭉 뻗어나간다
        int cl = parent * 2 + 1;    // 왼쪽 자식의 인덱스
        int cr = cl + 1;            // 오른쪽 자식의 인덱스
        child = (cr <= right && a[cr] > a[cl]) ? cr : cl;
        /*
            삼항연산분기조건
            1. cr이 현재 현재 범위를 넘으면 안됨.
                오른쪽 자식의 인덱스가 현재 정렬중인 범위를
                넘어서면 안되기 때문에.
                왼쪽자식을 검사하지 않는 이유는 어차피 왼쪽자식은 오른쪽 - 1이고,
                초기화할 때에 left보다 크게 된다

            2. a[cr]이 a[cl]보다 커야함
                이거를 중심적으로 보면 됨.
                오른쪽 자식의 값이 왼쪽 자식 값보다 크냐
                를 판단하고 있는데, 이에 따라서
                더 값이 큰 자식의 인덱스가 담기게 된다
        */

        // 자식의 최댓값이 루트노드 최댓값보다 작다? 이는 최대힙의 구성요건이 갖춰졌음을 의미한다
        // 부분적으로만 보면 틀린 설명이지만, 아래부터 위로 검사하면서 올라오는 반복이기 때문에
        // heatsort의 포문과 함께 생각하면 됨
        // 구성요건이 맞춰졌기 때문에 더 이상의 작업을 하지 않고 끝낸다
        if (temp >= a[child])
            break;
        a[parent] = a[child];
    }
    // parent에는 최댓값이 들어갈 적절한 자리가 담기게 된다
    // 만약 처음부터 올바른 힙 트리였다면 left 값, 즉 루트 노드값이,
    // 아니라면 자식중에 적절한 위치가 되어서 부모가 자식이 되는 상황.. 이된다
    a[parent] = temp;
}

void heapsort(int a[], int n) {
    int i;
    // 아래의 포문을 통해 단말노드가 아닌 노드들을 순회하며, 각 부분트리에 대해 힙트리로 만든다.
    for (i = (n - 1) / 2; i >= 0; i--) {
        downheap(a, i, n-1);
    }

    // 아래의 포문을 통해 모든 노드에서 정렬작업을 수행한다.
    for (i = n - 1; i > 0; i--) {
        swap(int, a[0], a[i]);
        // printf("%d ", i);
        downheap(a, 0, i - 1);
    }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0MjgzNjQ3NDRdfQ==
-->