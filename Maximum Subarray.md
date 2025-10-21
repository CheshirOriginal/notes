https://leetcode.com/problems/maximum-subarray/description/
### Условие

Дан массив целых чисел `nums`.  
Найди **наибольшую сумму непрерывного подмассива** (подмассива, состоящего из последовательных элементов).

Верни это значение.
### Пример
**Example 1:**
**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6

**Example 2:**

**Input:** nums = [1]
**Output:** 1
### Решение

Лучшее решение — **Алгоритм Кадане**

```cpp
//Идём по массиву и накапливаем текущую сумму. 
//Если в какой-то момент текущая сумма становится отрицательной — сбрасываем её,  
//так как отрицательная сумма только ухудшит все следующие подмассивы.

class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int curMax = 0, max = INT_MIN;
        for (const auto& num : nums) {
            curMax = std::max(curMax + num, num);
            max = std::max(curMax, max);
        }
        return max;
    }
};
```

Решение через разделяй и властвуй

```cpp
class Solution {
public:
    vector<int> pre, suf;
    int maxSubArray(vector<int>& nums) {
        pre = suf = nums;
        for(int i = 1; i < size(nums); i++)  pre[i] += max(0, pre[i-1]);
        for(int i = size(nums)-2; ~i; i--)   suf[i] += max(0, suf[i+1]);
        return maxSubArray(nums, 0, size(nums)-1);
    }
    int maxSubArray(vector<int>& A, int L, int R){
        if(L == R) return A[L];
        int mid = (L + R) / 2;
        return max({ maxSubArray(A, L, mid), maxSubArray(A, mid+1, R), pre[mid] + suf[mid+1] });
    }	
};
```
