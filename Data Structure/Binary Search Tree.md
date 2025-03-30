**Inorder**: left > root > right
  - keys are sorted
**Preorder**: root > left > right
**Postorder**:  left > right > root

**Algorithm of searching**:
- left > smaller than you
- right > greater than you 
minimum element > left all along

**Finding Successor**:
- case 1: right (x) is non-empty:
   - succ(x)= minimum in right (x) 
- case 2: right (x) is empty
  - go up the tree until the current node is a left node, succ(x) = parent of that node
  - if you reached the root, x is the largest element.
**Insertion**:
v > the value to insert
x > the current node
- if v>x more right else move left
- until x is null, u have found the correct position

cost of searching/insertion in the tree is the depth length, best case scenario is a complete tree, worst case is a one branch tree

**Deletion**:
- case 1: x has no children: make the parent of x point to null
- case 2: x has one child: make the parent to x point to the child of x and Visca versa
- case 3: x has two children: 
![[Pasted image 20250213233134.png]]





```java
class Node {
    int value;
    Node left;
    Node right;

    Node(int val) {
        value = val;
        left = null;
        right = null;
    }
}

class BST {
    private Node root;

    BST() {
        root = null;
    }

    Node getRoot() {
        return root;
    }

    void insert(int value) {
        if (root == null) {
            root = new Node(value);
            return;
        }

        Node current = root;
        while (true) {
            if (value < current.value) {
                if (current.left == null) {
                    current.left = new Node(value);
                    break;
                }
                current = current.left;
            } else if (value > current.value) {
                if (current.right == null) {
                    current.right = new Node(value);
                    break;
                }
                current = current.right;
            } else {
                break; // Duplicate values are not allowed
            }
        }
    }

    void deleteValue(int value) {
        root = deleteNode(root, value);
    }

    private Node deleteNode(Node node, int value) {
        if (node == null) return null;

        if (value < node.value) {
            node.left = deleteNode(node.left, value);
        } else if (value > node.value) {
            node.right = deleteNode(node.right, value);
        } else {
            // Case 1: No children
            if (node.left == null && node.right == null) {
                return null;
            }

            // Case 2: One child
            if (node.left == null) {
                return node.right;
            } else if (node.right == null) {
                return node.left;
            }

            // Case 3: Two children
            Node successor = findMin(node.right);
            node.value = successor.value;
            node.right = deleteNode(node.right, successor.value);
        }
        return node;
    }

    private Node findMin(Node node) {
        while (node.left != null) node = node.left;
        return node;
    }

    void inOrderTraversal() {
        inOrderHelper(root);
        System.out.println();
    }

    private void inOrderHelper(Node node) {
        if (node == null) return;
        inOrderHelper(node.left);
        System.out.print(node.value + " ");
        inOrderHelper(node.right);
    }

    int findMax() {
        if (root == null) throw new RuntimeException("Tree is empty");
        Node current = root;
        while (current.right != null) current = current.right;
        return current.value;
    }

    int findMin() {
        if (root == null) throw new RuntimeException("Tree is empty");
        Node current = root;
        while (current.left != null) current = current.left;
        return current.value;
    }

    int findSuccessor(int value) {
        Node target = root;
        while (target != null && target.value != value) {
            if (value < target.value) target = target.left;
            else target = target.right;
        }

        if (target == null) throw new RuntimeException("Value not found");
        Node successor = findSuccessor(root, target);
        if (successor == null) throw new RuntimeException("No successor found");
        return successor.value;
    }

    private Node findSuccessor(Node root, Node target) {
        Node successor = null;
        while (root != null) {
            if (root.value > target.value) {
                successor = root;
                root = root.left;
            } else {
                root = root.right;
            }
        }
        return successor;
    }
}

public class Main {
    public static void main(String[] args) {
        BST bst = new BST();

        bst.insert(50);
        bst.insert(30);
        bst.insert(70);
        bst.insert(20);
        bst.insert(40);
        bst.insert(60);
        bst.insert(80);

        System.out.print("In-order traversal: ");
        bst.inOrderTraversal();

        System.out.println("Min value: " + bst.findMin());
        System.out.println("Max value: " + bst.findMax());

        System.out.println("Successor of 50: " + bst.findSuccessor(50));
        System.out.println("Successor of 40: " + bst.findSuccessor(40));

        System.out.println("Deleting 50...");
        bst.deleteValue(50);
        System.out.print("In-order traversal after deletion: ");
        bst.inOrderTraversal();
    }
}
```
