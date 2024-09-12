# iOS_Lead_Interview By Shobhakar Tiwari
This readme file is helpful for people who is preparing for iOS Interview onsite roles

1. **What could be the output of the following code?**

    ```swift
    DispatchQueue.main.async {
        print(Thread.isMainThread)
        DispatchQueue.main.async {
            print(Thread.isMainThread)
        }
    }
    ```

    **Expected Output:**
    
    ```
    false
    true
    ```

### Explanation:
- **false**: The first print statement occurs inside an asynchronous block, meaning it's likely running on a background thread.
- **true**: The second print statement runs on the main thread because `DispatchQueue.main.async` schedules it back on the main thread.



