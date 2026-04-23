# CS-300 Module Eight Journal

## Overview
This repository contains two key artifacts from the CS-300 Data Structures and Algorithms course, demonstrating the evaluation and implementation of data structures to solve a real-world advising program scenario.

### Included Artifacts
1. **[Runtime and Memory Analysis (Project One)](Run-time%20and%20Memory%20Analysis.md):** Analyzes the time and memory complexities of Vectors, Hash Tables, and Binary Search Trees (BST) to determine the most optimal data structure for loading, searching, and sorting academic course data.
2. **[Binary Search Tree Implementation (Project Two)](ProjectTwo.cpp):** The completed C++ program utilizing a BST to read course data from a CSV file, validate prerequisites, search for specific courses, and print a fully ordered alphanumeric course list.

## Reflection

**What was the problem you were solving in the projects for this course?**
The core problem centered on designing an academic advising program for ABCU that could efficiently load a list of courses and their prerequisites from a CSV file. The program needed to store this data in a way that allowed users to quickly look up specific course information and, most importantly, print out a completely sorted alphanumeric list of all available courses.

**How did you approach the problem? Consider why data structures are important to understand.**
I approached the problem by first evaluating three different data structures—Vectors, Hash Tables, and Binary Search Trees—through a Big O runtime and memory analysis. Understanding data structures was critical here because each structure has unique trade-offs. While Hash Tables offer $O(1)$ lookups, they lack inherent sorting. Vectors are memory-efficient but require slow $O(n)$ search and sort algorithms. Ultimately, I selected the Binary Search Tree because it intrinsically maintains alphanumeric sorting upon insertion and provides an efficient $O(\log n)$ lookup, solving both main requirements gracefully.

**How did you overcome any roadblocks you encountered while going through the activities or project?**
One significant roadblock was implementing the prerequisite validation logic to ensure that every prerequisite listed for a course actually existed within the dataset. I overcame this by breaking the problem down into modular helper functions and leveraging the BST's traversal capabilities. By stepping through the tree and checking each node's prerequisites against the tree itself using the existing search function, I was able to validate the data without overly complicating the primary logic.

**How has your work on this project expanded your approach to designing software and developing programs?**
This project shifted my focus from just writing code that "works" to writing code that scales efficiently. By mathematically analyzing the Big O complexities before writing the final application, I learned the importance of foundational planning. My approach now involves heavily prioritizing the selection of the correct underlying data structure during the design phase to avoid complex workarounds later.

**How has your work on this project evolved the way you write programs that are maintainable, readable, and adaptable?**
Working with pointers and complex BST node structures forced me to adopt strict organizational habits. I learned to encapsulate logic into distinct, single-purpose functions (such as separate methods for in-order traversals, insertions, and formatting). Incorporating clear comments and consistent naming conventions was essential to tracking node references without confusion. These practices have made my code significantly more maintainable and easier for other developers to read and adapt.
