##  iOS Lead Interview Questions 
> Your Cheat Sheet For iOS Interview 

## Prepared & maintained by [Shobhakar Tiwari](https://github.com/shobhakartiwari)  

## About me:   [DM for iOS Mock interview](https://www.linkedin.com/in/shobhakar-tiwari/)

Hi, I am Shobhakar Tiwari • I have taught and mentored many developers, and their efforts landed them high-paying tech jobs in United States, helped many tech companies in solving their unique problems, and created many framework for airline, e-Commerce based companies. I am passionate coding and always try to contribute to the developer community.

You can check my contributions over:

- [StackOverflow](https://stackoverflow.com/users/3400991/shobhakar-tiwari?tab=profile)
- [LinkedIn](https://www.linkedin.com/in/shobhakar-tiwari/)
- [GitHub](https://github.com/shobhakartiwari)

- 🔗 Feel free to explore my repositories to get a taste of my work, and don't hesitate to get in touch if you have any questions or collaboration ideas. Happy coding! 🎉  



## 1. **What could be the output of the following code?**
        
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
 

## Explanation:
</br>The outer DispatchQueue.main.async block is queued to run on the main queue.</br>
</br>When this block executes, it will:</br>
</br>a. Print the result of Thread.isMainThread</br>
</br>b. Queue another block to run on the main queue</br>
</br>The inner DispatchQueue.main.async block will then be executed, printing the result of Thread.isMainThread again.</br>
  

##  2. **What is a dispatch barrier, and how can it be used in Swift?**

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


##  3. Remove Duplicates from Sorted Linked List

**Problem Description:**
Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

## Solution:

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

## Expected Output:
<br>Node value: 1
<br>Node value: 2
<br>Node value: 3

##  4.What do you think the memory addresses of array1 and array2 will be?
- If they are the same, why? When will it change?
- If they are different, why?

```swift
  var array1 = [1, 2, 3]
  var array2 = array1
```

##  Expected Output:
 <br> they are the same due to Swift's optimization technique called Copy-on-Write (CoW).


## 5.What will be the output when this code is executed?

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

##  Expected Output:
<br>Will set to 3
<br>Did set from 1 to 3
- However, property observers get triggered only after the self-scope and do not get triggered during the initialisation. I tried validating it with Playground and got the output only after the object was created.
  
## 6. Create a dictionary where: The first key stores an array of integers. The second key holds an array of doubles. The third key contains an array of strings.Sort the arrays, but if you encounter a string array, throw an error: "Unsupported type: Sorting is not possible."

##  Solution:
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

## 8. Consider the following SwiftUI code snippet:

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
## Options:
  1. **The @State variable count cannot be modified inside the Button actions.**
  2. **The Text view does not update when count changes.**
  3. **There is no issue; the code works as intended.**
  4. **The @State variable should be declared as let instead of var.**


## 9. ##  Consider the following Combine code snippet:
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
## Options?
A). 6 , 8 </br>
B). 2, 4, 6, 8 </br>
C). 6, 8,12,16 </br>
D). No output </br>
     
Feel free to share your thoughts or answers in the comments below! 👇

## 10. What is the output of the following code snippet?
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
## Choose the best option:
#1. Assigned </br>
#2. Not Assigned </br>
#3. Compilation error </br>
#4. None of the given options</br>

## 11. What will the LazyVGrid display when the following code is executed?

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
## Choose the option:
#1. 6 items displayed in 2 rows with uneven column widths </br>
#2. 6 items displayed in 1 column </br>
#3. 6 items displayed in 3 rows with equal column widths </br>
#4. 6 items displayed in 2 rows with equal column widths </br>

## 12. What is the main difference between throw and throws in Swift?
## Choose the best answer:
#1. throws is used to mark a function that can throw an error, while throw is used to actually throw an error within the function.  </br>
#2. throw is used to catch errors, while throws is used to handle them outside the function.  </br>
#3. throw is used to pass errors silently, and throws is used for logging errors.  </br>
#4. throws is used to declare error types, while throw is used to mark them in code.  </br>


## 13. Condition 0 != 0 is true. How?

## Choose the best answer: </br>
#1. func checkEqual<T: Equatable>(value1:T, value2:T) -> Bool { return value1 == value2 }  </br>
#2. extension Int { static func ==(lhs: Int, rhs: Int) -> Bool { return lhs == rhs } } </br>
#3. extension Int { static func !=(lhs: Int, rhs: Int) -> Bool { return lhs != rhs } }	  </br>
#4. extension Int { static func ==(lhs: Int, rhs: Int) -> Bool { return lhs == rhs } }	  </br>


## 14. What will be the output of this code ?

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
## Choose the best answer: </br>
#answer.true, true, false  </br>   


Reason: -

 it's common for developers to believe the .sync() will be executed in the otherQueue. In fact, to avoid unnecessary thread switch, .sync() will continue to execute on the calling thread. So in the code from the post, the closure provided to otherQueue.sync() will execute on the main thread.

</br>DispatchQueue's .sync() method waits until a lock can be acquired on queue but doesn't actually change the running thread, whereas .async() will run the closure in the queue's thread once it has acquired the thread's lock.

</br>Therefore the outcome is .) true, true, false

## 15. In Swift, how do you ensure thread-safe property access similar to the atomic keyword in Objective-C?

## Options:  </br>
#1. Swift properties are atomic by default. </br>
#2. Swift uses a @synchronized attribute to make properties atomic.  </br>
#3. Use a serial DispatchQueue or NSLock to manage access to shared properties.  </br>
#4. Swift has an atomic keyword that needs to be added to property declarations.  </br>
Answer : Option #3

## 16. What should be the output from this code : -
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

## Output : </br>
2</br>
1</br>
All tasks completed</br>
Tasks</br>

## Explanation : </br>
Both tasks will start almost simultaneously on global queues.</br>
After about 1 second, "2" will be printed (from Task 2).</br>
After about 2 seconds, "1" will be printed (from Task 1).</br>
Once both tasks have completed, the notification block will run on the main queue</br>
 

## 17. What potential issues do you see with this code? How would you improve it? 
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


## 18. Find the output of this: 
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

## 19. Analyze this class for race conditions. How would you fix this using Swift concurrency tools!
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

## Explanation ::

- Race conditions: When two or more threads try to modify the same data concurrently, unpredictable results may occur. This needs careful synchronization.
- DispatchQueue: Use serial queues or DispatchQueue barriers to manage thread safety without locks.
- NSLock: This is another option for mutual exclusion, but it can introduce deadlocks if not used carefully.
- Atomic properties: Can help ensure thread safety but are more complex to implement in Swift directly.
- Serial queue vs. Barrier: Serial queues ensure that only one task runs at a time. Barriers, when used on concurrent queues, block other tasks while the barrier task runs.

## solution: 1. OSAllocatedUnfairLock
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
 
## Solution 2. NSLOCK 
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
  
## Explanation::
- Uses defer to ensure the lock is always unlocked.
- Adds small delays to allow interleaving of read and write operations.
- Uses a DispatchGroup to ensure the program doesn't exit before operations complete.
- Prints a message when all operations are done.

##  20. Scenario: Refactor the UserPreferences class to support multiple storage methods including UserDefaults, Plist.

- You're working with a UserPreferences singleton in an iOS app that currently uses UserDefaults for storing settings. The app needs the flexibility to switch between different storage methods like UserDefaults, Plist. How would you refactor this to allow easy switching between storage options?

```swift
/// 🚀 #iOS Lead Onsite #Interview Series - Question #20 🚀
class UserPreferences {
    static let shared = UserPreferences()

    private let userDefaults = UserDefaults.standard

    private init() {}

    // Method to set a value for a given key
    func setValue(_ value: String, forKey key: String) {
        userDefaults.set(value, forKey: key)
    }

    // Method to get a value for a given key
    func getValue(forKey key: String) -> String? {
        return userDefaults.string(forKey: key)
    }
}

// Example usage
let preferences = UserPreferences.shared
preferences.setValue("Shobhakar", forKey: "username")

if let username = preferences.getValue(forKey: "username") {
    print("Retrieved username: \(username)")  // Output: Retrieved username: Shobhakar
}
```
## Answer :: 
```Swift
import Foundation
 import CoreData

 enum StorageMethod {
     case userDefaults
     case plist
     case coreData
 }

 class UserPreferences {
     static let shared = UserPreferences()
     
     private var storageMethod: StorageMethod = .userDefaults
     
     private init() {}

     // Method to set the storage method
     func setStorageMethod(_ method: StorageMethod) {
         storageMethod = method
     }

     // Set value based on the current storage method
     func setValue(_ value: String, forKey key: String) {
         switch storageMethod {
         case .userDefaults:
             UserDefaults.standard.set(value, forKey: key)
         case .plist:
             saveToPlist(value: value, forKey: key)
         case .coreData:
             saveToCoreData(value: value, forKey: key)
         }
     }

     // Get value based on the current storage method
     func getValue(forKey key: String) -> String? {
         switch storageMethod {
         case .userDefaults:
             return UserDefaults.standard.string(forKey: key)
         case .plist:
             return loadFromPlist(forKey: key)
         case .coreData:
             return loadFromCoreData(forKey: key)
         }
     }

     // PList methods
     private func saveToPlist(value: String, forKey key: String) {
         let filePath = getPlistPath()
         var plistDict = NSDictionary(contentsOf: filePath) as? [String: String] ?? [:]
         plistDict[key] = value
         (plistDict as NSDictionary).write(to: filePath, atomically: true)
     }

     private func loadFromPlist(forKey key: String) -> String? {
         let filePath = getPlistPath()
         let plistDict = NSDictionary(contentsOf: filePath) as? [String: String]
         return plistDict?[key]
     }

     private func getPlistPath() -> URL {
         let fileManager = FileManager.default
         let urls = fileManager.urls(for: .documentDirectory, in: .userDomainMask)
         return urls[0].appendingPathComponent("UserPreferences.plist")
     }

     // Core Data methods
     private func saveToCoreData(value: String, forKey key: String) {
         // Assuming CoreData setup is done
         let context = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
         let newPreference = UserPreferenceEntity(context: context)
         newPreference.key = key
         newPreference.value = value
         do {
             try context.save()
         } catch {
             print("Failed to save to Core Data: \(error)")
         }
     }

     private func loadFromCoreData(forKey key: String) -> String? {
         let context = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
         let request: NSFetchRequest<UserPreferenceEntity> = UserPreferenceEntity.fetchRequest()
         request.predicate = NSPredicate(format: "key == %@", key)
         
         do {
             let results = try context.fetch(request)
             return results.first?.value
         } catch {
             print("Failed to load from Core Data: \(error)")
             return nil
         }
     }
 }

 // Example usage
 let preferences = UserPreferences.shared
 preferences.setStorageMethod(.plist)  // Switch to Plist storage
 preferences.setValue("Shobhakar", forKey: "username")

 if let username = preferences.getValue(forKey: "username") {
     print("Retrieved username: \(username)")  // Output: Retrieved username: Shobhakar
 }
```


## 21. Are lazy vars computed more than once in Swift? 
## Explanation:: 
- Lazy vars are not thread safe by default, meaning if a lazy property is not yet initialized & multiple threads try to access it, there is a possiblility that it will be initialized more than once.

## 22. Structs Inside Classes and Memory Management in Swift?
- In Swift, structs are value types and are generally stored on the stack, while classes are reference types and are stored on the heap. If you have a struct inside a class, where is the memory for the struct allocated, and why?

## Explanation:: 
- In Swift, the memory allocation for value types and reference types depends on their context.

- Structs are value types and are generally stored on the stack when used alone or in isolation.
Classes are reference types and are stored on the heap.
Now, if you have a struct inside a class, the memory for that struct is allocated on the heap, along with the memory for the class instance. This happens because when you create an instance of a class, all of its properties (whether value types or reference types) are stored as part of the class’s memory block on the heap. The class instance holds a reference to its memory location on the heap, and all its properties—including the struct—are stored in that same memory.

- Why is it stored on the heap?
When a value type (such as a struct) is a property of a reference type (such as a class), it becomes part of the class's internal data. Since the class is stored on the heap, all of its contents, including value types, are stored on the heap to ensure they remain alive as long as the class instance exists. In this case, the struct is "lifted" to the heap with the class for proper memory management.

## 23. What is Smart MVVM?
Smart MVVM is an enhanced version of the traditional MVVM (Model-View-ViewModel) architectural pattern specifically designed for iOS applications. It aims to improve the separation of concerns, enhance maintainability, and increase the efficiency of data binding between the UI and business logic. Smart MVVM integrates reactive programming concepts, dependency injection, and asynchronous data handling to create a more robust development experience.

### Key Differences from Traditional MVVM:

1. **Reactive Programming**: 
   - Smart MVVM often utilizes reactive frameworks like Combine or RxSwift to facilitate automatic updates in the UI based on changes in the ViewModel. This minimizes boilerplate code and improves responsiveness.

2. **Dependency Injection**: 
   - In Smart MVVM, dependency injection is used to decouple components, making the ViewModel more testable and maintainable. It allows for easy swapping of implementations, which is beneficial for unit testing.

3. **Asynchronous Data Handling**: 
   - Smart MVVM embraces async/await patterns for handling asynchronous operations such as network requests. This leads to cleaner, more readable code and better error handling.

### Benefits of Using Smart MVVM:

- **Clean Architecture**: Enforces clear separation of responsibilities, making the codebase easier to manage and extend.
- **Enhanced Testability**: The use of DI and decoupled components allows for easier unit testing of the ViewModel.
- **Responsive UI**: Reactive programming ensures that the UI remains in sync with the underlying data model seamlessly.

## 24. When displaying images in a UITableView, how would you approach caching those images to improve performance? What factors would you consider in deciding how much data to store in the cache?
- To improve performance when displaying images in a UITableView, I would implement a caching strategy that includes both in-memory and on-disk caching.

### In-Memory Caching:
- I would use an in-memory cache to store recently accessed images. This allows for quick retrieval as the user scrolls, improving the overall responsiveness of the app. Libraries like SDWebImage or Kingfisher can be helpful because they have built-in memory caching mechanisms.
On-Disk Caching:

- In addition to in-memory caching, I would implement on-disk caching to store images for offline access and reduce redundant network requests. This is especially useful for images that are not frequently updated.

### Determining Cache Size:
- To determine how much data to store in the cache, I would consider the following factors:
- Available Memory: Monitor the app’s memory usage to set a reasonable limit for the in-memory cache, typically around 10-20% of available memory.
- Image Size and Frequency of Access: Analyze the average size of images and their access patterns. Frequently accessed images can be prioritized for caching.
- Cache Eviction Policy: Implement an eviction policy, such as Least Recently Used (LRU), to remove the least accessed images from the cache when the size - - limit is reached.

### Cache Invalidation:
- Lastly, I would implement strategies for cache invalidation, such as checking for updates on the server or allowing users to refresh images manually to ensure that they see the most current data.
- By combining these strategies, I can optimize image loading in the UITableView while managing memory effectively and enhancing user experience.

## 25. If an iOS app is suspended, will it still receive push notifications? If yes, will the push notification delegate method execute when the notification is received?
- Answer: Yes, an iOS app can receive push notifications even when it’s suspended. In iOS, push notifications are managed by the system rather than the app itself. When a push notification arrives for a suspended app, the system displays the notification in the notification center.
- However, the delegate method (userNotificationCenter(_:didReceive:withCompletionHandler:)) will not execute immediately when the app is in the background or suspended. Instead, the method is called only if:
- The user taps the notification, which launches or brings the app to the foreground.
- The notification is configured with a "content-available" key, allowing for silent pushes that might wake the app in the background (if permitted).
To summarize:
- Yes, the app can receive push notifications while suspended.
- No, the delegate method won’t execute unless the user interacts with the notification or the notification is configured for background updates.

## 26. Apple push notifications Interview Questions ( i have created this supporting till iOS 13 , will update this shortly)
- Download complete flow diagram along with interview questions from this link : https://github.com/shobhakartiwari/Push-Notifications-Interview-guide.git

## 27. You need to migrate a Core Data model that has significant changes, including adding new entities, relationships, and attributes. Which of the following approaches is most appropriate for handling this migration while preserving existing data?
- 1. Use Lightweight Migration by setting NSMigratePersistentStoresAutomaticallyOption and NSInferMappingModelAutomaticallyOption to true in your persistent store options, as Core Data will infer the mapping model for complex changes.

- 2. Create a Custom Migration Mapping Model with a custom mapping model file to define transformation logic and manually handle complex data migrations between old and new versions.

- 3. Remove the old data store and start fresh with a new Core Data stack, letting users start with clean data in the updated model.

- 4. Enable Automatic Schema Updating by modifying the Core Data model directly in place without versioning, as Core Data will automatically adjust to new attributes and relationships.
## Answer: 
- The correct answer is B.Lightweight Migration (Option A) only supports simple changes like adding attributes or entities without significant restructuring. For complex changes, creating a custom mapping model (Option B) is necessary, where developers can explicitly define how data should transform and map between versions, ensuring data integrity through the migration.

## 28. Given the code snippet below, which aims to create an array of tuples containing the index and value of each element in an array, rewrite it in a more "Swifty" and efficient way.
```swift
let list = [Int](1...5)
var arrayOfTuples = [(Int, Int)]()

for (index, element) in list.enumerated() {
    arrayOfTuples += [(index, element)]
}

print(arrayOfTuples)  // Expected output: [(0, 1), (1, 2), (2, 3), (3, 4), (4, 5)]

```
 ### Answer: - 
```swift
let list = [Int](1...5)
let arrayOfTuples = Array(list.enumerated())
print(arrayOfTuples)  // Expected output: [(0, 1), (1, 2), (2, 3), (3, 4), (4, 5)]
```
## 29. Question: Can you discuss a specific instance where you used Dependency Injection to solve a problem in an iOS project?
- In one of my iOS projects, we needed a modular and testable way to handle network requests across multiple view controllers. Initially, network dependencies were tightly coupled, making it hard to mock data for unit testing. To solve this, I introduced Dependency Injection by designing a protocol, NetworkServiceProtocol, with required networking methods and then created a NetworkService class that conformed to this protocol.

- Using DI, I passed NetworkServiceProtocol instances into the view controllers rather than directly instantiating them. This allowed us to inject mock network services during testing, making it much easier to validate different network responses without calling actual endpoints. This approach reduced code duplication, improved testability, and significantly lowered maintenance costs as we scaled the project.

## 30.  Given an array of integers with duplicate elements (which may be sorted or unsorted), how would you filter out the unique elements? 🤔
The challenge here isn’t just to solve the problem — but to solve it efficiently. 
```swift
///  Approach 1: Without Using a Set
func removeDuplicatesWithoutSet(from array: [Int]) -> [Int] {
    var seen: [Int] = []
    return array.filter { element in
        if seen.contains(element) {
            return false
        } else {
            seen.append(element)
            return true
        }
    }
}

/// Approach 2: Using a Set for Faster Lookups
func removeDuplicatesUsingSet(from array: [Int]) -> [Int] {
    var seen = Set<Int>()
    return array.filter { seen.insert($0).inserted }
}
```
- Answer : Approach 2. Time Complexity: O(n) 🚀 This method achieves linear time complexity thanks to the O(1) average lookup time of the Set. It's the preferred approach for larger datasets! 


### iOS Developer Interview: Scenario-Based Exploration

##31. Scenario: You’re assigned to improve the performance of a slow-running iOS app. How would you approach this task, and what tools or methods would you use to diagnose and fix the issues?
- for answer follow my article over medium: https://medium.com/@shobhakartiwari/ios-developer-interview-scenario-based-exploration-c3c941a024ae

## 32. fint the output - 
```swift
let value = [1,2,3, 4,5,6,7,8,9]
let result = value.lazy.filter{ value in
value % 2 != 0
}.map { value in
    value * value
}.prefix (3)
print (Array (result))
```
- Answer : [1,9,25]

## 33. How errors been handled in swift?
### 1. throws Keyword
- Functions that might produce an error are marked with the throws keyword. Calling such functions requires the use of try, try?, or try!.
```swift
  func readFile(at path: String) throws -> String {
    // Code to read a file, throwing an error if the file isn't found
}
```
### 2. try, try?, and try! Explained
- try: Used with do-catch to handle errors explicitly.
- try?: Converts the result into an optional. Returns nil if an error occurs.
- try!: Forces the operation to execute. Crashes at runtime if an error is thrown.
```swift
do {
    let content = try readFile(at: "path/to/file")
    print(content)
} catch {
    print("Error: \(error)")
}

// Optional error handling
let content = try? readFile(at: "path/to/file")

// Force unwrapping (not recommended)
let content = try! readFile(at: "path/to/file")
```
### 3. do-catch for Explicit Error Handling
- Use a do-catch block to handle errors thrown by a function. You can also pattern-match specific errors.
```swift
do {
    let content = try readFile(at: "path/to/file")
    print("File content: \(content)")
} catch FileError.notFound {
    print("File not found.")
} catch {
    print("An unexpected error occurred: \(error).")
}
```
- you can also create your own custom error enum and throw it. 

## 34. Difference between classes and structures. [ credit goes to K Motwani ]
<img width="683" alt="Screenshot 2024-11-21 at 10 28 31 PM" src="https://github.com/user-attachments/assets/6eab8f11-6f6b-49a6-8c99-09c07a397163">

## 35. What is a computed property?
- A computed property in Swift is a property that doesn’t store a value directly. Instead, it provides a getter (and optionally a setter) to retrieve or compute a value indirectly each time it’s accessed.
```swift
struct Circle {
    var radius: Double

// Computed property is defined with var for diameter with a getter and setter
    var diameter: Double {
        get {
            return radius * 2
        }
        set {
            radius = newValue / 2
        }
    }
}

var circle = Circle(radius: 5)
print(circle.diameter)  // Output: 10

circle.diameter = 20    // Sets `radius` to 10
print(circle.radius)     // Output: 10
```

## 36. What are access specifiers?
<img width="676" alt="Screenshot 2024-11-21 at 10 31 20 PM" src="https://github.com/user-attachments/assets/81b512cf-6569-4a54-a773-5ab3fdb76edd">

## 37. Difference between GCD and Operation queues ?
<img width="670" alt="Screenshot 2024-11-21 at 10 31 58 PM" src="https://github.com/user-attachments/assets/46ea4ae6-31be-4e1e-9f0a-c860a629c323">

## 38. Explain the lifecycle of a view controller in iOS.
- View controller’s lifecycle
init :- Initialize properties and objects within the view controller. <br/>
load view :- Manually create and assign the view if needed.<br/>
viewDidLoad :- Setup initial configurations and view setup.<br/>
ViewWillAppear :- Update views, refresh data, and make final layout changes.<br/>
ViewWillLayoutSubviews :- make adjustments to the layout of your views before they are displayed on the screen<br/>
ViewDidLayoutSubviews :- make adjustments to your views after their layout has been finalized<br/>
viewDidAppear :- Trigger animations or track view display events.<br/>
viewWillDisappear :- Save data or state changes, prepare for view to disappear.<br/>
viewDidDisappear :- Stop ongoing tasks that are unnecessary off-screen.<br/>
deinit :- Clean up resources before the view controller is removed.<br/>
didReceivememoryWarning() :- Release unused memory in low-memory situations.<br/>
viewWillTransition(to:with:) :- handle changes in the view’s layout when the device rotates or when the view’s size class changes (like switching between portrait and landscape modes , interface orientation of the device)<br/>

## 39. What is URLSession, and how do you use it to make network requests?
- URLSession is a class in Swift that provides an API for downloading, uploading, and managing network data. It is used for making network requests like GET, POST, PUT, etc and handling responses in iOS apps.It is part of the Foundation framework and can manage background downloads, API calls, file transfers, and more.

## 40. Difference between synchronous and asynchronous tasks in Swift?
<img width="675" alt="Screenshot 2024-11-21 at 10 34 07 PM" src="https://github.com/user-attachments/assets/03f25ade-2fdd-40fe-968a-f4e9c20a5425">

## 41. How do you handle background tasks in iOS?
- Background tasks allow apps to continue executing code when they’re not actively in use. Handling background tasks, such as background fetch, data uploads, location updates, or processing that should continue for a limited time.

## 42. What is SwiftUI and its benefits? Difference between Swift UI and UI Kit?
- SwiftUI is a declarative framework introduced by Apple in 2019 to build user interfaces across all Apple platforms using a unified codebase. SwiftUI simplifies the UI development process, allowing developers to describe how the UI should look and behave in a declarative way, which can reduce the complexity of UI code and enable faster, more maintainable UI development.
- Benefits :-
 <img width="669" alt="Screenshot 2024-11-21 at 10 35 54 PM" src="https://github.com/user-attachments/assets/cfcf4389-a565-4e07-b144-3600b472200d">

## 43. Explain the difference between @State and @Binding in SwiftUI ?
<img width="671" alt="Screenshot 2024-11-21 at 10 36 30 PM" src="https://github.com/user-attachments/assets/2d49e5db-23ab-4e32-ac3c-949b54fbafc3">

## 44. What is @environment and @Published in SwiftUI ?
<img width="674" alt="Screenshot 2024-11-21 at 10 36 57 PM" src="https://github.com/user-attachments/assets/8637356b-06c9-49b6-840a-ac0d62665415">

## 45. What is a Protocol and Where Do We Use It?  [ credit goes to : Mihail Salari]
- A protocol defines a blueprint of methods, properties, and other requirements for a particular task. It’s like an interface in other programming languages.
- You can use protocols in various situations:
- Delegation: For passing data between objects.
- Data Modeling: When you need a consistent API for your models.
- Reusable Components: Protocols can make your components more flexible and reusable.
- In design patterns like Strategy, Observer, and Factory.
- For mocking in unit tests.

## 46. What is a Semaphore in Swift?
- A semaphore is a powerful synchronization tool that controls access to a resource by multiple threads. In Swift, you can use DispatchSemaphore to implement it.
```swift
let semaphore = DispatchSemaphore(value: 1)

DispatchQueue.global().async {
    semaphore.wait()
    /// Critical section of code
    semaphore.signal()
}
```
## 47. What is Parallel Programming in Swift?
- Parallel programming refers to performing multiple tasks simultaneously. In Swift, this is typically accomplished using DispatchQueue.concurrentPerform.
```swift
DispatchQueue.concurrentPerform(iterations: 10) { index in
    print("This is task \(index)")
}
```
## 48. What is an Autorelease Pool and What is Its Main Reason? 
- Autorelease pools store objects that are sent autorelease message. The main reason for using an autorelease pool is to control memory usage during the lifetime of an app, particularly within loops.
```swift
autoreleasepool {
    /// Your code that creates many temporary autoreleased objects
}
```
- Autorelease pools help in reducing the peak memory footprint, providing a more responsive user experience.

## 48. Can delegation be implemented without a protocol? If yes, then why do we need a Protocol? [ FAANG Asked Question]
- Yes, delegation can be implemented without a protocol in Swift, but using a protocol makes delegation more structured, reusable, and type-safe. Here's a breakdown of why you can do it without a protocol and why using a protocol is beneficial:
``` swift
class Manager {
    var delegate: AnyObject? // No protocol required
    func performTask() {
        (delegate as? Employee)?.executeTask()
    }
}

class Employee {
    func executeTask() {
        print("Task executed by Employee.")
    }
}

let manager = Manager()
let employee = Employee()
manager.delegate = employee // Assigning without a protocol
manager.performTask() // Output: Task executed by Employee.
```
## 49. Why Do We Need Protocols for Delegation?
- While it’s possible to implement delegation without a protocol, using a protocol offers key advantages:

- 1. Enforces a Contract

- A protocol ensures that the delegate adheres to a defined contract by implementing required methods. This avoids runtime errors caused by missing or incorrectly implemented methods.
```swift
protocol TaskDelegate {
    func executeTask()
}

class Manager {
    var delegate: TaskDelegate? // Protocol-constrained delegate
    func performTask() {
        delegate?.executeTask()
    }
}

class Employee: TaskDelegate {
    func executeTask() {
        print("Task executed by Employee.")
    }
}

let manager = Manager()
let employee = Employee()
manager.delegate = employee
manager.performTask() // Output: Task executed by Employee.

```
- Compile-Time Safety
- Without a protocol, you must cast the delegate to a specific type, which can fail at runtime. Protocols eliminate this risk by allowing the compiler to check conformance.
- manager.delegate = "InvalidDelegate" // Compile-time error if delegate must conform to `TaskDelegate`.
- Flexibility and Reusability
- Using a protocol makes the delegation pattern more flexible, allowing different types to conform to the protocol. This enables reusability across various classes or modules.
```swift
class Freelancer: TaskDelegate {
    func executeTask() {
        print("Task executed by Freelancer.")
    }
}

let freelancer = Freelancer()
manager.delegate = freelancer
manager.performTask() // Output: Task executed by Freelancer.
```
- 4. Decoupling
- Protocols reduce tight coupling between the delegator and delegate classes. The delegator doesn't need to know the exact type of the delegate, only that it conforms to the protocol.
- 5. Scalability
-  Protocols support default implementations via extensions, making it easier to scale functionality without modifying existing classes.

## 50. When to use , the Singleton pattern?
### When you need to manage a shared resource, such as:
- A network manager for making API requests.
- A database connection or cache manager.
- A logging system to record events.

### Global State Management
- When maintaining global state, such as user session data or configuration settings, that should only exist once throughout the app lifecycle.
### Centralized Coordination
- For managing cross-cutting concerns like:
- A theme manager for UI appearance.
- A notification manager to handle system-wide notifications.
### Thread-Safe Lazy Initialization
- If creating the instance is expensive or requires coordination across threads, a Singleton provides a mechanism for thread-safe, lazy initialization.

## When not to use the singleton pattern?
- Tight Coupling (If classes depend directly on a Singleton, it creates tight coupling, making the code harder to test and maintain. This violates the Dependency Inversion Principle.)
- Global State Pollution ( Overusing Singletons for mutable shared states can lead to hard-to-debug issues, especially in concurrent or multi-threaded environments.
Example Problem: Two parts of the app modify the same Singleton state simultaneously, causing unintended side effects.)
- Testing and Mocking Challenges (Singletons make it difficult to test code in isolation since you cannot easily mock or replace their functionality.
Better Alternative: Use protocols and dependency injection to allow mock implementations during tests.)
- Overhead for Simple Scenarios (If the same functionality can be achieved with simpler patterns, avoid the Singleton for unnecessary complexity. For instance, static methods or structs may suffice.)
-Hidden Dependencies ( Singletons introduce hidden dependencies that are not immediately apparent from the API of a class or module, making the code less readable and harder to refactor.)

## 51. Why are IBOutlets weak by default? What happens when we make them strong? Is it even a problem?
- IBOutlets are weak by default to avoid retain cycles since the view hierarchy already holds strong references to its subviews. Making them strong can cause memory leaks if the view controller or its views are not properly deallocated.

## 52. How do cocoapods work?
- CocoaPods is a dependency manager for iOS that automates adding third-party libraries to projects. You define dependencies in a Podfile, run pod install to download them, and use the generated .xcworkspace to manage your app alongside the libraries. It handles downloading, linking, and versioning efficiently.

## 53. Why did Apple chose to go with structures to implement primitive types in Swift?
- Apple chose structures for primitive types in Swift due to their value semantics, which ensure safety, thread safety, and simplicity. Structures are optimized for performance, use less memory (stack allocation), and avoid issues like shared mutable state, making them ideal for Swift’s design goals.

## 54. What is @Frozen keyword in swift ?
- follow my medium article on this: https://medium.com/@shobhakartiwari/understanding-the-frozen-attribute-in-swift-a-guide-for-developers-bbb98cf8c235

## 55. What are ‘@dynamicCallable’ and ‘dynamic member lookup’ in Swift?
- @dynamicCallable allows an object to be called like a function, with dynamic arguments. You define a method to handle the call, like dynamicallyCall(withArguments:)
```swift
@dynamicCallable
struct Math {
    func dynamicallyCall(withArguments args: [Int]) -> Int {
        return args.reduce(0, +)
    }
}
let math = Math()
let result = math(1, 2, 3)  // Output: 6
```
- @dynamicMemberLookup allows accessing properties of an object dynamically, similar to key-value coding. You can access members on an object even if they aren’t explicitly defined in the type, enabling dynamic member resolution at runtime.

## 56. Can you compare the use cases for value types and reference types in app development?
- For answer follow my answer : https://github.com/shobhakartiwari/structure-vs-classes.git

## 57. Which Sorting Algorithm Does Swift's High-Order Function `sorted()` Use?
- TimSort
```swift
Timsort is a powerful hybrid sorting algorithm that blends the best features of merge sort and insertion sort. Here's why it stands out:

🛡️ Stability: Preserves the relative order of equal elements, making it ideal for sorting complex data types.
⚡ Efficiency: Handles large datasets with a worst-case time complexity of O(n log n), ensuring reliable performance.
🔄 Adaptability: Excels with partially sorted arrays, optimizing sorting operations for real-world scenarios.
This combination of speed, stability, and adaptability makes Timsort a go-to algorithm for efficient sorting in Swift and beyond. 🚀
```

## 58. What will be the output of the following Swift code used in Shobhakar Tiwari's iOS Mock Interview session?
<img width="321" alt="Screenshot 2024-12-07 at 11 53 21 PM" src="https://github.com/user-attachments/assets/9a3d04c5-b9ae-4291-8b7d-027bfabcebc2">

- Answer : iOS Swift Mock


## Contributing
Shobhakar Tiwari welcome contributions! Please check out our [CONTRIBUTING.md](https://raw.githubusercontent.com/shobhakartiwari/iOS_Lead_Interview/refs/heads/main/CONTRIBUTING.md) file for guidelines on how to get started.

