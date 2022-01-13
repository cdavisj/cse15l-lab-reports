# **index.html**

This is some sample content for my webpage. Here is a [link](other.html) to my other page.

## Hobbies:
1. Making coffee
2. Learning programming languages (C++, Python, Java, JS, etc.)
3. Chess
4. Speed cubing

Here is some sample C++ code for fun:
```c++
/*
* File: List.h
* 
* Author: Caleb Davis
* 
* Date: 03-01-21
* 
* Description:
*   Basic template class implementation for a doubly linked list.
* 
*/

#ifndef LIST_H
#define LIST_H

template<typename data_type>
class List
{
public:

    /*
    *   ---------------------------------------------
    *   |         CONSTRUCTORS & DESTRUCTOR         |
    *   ---------------------------------------------
    */

    // default constructor
    List()
        : head(nullptr), curr(nullptr), tail(nullptr)
    {
    }

    // constructor to set size and default data 
    List(int init_size, data_type init_data = data_type())
        : head(nullptr), curr(nullptr), tail(nullptr)
    {
        // push back init data into list init size times
        for (int i = 0; i < init_size; i++)
        {
            push_back(init_data);
        }
    }

    // destructor
    ~List()
    {
        // remove all items from list
        clear();
    }

    /*
    *   ---------------------------------------------
    *   |                 CAPACITY                  |
    *   ---------------------------------------------
    */

    // check if list is empty
    bool empty()
    {
        if (head == nullptr)
        {
            return true;
        }
        else
        {
            return false;
        }
    }

    // number of items in list
    int size()
    {
        if (empty())
        {
            return 0;
        }
        else
        {
            // create counter
            int count = 0;

            // begin curr at head
            curr = head;

            // loop through list incrementing counter
            while (curr != nullptr)
            {
                // increment counter
                count++;

                // go to next item
                curr = curr->next;
            }

            // return counter
            return count;
        }
    }

    /*
    *   ---------------------------------------------
    *   |              ELEMENT ACCESS               |
    *   ---------------------------------------------
    */

    // return reference item at front of list
    data_type& front()
    {
        return head->data;
    }

    // return reference to item at back of list
    data_type& back()
    {
        return tail->data;
    }

    /*
    *   ---------------------------------------------
    *   |                 MODIFIERS                 |
    *   ---------------------------------------------
    */

    // push item into back of list
    void push_back(data_type new_data = data_type())
    {
        // create new node
        curr = new node;

        // store provided data in new node
        curr->data = new_data;

        // make next nullptr, since node will be new tail
        curr->next = nullptr;

        if (empty()) // first node
        {
            // make head and tail point to new node
            head = tail = curr;

            // make prev nullptr
            curr->prev = nullptr;
        }
        else // already one or more nodes
        {
            // link tail to new node
            tail->next = curr;
            curr->prev = tail;

            // move tail forward to new node
            tail = curr;
        }
    }

    // push item into front of list
    void push_front(data_type new_data = data_type())
    {
        // create new node
        curr = new node;

        // store provided data in new node
        curr->data = new_data;

        // make prev nullptr, since node will be new head
        curr->prev = nullptr;

        if (empty()) // first node
        {
            // make head and tail point to new node
            head = tail = curr;

            // make prev nullptr
            curr->next = nullptr;
        }
        else // already one or more nodes
        {
            // link head to new node
            head->prev = curr;
            curr->next = head;

            // move head back to new node
            head = curr;
        }
    }

    // pop item from back of list
    data_type pop_back()
    {
        if (empty())
        {
            // print error message
            std::cout << "Error: Cannot pop_back. List is empty.\n";

            // return empty data_type
            return data_type();
        }
        else if (head == tail) // only one node in list
        {
            // create variable for data to be removed
            data_type removed_data = tail->data;

            // remove node
            delete tail;

            // reset class node pointers to nullptr
            head = curr = tail = nullptr;

            // return value of data removed
            return removed_data;
        }
        else // more than one node in list
        {
            // create variable for data to be removed
            data_type removed_data = tail->data;

            // set curr to point to tail
            curr = tail;

            // move tail back
            tail = tail->prev;

            // make tail next nullptr
            tail->next = nullptr;

            // remove old tail
            delete curr;

            // return value of data removed
            return removed_data;
        }
    }

    // pop item from front of list
    data_type pop_front()
    {
        if (empty())
        {
            // print error message
            std::cout << "Error: Cannot pop_front. List is empty.\n";

            // return empty data_type
            return data_type();
        }
        else if (head == tail) // only one node in list
        {
            // create variable for data to be removed
            data_type removed_data = tail->data;

            // remove node
            delete head;

            // reset class node pointers to nullptr
            head = curr = tail = nullptr;

            // return value of data removed
            return removed_data;
        }
        else // more than one node in list
        {
            // create variable for data to be removed
            data_type removed_data = head->data;

            // set curr to head
            curr = head;

            // move head forward
            head = head->next;

            // make head prev nullptr
            head->prev = nullptr;

            // remove old tail
            delete curr;

            // return value of data removed
            return removed_data;
        }
    }

    // clear the list
    void clear()
    {
        if (empty())
        {
            return;
        }
        else
        {
            // store size in temporary variable
            int old_size = size();

            // loop for length of list
            for (int i = 0; i < old_size; i++)
            {
                // remove node
                pop_back();
            }
        }
    }

    // resize the list with new values (if increasing size)
    void resize(int new_size, data_type new_data = data_type())
    {
        // variable for old size
        int old_size = size();

        // if new size is less than 0, do nothing
        if (new_size < 0)
        {
            // print error message
            std::cout << "Error: Cannot resize list. New size must be non-negative.\n";

            return;
        }
        else if (new_size == 0) // want empty list
        {
            clear();
        }
        else if (new_size > 0) // want non-empty list
        {
            // if new size passed is the same as the current size, do nothing
            if (new_size == old_size)
            {
                return;
            }
            else if (new_size > old_size)
            {
                // loop and push back list for the difference in sizes with default values
                for (int i = 0; i < new_size - old_size; i++)
                {
                    push_back(new_data);
                }
            }
            else // new_size < old_size
            {
                // loop and pop back list for the difference in sizes
                for (int i = 0; i < old_size - new_size; i++)
                {
                    pop_back();
                }
            }
        }
    }

    // reverse the items in the list
    void reverse()
    {
        // store size of list in variable
        int list_size = size();

        // create list for holding reverse
        List<data_type> reverse;

        // to store reverse of current list in reverse list
        for (int i = 0; i < list_size; i++)
        {
            // push back the reverse list with the back of the current list
            reverse.push_back(pop_back());
        }

        // to store reverse list inside current list
        for (int i = 0; i < list_size; i++)
        {
            // push back the current list with the front of the reverse list
            push_back(reverse.pop_front());
        }
    }

    /*
    *   ---------------------------------------------
    *   |                  DISPLAY                  |
    *   ---------------------------------------------
    */

    // print list going forwards
    void print()
    {
        if (empty())
        {
            // print error message
            std::cout << "Error: Cannot print list. List is empty.\n";

            return;
        }
        else // list has items
        {
            // begin curr at head
            curr = head;

            // loop through list
            while (curr != nullptr)
            {
                // display curr data
                std::cout << curr->data << " ";

                // progress curr forwards
                curr = curr->next;
            }

            // go to new line
            std::cout << std::endl;
        }
    }

    // print list going backwards
    void print_reverse()
    {
        if (empty())
        {
            // print error message
            std::cout << "Error: Cannot print list. List is empty.\n";

            return;
        }
        else // list has items
        {
            // begin curr at tail
            curr = tail;

            // loop through list
            while (curr != nullptr)
            {
                // display curr data
                std::cout << curr->data << " ";

                // progress curr backwards
                curr = curr->prev;
            }

            // go to new line
            std::cout << std::endl;
        }
    }

private:
    // struct for nodes
    struct node
    {
        // data for node to hold
        data_type data;

        // pointers to next and previous nodes
        node* next;
        node* prev;
    };

    // node pointers for head, tail, and current nodes
    node* head;
    node* curr;
    node* tail;
};

#endif /* LIST_H */
```