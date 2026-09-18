# Assignment W3-4>2/3 Sorting II
## Lab Objective
The lab objective is to understand how insertion sort algorithm’s operation works. Recognizing that insertion sort splits an array into sorted, and unsorted halves, and that the algorithm may exhibit O(N^2) behavior because of the comparisons, and shifts. This helped me discover why the algorithm is able to sort the entire array, and is affected by altering the insertion sort’s starting point. And, refining a search algorithm to improve it over time. Learning how target characters are located to minimize the number of operations carried out.

## Assignment Code W3-4>2/3 Sorting II
```ruby
def insertion_sort_start_at_one(array)
  i = 1

  while i < array.length
    key = array[i]
    j = i - 1

    while j >= 0 && array[j] > key
      array[j + 1] = array[j]
      j -= 1
    end

    array[j + 1] = key
    i += 1
  end

  return array
end

array = [5, 4, 3, 2, 1]

puts "Original array:"
puts array.inspect

puts "Sorted array:"
puts insertion_sort_start_at_one(array).inspect
```

## Analysis and Reflection
### 1. Why does insertion sort exhibit quadratic behavior in the average and worst cases?
Because the array grows in size as insertion sort may shift the number of comparisons necessary to compare with many elements in the sorted section during each cycle. This is necessary to examine about half of the sorted portion. This can be shown as a formula of:
1 + 2 + 3 + ... + (N - 1)
The average-case time complexity is O(N^2), and can increase to N^2.
Representing how every new key must go through the entire sorted portion from the worst scenario such as the array in descending order. We can write it as:
1 + 2 + 3 + ... + (N - 1) = N(N - 1) / 2
Which is why O(N) is the worst-case complexity.
### 2. Why is starting at i = 1 important to the correctness of insertion sort?
Because insertion is important to assume that the array’s section before index I has already been sorted. Only A[0], should be present in the section preceding it with i = 1, which is sorted as a single element already sorted in order. Allowing the sorted portion to grow by one element at a time until the array is sorted entirely. Elements that do not need to be necessarily included into the sorted area are skipped starting at i = 2, and i = 3. This becomes a result as the algorithm no longer ensures the complete array will be sorted.
### 3. What is the difference between reducing operations and reducing Big-O complexity?
First, an algorithm can be made faster by reducing the number of operations without affecting its overall Big-O complexity. As an example the number of comparisons when “X” appears closer to the beginning can be significantly decreased by altering a search algorithm so that it terminates when “X” is detected. Meaning, the algorithm would continue looking at all the N characters if “X” is at the end or doesn’t exist.  Improving the search execution with fewer operations in many scenarios for O(N) even if the program performs with fewer operations.
### 4. Why can two implementations with the same worst-case Big-O complexity perform differently in practice?
The reason is Big-O does not describe every single operation instead introducing how an algorithm expands the size of the input as it increases. As an example both containsX variants is O(N). But, because “X” is discovered the upgraded version stops right away. Making fewer comparisons than the original version of “X” appearing close from the beginning. Explaining how two algorithms can perform differently even if they have the same Big-O complexity.

## Flowchart
<img width="782" height="539" alt="W3-4_2_3 Sorting II(1)" src="https://github.com/user-attachments/assets/e4568790-399e-431e-bdc6-7fa46a7975c9" />

## Challenges
The challenges I've experienced was first understanding how insertion sort starts with i = 1. Because the method relies with the first element in the present in the first sorted section. Which is why the beginning at i =2 or i = 3 could minimize the number of operations while making the method incorrect was another challenge. I corrected this by skipping iterations meaning some elements could never be properly inserted into the sorted portion. This helped me discover how the algorithm’s Big-O complexity does not necessarily need to be altered by decreasing the number of operations. But, I could enhance the search algorithm for O(N) by stopping early, and doing a lot less work in practice.

## Assignment W3-4>2/3 Sorting II Video
https://drive.google.com/file/d/1ZNmCnJZ-InNihJepJYxk6Z1qKWfKAxkY/view?usp=sharing
