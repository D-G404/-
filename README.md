# leetcode刷题路线  
[2024 4/17 数组 二分查找 移除元素](#二分查找题目)   
[2024 4/18 数组 有序数组的平方(长度最小的子数组) 螺旋矩阵II ](#有序数组的平方题目)   
[2024 4/19 链表 203.移除链表元素 707.设计链表 206.反转链表 ](#移除链表元素题目)   
[2024 4/20 链表 两交换链表中的节点 删除链表的倒数第N个节点 链表相交 环形链表II  ](#两两交换链表题目)   
[2024 4/22 哈希表 有效的字母异位词 两个数组的交集 快乐数 两数之和  ](#有效的字母异位词题目)  
[2024 4/23 哈希表 四数相加II 赎金信  三数之和 四数之和()](#四数相加II)  
[2024 4/24 字符串 反转字符串 反转字符串II  卡码网：54.替换数字 .翻转字符串里的单词 卡码网：55.右旋转字符串 ](#反转字符串)  
[2024 4/25 字符串 实现strStr KMP() 重复的子字符串 ](#实现strStr)

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
##  长度最小的子数组(最小连续子数组)题目：
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
##  螺旋矩阵：  
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
##  移除链表元素题目
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/1932d49d-1bda-469c-80db-f4cb8c0b7e16)
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        //虚拟头结点 能够解决 全是一个数的情况
        ListNode* newHead = new ListNode(-1,head);
        ListNode* pretemp = newHead;
        ListNode* temp = head;
        while(temp != nullptr){
            if(temp->val == val){
                pretemp->next = temp->next;
                delete temp;
                temp = pretemp->next;
            }else{
                pretemp = pretemp->next;
                temp = temp->next;
            }
        }
        return newHead->next;
    }
};
```
## 设计链表题目：
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/3ad19374-451c-448f-935b-e3374c238291)
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/afb69101-2e92-4b3f-8fca-72918d2e0a8d)
```
struct node{
    int val;
    node* next;
    node():val(0),next(nullptr){}
    node(int val ,node* next = nullptr):val(val),next(next){}
};

class MyLinkedList {
public:
    MyLinkedList() {
        head = new node();
        length = 0;
    }
    
    int get(int index) {
        if (index < 0 || index >= length) 
            return -1;
        node* temp = head->next;
        while(index--){
            temp = temp->next;
        }
        return temp->val;
    }
    
    void addAtHead(int val) {
        node* temp = new node(val,head->next);
        head->next =temp;
        length++;
    }
    
    void addAtTail(int val) {
        node* temp = head;
        while(temp->next != nullptr){
            temp = temp->next;
        }
        temp->next = new node(val);
        length++;
    }
    
    void addAtIndex(int index, int val) {
        if(index < 0|| index > length)
            return;
        node* temp = head;
        while(index--){
            temp = temp->next;
        }
        temp->next = new node(val,temp->next);
        length++;
    }
    
    void deleteAtIndex(int index) {
        if(index < 0|| index >= length)
            return;
        node* temp = head;
        while(index--){
            temp = temp->next;
        }
        node* tempnext = temp->next;
        temp->next = temp->next->next;
        delete tempnext;
        tempnext = nullptr;
        length--;
    }
private:
    node* head;
    int length;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```
## 反转链表题目：
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/3ba697b9-b053-41da-9ca6-d0cbe6e53b5a)
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        //分别对只有一个节点、没有节点、两个节点情况处理
        if(head == nullptr || head->next == nullptr)
            return head;
        if(head->next->next == nullptr){
            ListNode* temp = head->next;
            temp->next = head;
            head->next = nullptr;
            return temp;
        }
        //temp1 2 3 向后一位一位的移动
        ListNode* temp1 = head,*temp2 = head->next,*temp3 = head->next->next;
        temp1->next = nullptr;
        while(temp3 != nullptr){  
            temp2->next = temp1;
            temp1 = temp2;
            temp2 = temp3;
            temp3 = temp3->next;
        }
        temp2->next = temp1;
        return temp2;
    }
};
```
##  两两交换链表题目:
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/01ffb1b0-d4d6-41a6-aa3b-f5a22a9b6c24)
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        //奇怪的写法 加上了头结点，出错概率高
        if(head == nullptr || head->next == nullptr)
            return head;
        ListNode* dummy =new ListNode(-1, head);
        ListNode* temp1 = dummy,*temp2 = head,*temp3 = head->next,*temp4 = head->next->next;
        while(temp4 != nullptr){
            temp1->next = temp3;
            temp3->next = temp2;
            temp1 = temp2;
            temp2 = temp4;
            temp3 = temp4->next;
            if(temp4 && temp4->next == nullptr){
                temp1->next = temp4;
                return dummy->next;
            }          
            else
                temp4 = temp4->next->next;
        }
        temp1->next = temp3;
        temp3->next = temp2;
        temp2->next = nullptr;
        return dummy->next;
    }
};
```
##  删除链表的倒数第N个节点:  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/0db8434d-d296-47fc-9b1b-376e960a876a)
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        //设计技巧：先让快指针走n步 找到删除的前一个节点,头结点更简单
        ListNode* dummy = new ListNode(-1,head);
        ListNode*fast = dummy,*slow = dummy;
        while(n--)
            fast = fast->next;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next;
        }
        // if(fast == nullptr)
        //     return nullptr;
        ListNode* temp = slow->next;
        slow->next = slow->next->next;
        delete temp;
        return dummy->next;
    }
};
```
## 链表相交:  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/d273fc48-c2d1-41e3-aec2-f1e02213ac21)
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        //指针思路和哈希表思路 采用指针思路：要是相交最后的元素相同
        ListNode* temp1 = headA,*temp2 = headB;
        if(temp1 == nullptr || temp2 == nullptr) //注意判断都是空的情况
            return nullptr;
        int len1 = 1 ,len2 = 1;
        while(temp1->next){
            len1++;
            temp1 = temp1->next;
        }
         while(temp2->next){
            len2++;
            temp2 = temp2->next;
        }
        if(temp1 != temp2)
            return NULL;
        temp1 = headA,temp2 = headB;
        if(len1 > len2){
            int dif = len1-len2;
            while(dif--){
                temp1 = temp1->next;
            }
            while(temp1 != temp2){
                temp1 = temp1->next;
                temp2 = temp2->next;
            }
        }else{
            int dif = len2-len1;
            while(dif--){
                temp2 = temp2->next;
            }
            while(temp1 != temp2){
                temp1 = temp1->next;
                temp2 = temp2->next;
            }
        }
        return temp1;

    }
};
```
##  环形链表II  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/7f67b53c-83b2-4d66-8abc-1829ddeffe40)
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        //快慢指针判断环，从起点和相遇点出发再次相遇则是入环点
        ListNode *fast = head,*slow = head;
        while(fast != nullptr && fast->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
            if(fast == slow)
                break;
        }
        if(fast == nullptr || fast->next == nullptr)
            return nullptr;
        slow = head;
        while(fast != slow){
            slow = slow->next;
            fast = fast->next;
        }
        return slow;
    }
};
```
## 有效的字母异位词题目：  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/8d40ec0c-f9e2-4705-a347-1cb31ad4a059)
```
class Solution {
public:
    bool isAnagram(string s, string t) {
        //三次遍历解决
        unordered_map<char,int> umap;
        for(auto num: s){
            umap[num]++;
        }
        for(auto num: t){
            umap[num]--;
        }
        for(auto num:umap){
            std::cout << num.first;
            if(num.second != 0)
                return false;
        }
        return true;
    }
};
```
## 两个数组的交集  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/9839880d-0f37-440c-8efd-8834039043c4)
```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int,int> mp;
        vector<int>res;
        //第一次加入map中，第二次查询重复
        for(auto num: nums1){ //第一次遍历要控制num键值对 对应的值为1，从而保证后面只遍历一次
            if(mp[num] == 0)
                mp[num]++;
        }  
        for(auto num: nums2){
            if(mp[num] == 1){
                 mp[num]++;
                 res.push_back(num);
            }         
        }
        return res;
    }
};
```
## 快乐数  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/f69b14f0-a367-4237-b1d5-f0348ac5f487)
```
class Solution {
public:
    bool isHappy(int n) {
        //第二次进入循环说明会无限循环下去 有没有可能无限增大，不可能
        unordered_map<int,int> umap;
        while(1){
            if(n == 1)
                return true;
            if(umap[n] == 1)
                return false;
            umap[n]++;
            int sum = 0;
            while(n){
                sum += pow(n%10,2);
                n /= 10;
            }
            n =sum;
        }
    }
};
```
## 两数之和  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/864c1686-8fa2-40b8-b4de-8c4f25b67c28)
```
class Solution {
public:
    //两数之和 注意值重复的情形 一遍遍历也可以了 解法复杂一点了
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int,int> umap;
        vector<int> v;
        for(int i=0;i < nums.size();i++){
            umap[nums[i]] = i;
        }
        for(int i =0;i < nums.size();i++){
            if(umap.find(target - nums[i]) != umap.end() ){
                auto it = umap.find(target - nums[i]);
                if(it->second != i){
                    v.push_back(it->second);
                    v.push_back(i);
                    return v;
                }
            }
        }
        return v;
    }
};
```
##  四数相加II
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/2620b7a2-7d75-423f-a2fb-17b6d7ed1375)
```
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        //四数相加需要构造出两对，时间复杂度o（n方）
        int sum = 0;
        unordered_map<int,int>umap1,umap2;
        for(int i=0;i < nums1.size();i++){
            for(int j=0;j < nums2.size();j++){
                umap1[nums1[i]+nums2[j]]++;
            }
        }

        for(int i=0;i < nums3.size();i++){
            for(int j=0;j < nums4.size();j++){
                umap2[nums3[i]+nums4[j]]++;
            }
        }

        for(auto num1: umap1)
            for(auto num2: umap2){
                if(num1.first+num2.first == 0)
                    sum += num1.second*num2.second;
            }
        return sum;
    }
};
```
## 赎金信  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/564b78db-733c-4d4d-bc9a-9d17bc36be32)  
```
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        unordered_map<char,int>umap;
        for(int i=0;i < ransomNote.size();i++){
            umap[ransomNote[i]]++;
        }
        
        for(int i=0;i < magazine.size();i++){
            if(umap.find(magazine[i]) != umap.end())
                umap[magazine[i]]--;
            if(umap[magazine[i]] == 0)
                umap.erase(magazine[i]);
        }   
        
        if(umap.empty())
            return true;
        return false;
            
    }
};
```
## 三数之和
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/73c2c498-c22d-4ef3-a4a6-96462c11333a)  
```
class Solution {
public:
    static bool customCompare(std::vector<int>& a,std::vector<int>& b) {
        // 比较第一个元素
        if (a[0] != b[0]) {
            return a[0] < b[0]; // 按照第一个元素排序
        }
        // 如果第一个元素相同，则比较第二个元素
        if (a[1] != b[1]) {
            return a[1] < b[1]; // 按照第二个元素排序
        }
        // 如果前两个元素都相同，则比较第三个元素
        return a[2] < b[2]; // 按照第三个元素排序
    }
    //除了哈希表也有排序双指针的思路 哈希表去重难搞 下次三数之和再也不用哈希表了。。有几个超时的
    vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> res;
    vector<int>res_push;
    sort(nums.begin(),nums.end());
    unordered_map<int,int>umap;
    for(int i = 0; i < nums.size(); i++){
        umap[-nums[i]] = i;
    }

    for(int i = 0;i < nums.size(); i++)
        for(int j = i+1; j <nums.size(); j++){
            auto it = umap.find(nums[i]+nums[j]);
            if(it != umap.end() && it->second != i && it->second != j && -(it->first)>=nums[j]){
                int tmp1 = nums[i],tmp2 = nums[j],tmp3 = -(it->first);
                res_push.push_back(nums[i]);
                res_push.push_back(nums[j]);
                res_push.push_back(-(it->first));
                res.push_back(res_push);
                res_push.clear();
            }
        }
    //cout << res.size()-1;
    //注意去重不能这样去，vector map都不能这样
    // for(int i = 0;i < res.size()-1;i++){
    //     // if(res[i] == res[i+1])
    //     //     res.erase(res.begin()+i);
    // }
    sort(res.begin(), res.end(), customCompare);
    if(!res.empty()){
        auto it1 = res.begin();
    auto it2 = res.begin()+1;
    while(it2 != res.end()){
       if(*it1 == *it2){
            it2 = res.erase(it2);
       }else{
            it1++;it2++;
       }   
    }
    } 
    return res;
}
};

```
## 四数之和
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/e2ee0ac8-5721-49a3-ae2a-c8da408e4a81)  
```

```
## 反转字符串  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/7b202eeb-5bce-4399-bd36-f8a15b7ceafa)  
```
class Solution {
public:
    void reverseString(vector<char>& s) {
        int len = s.size();
        for(int i = 0; i < s.size()/2; i++){
            char temp = s[i];
            s[i] = s[len-i-1];
            s[len-i-1] = temp;
        }
    }
};
```
## 反转字符串II
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/6a0fa55e-af3d-4362-a390-4ccc92dcac08)  
```
class Solution {
public:
    string reverseStr(string s, int k) {
        //reverse函数以及sort函数都位于algorithm包中，另外此字符串可以原地移动
        //注意substr第二个参数是移动多少位
        string res = "";
        int n = s.length()/k;
        for(int i = 0;i < n;i++){
            if(i % 2 == 0){
                string temp = s.substr(i*k,k);
                reverse(temp.begin(),temp.end());
                res += temp;
            }else{
                string temp = s.substr(i*k,k);
                res += temp;
            }
        }
        if(n%2 == 0){
            string temp = s.substr(n*k,k);
            reverse(temp.begin(),temp.end());
            res += temp;
        }else{
            string temp = s.substr(n*k,s.size());
            res += temp;
        }
        return res;
    }
};
```
## 替换数字  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/fd0365e8-f020-4aac-b25a-c00571684cd4)
```
#include<iostream>
using namespace std;

int main(){
    string s;
    cin>>s;
    for(int i = 0;i < s.size();i++){
        if(s[i]<='9' && s[i]>='0'){
            s.erase(i,1);
            s.insert(i,"number");
        }
    }
    cout<<s;
}
```
## 翻转字符串里的单词  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/dd29dc43-8f4a-4df4-b802-99e5e2105c81)
```
class Solution {
public:
    string reverseWords(string s) {
        //单词根据空格分割 存在vector并翻转 需要注意erase操作就不用+1了 如果+1了还要减回去
        vector<string>v;
        for(int i = 0;i < s.size();i++){
            while(i < s.size() && s[i] == ' '){
                 s.erase(i,1);
            }
            break;
        }
        for(int i = (int)(s.size())-1;i >= 0;i--){
            while(i >= 0 && s[i] == ' '){
                 s.erase(i,1);
            }
            break;
        }
        for(int i = 0;i < s.size();i++){
            if(s[i] == ' ' && s[i+1] == ' '){
                s.erase(i,1);
                i--;
            }            
        }
        stringstream ss(s);
        string temp;
        while(getline(ss,temp,' ')){
            v.push_back(temp);
        }
        s = "";
        reverse(v.begin(),v.end());
        for(int i =0;i < v.size();i++){
            if(i != v.size()-1){
                s += v[i];
                s += " ";
            }else{
                s += v[i];
            }
        }
        return s;
    }
};
```
## 右旋字符串  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/c018fce7-b870-4a21-b840-afd3c883e9a5)  
```
#include<iostream>

int main(){
    int num;
    std::cin >> num;
    std::string str;
    std::cin >> str;
    std::string temp = str.substr(str.size()-num,num);
    str.erase(str.size()-num,num);
    temp += str;
    std::cout << temp;
}
```
## 实现strStr  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/1cb15af7-c630-419e-801b-e9fcc1a8bd18)  
```

```
## 重复的子字符串  
![image](https://github.com/D-G404/leetcode-practice/assets/75080033/4851e7df-5181-43eb-8003-434b0ed5d25b)
```

```



