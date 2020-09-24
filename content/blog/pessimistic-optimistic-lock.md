---
title: Pessimistic & Optimistic DB Lock
date: 2020-09-18T19:20:00.000Z
tags: database locks pessimistic-lock optimistic-lock concurrency transaction
---
**Why do we need locking in database in the first place ?**

Ans: To manage write operations in DB in case of concurrent transaction.

![](/assets/screenshot-2020-09-19-at-12.47.50-am.png)

Marked the **quantity -1** in the above diagram ?

The basic problem that would be caused if we don't have proper locking mechanism in the DB.
*Umm.. What is the solution then ?*

There comes the title of the article. Implementing either one of pessimistic lock or optimistic lock.

Again what are these locks.
*Don't worry. Things gets better with time. Please continue reading...*

### **<u>Definition</u>**

**1. Optimistic Locking** : The two simultaneous transactions are allowed until one of them (the last one in case of two) fails to modify (update) the row because it contains old data (dirty data/ stale data).

**2. Pessimistic Locking** : One of the simultaneous transactions is allowed to continue the operation (modifying the row of the table) and the second one is kept blocked, executed only after the first completes the transaction (success or failure).

<br>

### <u>Basic Example</u>

Consider two people trying to book a window seat in the Indigo Airlines.

***Pessimistic Locking***

1. As soon as one of the transaction start, other transaction is put in the wait queue.
2. The second transaction is blocked until the first transaction completes or fails.

***Optimistic Locking***

1. Allows both the users to get inside the purchase flow (both transactions are allowed)
2. The service lets them (transactions) to acquire the desired seat.
3. Then they have to proceed for payment for the transaction to be complete. 
4. Before payment (usually done at multiple places), a property of the row is evaluated to check for staleness (version in JPA/Hibernate, timestamp or others in some cases as well). 
5. In other words, the seat availability check is done again (by comparing with the old saved data).
6. If the seat is available (if the data matches with the old data before the operation), it is proceeded for payment and committed to the DB. Makes the other user fail at one of a similar check points.
7. Else the transaction fails

*Still didn't get. Don't worry.*
Consider another case of a ecommerce purchase with the following diagrams.

<br>

### <u>Explanation with Sequence Diagram</u>

***Pessmistic Locking***

![](/assets/screenshot-2020-09-19-at-12.48.26-am.png)

***Optimistic Locking***

![](/assets/screenshot-2020-09-19-at-12.49.24-am.png)

<br>

### <u>When to use what ?</u>

Optimistic locking is preferred when there isn't much chance of concurrent updations (the act of multiple transaction updating the same row in a table). The cost is for the resolution of the aborted transaction.

Pessimistic locking is to be used when there are higher chances of concurrent updations. This way transaction are blocked

- - -
<br>

*If you have come till here. Thank you for reading completely.*

*I hope that this could help you learn the concept clearly.*