# Sort

## 버블 정렬
- 서로 인접한 요소 간의 대소 비교를 통해 정렬
- 큰 요소를 swap 으로 뒤로 보내며 정렬
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(n - i - 1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
```
- O(n^2) / stable / in-place
ex)
1. 5, 8, 6, 4, 2, 1
2. 5, 6, 4, 2, 1, 8
3. 5, 4, 2, 1, 6, 8
4. 4, 2, 1, 5, 6, 8
5. 2, 1, 4, 5, 6, 8
6. 1, 2, 4, 5, 6, 8

## 삽입 정렬
- 원소의 인덱스보다 낮은 곳에 있는 원소들을 탐색하여 알맞은 위치에 삽입
- 원소의 인덱스보다 낮은 요소들은 이미 정렬된 상태
```python
def insertion_sort(arr):
    n = len(arr)
    for i in range(1, n):
        for j in range(i, 0, -1):
            if arr[j] < arr[j-1]:
                arr[j], arr[j-1] = arr[j-1], arr[j]
            else:
                break

```
- O(n^2) (이미 정렬이 되어있다면 O(n)) / stable / in-place
ex)
1. 5, 8, 6, 4, 2, 1
2. 5, 8, 6, 4, 2, 1
3. 5, 6, 8, 4, 2, 1
4. 4, 5, 6, 8, 2, 1
5. 2, 4, 5, 6, 8, 1
6. 1, 2, 4, 5, 6, 8
## 선택 정렬
- 원소의 인덱스보다 높은 곳에 있는 원소들을 선택하여 가장 작은 원소를 찾아 swap
- 원소의 인덱스보다 낮은 요소들은 이미 정렬된 상태
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
```
- O(n^2) / stable / in-place
ex)
1. 5, 8, 6, 4, 2, 1
2. 1, 8, 6, 4, 2, 5
3. 1, 2, 6, 4, 8, 5
4. 1, 2, 4, 6, 8, 5
5. 1, 2, 4, 5, 8, 6
6. 1, 2, 4, 5, 6, 8

## 퀵 정렬
- 재귀를 사용해 정렬하는 알고리즘
- pivot 을 기준으로 작은 값은 왼쪽, 큰 값은 오른쪽으로 이동
- pivot 값의 선택에 따라 성능이 달라짐
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left, right, equal = [], [], []
    for i in arr:
        if i < pivot:
            left.append(i)
        elif i > pivot:
            right.append(i)
        else:
            equal.append(i)
    return quick_sort(left) + equal + quick_sort(right)
```
- O(nlogn) / stable / not in place
  파니셔닝 O(n) * 분할 O(logn)
- 최악의 경우: O(n^2)
  계속해서 최대값이나 최소값을 pivot 으로 선택하는 경우 배열의 길이가 1이 될 때까지 n-1번을 분할해야함
ex) 
1. [5, 8, 6, 4, 2, 1]
2. [2, 1] + [4] + [5, 8, 6]
3. [1] + [2] + [4] + [5, 6] + [8]
4. [1] + [2] + [4] + [5] + [6] + [8]
5. [1, 2, 4, 5, 6, 8]

### not stable / in-place 버전
```python
def quick_sort(arr, left, right):
    if left < right:
        pivot_idx = partition(arr, left, right)
        quick_sort(arr, left, pivot_idx - 1)
        quick_sort(arr, pivot_idx + 1, right)

def partition(arr, left, right):
    pivot_idx = left # 피벗의 인덱스를 제일 왼쪽 값으로 설정
    pivot = arr[left]
    
    arr[pivot_idx], arr[right] = arr[right], arr[pivot_idx]
    
    i = left
    for j in range(left, right):
        if pivot > arr[j]:
            arr[i], arr[j] = arr[j], arr[i]
            i += 1
    
    arr[i], arr[right] = arr[right], arr[i]

    return i
```
## 병합 정렬
- 재귀를 사용해 정렬하는 알고리즘
- 원소가 하나만 남을 때까지 계속해서 분할한 다음 대소관계를 고려하여 재배열하여 병합
```python
def merge_sort(arr):
    if len(arr) <= 1:
    	return arr
        
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    merged_array = []
    l, r = 0, 0
    while l < len(left) and r < len(right):
    	if left[l] <= right[r]:
        	merged_array.append(left[l])
        	l += 1
        else:
            merged_array.append(right[r])
        	r += 1
    merged_array += left[l:]
    merged_array += right[r:]
    
    return merged_array
```
- 시간 복잡도: O(nlogn)
ex)
1. [5, 8, 6, 4, 2, 1]
2. [5, 8, 6] [4, 2, 1]
3. [5, 8] [6] [4, 2] [1]
4. [5] [8] [6] [4] [2] [1]
5. [5, 8] [4, 6] [1, 2]
6. [4, 5, 6, 8] [1, 2]
7. [1, 2, 4, 5, 6, 8]
### 퀵 정렬 vs 병합 정렬
퀵 정렬은 캐시 메모리에 자주 접근하여 병합 정렬 보다 성능이 더 좋음(참조 지역성의 원리)

## 힙 정렬
- 트리 기반의 자료구조
- 완전 이진트리를 기반으로 하는 정렬 알고리즘
- 형제 노드들의 대소 관계는 정해져 있지 않음
- 최소 힙: 부모 노드의 값이 자식 노드의 값보다 작거나 같은 완전 이진트리
- 최대 힙: 부모 노드의 값이 자식 노드의 값보다 크거나 같은 완전 이진트리
- 시간 복잡도: O(nlogn)