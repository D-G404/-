# leetcode刷题路线  
[2024 4/17 数组 二分查找 移除元素](#二分查找题目)   
[2024 4/18 数组 有序数组的平方(长度最小的子数组) 螺旋矩阵II ](#有序数组的平方题目)   
[2024 4/19 链表 203.移除链表元素 707.设计链表 206.反转链表 ](#移除链表元素题目)   
[2024 4/20 链表 两交换链表中的节点 删除链表的倒数第N个节点 链表相交 环形链表II  ](#两两交换链表题目)   

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




