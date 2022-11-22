Common data types
================

List
-----
List data structure while not as fast for traversal as an array(less cache friendly, requires pointer traversal[slower memory access]), they allow for easy insertion and deletion of objects from a list.


```c
struct list {
	struct list* next;
	void* data;
};
struct node {
	struct node* next;
	struct node* prev;
	void* data;
};
```


List traversal:
```c
// some struct list* list;
for(struct list* l = list; l != NULL; l = l->next) {

}
```
List insertion:
```c
// In the middle/beginning.
struct list* temp = l->next;
l->next = new;
new->next = temp;
// new->prev = l;
// new->next->prev = new;

// At the end
struct list* l = list
for(; l->next != NULL; l = l->next) { }
l->next = new;
```
List removal:
```c
// Made easier by Prev;
struct list* temp = toBeRemoved->prev; // 1. Grab the element just before the one you want to get rid of;

// First element;
if(temp->prev == NULL) {
	l->next->prev = NULL;
	l = l->next;
	toBeRemoved->next = NULL;
	toBeRemoved->data = NULL;
	return toBeRemoved;
	// Removed list element needs to be memory managed 
}

temp->next = temp->next->next;
if(temp->next != NULL) // if last element exists
	temp->next->prev = temp;
toBeRemoved->prev = NULL;
toBeRemoved->next = NULL;
return toBeRemoved;

```

Queue
-----
Is a container for a list, it contains a head and tail denoting the beginning and end of the list;

```c
struct queue {
	struct list* head;
	struct list* tail;
}
```

Bucket List
-----------
Is an array of lists. Once the list hits a certain size, "the bucket overflows". A new bucket(element of the array(list) gets started).

```c

```


Circular buffers
----------------
It's as if you took an array and arranged it into a circle(the tail+1 of the array is the start of the array).
Circular buffers, allows to implement, re-usuable arrays for duplex communication.

Both sides share:
1. A buffer.
2. Their respective read/write pointers.

```c
[0][1][2][3]
 ^     ^
readPointer 
	 writePointer

```