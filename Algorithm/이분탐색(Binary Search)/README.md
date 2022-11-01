# 이분 탐색(Binary Search)
  * [What is 이분 탐색?](#what-is-------)
    + [특징](#--)
  * [이분 탐색 구현해보기 (with python)](#-------------with-python-)
  * [이분 탐색은 언제 쓰는게 좋을까?](#------------------)


## What is 이분 탐색?

```
'정렬되어 있는 리스트' 에서 탐색 범위를 '절반씩' 좁혀가며 데이터를 탐색하는 방법

이진 탐색이라고도 합니다~
```

### 특징

- 리스트가 정렬되어 있어야만 사용할 수 있다.
- 변수✨3개 (`start`, `end`, `mid`)를 사용하여 탐색한다.

직접 정렬해보자~

- list (n = 17), 찾을 값 = `37`

| index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| value | 1 | 3 | 5 | 7 | 11 | 13 | 17 | 19 | 23 | 29 | 31 | 37 | 41 | 43 | 47 | 53 | 59 |
- 첫 번째 시도
1. `start` = 0, `end` = 16 → `mid` = 8 (`mid = (start+end)/2`), 탐색 범위 : 16 (n)
2. 37 > 23 (mid)보다 크므로 탐색 범위를 mid의 **오른쪽 범위로 좁힌다.**
3. `start` = `mid + 1`이 되는것✨

- 두 번째 시도
1. `start` = 9, `end` = 16, `mid` = 12 (25/2), 탐색 범위 : 8 (16/2)
2. 37 < 41(mid)보다 작으므로 탐색범위를 mid의 **왼쪽 범위로 좁힌다.**
3. `end` = `mid - 1`이 되는 것✨

- 세 번째 시도
1. `start` = 9, `end`= 11, `mid`= 10 (20/2), 탐색 범위 : 3 (8/2)
2. 37 > 31 (mid)보다 크므로 탐색 범위를 mid의 **오른쪽 범위로 좁힌다.**

- 네 번째 시도
1. `start` = 11, `end` = 11, `mid` = 11, 탐색 범위 : 1 (3/2)
2. `list[mid]` **→ list[11]은 37이므로 원하는 값을 찾았다!🎃**

- 종료 조건 : 값을 찾으면 종료, 즉 list[mid] = value 면 종료!
- 단계가 지나감에 따라 탐색할 배열의 크기가 `절반씩` 줄어든다.✨
    - 즉 N, N/2, N/4, N/8, ... , 1 로 실행되는 것이므로
    - 시간 복잡도는 `O(logN)`이 된다.

---

## 이분 탐색 구현해보기 (with python)

1. **반복문으로 구현하기**

```python
def binary_search(arr, target):
	start = 0
	end = len(arr) - 1
	mid = 0

	while start <= end: 
		mid = (start + end) // 2

		if arr[mid] == target: # 원하는 값 찾은 경우 index 반환
			return mid
		elif arr[mid] > target: # mid의 왼쪽으로 제한
			end = mid - 1
		else # mid의 오른쪽으로 제한
			start = mid + 1

	return -1
```

1. **재귀로 구현하기**

```python
def binary_search(arr, target, start, end):
	if start > end:
		return -1
	
	mid = (start + end) // 2
	
	if arr[mid] == target:
		return mid
	elif arr[mid] > target: # mid의 왼쪽으로 제한해서 다시 탐색
		return binary_search(arr, target, start, mid - 1)
	else: # mid의 오른쪽으로 제한해서 다시 탐색
		return binary_search(arr, target, mid + 1, end)
```

1. **파이썬 이진 탐색 라이브러리**
- `bisect_left(arr, x)`
    - 정렬된 순서를 유지하면서 리스트 arr에 데이터 x를 삽입할 가장 왼쪽 인덱스를 찾는 메소드
- `bisect_right(arr, x)`
    - 정렬된 순서를 유지하도록 리스트 arr에 데이터 x를 삽입할 가장 오른쪽 인덱스를 찾는 메소드

```python
from bisect import bisect_left, bisect_right
a = [1, 2, 4, 4, 8]
x = 4

print(bisect_left(a, x))
>>> 2
# 리스트 a에 4를 삽입할 가장 왼쪽 인덱스는 2이다.

print(bisect_right(a, x))
>>> 4
# 리스트 a에 4를 삽입할 가장 오른쪽 인덱스는 4이다.
```

![image](https://user-images.githubusercontent.com/85485290/199229261-eda67afe-89bf-400d-ac55-39cbb71179cb.png)

---

## 이분 탐색은 언제 쓰는게 좋을까?

- 예제 - [백준 2805 나무자르기](https://www.acmicpc.net/problem/2805)
    - 입력
        - 첫째줄 : 나무의 수 N, 가져가려고 하는 나무의 길이 M
        - 둘째줄 : 나무의 높이 (나무의 높이 합은 항상 M보다 크거나 같아서 항상 나무를 가져갈 수 있음)
        
        ```
        4 7
        20 15 10 17
        ```
        
    - 출력
        - 적어도 M미터의 나무를 가져가기 위해 절단기에 설정할 수 있는 높이의 최대값을 출력
        
        ```
        15
        ```
        

- 절단기 높이가 height일 때, M 이상의 나무를 얻을 수 있는가?

```python
n = 0
m = 0
trees = []

def get_total_tree(height):
    total_tree = 0
    for tree in trees:
        if tree > height:
            total_tree += tree - height
    return total_tree

if __name__ == '__main__':
    n, m = map(int, input().split())
    trees = list(map(int, input().split()))

    # 절단기 높이 범위
    start = 1
    end = max(trees)

    while start <= end:
        mid = (start + end) // 2

        if get_total_tree(mid) >= m:
            start = mid + 1
        else:
            end = mid - 1

    print(end)
```

- ‘정렬된 리스트’에서 ‘특정 범위’에 속하는 원소의 개수를 구할 때 사용하기

```python
from bisect import bisect_left, bisect_right

def count_by_range(arr, left_value, right_value):
    right_index = bisect_right(arr, right_value)
    left_index = bisect_left(arr, left_value)
    print('right : ', right_index, 'left :', left_index)
    return right_index - left_index

a = [1, 2, 3, 3, 3, 3, 4, 4, 8, 9]
print(count_by_range(a, 4, 4))
>>> right : 8 left : 6
>>> 2
# 리스트 a에 4는 총 2개 존재한다.

print(count_by_range(a, -1, 3))
>>> right : 6 left : 0
>>> 6
# 리스트 a에 -1~3사이의 값은 총 6개 존재한다.
```
