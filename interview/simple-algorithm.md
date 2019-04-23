# simple algorithm

**单链表倒数第k个:**

```java
public static Integer lastK(Node head, int k){
        if(k<= 0){
            return null;
        }
        Node p = head;
        while(--k>0){
            if(p == null){
                return null;
            }
            p=p.next;
        }

        if(p == null){
            return null;
        }

        Node p2 = head;
        while(p.next != null){
            p = p.next;
            p2 = p2.next;
        }

        return p2.data;
    }
```

**判断单链表是否有环**

```java
  public static boolean circle(Node head){
       Node slow = head, fast = head;

       while (fast != null && fast.next != null){
           slow = slow.next;
           fast = fast.next.next;

           if(slow == fast){
               return true;
           }
       }

       return false;
    }
```

**单向无环链表是否相交**

```java
public boolean intersect(Node headA, Node headB) {
    if (null == headA || null == headB) {
        return false;
    }
    if (headA == headB) {
        return true;
    }
    while (null != headA.next) {
        headA = headA.next;
    }
    while (null != headB.next) {
        headB = headB.next;
    }
    return headA == headB;
}
```

