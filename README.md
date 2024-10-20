##  iOS Lead Interview Questions 
> Your Cheat Sheet For iOS Interview

### Prepared & maintained by [Shobhakar Tiwari](https://github.com/shobhakartiwari) 

## About me::   [DM for iOS Mock interview](https://www.linkedin.com/in/shobhakar-tiwari/)

Hi, I am Shobhakar Tiwari • I have taught and mentored many developers, and their efforts landed them high-paying tech jobs in United States, helped many tech companies in solving their unique problems, and created many framework for airline, e-Commerce based companies. I am passionate coding and always try to contribute to the developer community.

You can check my contributions over:

- [StackOverflow](https://stackoverflow.com/users/3400991/shobhakar-tiwari?tab=profile)
- [LinkedIn](https://www.linkedin.com/in/shobhakar-tiwari/)
- [GitHub](https://github.com/shobhakartiwari)

- 🔗 Feel free to explore my repositories to get a taste of my work, and don't hesitate to get in touch if you have any questions or collaboration ideas. Happy coding! 🎉  



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
    
    true
    true
 

### Explanation:
</br>The outer DispatchQueue.main.async block is queued to run on the main queue.</br>
</br>When this block executes, it will:</br>
</br>a. Print the result of Thread.isMainThread</br>
</br>b. Queue another block to run on the main queue</br>
</br>The inner DispatchQueue.main.async block will then be executed, printing the result of Thread.isMainThread again.</br>
  

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


# 13. ## Condition 0 != 0 is true. How?


Choose the best answer: </br>
#1. func checkEqual<T: Equatable>(value1:T, value2:T) -> Bool { return value1 == value2 }  </br>
#2. extension Int { static func ==(lhs: Int, rhs: Int) -> Bool { return lhs == rhs } } </br>
#3. extension Int { static func !=(lhs: Int, rhs: Int) -> Bool { return lhs != rhs } }	  </br>
#4. extension Int { static func ==(lhs: Int, rhs: Int) -> Bool { return lhs == rhs } }	  </br>


# 14. ## What will be the output of this code ?

```swift
func doTest() {
    let otherQueue = DispatchQueue(label: "otherQueue")
    
    DispatchQueue.main.async {
        print("\(Thread.isMainThread)")  // Prints whether it's on the main thread or not
        otherQueue.sync {
            print("\(Thread.isMainThread)")  // Prints whether it's on the main thread or not
        }
        otherQueue.async {
            print("\(Thread.isMainThread)")  // Prints whether it's on the main thread or not
        }
    }
}
```
Choose the best answer: </br>
#answer.true, true, false  </br>   


Reason: -

 it's common for developers to believe the .sync() will be executed in the otherQueue. In fact, to avoid unnecessary thread switch, .sync() will continue to execute on the calling thread. So in the code from the post, the closure provided to otherQueue.sync() will execute on the main thread.

</br>DispatchQueue's .sync() method waits until a lock can be acquired on queue but doesn't actually change the running thread, whereas .async() will run the closure in the queue's thread once it has acquired the thread's lock.

</br>Therefore the outcome is .) true, true, false

# 15. ## In Swift, how do you ensure thread-safe property access similar to the atomic keyword in Objective-C?

#Options:  </br>
#1. Swift properties are atomic by default. </br>
#2. Swift uses a @synchronized attribute to make properties atomic.  </br>
#3. Use a serial DispatchQueue or NSLock to manage access to shared properties.  </br>
#4. Swift has an atomic keyword that needs to be added to property declarations.  </br>
Answer : Option #3

# 16. ## What should be the output from this code : -
```swift
unc testDispatchGroup() {
    let group = DispatchGroup ( )
    group.enter ()
    DispatchQueue.global().async {
        sleep(2)
        print("1")
        group.leave()
    }
        
    group.enter()
    DispatchQueue.global().async {
        sleep(1)
        print("2")
        group.leave( )
    }
    
    group.notify( queue: .main) {
        print( "All tasks completed")
        print( "Tasks" )
    }
}

```

#Output : </br>
2</br>
1</br>
All tasks completed</br>
Tasks</br>

## Explanation : </br>
Both tasks will start almost simultaneously on global queues.</br>
After about 1 second, "2" will be printed (from Task 2).</br>
After about 2 seconds, "1" will be printed (from Task 1).</br>
Once both tasks have completed, the notification block will run on the main queue</br>
 

# 17. ## What potential issues do you see with this code? How would you improve it? 
```swift

// iOS Interview Question #16
func testThreadSafetyiniOSQuestion() {
    let group = DispatchGroup()
    var sharedResource = 0
    
    for _ in 1...1000 {
        group.enter()
        DispatchQueue.global().async {
            sharedResource += 1
            group.leave()
        }
    }
    
    group.notify(queue: .main) {
        print("Final value: \(sharedResource)")
    }
}

// function call
testThreadSafetyiniOSQuestion()
```
## Answer : </br>

## Key Issues
Race Condition: Multiple threads access and modify sharedResource without synchronization.
Non-Atomic Operation: The += operation isn't atomic, leading to potential data races.
Unpredictable Results: The final value of sharedResource is likely to be less than 1000 and may vary between runs.


## Solutions 1. Using Atomic Operations
```swift
import Foundation

class AtomicInteger {
    private var value: Int32
    private let lock = DispatchSemaphore(value: 1)
    
    init(value: Int32 = 0) {
        self.value = value
    }
    
    func increment() -> Int32 {
        lock.wait()
        defer { lock.signal() }
        value += 1
        return value
    }
}

func improvedThreadSafetyQuestion() {
    let group = DispatchGroup()
    let sharedResource = AtomicInteger()
    
    for _ in 1...1000 {
        group.enter()
        DispatchQueue.global().async {
            _ = sharedResource.increment()
            group.leave()
        }
    }
    
    group.notify(queue: .main) {
        print("Final value: $$sharedResource.increment())")
    }
}
```


## Solutions 2. Using Actor (Swift 5.5+)
```swift
actor SharedResource {
    private(set) var value = 0
    
    func increment() -> Int {
        value += 1
        return value
    }
}

func actorBasedSolution() async {
    let resource = SharedResource()
    await withTaskGroup(of: Void.self) { group in
        for _ in 1...1000 {
            group.addTask {
                await resource.increment()
            }
        }
    }
    print("Final value: $$await resource.value)")
}
```

## Solutions 3.Using Serial Queue
```swift
func serialQueueSolution() {
    let group = DispatchGroup()
    var sharedResource = 0
    let serialQueue = DispatchQueue(label: "com.example.serialQueue")
    
    for _ in 1...1000 {
        group.enter()
        serialQueue.async {
            sharedResource += 1
            group.leave()
        }
    }
    
    group.notify(queue: .main) {
        print("Final value: $$sharedResource)")
    }
}
```


# 18. Find the output of this: 
```swift
func createCounter () -> () -> Void {
    var counter = 0
    func incrementCounter () {
        counter += 1
        print (counter)
    }
    return incrementCounter
}

let closure = createCounter()
closure()
closure()
closure()
```

#19 Analyze this class for race conditions. How would you fix this using Swift concurrency tools!
```swift
///#iOS Onsite #Interview Series - Question #19
class DataCache {
    private var cache: [String: Data] = [:]
    
    func setData(for key: String, data: Data) {
        cache[key] = data
    }
    
    func getData(for key: String) -> Data? {
        return cache[key]
    }
}
```

# Explanation ::

- Race conditions: When two or more threads try to modify the same data concurrently, unpredictable results may occur. This needs careful synchronization.
- DispatchQueue: Use serial queues or DispatchQueue barriers to manage thread safety without locks.
- NSLock: This is another option for mutual exclusion, but it can introduce deadlocks if not used carefully.
- Atomic properties: Can help ensure thread safety but are more complex to implement in Swift directly.
- Serial queue vs. Barrier: Serial queues ensure that only one task runs at a time. Barriers, when used on concurrent queues, block other tasks while the barrier task runs.

# solution: 1. OSAllocatedUnfairLock
 To use a low-level lock like OSAllocatedUnfairLock to synchronize access to the cache. This method will block any other thread from accessing the cache while a write or read operation is in progress, preventing race conditions while maintaining high performance.
```swift
import os.lock

class DataCache {
    private var cache: [String: Data] = [:]
    private var lock = OSAllocatedUnfairLock()
    
    func setData(for key: String, data: Data) {
        lock.lock()  // Acquire the lock
        cache[key] = data
        lock.unlock()  // Release the lock
    }
    
    func getData(for key: String) -> Data? {
        lock.lock()  // Acquire the lock
        let data = cache[key]
        lock.unlock()  // Release the lock
        return data
    }
}

## Pros ::
- Low-overhead locking mechanism.
- Fast performance for high-concurrency scenarios.
- Maintains the same synchronous function signatures, making it easy to integrate into existing codebases.
## Cons:
- More prone to human error (e.g., forgetting to release the lock).
- Can introduce deadlocks if not handled carefully.
```

>- Why to choose this ?
>- It’s simple to use, relatively lightweight and performs well for many simple cases such as protecting a resource. It saves you from having to change all the existing calling code, were you to use an actor instead. Also, actors are not « free » either with respect to context switching.
In turn, Apple has guidance to avoid using Mutex and various other thread locking mechanisms unless you absolutely need them (which is very unlikely) when making use of concurrency.
>- [Credit:  Thanks to Michael Long & Patrick D for providing these inputs]
 
# Solution 2. NSLOCK 
```swift
import Foundation

class DataCache {
    private var cache: [String: Data] = [:]
    private let lock = NSLock()

    func setData(for key: String, data: Data) {
        lock.lock()
        defer { lock.unlock() }
        cache[key] = data
    }

    func getData(for key: String) -> Data? {
        lock.lock()
        defer { lock.unlock() }
        return cache[key]
    }
}

let cache = DataCache()
let group = DispatchGroup()

// Writing to cache
group.enter()
DispatchQueue.global().async {
    for i in 0..<100 {
        let data = "Data \(i)".data(using: .utf8)!
        cache.setData(for: "key1", data: data)
        Thread.sleep(forTimeInterval: 0.01) // Small delay to allow reading
    }
    group.leave()
}

// Reading from cache
group.enter()
DispatchQueue.global().async {
    for _ in 0..<100 {
        if let data = cache.getData(for: "key1"),
           let stringData = String(data: data, encoding: .utf8) {
            print("Read data: \(stringData)")
        }
        Thread.sleep(forTimeInterval: 0.01) // Small delay to allow writing
    }
    group.leave()
}

// Wait for both operations to complete
group.wait()
print("All operations completed")
```

## Solution 3. actor
```swift
## Pros:

- Simplifies thread-safe code, as Swift automatically handles access to cache.
- No risk of deadlocks or race conditions.

## Cons:
- Changes the API to asynchronous, which could introduce complexity at the call site (requiring await).
```

## Solution 4. Dispatch Barriers 
```swift
class DataCache<T> {
    private var cache: [String: T] = [:]
    private let queue = DispatchQueue(label: "com.datacache.queue", attributes: .concurrent)
    
    // Set data (write) using a barrier to ensure exclusive access
    func setData(for key: String, data: T) {
        queue.async(flags: .barrier) {  // The barrier flag ensures exclusive write access
            self.cache[key] = data
        }
    }
    
    // Get data (read) allowing concurrent reads
    func getData(for key: String) -> T? {
        return queue.sync {  // Sync call to ensure thread safety
            return self.cache[key]
        }
    }
}

```
- Why Use Dispatch Barrier?
- Efficiency: It allows for concurrent reads, which is highly efficient, especially when your application reads more than it writes.
- Exclusive Writes: The barrier ensures that only one write operation occurs at a time, making writes safe and preventing data races.
## Pros:
- Optimized for Reads: Since reads don’t block each other, this approach is highly performant when you have more frequent reads than writes.
- Simple and Effective: It provides simple, easy-to-use concurrency control without changing function signatures to async, which maintains ease of use in synchronous contexts.
## Cons:
- Potential Write Delays: If there are many read operations, writes can potentially be delayed, as they must wait for the queue to be free.
  
# Explanation::
- Uses defer to ensure the lock is always unlocked.
- Adds small delays to allow interleaving of read and write operations.
- Uses a DispatchGroup to ensure the program doesn't exit before operations complete.
- Prints a message when all operations are done.
