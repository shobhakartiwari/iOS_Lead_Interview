# iOS_Lead_Interview By Shobhakar Tiwari [ Helping clear iOS Interviews ]
This readme file is helpful for people who is preparing for iOS Interview onsite roles
# 1. **What could be the output of the following code?**

 ```swift
    DispatchQueue.main.async {
        print(Thread.isMainThread)
        DispatchQueue.main.async {
            print(Thread.isMainThread)
        }
    }
    
```
    **Expected Output:**
    
    false
    true
 

### Explanation:
- **false**: The first print statement occurs inside an asynchronous block, meaning it's likely running on a background thread.
- **true**: The second print statement runs on the main thread because `DispatchQueue.main.async` schedules it back on the main thread.
  

# 2. **What is a dispatch barrier, and how can it be used in Swift?**

    A dispatch barrier is a mechanism in GCD (Grand Central Dispatch) used to ensure that a specific task in a concurrent queue is executed in isolation. It guarantees that the task runs exclusively, while other tasks in the queue are suspended, making it useful for thread-safe data access in concurrent environments.

    Below is an example that demonstrates the use of `DispatchQueue` with a **dispatch barrier**:

   ```swift
    import Foundation

    // A concurrent queue
    let queue = DispatchQueue(label: "com.example.concurrentQueue", attributes: .concurrent)

    var sharedResource = [String]()

    // Adding elements concurrently
    for i in 1...5 {
        queue.async {
            print("Reading task \(i) - Shared resource: \(sharedResource)")
        }
    }

    // Writing to the shared resource using a barrier
    queue.async(flags: .barrier) {
        sharedResource.append("New Data")
        print("Writing task: Added new data")
    }

    // Continue with reading tasks after the barrier
    for i in 6...10 {
        queue.async {
            print("Reading task \(i) - Shared resource: \(sharedResource)")
        }
    }

    // Add a delay to ensure the program doesn't terminate immediately.
    DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
        print("Final Shared Resource: \(sharedResource)")
    }
```

    **Expected Output:**
    
    Reading task 1 - Shared resource: []
    Reading task 2 - Shared resource: []
    Reading task 3 - Shared resource: []
    Reading task 4 - Shared resource: []
    Reading task 5 - Shared resource: []
    Writing task: Added new data
    Reading task 6 - Shared resource: ["New Data"]
    Reading task 7 - Shared resource: ["New Data"]
    Reading task 8 - Shared resource: ["New Data"]
    Reading task 9 - Shared resource: ["New Data"]
    Reading task 10 - Shared resource: ["New Data"]
    Final Shared Resource: ["New Data"]
    
    The barrier ensures that no reading task occurs while the shared resource is being modified.


# 3. Remove Duplicates from Sorted Linked List

**Problem Description:**
Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

**Solution:**

```swift
class SinglyLinkedList {
    var val: Int
    var next: SinglyLinkedList?
    
    init() {
        self.val = 0
        self.next = nil
    }
    
    init(_ val: Int) {
        self.val = val
        self.next = nil
    }
    
    init(_ val: Int, _ next: SinglyLinkedList?) {
        self.val = val
        self.next = next
    }
}

class Solution {
    func deleteDuplicates(_ head: SinglyLinkedList?) -> SinglyLinkedList? {
        guard head != nil, head?.next != nil else {
            return head
        }
        
        var current = head
        
        while current?.next != nil {
            if current!.val == current!.next!.val {
                current!.next = current!.next!.next
            } else {
                current = current!.next
            }
        }
        
        return head
    }
}
```

  **Expected Output:**

<br>Node value: 1
<br>Node value: 2
<br>Node value: 3




# 4.What do you think the memory addresses of array1 and array2 will be?
- If they are the same, why? When will it change?
- If they are different, why?

```swift
  var array1 = [1, 2, 3]
  var array2 = array1
```

 **Expected Output:**
 <br> they are the same due to Swift's optimization technique called Copy-on-Write (CoW).


 # 5.What will be the output when this code is executed?

 ```swift
Consider the following code snippet:
class MyClass {
 var myProperty: Int = 0 {
 willSet {
 print("Will set to \(newValue)")
 }
 didSet {
 print("Did set from \(oldValue) to \(myProperty)")
 }
 }
 init() {
 defer { myProperty = 1 }
 myProperty = 2
 }
}
let instance = MyClass()
instance.myProperty = 3
```

**Expected Output:**
<br>Will set to 1
<br>Did set from 2 to 1
<br>Will set to 3
<br>Did set from 1 to 3

 # 6. Create a dictionary where: The first key stores an array of integers. The second key holds an array of doubles. The third key contains an array of strings.Sort the arrays, but if you encounter a string array, throw an error: "Unsupported type: Sorting is not possible."

 **Solution:**
 ```swift
let dict: [String: Any] = ["array1": [8, 2, 3, 5, 1],
                           "array2": [3.0, 1.0, 2.0],
                           "array3": ["d", "b", "a", "c"]]

func sortArrayFor(dict: [String: Any]) throws {
    for (key, value) in dict {
        if let integerArray = value as? [Int] {
            let sortedIntArray = integerArray.sorted()
            print("\(key) sorted integer array: \(sortedIntArray)")
        } else if let doubleArray = value as? [Double] {
            let sortedDoubleArray = doubleArray.sorted()
            print("\(key) sorted double array: \(sortedDoubleArray)")
        } else if let stringArray = value as? [String] {
            throw StringArrayError(kind: .unsupportType)
        }
    }
}

struct StringArrayError: Error {
    enum ErrorKind {
        case unsupportType
    }
    let kind: ErrorKind
}

do {
    try sortArrayFor(dict: dict)
} catch {
    print("\(error)")
}

```