# Data Structures and Algorithms

# Big O

Let’s compare two codes. They accomplish the same thing, but in different ways. Big O let’s us mathematically figure out which of the two is better, which one runs more efficiently. 

Time Complexity - **the amount of time taken by an algorithm to run, as a function of the length of the input**.

We don’t measure time complexity in *time* - we measure it in number of operations. Number of operation would be the same on a slow and on a faster computer. Time will be different though. This way time complexity will always be the same on any computer. 

Space complexity - **the total amount of memory space used by an algorithm/program, including the space of input values for execution.**

[The Big O Notation](https://www.notion.so/The-Big-O-Notation-717853ec1e8240338e14c34040faab29?pvs=21)

## Big O: worst case

Ο omicron - worst case - big O

Θ theta - average case

Ω omega - best case

When we measure Big O we always measure the worst case.

## O(n)

Number of operations is proportional to whatever n is.

```jsx
function logItems(n) {
	for (let i=0; i<n; i++) {
		console.log(i)
	}
}

// we pass it a number
// the operation will run 
// as many times 
// as the n we passed
```

![Untitled](Data%20Structures%20and%20Algorithms/Untitled.png)

Vertical Axis: number of operations

Horizontal Axis: n

## Drop Constants

One of the ways to simplify the notation.

```jsx
function logItems(n) {
	for (let i=0; i<n; i++) {
		console.log(i)
	}
	for (let j=0; j<n; j++) {
		console.log(j)
	}
}

// both of these for loops will run n times
// code ran n+n times => so 2n
// so this is O(2n)
// doesn't matter if it's 2n 3n or 100n 
// if there's a constant - we drop it 
```

### Good explanation from [Quora](https://www.quora.com/Why-do-we-leave-the-constants-while-calculating-time-complexity-for-algorithms)

The reason that we don't use constants with big O notation is because, theoretically they don't matter much. What we are calculating with time-complexity is the speed at which something will grow, having a constant on there will be completely irrelevant when a large enough input is used.

Take two algorithms: Bucket Sort, and Bubble Sort. From analysis we can see the Bucket Sort has O(n) whereas Bubble Sort has O(n*n). Let us say, for instance that in practice our Bucket Sorting algorithm has a time constant multiple of 200 whereas our Bubble Sort is just a constant of 2.

So given an input of 30 items our Bucket Sort could be expected to run at something like:

200 * 30 = 6000

and our Bubble Sort at:30 * 30 * 2 = 1200

Well wait a second Bubble Sort runs quicker here, but its complexity is quadratic as compared to Bucket Sorts linear time with a constant. Ok, now lets see what happens with an input size of 1,000,000.

Bucket Sort:200 * 1,000,000 = 200,000,00

Bubble Sort:1,000,000 * 1,000,000 * 2 = 2,000,000,000,000

So as you can see, as the input size gets large enough, the constants aren't really going to mean anything.

## O(n**2)

```jsx
function logItems(n) {
	for (let i=0; i<n; i++) {
		for (let j=0; j<n; j++) {
		console.log(i, j)
		}
	}
}

logItems(10)

// we had 100 outputs 
// the number of items that were output is n*n
```

![Screenshot 2022-09-11 13.57.16.png](Data%20Structures%20and%20Algorithms/Screenshot_2022-09-11_13.57.16.png)

O(n) is better. It will complete the operation in fewer operations.

### Simplifying

```jsx
function logItems(n) {
	for (let i=0; i<n; i++) {
		for (let i=0; i<n; i++) {
			for (let k=0; k<n; k++) {
				console.log(i, j, k)
			}
		}
	}
}

logItems(10)

// the output is 1000
// n*n*n => n**3
// doesn't matter if it's n**5 or n**100
// we will still say it's n**2
```

### Drop non-dominants

```jsx
function logItems(n) {
	for (let i=0; i<n; i++) {
		for (let j=0; j<n; j++) {
		console.log(i, j)
		}
	}
	for (let k=0; k<n; k++) {
		console.log(k)
	}
}

// nested loop - O(n**2)
// the other loop - O(n)
// add => O(n**2 + n)
// n**2 is the dominant 
// n is non-dominant 
// so we can remove the non-dominant 
```

## O(1)

Also referred to as Constant Time

```jsx
function addItems(n) {
 return n+n
}

// there is just one operation 
// the number of operations doesn't change with n
```

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%201.png)

This is the most efficient one. 

## O(log n)

An examples of O(log n) is **divide and conquer algorithm. It** is a strategy of solving a large problem by:

1. breaking the problem into smaller sub-problems
2. solving the sub-problems, and
3. combining them to get the desired output.

![Screenshot 2022-09-11 15.37.18.png](Data%20Structures%20and%20Algorithms/Screenshot_2022-09-11_15.37.18.png)

Very flat, very efficient.

## Different terms for Inputs

```jsx
function logItems(a, b) {
	for (let i=0; i<a; i++) {
		console.log(i)
	}
	for (let j=0; j<b; j++) {
		console.log(j)
	}
}

// you can't say it's O(n) + O(n), because you have two different numbers
// time complexity is O(a + b) 
// you can't simplify it further than that 

function logItems(a, b) {
	for (let i=0; i<a; i++) {
		for (let j=0; j<b; j++) {
		console.log(i, j)
		}
	}
}

// with nested loops you will have O(a * b) 
```

## Arrays

```jsx
[11, 3, 23, 7]

myArray.push(17)

[11, 3, 23, 7, 17]

myArray.pop()

[11, 3, 23, 7]

// push and pop don't change the indexes of the other elements
// push and pop are both O(1)

[11, 3, 23, 7]

myArray.shift()

[3, 23, 7]

myArray.unshift(11)

[11, 3, 23, 7]

// shift and unshift reindex the array
// the time complexity of shift and unshift is O(n)

[11, 3, 23, 7]

myArray.splice(1, 0, 'Hi')

[11, 'Hi', 3, 23, 7]

// even though you are adding to the middle it's still O(n)
```

Let's say we're going to find the number seven in this array. To do that, we'd have to start at the beginning and check every element if it’s seven. You search by value until you find the element you are looking for. Searching by value is O(n). 

But if you search by index and you say, tell me what's at the index of three, those indexes allow you to go directly to that place in memory as an owner of one operation.

From this we can see advantages and disadvantages of arrays:

ADVANTAGE: we have indexes, so we can find something in an array with a million items in it and go to item 400000. The complexity will be O(1). 

DISADVANTAGE: you are adding something to the beginning, you are reindexing everything.

So the big thing that you have to look at when you're looking at a data structure is what are you using this for? If you need to access things by index arrays are a great data structure. But if you're going to be adding and removing a lot of items from the beginning, maybe not the best data structure for you, and maybe you should look at a different data structure, if that is your use case.

But either way, you're making your decision based on Big O.

## WRAP UP

[Know Thy Complexities!](https://www.bigocheatsheet.com/)

![Screenshot 2022-09-11 16.37.19.png](Data%20Structures%20and%20Algorithms/Screenshot_2022-09-11_16.37.19.png)

If n = 100

![Screenshot 2022-09-11 16.37.56.png](Data%20Structures%20and%20Algorithms/Screenshot_2022-09-11_16.37.56.png)

If n = 1000

![Screenshot 2022-09-11 16.33.20.png](Data%20Structures%20and%20Algorithms/Screenshot_2022-09-11_16.33.20.png)

# Data Structures

## Linked List

### Linked Lists vs Array

Array

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%202.png)

Arrays have indexes.

Arrays are in contiguous places in memory.

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%203.png)

Linked List

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%204.png)

LLs don’t have indexes.

LLs are all over the place.

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%205.png)

Link lists also have a variable called head (points to the first item) and tail (points to the last item). Each item points to the next. Last one points to null (**null terminated list**)

### Big O

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%206.png)

### Under the hood

What is a Node made up of? It is both the value and the pointer.

Visual presentation of a Linked List.

![Screenshot 2022-09-21 18.59.20.png](Data%20Structures%20and%20Algorithms/Screenshot_2022-09-21_18.59.20.png)

![Screenshot 2022-09-21 18.59.24.png](Data%20Structures%20and%20Algorithms/Screenshot_2022-09-21_18.59.24.png)

### Constructor

```jsx
class Node {
    constructor(value){
        this.value = value
        this.next = null
    }
}

class LinkedList {
    constructor(value) {
        const newNode = new Node(value)
        this.head = newNode
        this.tail = this.head
        this.length = 1
    }
}

let myLinkedList = new LinkedList(4)
```

Output

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%207.png)

### Add push method

```jsx
class LinkedList {
     constructor(value) {
         const newNode = new Node(value)
         this.head = newNode
         this.tail = this.head
         this.length = 1
     }
 
     push(value) {
         const newNode = new Node(value)
         if (!this.head) {
             this.head = newNode
             this.tail = newNode
         } else {
             this.tail.next = newNode
             this.tail = newNode
         }
         this.length++
         return this
     }
 }
 
 let myLinkedList = new LinkedList(7)
 myLinkedList.push(4)
```

Output

![Untitled](Data%20Structures%20and%20Algorithms/Untitled%208.png)

### Add pop method

```jsx
pop() {
         if(!this.head) return undefined
         let temp = this.head
         let pre = this.head
         
				while(temp.next) {
             pre = temp
             temp = temp.next
         }
         
				this.tail = pre
         this.tail.next = null
         this.length--
         if(this.length === 0) {
             this.head = null
             this.tail = null
         }
         return temp
     }
```

## Stacks

LIFO - last in first out 

## Queue

# Algorithms

A sequence of instructions to solve a problem.

Characteristics:

- clear
- unambiguous
- well defined inputs and outputs
- finiteness
- feasible
- language independent

Types:

- Brute Force
- Recursive
- Searching
- Sorting
- Hashing

## Recursion

It is a function that calls itself… until it doesn’t.

Rules of recursion:

- the process is the same every time
- each time you call it, you make the problem smaller

Base Case - the function doesn’t call itself

Recursive Case - the function has to call itself

```
This'll work correctly
When the ball is found, the function isn't called anymore
function openGiftBox() {
	if (isBall) return ball
	openGiftBox()
}

This'll create an infinite loop and lead to Stack Overflow
function openGiftBox() {
	if (isBall) return ball
	openGiftBox()
}

You don't have a return
so the function won't stop even if the statement is true
so this will still lead to Stack Overflow
function openGiftBox() {
	if (isBall) console.log('ball')
	openGiftBox()
}
```

### Call Stack

```jsx
function funcThree() {
    console.log('Three')
}
    
function funcTwo() {
    funcThree()
    console.log('Two')
}
    
function funcOne() {
    funcTwo()
    console.log('One') 
}
    
funcOne()
```

### Factorial

4! = 4*3*2*1
4! = 4*3!
3! = 3*2!
2! = 2*1!

```jsx
function factorial(n) {
	if(n === 1) return 1
	return n * factorial(n-1)
}

factorial(4) // output: 24
```

## Bubble Sort

**Bubble Sort** is the simplest **[sorting algorithm](https://www.geeksforgeeks.org/sorting-algorithms/)** that works by repeatedly swapping the adjacent elements if they are in the wrong order. This algorithm is not suitable for large data sets as its average and worst-case time complexity is quite high.

```jsx
function bubbleSort(array) {
    for(let i = array.length - 1; i > 0; i--) {
        for(let j = 0; j < i; j++) {
            if(array[j] > array[j+1]) {
                let temp = array[j]
                array[j] = array[j+1]
                array[j+1] = temp        
           }
        }
    }
    return array
}
 

bubbleSort([4,2,6,5,1,3])
```

## Selection Sort

The **selection sort algorithm** sorts an array by repeatedly finding the minimum element (considering ascending order) from the unsorted part and putting it at the beginning.

The algorithm maintains two subarrays in a given array.

- The subarray which already sorted.
- The remaining subarray was unsorted.

In every iteration of the selection sort, the minimum element (considering ascending order) from the unsorted subarray is picked and moved to the sorted subarray.

```jsx
function selectionSort(array) {
    for(let i = 0; i < array.length - 1; i++) {
        let min = i
        for(let j = i+1; j < array.length; j++) {
            if(array[j] < array[min]) {
                min = j
            }
        }
        if(i !== min) {
            let temp = array[i]
            array[i] = array[min]
            array[min] = temp

// [array[min], array[i]] = [array[i], array[min]]
        }
    }
    return array
}

selectionSort([4,2,6,5,1,3])
```

## Insertion Sort

**Insertion sort** is a simple sorting algorithm that works similar to the way you sort playing cards in your hands. The array is virtually split into a sorted and an unsorted part. Values from the unsorted part are picked and placed at the correct position in the sorted part.

### **Characteristics of Insertion Sort:**

- This algorithm is one of the simplest algorithm with simple implementation
- Basically, Insertion sort is efficient for small data values
- Insertion sort is adaptive in nature, i.e. it is appropriate for data sets which are already partially sorted.

O(n)

```jsx
function insertionSort(array) {
    let temp
    for(let i = 1; i < array.length; i++) {
        temp = array[i]
        for(var j = i - 1; array[j] > temp && j > -1; j--) {
            array[j+1] = array[j]
        }
        array[j+1] = temp
   }
   return array
}

insertionSort([4,2,6,5,1,3])
```

## Merge Sort

### Merge Code

```jsx
function merge(array1, array2) {
    let combined = []
    let i = 0
    let j = 0
    while(i < array1.length && j < array2.length) {
        if(array1[i] < array2[j]) {
            combined.push(array1[i])
            i++
        } else {
            combined.push(array2[j])
            j++
        }
    }
    while(i < array1.length) {
        combined.push(array1[i])
        i++
    }
    while(j < array2.length) {
        combined.push(array2[j])
        j++
    }
    return combined
}

merge([1,3,7,8], [2,4,5,6])
```

### Merge Sort

```jsx
function merge(array1, array2) {
    let combined = []
    let i = 0
    let j = 0
    while(i < array1.length && j < array2.length) {
        if(array1[i] < array2[j]) {
            combined.push(array1[i])
            i++
        } else {
            combined.push(array2[j])
            j++
        }
    }
    while(i < array1.length) {
        combined.push(array1[i])
        i++
    }
    while(j < array2.length) {
        combined.push(array2[j])
        j++
    }
    return combined
}

function mergeSort(array) {
    if(array.length === 1) return array

    let mid = Math.floor(array.length/2)
    let left = array.slice(0,mid)
    let right = array.slice(mid)
    
    return merge(mergeSort(left), mergeSort(right))
}

mergeSort([3,1,4,2])
```

## Binary Search