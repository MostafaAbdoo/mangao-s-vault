# Chaining
```java
package hashTable;  
  
public class Entry<T> {  
    private int key;  
    private T value;  
    Entry<T> next;  
  
    public Entry(int key, T value) {  
        this.key = key;  
        this.value = value;  
        this.next = null;  
    }  
  
    public T getValue() {  
        return value;  
    }  
  
    public int getKey() {  
        return key;  
    }  
  
    public void setNext(Entry<T> next) {  
        this.next = next;  
    }  
}
```

```java
package hashTable;  
// chaining  
public class hashTableArray<T> {  
    private Entry<T>[] array;  
    private int size;  
  
    @SuppressWarnings("unchecked")  
    public hashTableArray(int size) {  
        this.size = size;  
        array = (Entry<T>[]) new Entry[size];  // Type-safe array creation  
    }  
  
    private int getHash(int key) {  
        return key % size;  
    }  
  
    public void put(int key, T value) {  
        int index = getHash(key);  
        Entry<T> current = array[index];  
  
        // Check for duplicate key and update value  
        while (current != null) {  
            if (current.getKey() == key) {  
                current.setNext(new Entry<>(key, value)); // Update value if key exists  
                return;  
            }  
            current = current.next;  
        }  
  
        // Insert new entry at the beginning (chaining)  
        Entry<T> newEntry = new Entry<>(key, value);  
        newEntry.setNext(array[index]);  
        array[index] = newEntry;  
    }  
  
    public T get(int key) {  
        int index = getHash(key);  
        Entry<T> current = array[index];  
  
        while (current != null) {  
            if (current.getKey() == key) {  
                return current.getValue();  
            }  
            current = current.next;  
        }  
        return null; // Key not found  
    }  
}
```
