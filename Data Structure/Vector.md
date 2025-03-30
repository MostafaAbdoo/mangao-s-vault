```java
public class vector {  
    private int size, capacity;  
    protected int[] data;  
  
    public vector(int capacity) {  
        this.capacity = capacity;  
        data = new int[capacity];  
        size = 0;  
    }  
  
    public vector() {  
        this(2);  
    }  
  
    private void resize() {  
        int[] newData = new int[2 * capacity];  
        System.arraycopy(data, 0, newData, 0, size);  
        data = newData;  
        capacity *= 2;  
    }  
    public boolean isEmpty() {  
        return size == 0;  
    }  
    public void push(int x) {  
        if (size == capacity) {  
            resize();  
        }  
        data[size++] = x;  
    }  
    public void pop() {  
        if (size > 0) {  
            size--;  
        } else {  
            throw new ArrayIndexOutOfBoundsException("Vector is empty");  
        }  
    }  
  
    public int front() {  
        if (size > 0) {  
            return data[0];  
        } else {  
            throw new ArrayIndexOutOfBoundsException("Vector is empty");  
        }  
    }  
  
    public int back() {  
        if (size > 0) {  
            return data[size - 1];  
        } else {  
            throw new ArrayIndexOutOfBoundsException("Vector is empty");  
        }  
    }  
  
    public void insert(int val, int ind) {  
        if (ind <= size && ind >= 0) {  
            if (size == capacity) {  
                resize();  
            }  
            for (int i = size; i > ind; i--) {  
                data[i] = data[i - 1];  
            }  
            data[ind] = val;  
            size++;  
        } else {  
            throw new ArrayIndexOutOfBoundsException("Invalid index");  
        }  
    }  
  
    public void clear() {  
        size = 0;  
    }  
  
    public int getSize() {  
        return size;  
    }  
  
    public void display() {  
        for (int i = 0; i < size; i++) {  
            System.out.print(data[i] + " ");  
        }  
        System.out.println();  
    }  
  
  
}
```

