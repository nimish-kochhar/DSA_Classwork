```java
class LRUCache {
    class Node{
        Node next;
        Node prev;
        int key;
        int val;
        Node(int key,int val){
            this.val = val;
            this.key = key;
        }
    }
    private final HashMap<Integer,Node> map = new HashMap<>();
    private final int capacity;
    private final Node head;
    private final Node tail;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head = new Node(-1,-1);
        tail = new Node(-1,-1);

        head.next = tail;
        tail.prev = head;
               
    }
    
    public int get(int key) {

        if(!map.containsKey(key)) return -1;
        
            Node node = map.get(key);
            removeNode(node);
            addNode(node);
            return node.val;
        


        
    }
    
    public void put(int key, int value) {
        

        if(map.containsKey(key)){
            Node node = map.get(key);
            node.val = value;
            removeNode(node);
            addNode(node);
        }else{

        if(map.size() == capacity){
            Node lru = tail.prev;
            map.remove(lru.key);
            removeNode(lru);
        }
            Node node = new Node(key,value);
            addNode(node);
            map.put(key,node);
        

        }
        }

        
    
   

    void removeNode(Node node){
        Node pre = node.prev;
        Node nex = node.next;
        pre.next = nex;
        nex.prev = pre;
    }
    void addNode(Node node){
        Node temp = head.next;

        node.next = temp;
        node.prev = head;

        head.next = node;
        temp.prev = node;
    }
}
```