# Assignment W3-4>1/3 Sorting-I
## Lab Objective
My lab objective is understanding how Big-O notation is used to measure algorithm efficiency. This helped me gain understanding on what it means to distinguish between linear, and quadratic complexity, nested loops, constant operations, and sequential loops that have an impact on the algorithm’s effectiveness.

## W3-4>1/3 Sorting-I Assignment Code
```ruby
def double_then_sum(array)
  doubled_array = []

  array.each do |number|
    doubled_array << number * 2
  end

  sum = 0

  doubled_array.each do |number|
    sum += number
  end

  return sum
end

def multiple_cases(array)
  array.each do |string|
    puts string.upcase
    puts string.downcase
    puts string.capitalize
  end
end

def every_other(array)
  array.each_with_index do |number, index|
    if index.even?
      array.each do |other_number|
        puts number + other_number
      end
    end
  end
end

puts "Double Then Sum:"
puts double_then_sum([1, 2, 3, 4])

puts "\nMultiple Cases:"
multiple_cases(["Hello", "WORLD", "Ruby"])

puts "\nEvery Other:"
every_other([1, 2, 3, 4])
```

## Analysis and Reflection

### 1. Why are constants normally ignored in Big-O notation?

Because Big-O notation concentrates on how the amount of labor increases as the input size increases, constants are disregarded. For example if there are 4 constants, and 16 does not alter the general growth pattern, then 4N + 16 is O(N). The complexity is determined by the N term.

### 2. What is the difference between O(N) and O(N²) growth?

The difference is O(N) is linear growth meaning that the workload grows around the same rate as the input size. Showing how the quantity of effort doubles if N doubles. Quadratic growth or O(N^2), grows more quickly. As the quantity of effort rises around four times if N doubles. Representing how the dataset grows when O(N^2) becomes significantly less efficient versus O(N).

### 3. Why can sequential loops and nested loops result in different time complexities?

They execute one after another sequential loops, which typically add their operations together. As an example if two loops run N times apiece the total is N + N = 2N, reducing to O(N). This is because the inner loop executes for every iteration to the outer loop, nested loops, which are distinct. The total is N * N = N^2, meaning when both loops run N times, yields O(N^2).

### 4. Why does understanding time complexity become increasingly important as the size of a dataset grows?

As the dataset expands, time complexity becomes important because inefficient algorithms would need a lot more operations. Compared to an O(N^2) algorithm, an O(N) algorithm evolves more slowly. The difference may not be noticeable for small datasets. However, an inefficient algorithm can take longer to finish when dealing with huge datasets. Especially Big-O because they make it easier for programmers to select algorithms that can effectively manage larger datasets.

### Flowchart
<img width="642" height="319" alt="W3-4_1_3 Sorting-I drawio" src="https://github.com/user-attachments/assets/c32113ff-f824-4c40-a594-4519ea3d8927" />

## Challenges
Some of the challenges I’ve experienced were distinguishing between nested, and sequential loops. Since, first I needed to consider why two loops didn’t always imply O(N). Then discovering nested loops quadruple the amount of effort, while sequential loops combine together. Then understanding why constants are disregarded in Big-O notation. And, at last was seeing how Big-O is focussed on how the algorithm grows as the input size becomes larger instead of the same exact number of the operations.

## My Assignment W3-4>1/3 Sorting-I Video
https://drive.google.com/file/d/1AThmmKi4YNdTYk1SMQ0F5zcvHdpNzyv_/view?usp=sharing
