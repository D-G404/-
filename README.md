刷题路线  
2024/17 数组 二分查找 移除元素

二分查找题目：![image](https://github.com/D-G404/leetcode-practice/assets/75080033/4352bb83-116f-4203-912b-c6862e31fc16)
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int start = 0,end = nums.size()-1;
        int mid = (start+end)/2;
        while(start <= end){
            if(nums[mid] == target)
                return mid;
            else if(nums[mid] > target){
                end = mid-1;
            }else{
                start = mid+1;
            }
            mid = (start+end)/2;
        }
        return -1;
    }
};
```
移除元素题目：![image](https://github.com/D-G404/leetcode-practice/assets/75080033/e170bbfe-6884-4d5f-864e-2882fce3bf38)
```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int len = 0;
        //统计val数目
        for(int i = 0;i < nums.size();i++){
            nums[i-len] = nums[i];
            if(nums[i] == val){
                len++;
            }
        }
        return nums.size()-len;
    }
};
```


