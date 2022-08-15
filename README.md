# Playing-With-List-Class
<h3>Few C++ functions that make use of list class</h3>


SortedList class that extends the List class and provide few changes including:
<br><b>Override iterator "insert( iterator itr, const Object & x )"</b> that inserts x before
position itr. Check that x is larger than the previous item and less or equal to the next
item. If not, the function should throw an exception SortedOrderMismatchException. The
function returns the position of the inserted item.
<br><b>Add iterator "insert( const Object & x )"</b> that inserts x at the appropriate position in the
list i.e., keeping it sorted. The function returns the position of the inserted item.
<br><b>Add iterator "erase( const Object & x )"</b> that removes x from the list, keeping it sorted.
The function returns the position of the next item, if there is one, or the list end-marker.
<br><b>Friend function "sorted_list<Object>* intersection( const sorted_list<Object>& list1,
const sorted_list<Object> & list2)", that computes the intersection of two given sorted 
lists.
<br><b>Friend function "sorted_list<Object>* Union( const sorted_list<Object>& list1, const sorted_list<Object> & list2)", that computes the union of two given sorted lists.
```
#pragma once
#include "List.h"

class SortedOrderMismatchException { };

template <typename Object>
class SortedClass : public List<Object> {
    public:
        List<Object>::iterator insert(const Object& x) {
            auto itr = List<Object>::begin();
            for (; itr != List<Object>::end() && *itr<x; itr++) {}
            return List<Object>::insert(itr, x);
        }
        List<Object>::iterator erase(const Object& x) {
            for (auto itr = List<Object>::begin(); itr != List<Object>::end(); itr++) {
                if (*itr == x) {
                    itr = List<Object>::erase(itr);
                    return itr;
                }
            }
            cout << "object not found";
            return List<Object>::begin();
        }
        List<Object>::iterator insert(List<Object>::iterator itr, const Object& x) {
            if (itr == List<Object>::begin() && *itr >= x) {
                itr = List<Object>::insert(itr, x);
                return itr;
            }
            else if (*itr >= x && *(--itr) <= x) {
                itr = List<Object>::insert(++itr, x);
                return itr;
            }
            throw SortedOrderMismatchException{};
        }
        friend SortedClass<Object>* intersection(const SortedClass<Object>& list1, const SortedClass<Object>& list2) {
            SortedClass<Object>* result = new SortedClass<Object>;
            for (auto itr1 = list1.begin(); itr1 != list1.end(); itr1++) {
                for (auto itr2 = list2.begin(); itr2 != list2.end(); itr2++) {
                    if ((*itr2) == (*itr1)) 
                        result->insert(*itr2);
                }
            }
            return result;
        }
        friend SortedClass<Object>* Union(const SortedClass<Object>& list1, const SortedClass<Object>& list2) {
            SortedClass<Object>* result = new SortedClass<Object>;
            for (auto itr1 = list1.begin(); itr1 != list1.end();itr1++) 
                result->insert(*itr1);
            for (auto itr2 = list2.begin(); itr2 != list2.end(); itr2++) 
                result->insert(*itr2);

            return result;
        }
};
```
  
Function that returns true if the given value is in the container, and false if not. It uses iterators so it works with any type of container and data that is compatible with iterators.
```
template< class Container, typename Value >
bool member(const Container& cont, const Value& val) {
    for (auto itr = cont.begin(); itr != cont.end(); itr++)
        if (val == *itr) return true;
}
```

