# iOS Lead Interview Questions                     
### Prepared and maintained by [Shobhakar Tiwari](https://github.com/shobhakartiwari)     

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

# 8. ## Consider the following SwiftUI code snippet:

```swift
import SwiftUI

struct ContentView: View {
    @State private var count = 0

    var body: some View {
        VStack {
            Text("Count: $$count)")
            
            Button("Increment") {
                count += 1
            }
            
            Button("Reset") {
                count = 0
            }
        }
    }
}
```
### Options:
  1. **The @State variable count cannot be modified inside the Button actions.**
  2. **The Text view does not update when count changes.**
  3. **There is no issue; the code works as intended.**
  4. **The @State variable should be declared as let instead of var.**


# 9. ##  Consider the following Combine code snippet:
```swift
let publisher = PassthroughSubject<Int, Never>()
let subscription = publisher
 .map { $0 * 2 }
 .filter { $0 > 5 }
 .sink { print($0) }

publisher.send(1)
publisher.send(2)
publisher.send(3)
publisher.send(4)
```
### ### Options?
A). 6 , 8 </br>
B). 2, 4, 6, 8 </br>
C). 6, 8,12,16 </br>
D). No output </br>
     
Feel free to share your thoughts or answers in the comments below! 👇

# 10. ## What is the output of the following code snippet?
```swift
class SomeClass {
 var member: String?
 func setMember(member: String) {
 self.member = member
 }
}
var someClass: SomeClass?
if someClass?.setMember(member: "Swift") != nil {
 print("Assigned")
} else {
 print("Not Assigned")
}
```
### ### Choose the best option:
#1. Assigned </br>
#2. Not Assigned </br>
#3. Compilation error </br>
#4. None of the given options</br>

# 11. ## What will the LazyVGrid display when the following code is executed?

```swift
struct ContentView: View {
    let items = Array(1...6).map { "Item \($0)" }
    
    let columns = [
        GridItem(.fixed(100)),
        GridItem(.flexible()),
        GridItem(.fixed(100))
    ]
    
    var body: some View {
        LazyVGrid(columns: columns) {
            ForEach(items, id: \.self) { item in
                Text(item)
                    .padding()
                    .background(Color.blue)
            }
        }
        .padding()
    }
}
```
### ### Choose the option:
#1. 6 items displayed in 2 rows with uneven column widths </br>
#2. 6 items displayed in 1 column </br>
#3. 6 items displayed in 3 rows with equal column widths </br>
#4. 6 items displayed in 2 rows with equal column widths </br>

# 12. ## What is the main difference between throw and throws in Swift?


Choose the best answer:
#1. throws is used to mark a function that can throw an error, while throw is used to actually throw an error within the function.  </br>
#2. throw is used to catch errors, while throws is used to handle them outside the function.  </br>
#3. throw is used to pass errors silently, and throws is used for logging errors.  </br>
#4. throws is used to declare error types, while throw is used to mark them in code.  </br>

 
