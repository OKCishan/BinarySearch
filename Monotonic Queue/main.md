```
for (i = 0; i < ARR.size(); i++) {
  remove ARR[i-K] from the sliding window
  insert ARR[i] into the sliding window
  output the smallest value in the window
}
```

The sliding window algorithm does the ***remove, insert and output step in amortized constant time.*** <br />
Or rather the time it takes to run the algorithm is ``` O(ARR.size()) ```. <br />

Ref: ```https://people.cs.uct.ac.za/~ksmith/articles/sliding_window_minimum.html```

By putting in a bit more effort when inserting an element into the window we can get amortized O(1) run-time. <br />
Say our window contains the elements {1, 6, 7, 2, 4, 2}. We want to add the element 5 to our window. 
Notice that all elements in the window greater than 5 will now never be the minimum in the window for future i values, so we might as well get rid of them. <br />
The trick to this is to store the numbers in a deque [1] and whenever inserting a number x we remove all numbers at the back of the deque which are greater than equal to x. <br />
Notice that if the deque was sorted before inserting, it will still be sorted after inserting x. Since the deque starts off sorted, it remains sorted throughout the sliding window algorithm. <br />
So the front of the deque will always be the smallest value. <br />

The front of the queue might have a value which shouldn't be in the window anymore, but we can use the lazy delete idea with the deques as well. <br />
Now each element of ARR is inserted into the deque and deleted from the deque at most once. <br />
This gives as a total run-time of O(N) for the algorithm (amortized O(1) per insertion/deletion). Pretty sweet: <br />

```
void sliding_window_minimum(std::vector<int> & ARR, int K) {
  // pair<int, int> represents the pair (ARR[i], i)
  std::deque< std::pair<int, int> > window;
  for (int i = 0; i < ARR.size(); i++) {
     while (!window.empty() && window.back().first >= ARR[i])
       window.pop_back();
     window.push_back(std::make_pair(ARR[i], i));

     while(window.front().second <= i - K)
       window.pop_front();

     std::cout << (window.front().first) << ' ';
  }
}
```

***Sliding Window maximum:*** <br />
```
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        deque<pair<int, int>> window;
        for(int i=0; i<nums.size(); i++) {
            // Carefully inserting into window:
            // - remove all elements lower than nums[i] from back since they cannot be maximum going forward
            while(!window.empty() and window.back().first < nums[i]) {
                window.pop_back();
            }
            
            window.push_back(make_pair(nums[i], i));
            
            while(window.front().second <= (i-k)) window.pop_front();
            
            // since window has been decreasing continously, so maxm is at front
            if(i >= k-1) ans.push_back(window.front().first);
        }
        
        return ans;
    }
```
