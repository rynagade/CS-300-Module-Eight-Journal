# CS 300 Project One: Data Structures and Algorithms Analysis

## 1. Data Definitions and Unified Menu Pseudocode

The baseline objects and the interactive menu apply to all three data structures (Vector, Hash Table, and Binary Search Tree).

```text
Struct Course:
    courseNumber (string)
    title (string)
    prerequisites (list of strings)

Function DisplayMenu():
    Print "Welcome to the course planner."
    Print "  1. Load Data Structure."
    Print "  2. Print Course List."
    Print "  3. Print Course."
    Print "  9. Exit"
    Print "What would you like to do?"

Function Main():
    Declare courses = null
    Declare Integer choice = 0
    Declare String fileName = "ABCU_Advising_Program_Input.csv"

    While choice is not equal to 9:
        Call DisplayMenu()
        Input choice
        
        If choice == 1:
            courses = LoadCourses(fileName)
            Print "Data loaded successfully."
        Else If choice == 2:
            If courses is not null:
                Print "Here is a sample schedule:"
                Call PrintCoursesAlphanumeric(courses)
            Else:
                Print "Data has not been loaded. Please load data first."
        Else If choice == 3:
            If courses is not null:
                Print "What course do you want to know about?"
                Input courseNumber
                Call SearchCourse(courses, courseNumber)
            Else:
                Print "Data has not been loaded. Please load data first."
        Else If choice == 9:
            Print "Thank you for using the course planner!"
        Else:
            Print "Invalid option. Please try again."
```

## 2. Vector Pseudocode

```text
// Loading Data into a Vector
Function LoadCourses(fileName) Returns Vector<Course>:
    Declare Vector<Course> courses
    Open fileName for Read as inputFile
    
    If inputFile cannot be opened:
        Print "Error: unable to open file."
        Return courses

    For each line in inputFile:
        If line is empty:
            Print "Format error: blank line found."
            Continue

        tokens = Split line by comma
        If length(tokens) < 2:
            Print "Format error: missing course number or title."
            Continue

        Declare Course course
        course.courseNumber = tokens[0]
        course.title = tokens[1]
        
        For i = 2 to length(tokens) - 1:
            Append tokens[i] to course.prerequisites

        Append course to courses
    Close inputFile

    // Validate Prerequisites exist
    For each course in courses:
        For each prereq in course.prerequisites:
            Declare Boolean found = false
            For each checkCourse in courses:
                If UPPERCASE(checkCourse.courseNumber) == UPPERCASE(prereq):
                    found = true
                    break
            If NOT found:
                Print "Format Error: Prerequisite " + prereq + " does not exist."
    Return courses

// Searching and Printing a Course
Function SearchCourse(courses: Vector<Course>, courseNumber: string):
    For each course in courses:
        If UPPERCASE(course.courseNumber) == UPPERCASE(courseNumber):
            Print course.courseNumber + ", " + course.title
            If course.prerequisites is empty:
                Print "Prerequisites: None"
            Else:
                Print "Prerequisites: " + join(course.prerequisites, ", ")
            Return
    Print "Course not found."

// Printing Courses Alphanumerically
Function PrintCoursesAlphanumeric(courses: Vector<Course>):
    // Simple conceptual selection sort
    For i from 0 to length(courses) - 2:
        For j from i + 1 to length(courses) - 1:
            If courses[j].courseNumber < courses[i].courseNumber:
                swap(courses[j], courses[i])
                
    For each course in courses:
        Print course.courseNumber + ", " + course.title
```

## 3. Hash Table Pseudocode

```text
Struct HashTable:
    buckets (List of Vectors of Course)

// Loading Data into a Hash Table
Function LoadCourses(fileName) Returns HashTable:
    Declare HashTable courses = new HashTable()
    Open fileName for Read as inputFile
    
    If inputFile cannot be opened: Return courses

    For each line in inputFile:
        tokens = Split line by comma
        If length(tokens) < 2: Continue

        Declare Course newCourse
        newCourse.courseNumber = tokens[0]
        newCourse.title = tokens[1]
        For i = 2 to length(tokens) - 1:
            Append tokens[i] to newCourse.prerequisites
            
        courses.Insert(newCourse.courseNumber, newCourse)
    Close inputFile

    // Validate Prerequisites exist
    For each bucket in courses.buckets:
        For each course in bucket:
            For each prereq in course.prerequisites:
                If courses.Search(prereq) is null:
                    Print "Data Error: Prerequisite " + prereq + " does not exist."
    Return courses

// Searching and Printing a Course
Function SearchCourse(courses: HashTable, courseNumber: string):
    Course targetCourse = courses.Search(courseNumber)
    If targetCourse is not null:
        Print targetCourse.courseNumber + ", " + targetCourse.title
        Print "Prerequisites: " + join(targetCourse.prerequisites, ", ")
    Else:
        Print "Course not found."

// Printing Courses Alphanumerically
Function PrintCoursesAlphanumeric(courses: HashTable):
    Declare Vector<Course> sortedList
    // Gather all courses from the hash buckets into a flat list
    For each bucket in courses.buckets:
        For each course in bucket:
            Append course to sortedList
            
    // Sort array alphanumerically
    For i from 0 to length(sortedList) - 2:
        For j from i + 1 to length(sortedList) - 1:
            If sortedList[j].courseNumber < sortedList[i].courseNumber:
                swap(sortedList[j], sortedList[i])
                
    // Print sorted array
    For each course in sortedList:
        Print course.courseNumber + ", " + course.title
```

## 4. Binary Search Tree (BST) Pseudocode

```text
Struct Node:
    course: Course
    left: Node Pointer
    right: Node Pointer

Struct BinarySearchTree:
    root: Node Pointer

// Loading Data into a BST
Function LoadCourses(fileName) Returns BinarySearchTree:
    Declare BinarySearchTree bst
    Open fileName for Read as inputFile
   
    If inputFile is invalid: Return bst
       
    For each line in inputFile:
        tokens = Split line by comma
        If length(tokens) < 2: Continue
            
        Declare Course newCourse
        newCourse.courseNumber = tokens[0]
        newCourse.title = tokens[1]
        For i = 2 to length(tokens) - 1:
            Append tokens[i] to newCourse.prerequisites
                
        bst.Insert(newCourse) // Internally determines left/right order alphanumerically
    Close inputFile
    
    // Validate Prerequisites with helper method
    ValidatePrerequisites(bst.root, bst)
    Return bst

Function ValidatePrerequisites(node, bst):
    If node is not null:
        ValidatePrerequisites(node.left, bst)
        For each prereq in node.course.prerequisites:
            If bst.Search(prereq) is null:
                Print "Format Error: Prerequisite " + prereq + " does not exist."
        ValidatePrerequisites(node.right, bst)

// Searching and Printing a Course
Function SearchCourse(bst: BinarySearchTree, courseNumber: string):
    Course target = bst.Search(courseNumber)
    If target is not null:
        Print target.courseNumber + ", " + target.title
        Print "Prerequisites: " + join(target.prerequisites, ", ")
    Else:
        Print "Course not found."

// Printing Courses Alphanumerically
Function PrintCoursesAlphanumeric(bst: BinarySearchTree):
    // Standard in-order traversal intrinsically gives alphanumeric sorting
    PerformInOrderPrint(bst.root)

Function PerformInOrderPrint(node):
    If node is not null:
        PerformInOrderPrint(node.left)
        Print node.course.courseNumber + ", " + node.course.title
        PerformInOrderPrint(node.right)
```

## 5. Runtime and Memory Analysis

The worst-case runtime to open the file and load *n* courses was analyzed for each structure. Basic operations are assumed a cost of 1.  

| Data Structure | Reading lines / Creating Objects | Structural Insertion (Worst Case) | Prerequisite Validation (Worst Case) | Total *O* Runtime | Memory Evaluation |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Vector** | Iterating lines: $n \times O(1)$ | Vectors resize dynamically utilizing an $O(n)$ movement. Over $n$ items, total $O(n^2)$. | Search Vector takes $O(n)$. For $n$ courses, validating loops requires $O(n^2)$. | **$O(n^2)$** | $O(n)$ overhead. Data items sit cleanly adjacent in memory. |
| **Hash Table** | Iterating lines: $n \times O(1)$ | Extreme hash collisions forces $O(n)$ linked-list inserts. Over $n$ items, total $O(n^2)$. | Finding elements behind hash collisions acts as $O(n)$. For $n$ courses, $O(n^2)$. | **$O(n^2)$** | $O(n)$ plus hash buckets array $(k)$. Uses non-contiguous elements. |
| **Binary Search Tree** | Iterating lines: $n \times O(1)$ | Completely linearly-skewed linked-tree forces $O(n)$ limits. Over $n$ items, total $O(n^2)$.| Searching a heavily skewed structure acts as $O(n)$. For $n$ courses, $O(n^2)$. | **$O(n^2)$** | $O(n)$ items with pointer linking overhead (2 node pointers). |

> **Note:** The table reflects mathematical **worst-case ($O$ class)** behaviors. For data analysis purposes, everyday **Expected times** fare much better: Vector loads average $O(n)$, Hash Tables in $O(n)$, and balanced BSTs near $O(n \log n)$. 

## 6. Advantages and Disadvantages

### Vector
**Advantages:**
- Implements simple and contiguous scaling memory locations that run lean with zero pointer overhead constraints.
- Ideal for fast data processing where insertion ordering only matters at the end of execution.

**Disadvantages:**
- Retrieving an un-indexed entry necessitates traversing the entire data list element-by-element (Time Complexity: $O(n)$).
- Resizing memory can trigger expensive array relocation sequences if heavily utilized dynamically.

### Hash Table
**Advantages:**
- Retrieval speed averages to robust $O(1)$, resolving elements nearly instantly. This serves lookup tables incredibly well regardless of the table's scaling capability.

**Disadvantages:**
- The data lacks sorting internally. When resolving the alphanumeric list requirement, developers must extract items to a secondary array and explicitly apply redundant sort algorithms over it.
- Hashes depend almost purely on an optimized hashing formula; memory efficiency drops vastly depending on load-factors and collision chaining.

### Binary Search Tree (BST)
**Advantages:**
- Stores elements dynamically without restricting scaling capacity or producing collision-like delays compared to Hash buckets.
- Natively maintains the structure alphanumerically based on comparative inputs. Resolving an ordered list utilizes a trivial $O(n)$ complexity simply by executing an In-Order Traversal rather than costly active sorting equations.
- Scales queries excellently on balanced structures (average $O(\log n)$ search/insert time).

**Disadvantages:**
- Utilizes noticeably greater memory footprints due to continually managing linking references for individual Left and Right children nodes.
- Exceedingly vulnerable to sequential input data logically resolving continuously linked components (beating its $O(\log n)$ intent into $O(n)$ search speeds).

## 7. Recommendation

I explicitly recommend utilizing the **Binary Search Tree (BST)** data structure as the ideal solution to the ABCU advising problem set. 

The primary goals outline not only rapidly searching individual courses to print prerequisites but efficiently generating sequentially listed alphanumeric courses. While Vectors severely drag behind searching algorithms, Hash Tables heavily compromise systemic capability with secondary-array extraction requirements and rigid manual sorting sub-routines necessary just to fulfill basic alphanumeric sorting directives. 

Binary Search Trees fit perfectly as the equilibrium choice because nodes inherently preserve sorting orders upon initial load operations. This completely bypasses iterative backend sorting obligations and serves up an alphanumerically matched course-list intrinsically through traversing the ordered branches. Ultimately, they execute lookups sufficiently at $O(\log n)$, fulfilling printing, searching, and memory parameters elegantly.
