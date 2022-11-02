# 퀵 정렬(Quick Sort)

### 퀵 정렬이란?
- 분할 정복 (divide and conquer) 방법을 통해 정렬
```
[분할 정복]
- 문제를 작은 2개의 문제로 분리하고 각각 해결한 다음, 결과를 모아 원래의 문제를 해결하는 방법
``` 
### 특징
- 퀵 정렬은 다른 원소와의 비교만으로 정렬을 수행하는 비교 정렬에 속한다.
- 배열을 비균등하게 분할하며, 불안정 정렬이다.

불안정 정렬: 중복된 값이 입력 순서와 동일하지 않게 정렬되는 알고리즘 


### 정렬 과정
<img src="https://user-images.githubusercontent.com/63101979/199392577-b431c2ba-0cad-49d7-890b-67d51838c41c.gif"  width="400" height="180"/>  

1. 배열에서 하나의 요소를 선택하고 이를 피벗(pivot) 이라고 한다.
2. 배열 내부의 모든 값을 검사하면서 피벗보다 작은 값들은 왼쪽에, 큰 값들은 오른쪽에 배치한다. (분할을 마친 뒤에 피벗은 더 이상 움직이지 않는다.)
3. 분할된 두 개의 작은 배열에 대해 이 과정을 반복한다. 더 이상 배열을 쪼갤 수 없을때까지 진행한다. 
- 피벗을 중심으로 문제를 **'분할'** 하고, 피벗을 기준으로 해서 작은 값과 큰 값을 나열하는 **'정복'** 과정을 거친 뒤, 모든 결과를 결합해서 큰 전체문제를 해결한다. 



### 구현
- 정복
    - 부분 배열을 정렬한다.
    - 부분 배열의 크기가 충분히 작지 않으면 분할 정복 방법을 호출한다.
```python
    def sort(low, high):
        if high <= low:
            return

        mid = partition(low, high)
        sort(low, mid - 1)
        sort(mid, high)
```

- 분할
    - 배열을 피벗 기준으로 비균등하게 2개로 분할한다.
```python
def partition(low, high):
        pivot = arr[(low + high) // 2]
        while low <= high:
            while arr[low] < pivot:
                low += 1
            while arr[high] > pivot:
                high -= 1
            if low <= high:
                arr[low], arr[high] = arr[high], arr[low]
                low, high = low + 1, high - 1
        return low
```

### 시간 복잡도
- 피벗 값을 어떻게 선택하느냐에 따라 달라진다.  

이상적인 경우 : 피벗 기준으로 동일한 개수의 작은 값과 큰 값들로 분할되는 경우 O(nlog(n))  

<img src="https://user-images.githubusercontent.com/63101979/199392815-f07b7a62-982f-4b4d-87b7-c2c4c8b0ff17.png"  width="700" height="300"/>  

최악의 경우 : 배열이 오름차순 또는 내림차순 정렬되어 있는 경우 O(n^2)  

<img src="https://user-images.githubusercontent.com/63101979/199392837-32d4d607-de0a-44bd-9c12-fdcd0aad1ad3.png"  width="500" height="350"/>  

(상용 코드에선 중앙값에 가까운 피벗을 선택할 수 있는 전략이 요구된다. )   
</br>
# 병합 정렬(Merge Sort)

### 병합 정렬이란?
- 분할 정복 방법을 통해 정렬  

### 특징
- 퀵 정렬과는 다르게 항상 O(nlog(n))의 시간 복잡도를 보장한다. 
- 퀵 정렬과는 반대로 안정 정렬에 속한다.
- 추가적인 메모리를 크게 사용한다.  
- 
### 정렬 과정
<img src="https://user-images.githubusercontent.com/63101979/199402824-951e0d86-fe22-48ef-ae20-578af131b1ca.gif"  width="400" height="180"/>  

- 배열의 길이가 1일 될 때까지 배열을 반으로 계속 나눈다.
- 나누어진 배열을 합치면서 정렬을 수행한다.
- 합칠 때는 합칠 두 배열의 길이의 총합만큼의 별도 배열을 준비한 후, 두 배열에서 작은 순서대로 가져와서 배열에 넣어준다.

>[퀵 정렬과의 차이점 ]
>- 퀵 정렬 : 우선 피벗을 통해 정렬 -> 영역을 분할
>- 합병 정렬 : 영역을 최대한 분할 -> 정렬  


### 구현
```python
def mergeSort(alist):
    if len(alist) <= 1:
        return alist
    mid = len(alist) // 2
    leftlist = alist[:mid]
    rightlist = alist[mid:]
    L = mergeSort(leftlist)
    R = mergeSort(rightlist)
    i = j = 0
    result = []
    while i < len(L) and j < len(R):
        if L[i] < R[j]:
            result.append(L[i])
            i+=1
        else:
            result.append(R[j])
            j+=1
    result += L[i:]
    result += R[j:]
    return result
```

### 시간 복잡도
배열을 계속해서 반으로 분리하기 때문에 완전 이진트리의 높이인 log(n)과 비교연산을 통해 새로운 배열을 채우는 과정이 모든 단계마다 수행되기 때문에 O(nlog(n))의 복잡도를 가진다.
