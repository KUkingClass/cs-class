# 이분 탐색(Binary Search)
  * [What is 이분 탐색?](#what-is-------)
    + [특징](#--)
  * [이분 탐색 구현해보기 (with python)](#-------------with-python-)
  * [이분 탐색은 언제 쓰는게 좋을까?](#------------------)
  * [lower bound와 upper bound](#lower-bound--upper-bound)
    + [lower bound](#lower-bound)
    + [upper bound](#upper-bound)
  * [이진 탐색 구현 시 헷갈리는 점✨](#------------------)
    + [헷갈리지 않게 구현하기 (수정예정 ^^)](#----------------------)


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

---
## lower bound와 upper bound

- `이진 탐색`은 리스트에서 정확히 원하는 값 (target)을 찾는 방법이었다.
- `lower/upper bound` 란?

```
target 값 보다 처음으로 크거나 작은 값이 나오는 '위치'를 찾는 방법

중복된 요소를 찾거나, 타겟의 상한선 및 하한선을 찾을 때 등,, 사용!
```

- `lower bound`

```
target 값보다 '같거나 큰 값'이 나오는 '처음 위치'를 찾음

즉, target 값 이상인 최초의 인덱스 반환
```

- `upper bound`

```
target 값보다 '처음으로' '큰 값'이 나오는 위치를 찾음

즉, target 값 초과인 최초의 인덱스 반환
```

- 중복된 요소를 찾을 수 있다고 한 이유 → lower ~ upper 사이의 인덱스들은 중복된 값을 가지고 있을것임

| index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| value | 1 | 2 | 2 | 3 | 3 | 3 | 4 | 6 | 7 |  |

- list = [1, 2, 2, 3, 3, 3, 4, 6, 7] (n = 9)
    - lower_bound(3) → 3 ✨
    - upper_bound(3) → 6 ✨
    
    - lower_bound(4) → 6
    - upper_bound(4) → 7
    
    - lower_bound(5) → 7
    - upper_bound(5) →  7
    
    - lower_bound(6) → ?
    - upper_bound(6) → ?

<img src="https://user-images.githubusercontent.com/85485290/199452451-831cee92-faa0-455c-aed8-2be3323ed6a9.png" width="400" />

- 리스트의 모든 수가 target보다 작다면 → 범위 밖을 리턴해줘야한다. (ex. target = 8)
    - 그래서 high를 n-1이 아니라 `n` 로 먼저 지정해야 한다! ✨
    

### lower bound

- 이진 탐색은 target 값을 찾으면 바로 리턴했었다.
- But, lower bound는 → arr[mid]가 target 값과 같거나 크면 바로 리턴하지 않고 ‘처음으로’ 나오는 위치를 찾기 위해서 범위를 더 좁히면서 탐색한다.✨

- [1, 2, 2, 3, 3, 3, 4, 6, 7]
- 위에서 lower_bound(3) 를 보면
    - low = `0`, high = `9`, mid = `4`
    - **arr[mid] = 3** → 어 3을 찾았네?
    - [1, 2, 2, 3, 3, 3, ,,] → mid는 지금 index=4를 가리키고 있다.
    - 근데 lower_bound는 3보다 크거나 같은 값의 ‘처음’ 위치를 알아내야하므로 계속 탐색해줘야함 (즉, 여기서 mid=4가 아니라 `mid=3`이 되어야 하는것)
    - 그래서 `high = mid`로 지정해서 범위를 좁힌다!⭐️

```python
def lower_bound(arr, target):
	low = 0
	high = len(arr)

	while low < high:
		mid = (low + high) // 2
		
		if arr[mid] >= target:
			high = mid # target 값보다 크거나 같은 값의 '처음' 위치를 알아내기 위해
		else:
			low = mid + 1 # mid - target - high 순 -> mid의 오른쪽 범위로 제한
	
	return low
```

- Q. 이진탐색과 다르게 lower ≤ high가 아닌 이유? (hint. lower ≤ high 로 지정하게 되면 무한 루프에서 빠져나오지 못한다)

- Q. high는 n-1이 아니라 n으로 지정해야 하는 이유를 알았다. 그렇다면 low도 0이 아니라 -1로 해줘야 하는 것 아닐까?

### upper bound

- upper bound는 arr[mid]가 target 값보다 커지는 ‘처음’ 위치를 리턴해야 하는 것
- 즉, arr[mid] > target 이라면, 크다고 바로 리턴하지 않고 최초로 큰 값이 나오는 위치를 찾기 위해 범위를 더 좁히며 탐색한다.✨

- [1, 2, 2, 3, 3, 3, 4, 6, 7]
- upper_bound(3) 를 보면
    - low = 0, high = 9, mid = 4
    - arr[mid] = 3 < target 이기 때문에 low = mid + 1로!
    - low = 4, high = 9, mid = 6
    - arr[mid] = 4 > target 이지만 ‘최초로’ 3보다 큰 값이 나오는 위치인지 확인해야함
    - `high=mid` 로 지정해서 범위를 좁힌다!⭐️

```python
def upper_bound(arr, target):
	low = 0
	high = len(arr)

	while low < high:
		mid = (low + high) // 2
		
		if arr[mid] > target:
			high = mid # target 값보다 큰 값의 '처음' 위치를 알아내기 위해
		else:
			low = mid + 1 # mid - target - high 순 -> mid의 오른쪽 범위로 제한
	
	return low
```

---

## 이진 탐색 구현 시 헷갈리는 점✨

1. 경계를
    - low = 0, high = n - 1
    - low = 0, high = n
    - low = -1, high = n

등,, 어느걸 선택할지

1. 탈출 조건으로 
    - `low ≤ high`
    - `low < high`
    - `low + 1 < high`

중 어느걸 선택해야 할지

1. 정답이 
    - (low + high) / 2 (mid)
    - high
    - low

어떤것인지

---

### 헷갈리지 않게 구현하기 (수정예정 ^^)

- 일단 low, high는 항상 ‘정답의 범위’를 나타낼 수 있도록 구간을 설정해야 한다.
    - 예를들어 low를 출력해야 하는 문제의 답이 최대 n일 때, high = n으로 설정하거나
    - high를 출력해야 하는 문제의 답이 최소 0일 때, low = 0으로 선언하면 안된다.
    - 이때는 → `high = n+1`, `low = -1`로 선언해야함!✨
    
- 초기값을 잘 선언해주었다면 `low + 1 < high` 동안 mid = `(low + high) / 2` 를 구해서
- arr[mid] > target 이면 `high = mid`를
- arr[mid] < target 이면 `low = mid`를 해주면 됨

- low + 1 < high 이기 때문에 low와 high 사이에 `무조건 1개의 칸이 있고`
- 항상 low < mid < high를 만족하고 있음
- 언젠가 low + 1 == high가 되면 탈출

- 이분 탐색이 끝나면 low와 high는 각각 정답이 바뀌는 경계에 위치하게 되고, 만약 정답이 가장 큰 걸 고르는 거면 `low`를, 뭔가 가장 작은 걸 고르는 거면 `high`를 출력

- binary_search

```python
def binary_search(arr, target):
	low = 0
	high = len(arr) - 1

	while low + 1 < high: 
		mid = (low + high) // 2

		if arr[mid] == target: # 원하는 값 찾은 경우 index 반환
			return mid
		elif arr[mid] > target: # mid의 왼쪽으로 제한
			high = mid
		else # mid의 오른쪽으로 제한
			low = mid

	return low
```

- lower_bound (target보다 크거나 같은 값의 최소 인덱스를 고르는 느낌이니까 low)

```python
def lower_bound(arr, target):
	low = -1
	high = len(arr)

	while low + 1 < high:
		mid = (low + high) // 2
		
		if arr[mid] >= target:
			high = mid
		else:
			low = mid
	
	return low
```

- upper_bound (target보다 큰 값의 최소 인덱스를 고르는 느낌이니까 low)

```python
def upper_bound(arr, target):
	low = -1
	high = len(arr)

	while low + 1 < high:
		mid = (low + high) // 2
		
		if arr[mid] > target:
			high = mid
		else:
			low = mid
	
	return low
```
