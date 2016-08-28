'
' Since the Basic4GL language does not support generic typing, separate structures and functions had to be created for each supported basic data type.
'
' Basic data types supported by Basic4GL:
'   "Integer". A 32 bit signed integer.
'   "Real". A 32 bit floating point value.
'   "String". A character string
'    
' Real and String variable names are post-fixed with '#' and '$', respectively; 
' Integer variable names may be post-fixed with '%', but it is not required by the compiler
'
' Lists for each basic type are separated into the following include files:
' Integer lists
include list.gb
' Real lists
include rlist.gb
' String lists
include slist.gb
' Note: Basic4GL does NOT support deallocating pointers until a program completes. Even if a value is removed from a list, the memory will remain allocated for the node
' A SetValueAt function has been provided for each list type if you would like to replace a value at an existing node without allocating a new pointer;
' SetValueAt will insert a new node at the beginning or end of a list if the index argument is out of range
'
' To create a list for a struc you defined:
' 1. Copy the contents of list.gb to a new file and Save As "mystruclist.gb"
' 2. Update the documentation in header of the file :)
' 3. Find & Replace every instance of "List" with "MyStrucList" 
' 4. Change type of variables named "value" to MyStruc pointers
'    eg:    "MyStruc &value", "dim MyStruc &value"
' 5. Set the return type for the "MyStrucListValueAt" function to a MyStruc pointer
'    eg:    "declare function MyStruc &MyStrucListValueAt"                         
' 6. Remove "const LIST_DEFAULT_VALUE = 0", and replace any other usage of "LIST_DEFAULT_VALUE" in mystruclist.gb with null
' optional: Change implementation of MyStrucListSetValueAt, MyStrucListNodeEquals and MyStrucListRemove functions based on the contents of your struc
'
' Your list strucs and declarations should look similar to the ones below:

'MyStrucList node, is doubly linked
struc MyStrucListNode
    dim MyStruc &value
    dim MyStrucListNode &prevNode, MyStrucListNode &nextNode
endstruc

'String list, is doubly linked
struc MyStrucList
    'First element in list
    dim MyStrucListNode &head
    'Last element in list
    dim MyStrucListNode &tail
endstruc   

'Function declarations
declare sub MyStrucListInsertBeginning(MyStrucList &list, MyStruc &value)
declare sub MyStrucListInsertNodeBeginning(MyStrucList &list, MyStrucListNode &newNode)
declare sub MyStrucListInsertEnd(MyStrucList &list, MyStruc &value)
declare sub MyStrucListInsertNodeEnd(MyStrucList &list, MyStrucListNode &newNode)
declare sub MyStrucListInsertAt(MyStrucList &list, index, MyStruc &value)
declare sub MyStrucListInsertNodeAt(MyStrucList &list, index, MyStrucListNode &newNode)
declare sub MyStrucListSetValueAt(MyStrucList &list, index, MyStruc &value)

declare sub MyStrucListInsertAfter(MyStrucList &list, MyStrucListNode &node, MyStrucListNode &newNode)
declare sub MyStrucListInsertBefore(MyStrucList &list, MyStrucListNode &node, MyStrucListNode &newNode)

declare sub MyStrucListRemove(MyStrucList &list, MyStrucListNode &node)
declare sub MyStrucListRemoveAt(MyStrucList &list, index)
declare sub MyStrucListRemoveFirst(MyStrucList &list, MyStruc &value)
declare sub MyStrucListRemoveLast(MyStrucList &list, MyStruc &value)

declare function MyStruc &MyStrucListValueAt(MyStrucList &list, index)
declare function MyStrucListNode &MyStrucListNodeAt(MyStrucList &list, index)
declare function MyStrucListNodeEquals(MyStrucListNode &node1, MyStrucListNode &node2)

declare function MyStrucListSize(MyStrucList &list)