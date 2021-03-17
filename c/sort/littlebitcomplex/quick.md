## Quick Sort 퀵소트
함께보기 : [[알고리즘] 퀵 정렬(quick sort)이란 - Heee's Development Blog (gmlwjd9405.github.io)](https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html)
- 그나마 좀 할만함
- 얘는 이해를 하고 넘어가는게 좋아요
- 작동원리
	- 퀵소트에는 "피벗"이라는게 있어요
	- 이 피벗을 기준으로 작은거는 왼쪽, 큰거는 오른쪽에 들어가는데
	- 부분배열마다 이 과정을 적용해주면 언젠가는 정렬이 됨!
- 호선쌤의 코드는 재귀호출을 이용했네요
- 그치만 재귀호출이 없어도 구현 가능하다는점을 마찬가지로 알아두세요 :)
```c
void quick(int a[], int left, int right) {
    int pl = left;
    int pr = right;
    int x = a[(pl + pr) / 2];
    // 이 코드에서는 피벗(x)를 원소의 가운데값으로 설정합니다
    do {
        /*
            왼쪽 부분에서 기준보다 작은 원소의 갯수를 셉니다.
            즉 이미 정렬된 부분은 패스하겠다는거!
            a[pl]이 x보다 커지면,
            즉 피벗에 맞게 정렬되지 않은 부분이라면
            아래의 과정에서 졍렬을 합니다.
            이는 pr에도 동일하게 적용되어요.
        */
        while (a[pl] < x)
            pl++;
        while (a[pr] > x)
            pr--;

        if (pl <= pr) {
            swap(int, a[pl], a[pr]);
            // 정렬되지 않은 양 끝 값을 서로 바꿉니다.
            pl++;
            pr--;
            // 그리고는 다시 하나씩 나아가요
        }
    
    // 위 과정을 pl과 pr이 만날떄 까지 반복!
    } while (pl <= pr);
    // 여기까지 진행하면, 가운데 요소를 피벗으로 삼아, 왼쪽은 더 작은값, 오른쪽은 더 큰 값이 들어갑니다.
    /*
        아직 전체가 정렬되지 않았을 수 있어서,
        아래의 재귀문에서 아직 정렬되지 않은 요소들을 마저 정렬해줘요.
        pr이 left보다 크다는것은, 아직 정렬이 덜 끝났다는걸 의미해요
        모두 정렬되어있어야 pr이 left까지 밀고 내려오니까!
        그래서 덜 끝난 부분에 대해서 (left부터 pr까지) 마저 정렬을 수행해줍니다.
        이는 오른쪽에도 똑같이 적용되는 사항!
    */
    if (left < pr)
        quick(a, left, pr);
    if (pl < right)
        quick(a, pl, right);
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcxMjE4MjQ1Nl19
-->