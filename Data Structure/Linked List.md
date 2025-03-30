```java
public class Node {  
    int data;  
    Node next;  
    Node(int data){  
        this.data=data;  
        this.next=null;  
    }  
}
public class LinkedList {  
    private Node head;  
    private int size = 0;  
    public void add(int data){  
        size++;  
        Node neww = new Node(data);  
        if(head==null){  
            head=neww;  
        }  
        else{  
            Node current = head;  
            while(current.next!=null)current=current.next;  
            current.next=neww;  
        }  
  
    }  
    public void insert(int data, int pos) throws Exception {  
        size++;  
        Node newNode = new Node(data);  
        if (pos == 0) {  
            newNode.next = head;  
            head = newNode;  
        } else {  
            int count = 0;  
            Node temp = head;  
            while (count != pos - 1 && temp != null && temp.next != null) {  
                temp = temp.next;  
                count++;  
            }  
            if (count == pos - 1) {  
                newNode.next = temp.next;  
                temp.next = newNode;  
            } else {  
                throw new IndexOutOfBoundsException("Invalid position");  
            }  
        }  
    }  
    public void delete(int pos)throws Exception{  
        size--;  
        if(pos==0){  
            head=head.next;  
            return;  
        }  
        Node tmp = head, nex = head;  
        int cnt=0;  
        while(cnt!=pos-1 && tmp!=null && tmp.next!=null){  
            tmp=tmp.next;  
            cnt++;  
        }  
        if(tmp==null || tmp.next==null){  
            throw new IndexOutOfBoundsException("Invalid position");  
        }  
        else if(cnt==pos-1){  
            tmp=nex.next.next;  
        }  
    }  
    public void display() {  
        Node current = head;  
        for(int i =0;i<size;i++) {  
            System.out.print(current.data + " -> ");  
            current = current.next;  
        }  
        System.out.println("null");  
    }  
    public boolean search(int data) {  
        Node temp = head;  
        while (temp != null) {  
            if (temp.data == data){  
                return true;  
            }  
            temp = temp.next;  
        }  
        return false;  
    }  
    public int length() {  
        int count = 0;  
        Node temp = head;  
        while (temp != null) {  
            count++;  
            temp = temp.next;  
        }  
        return count;  
    }  
}
```