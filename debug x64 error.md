```cpp
    // Searches the unordered_set for an element with keyVal as value and
    // returns an iterator to it if found, otherwise it returns myList.end()
    iterator find(const key_type& keyVal)
    {
        size_type bn = bucket(keyVal);
        //iterator _head = *(myVec.myData.myFirst + 2 * bn);
        iterator _head = myVec.myData.myFirst[2 * bn];
        iterator _tail = myVec.myData.myFirst[2 * bn + 1];
        // _head 一開始可能為mylist.end();
        // debug x64 如果不加會爆
        if (_head != myList.end()) {
            for (; _head != _tail; ++_head) {
                if (*_head == keyVal)
                    return _head;
            }
            // 再check 一次 就能測_tail
            if (*_tail == keyVal)
                return _tail;
        }
        return myList.end();
    }
```
