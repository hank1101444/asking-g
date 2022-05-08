```cpp
#ifndef DEQUE_H
#define DEQUE_H

#include "xutility.h" // include definition of class template ReverseIterator

// CLASS TEMPLATE DequeConstIterator
template< typename MyDeque >
class DequeConstIterator // iterator for nonmutable deque
{
private:
    using size_type = typename MyDeque::size_type;

public:
    using value_type = typename MyDeque::value_type;
    using difference_type = typename MyDeque::difference_type;
    using pointer = typename MyDeque::const_pointer;
    using reference = const value_type&;

    // construct with null pointer
    DequeConstIterator()
        : myOff(0),
        myCont(nullptr)
    {
    }

    // construct with offset off in *pDeque
    DequeConstIterator(size_type off, const MyDeque* pDeque)
        : myOff(off),
        myCont(pDeque)
    {
    }

    DequeConstIterator(const DequeConstIterator& right) // copy constructor
        : myOff(right.myOff),
        myCont(right.myCont)
    {
    }

    ~DequeConstIterator() // destructor
    {
    }

    // const return avoids: ( a1 = a2 ) = a3
    const DequeConstIterator& operator=(const DequeConstIterator& right)
    {
        if (&right != this) // avoid self-assignment
        {
            myCont = right.myCont;
            myOff = right.myOff;
        }
        return *this; // enables x = y = z, for example
    }

    reference operator*() const
    {
        //      size_type block = myOff % ( 4 * myCont->mapSize ) / 4;
        size_type block = myCont->getBlock(myOff);
        size_type off = myOff % 4;
        return myCont->map[block][off];
    }

    pointer operator->() const
    {
        return &(operator*());
    }

    DequeConstIterator& operator++() // preincrement
    {
        ++myOff;
        return *this;
    }

    DequeConstIterator operator++(int) // postincrement
    {
        DequeConstIterator tmp = *this;
        ++* this;
        return tmp;
    }

    DequeConstIterator& operator--() // predecrement
    {
        --myOff;
        return *this;
    }

    DequeConstIterator operator--(int) // postdecrement
    {
        DequeConstIterator tmp = *this;
        --* this;
        return tmp;
    }

    // increment by integer
    DequeConstIterator& operator+=(const difference_type off)
    {
        myOff += off;
        return *this;
    }

    // return this + integer
    DequeConstIterator operator+(const difference_type off) const
    {
        DequeConstIterator tmp = *this;
        return tmp += off;
    }

    // decrement by integer
    DequeConstIterator& operator-=(const difference_type off)
    {
        return *this += -off;
    }

    // return this - integer
    DequeConstIterator operator-(const difference_type off) const
    {
        DequeConstIterator tmp = *this;
        return tmp -= off;
    }

    // return difference of iterators
    difference_type operator-(const DequeConstIterator& right) const
    {
        return (static_cast<difference_type>(this->myOff - right.myOff));
    }

    reference operator[](const difference_type off) const // subscript
    {
        return *(*this + off);
    }

    // test for iterator equality
    bool operator==(const DequeConstIterator& right) const
    {
        return this->myCont == right.myCont && this->myOff == right.myOff;
    }

    // test for iterator inequality
    bool operator!=(const DequeConstIterator& right) const
    {
        return !(*this == right);
    }

    // test if this < right
    bool operator<(const DequeConstIterator& right) const
    {
        return this->myOff < right.myOff;
    }

    // test if this > right
    bool operator>(const DequeConstIterator& right) const
    {
        return right < *this;
    }

    // test if this <= right
    bool operator<=(const DequeConstIterator& right) const
    {
        return !(right < *this);
    }

    // test if this >= right
    bool operator>=(const DequeConstIterator& right) const
    {
        return !(*this < right);
    }

    const MyDeque* myCont; // keep a pointer to deque
    size_type myOff;       // offset of element in deque
};


// CLASS TEMPLATE DequeIterator
template< typename MyDeque >
class DequeIterator : public DequeConstIterator< MyDeque >
{ // iterator for mutable deque
private:
    using size_type = typename MyDeque::size_type;
    using MyBase = DequeConstIterator< MyDeque >;

public:
    using value_type = typename MyDeque::value_type;
    using difference_type = typename MyDeque::difference_type;
    using pointer = typename MyDeque::pointer;
    using reference = value_type&;

    DequeIterator() // construct with null deque pointer
    {
    }

    DequeIterator(size_type off, const MyDeque* pDeque)
        : MyBase(off, pDeque) // construct with offset off in *pDeque
    {
    }

    reference operator*() const // return designated object
    {
        return const_cast<reference>(MyBase::operator*());
    }

    pointer operator->() const // return pointer to class object
    {
        return const_cast<pointer>(MyBase::operator->());
        //      return &( operator*() );
    }

    DequeIterator& operator++() // preincrement
    {
        MyBase::operator++();
        return *this;
    }

    DequeIterator operator++(int) // postincrement
    {
        DequeIterator tmp = *this;
        MyBase::operator++();
        return tmp;
    }

    DequeIterator& operator--() // predecrement
    {
        MyBase::operator--();
        return *this;
    }

    DequeIterator operator--(int) // postdecrement
    {
        DequeIterator tmp = *this;
        MyBase::operator--();
        return tmp;
    }

    // increment by integer
    DequeIterator& operator+=(const difference_type off)
    {
        MyBase::operator+=(off);
        return *this;
    }

    // return this + integer
    DequeIterator operator+(const difference_type off) const
    {
        DequeIterator tmp = *this;
        return tmp += off;
    }

    // decrement by integer
    DequeIterator& operator-=(const difference_type off)
    {
        MyBase::operator-=(off);
        return *this;
    }

    using MyBase::operator-; // return difference of iterators

    // return this - integer
    DequeIterator operator-(const difference_type off) const
    {
        DequeIterator tmp = *this;
        return tmp -= off;
    }

    reference operator[](const difference_type off) const // subscript
    {
        return const_cast<reference>(MyBase::operator[](off));
    }
};

// CLASS TEMPLATE DequeVal
template< typename Ty >
class DequeVal // base class for deque to hold data
{
public:
    using value_type = Ty;
    using size_type = size_t;
    using difference_type = ptrdiff_t;
    using pointer = value_type*;
    using const_pointer = const value_type*;
    using reference = value_type&;
    using const_reference = const value_type&;
    using MapPtr = Ty**;

    DequeVal() // initialize values
        : map(),
        mapSize(0),
        myOff(0),
        mySize(0)
    {
    }

    // determine block from offset
    size_type getBlock(size_type off) const
    {
        return off % (4 * mapSize) / 4;
        //      return ( off / 4 ) & ( mapSize - 1 );
    }

    MapPtr map;        // pointer to array of pointers to blocks
    size_type mapSize; // size of map array, zero or 2^N
    size_type myOff;   // offset of initial element
    size_type mySize;  // current length of sequence
};


// CLASS TEMPLATE deque
template< typename Ty >
class deque // circular queue of pointers to blocks
{
private:
    using MapPtr = Ty**;
    using ScaryVal = DequeVal< Ty >;

public:
    using value_type = Ty;
    using size_type = size_t;
    using difference_type = ptrdiff_t;
    using pointer = value_type*;
    using const_pointer = const value_type*;
    using reference = value_type&;
    using const_reference = const value_type&;

    using iterator = DequeIterator< ScaryVal >;
    using const_iterator = DequeConstIterator< ScaryVal >;

    using reverse_iterator = ReverseIterator< iterator >;
    using const_reverse_iterator = ReverseIterator< const_iterator >;

    // construct empty deque
    deque()
        : myData()
    {
    }

    // fill constructor
    // Constructs a container with "count" elements.
    // Each element is initialized as 0.
    deque(size_type count)
        : myData()
    {
        resize(count);
    }

    // construct by copying right
    deque(const deque& right)
        : myData()
    {
        if (right.myData.mySize > (right.myData.mapSize - 1) * 4) {
            myData.map = new value_type * [2 * right.myData.mapSize]();
            myData.mapSize = right.myData.mapSize * 2;
        }
        else {
            //!=0 時才能開空間
            // 如果right.mydata.mapsize == 0 剛好 tmp 就是0
            int tmp = 0;
            if (right.myData.mapSize != 0) {
                tmp = right.myData.mySize / 4;
                if (right.myData.mySize % 4 != 0)
                    tmp++;
                int cal = 8;
                //加等號是因為 (mysize = 127, tmp = 32)有用到第32 行的空間 所以擴大時要兩倍
                while (tmp >= cal)
                    cal *= 2;

                tmp = cal;
                myData.map = new value_type * [tmp]();
            }
            myData.mapSize = tmp;
        }
        //size_type tmp_rightCol = right.myData.mySize;


        myData.mySize = right.myData.mySize;
        //myoff 表最前面
        myData.myOff = 0;
        int tmp = right.myData.mySize / 4;
        if (right.myData.mySize % 4 != 0)
            tmp++;
        // 開空間
        for (int i = 0; i < tmp; i++) {
            myData.map[i] = new value_type[4]();
        }

        /*
         for (int i = 0; i < right.myData.mySize; i++) {
             int row = ((right.myData.myOff + i) / 4) % right.myData.mapSize;
             int col = (right.myData.myOff + i) % 4;
             myData.map[i / 4][i % 4] = right.myData.map[row][col];
         }*/
         // 改iterator 寫法
        const_iterator it1 = right.begin();
        iterator it2 = begin();
        for (; it1 != right.end(); it1++, it2++)
            *it2 = *it1;

        //size_type tmp_right_off = right.myData.myOff;

        /*bool flag = true;
        int count = 0;
        for (int i = 0; i < tmp; i++) {
            for (int j = 0; j < 4; j++) {
                size_type row = (tmp_right_off / 4);
                size_type col = tmp_right_off % 4;
                tmp_right_off++;
                count++;
                myData.map[i][j] = right.myData.map[row][col];
                if (count == right.myData.mySize) {
                    flag = false;
                    break;
                }
            }
            if (!flag)
                break;
        }*/


    }

    // destroy the deque
    ~deque()
    {
        clear();
    }

    // Assigns new contents to the container, replacing its current contents,
    // and modifying its size accordingly.
    deque& operator=(const deque& right)
    {
        if (&right != this) // avoid self-assignment
        {
            if (right.size() == 0) {
                myData.mySize = 0;
                myData.myOff = 0;
                return *this;
            }
            myData.mySize = (size() < right.size() ? size() : right.size());
            iterator it1(myData.myOff, &myData);
            //iterator 第二參數為const 所以能得到 right.myData的&
            iterator it2(right.myData.myOff, &right.myData);
            for (int i = 0; i < myData.mySize; i++,++it1,++it2) {
                *it1 = *it2;
            }
            /*for (int i = myData.mySize; i < right.myData.mySize; ++i,++it2) {
                push_back(*it2);
            }*/
            //it2在前面的迴圈中已經調到下一個位置了
            while (myData.mySize != right.size()) {
                push_back(*it2);
                ++it2;
            }
        }
        return *this; // enables x = y = z, for example
    } // end function operator=

    // return iterator for beginning of mutable sequence
    iterator begin()
    {
        return iterator(myData.myOff, &myData);
    }

    // return iterator for beginning of nonmutable sequence
    const_iterator begin() const
    {
        return const_iterator(myData.myOff, &myData);
    }

    // return iterator for end of mutable sequence
    iterator end()
    {
        return iterator(myData.myOff + myData.mySize, &myData);
    }

    // return iterator for end of nonmutable sequence
    const_iterator end() const
    {
        return const_iterator(myData.myOff + myData.mySize, &myData);
    }

    // return iterator for beginning of reversed mutable sequence
    reverse_iterator rbegin()
    {
        return reverse_iterator(end());
    }

    // return iterator for beginning of reversed nonmutable sequence
    const_reverse_iterator rbegin() const
    {
        return const_reverse_iterator(end());
    }

    // return iterator for end of reversed mutable sequence
    reverse_iterator rend()
    {
        return reverse_iterator(begin());
    }

    // return iterator for end of reversed nonmutable sequence
    const_reverse_iterator rend() const
    {
        return const_reverse_iterator(begin());
    }

    // return iterator for beginning of nonmutable sequence
    const_iterator cbegin() const
    {
        return begin();
    }

    // return iterator for end of nonmutable sequence
    const_iterator cend() const
    {
        return end();
    }

    // return iterator for beginning of reversed nonmutable sequence
    const_reverse_iterator crbegin() const
    {
        return rbegin();
    }

    // return iterator for end of reversed nonmutable sequence
    const_reverse_iterator crend() const
    {
        return rend();
    }

    void resize(size_type newSize)
    {
        while (myData.mySize < newSize)
            push_back(value_type());

        while (newSize < myData.mySize)
            pop_back();
    }

    // return length of sequence
    size_type size() const
    {
        return myData.mySize;
    }

    // test if sequence is empty
    bool empty() const
    {
        return myData.mySize == 0;
    }

    const_reference operator[](size_type pos) const
    {
        return *(begin() + static_cast<difference_type>(pos));
    }

    reference operator[](size_type pos)
    {
        return *(begin() + static_cast<difference_type>(pos));
    }

    reference front()
    {
        return *begin();
    }

    const_reference front() const
    {
        return *begin();
    }

    reference back()
    {
        return *(end() - 1);
    }

    const_reference back() const
    {
        return *(end() - 1);
    }

    // insert element at beginning
    void push_front(const value_type& val)
    {
        if (myData.mySize == 0)
        {
            if (!myData.map) {
                myData.mapSize = 8;
                myData.map = new  value_type * [8]();
                myData.map[myData.mapSize - 1] = new value_type[4]();
            }
            else {
                // 最後面創空間
                if (myData.map[myData.mapSize - 1] == nullptr) {
                    myData.map[myData.mapSize - 1] = new value_type[4]();
                }
            }
            myData.mySize++;
            myData.myOff = 4 * myData.mapSize - 1;
            myData.map[myData.mapSize - 1][3] = val;
        }

        else
        {
            // 假設myoff 是 0 (-1%32 = 31)
            size_type newFront = (myData.myOff - 1) % (4 * myData.mapSize);
            if (newFront % 4 == 3 && myData.mySize >= 4 * (myData.mapSize - 1))
            {
                doubleMapSize();
                newFront = (myData.myOff - 1) % (4 * myData.mapSize);
            }

            myData.myOff = newFront;
            int row = (myData.myOff / 4);
            int col = myData.myOff % 4;
            //已經減過 所以判斷是不是最後一位
            //if (myData.myOff % 4 == 3) {
            if (myData.map[row] == nullptr) {
                myData.map[row] = new value_type[4]();
            }
            //}
            myData.mySize++;
            myData.map[row][col] = val;
        }
    }

    // erase element at beginning
    void pop_front()
    {
        size_type front = myData.myOff % (4 * myData.mapSize);
        if (front % 4 == 3)
        {
            size_type back = (myData.myOff + myData.mySize - 1) % (4 * myData.mapSize);
            if (back >= front - 3 && back < front)
            {
                delete[] myData.map[(front - 3) / 4];
                myData.map[(front - 3) / 4] = nullptr;
            }
        }

        --myData.mySize;
        if (myData.mySize == 0)
            myData.myOff = 0;
        else
            ++myData.myOff;
    }

    // insert element at end
    void push_back(const value_type& val)
    {
        if (myData.mySize == 0)
        {
            if (!myData.map) {
                myData.mapSize = 8;
                myData.map = new value_type * [8]();
                myData.map[0] = new value_type[4]();
            }
            else {
                if (myData.map[0] == nullptr)
                    myData.map[0] = new value_type[4]();
            }


            myData.mySize++;
            myData.map[0][0] = val;
        }
        else
        {

            //myoff 加上 mysize剛好為要push 的位置
            size_type newBack = (myData.myOff + myData.mySize) % (4 * myData.mapSize);
            if (newBack % 4 == 0 && myData.mySize >= 4 * (myData.mapSize - 1))
            {
                doubleMapSize();
                newBack = myData.myOff + myData.mySize;
            }
            /*else {
            }*/
            int row = newBack / 4;
            int col = newBack % 4;
            //if (col == 0) {
            if (myData.map[row] == nullptr) {
                myData.map[row] = new value_type[4]();
            }
            //}
            myData.mySize++;
            myData.map[row][col] = val;
            // suppose: 一個push back 一個push front pop_front三次 (myoff : 31->34 )
            // popfront沒先circular
            // push_front會調整到circular pushback 沒調所以要加上
            // 變成說實際位置只有在push 時才調整
            // 可加在後面 不影響答案
            myData.myOff %= (4 * myData.mapSize);
        }

    }

    // erase element at end
    void pop_back()
    {
        size_type back = (myData.myOff + myData.mySize - 1) % (4 * myData.mapSize);
        if (back % 4 == 0)
        {
            size_type front = myData.myOff % (4 * myData.mapSize);
            if (front > back && front <= back + 3)
            {
                delete[] myData.map[back / 4];
                myData.map[back / 4] = nullptr;
            }
        }

        --myData.mySize;
        if (myData.mySize == 0)
            myData.myOff = 0;
    }

    // erase all
    void clear()
    {
        if (myData.mapSize > 0)
        {
            for (size_type i = 0; i < myData.mapSize; i++)
                if (myData.map[i] != nullptr)
                    delete[] myData.map[i];
            delete[] myData.map;

            myData.mapSize = 0;
            myData.mySize = 0;
            myData.myOff = 0;
            myData.map = MapPtr();
        }
    }

private:

    // determine block from offset
    size_type getBlock(size_type off) const
    {
        return myData.getBlock(off);
        //      return ( ( myData.off / 4 ) & ( myData.mapSize - 1 ) );
    }

    void doubleMapSize()
    {
        size_type old_row = myData.myOff / 4;
        size_type oldSize = myData.mySize;
        size_type old_col = myData.myOff % 4;
        size_type oldMapSize = myData.mapSize;
        size_type newMapSize = myData.mapSize * 2;
        MapPtr tmp = new value_type * [2 * myData.mapSize]();
        int k = old_row;
        for (int i = old_row; i < myData.mapSize; i++, k++) {
            tmp[k] = myData.map[i];
        }
        for (int i = 0; i < old_row; i++, k++) {
            tmp[k] = myData.map[i];
        }
        delete[]myData.map;
        myData.map = tmp;
        myData.mapSize = newMapSize;



    }

    ScaryVal myData;
};

#endif
```
