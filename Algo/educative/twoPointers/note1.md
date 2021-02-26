## Introduction



Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.



给定一个排过序的数组和一个 target sum，找到数组中的一堆其和为target sum

两个指针，一个指着数组第一个元素，另一个指着数组末尾元素。

看他们俩的和：

1. 如果这两个数的和大于target sum，这意味着我们需要小一点的数，所以就要减小末端指针
2. 如果这两个数的和小于target sum， 这意味着我们需要大一点的数，增加首端指针。



```c++
class Solution {
public:
    /**
     * 
     * @param numbers int整型vector 
     * @param target int整型 
     * @return int整型vector
     */
    vector<int> twoSum(vector<int>& numbers, int target) {
        // write code here
        vector<int> ans;
        vector<int> temp;
        temp=numbers;
        int size = temp.size();
        sort(temp.begin(), temp.end());
        int left = 0, right = size - 1;
        while (left < right) {
            int curSum = numbers[left] + numbers[right];
            if (curSum == target) {
                break;
            }
            if (target > curSum)
                left++;
            else
                right--;
        }
        if (left<right) {
            for (int k = 0; k<size; k++) {
                if (left<size&&numbers[k]==temp[left]){
                    ans.push_back(k+1);
                    left=size;
                }
                else if (right<size && numbers[k] == temp[right]) {
                    ans.push_back(k+1);
                    right = size;
                }
                if (left==size&&right==size) return ans;
            }
            
        }
        return ans;
    }
};
```

