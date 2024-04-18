刷题路线  
[2024 4/17 数组 二分查找 移除元素](#二分查找题目)   
[2024 4/18 数组 有序数组的平方(长度最小的子数组) 螺旋矩阵II ](#有序数组的平方题目) 

## 二分查找题目：  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/4352bb83-116f-4203-912b-c6862e31fc16)
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
/*总结：start <= end，不能是start < end*/
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
/*总结：边统计数目边移动*/
};
```

##  有序数组的平方题目  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/6cc9a3c8-94e6-4c6c-b95d-22ff47a9943d)
```
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        //找出中间数 双指针比较
        int len = nums.size(),min = 0;
        for(int i = 0;i < len;i++){
            //绝对值比较大小
            if(abs(nums[i]) < abs(nums[min])){
                min = i;
            }
        }
        //并不是中心扩展，哪边小往哪边走
        int left = min-1,right = min+1;
        vector<int>v;
        v.push_back(pow(nums[min],2));
        while(left >= 0 && right < len){
            if(abs(nums[left]) < abs(nums[right])){
                v.push_back(pow(nums[left],2));
                left -= 1;
            }else{
                v.push_back(pow(nums[right],2));
                right += 1;
            }
        }
        if(left < 0){
            for(int i = right;i < len;i++){
                v.push_back(pow(nums[i],2));
            }
        }
        if(right >= len){
            for(int i = left;i >= 0;i--){
                v.push_back(pow(nums[i],2));
            }
        }
        return v;
    //总结：可以使用双指针两端平方递减的方式，避免找最小的元素，最后reverse一下就行
    }
};
```
长度最小的子数组(最小连续子数组)题目：
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/1ea33a2d-79cc-4e90-bbbc-19da7615f799)
```
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        //双指针调试，思路和代码比较复杂，原因是right在中间位置和末尾位置处理不一致
        //下次换个方式写
        int left = 0,right = 0,len = INT_MAX,sum = 0,sign=0;//设置了一个标志位表示到达最后一个元素
        while(left < nums.size()){
            if(sign == 1 && sum < target){
                if(len == INT_MAX)
                    return 0;
                return len;
            }
                
            if(sum < target){
                sum += nums[right++];
                if(right == nums.size()){
                    right -= 1;
                    sign = 1;
                }      
            }
            else{
                sum -= nums[left++];
            }
            if(sum >= target){
                if(sign == 1 ){
                    if(right-left+1 < len)
                        len = right-left+1;
                    continue;
                }
                if(right - left < len)
                    len = right - left;
            }        
            
        }
        return len;
    }
};
```
螺旋矩阵：  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/cad5383a-94b3-4f75-998c-c492460c98ba)
```
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> v(n,vector<int>(n));
        int i = 0,j = 0,sum =1;
        int upleft = 0,upright = n-1,downleft = 0,downright = n-1;
        v[0][0] = sum++; //先对第一个元素处理
        while(sum <= n*n){
            while(j < upright){ //i j分别代表横纵坐标
                v[i][++j] = sum++;
                if(sum > n*n) //注意是> ,不是>=
                    return v;
            }
            if(j > upright)
                j--;
            while(i < downright){
                v[++i][j] = sum++;
                if(sum > n*n)
                    return v;
            }
            if(i > downright)
                i--;
            while(j > downleft){
                v[i][--j] = sum++; 
                if(sum > n*n)
                    return v;
            }
            if(j < downleft)
                j++;
            while(i > downleft+1){
                v[--i][j] = sum++;
                if(sum > n*n)
                    return v;
            }
            if(i < downleft+1) //注意最后一个元素处理
                i++;
            upleft += 1; upright -= 1; downleft += 1; downright -= 1;
        }
        return v;
    }
};
```


