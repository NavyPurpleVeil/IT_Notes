IO Schedulers
---

IO Schedulers re-order and keep track of read/write requests to block devices;
Even though queue sizes have increased and the storage devices themself do re-ordering, IO Schedulers still prove effective at reducing perceived user latency and general throughput.

Single queue - Dispatches requests to a single hardware queue(one device one queue);
Multi queue - Dispatches requests to multiple hardware queues(one device multiple queues);

Linux supports having either Single queue or Multi queue schemes active globally; 

---

Single queue
---

**CFQ** - Reorders requests (:O)
**Deadline** - Keeps track of a FIFO queue which contain the deadline of the operation to complete etc., and a 2nd queue for Reads and Writes which is sorted and batched into per sector sequential requests; Roughly speaking other than exceptions, it prioritizes Expired Deadline over Reads over Writes. It will try to merge the requests into sequential blocks, as well as execute expired deadlines sequentially.
**noop** - FIFO queue, First in First Out, doesn't do any re-ordering ever;

---

Multi queue
---
**BFQ** - Using the low latency switch, don't see a reason to use anything but for desktop use.

---