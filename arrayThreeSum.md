# Array Three Sum


## Learning Objective
* Apply previous solutions to increasingly complex problems


## Interviewer Prompt
Given an array of distinct integers and an integer representing a target sum, write a function that returns an array of all triplets in the input array that sum to the target sum.


### Examples

```javascript
arrayThreeSum([12, 3, 1, 2, -6, 5, -8, 6], 0)   //should return [[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]
arrayThreeSum([5, 6 , 1, -9 , 7, 3 , 2], 35)    //should return []
arrayThreeSum([1, 15, -5, 12 , -3, 6 , 2], 10)  //should return [[ -3, 1, 12 ]]
```

## Interviewer Strategy Guide
Let's think about time complexity.  If your interviewee tries to go down the path of trying all possible combintations of three integers, that will have a time complexity of O(n^3).  Try to encourage them to find a more optimal approach.

### Hints
- The input array is NOT sorted, we still can sort the input array and then use that sorted array to help us optimize the solution.
- Suggest to the interviewee to think of how they would handle optimizing this problem if they only had to sum two integers.  A three sum can be thought of as trying to find if two integers sum to target valued added to each integer in the input array.

For example:
```javascript
arrayThreeSum([11, -1, -10], 0) 
//can be broken down to the below:

//checking if 11 is going to be one of the integers: 11 + ? + ? = 0
arrayTwoSum([-1,-10], 11 + 0)

//checking if -1 is going to be one of the integers: -1 + ? + ? = 0
arrayTwoSum([11,-10], -1 + 0)

//checking if -10 is going to be one of the integers: -10 + ? + ? =0
arrayTwoSum([11,-1], -10 + 0)
```

## Solutions

**Optimized Solution 1**: O(n^2) time complexity, O(n) space complexity
```javascript
function arrayThreeSum(arr, targetSum){

  //sorts the input arr from least to greatest
  arr.sort((a, b) => a-b)

  const solution = []

  for (let i = 0; i < arr.length-2; i++){
    let element = arr[i]
    let leftIndex = i + 1
    let rightIndex = arr.length - 1

    //for each element in the array check to see if any two other integers in the array add to the target sum
    while(leftIndex < rightIndex){
      let currentSum = element + arr[leftIndex] + arr[rightIndex]

      //if the currentSum is equal to the target sum add an array of those 3 integers to the solution array
      if(currentSum === targetSum){
        solution.push([element, arr[leftIndex], arr[rightIndex]])
        leftIndex ++
        rightIndex --
      } else if(currentSum > targetSum){
        rightIndex --
      } else if (currentSum < targetSum){
        leftIndex ++
      }
    }
  }
  return solution
}
```


**Optimized Solution 2**: O(n^2) time complexity, O(n) space complexity
- Note that this solution does not return a sorted solution like Solution 1

```javascript
function arrayThreeSum(arr, targetSum){
  const solution = []
  for(let i = 0; i< arr.length-1; i++){
    //the sum needed given we already know one element arr[i]
    let currentSum = targetSum - arr[i]
    //create a hash table to store all of the integers from arr[i] we have tried
    let memo ={}
    for (let j = i+1; j < arr.length; j++){
      if(memo[currentSum-arr[j]]){
        solution.push([arr[i], currentSum-arr[j], arr[j]])
      }else{
        memo[arr[j]] = true
      }
    }
  }
  return solution
}
```

## Resources
_Feel free to PR any useful resources! :)_

* [Sample Slides](https://docs.google.com/presentation/d/1LhdgluKGpMBijw7B9FlMrTyX0IN6FgFuT4ezNr0W2Cc/edit#slide=id.g99d99f4895_0_2987)
* [AlgoExpert Link](https://www.algoexpert.io/questions/Three%20Number%20Sum)

## Jessica's Exhaustive Comments:
```javascript
//POINTERS:
function arrayThreeSum (arr, targetSum) {
  //sorts our array from least to most
  arr.sort((a, b) => a-b)
  //initialize our return array
  let solution = [];
  //We're going to loop through the array--if we know we'll be initializing a pointer at the end of the array, then we know we don't need to check the element at length-1 because our end pointer will catch it.  So i only has to go to length-2
  for (let i = 0; i < arr.length-2; i++) {
    //first pointer
    let element = arr[i];
    //second pointer
    let leftIndex = i + 1;
    //third pointer
    //element    leftIndex                 rightIndex
    //[1,        3,         5, 6, 8, 11,    97]
    let rightIndex = arr.length-1;
    //while our inside pointers do not yet overlap
    while (leftIndex < rightIndex) {
      //what's the current sum of all our pointers?
      let currentSum = element + arr[leftIndex] + arr[rightIndex]
      //if our current sum is the input sum we're looking for
      if (currentSum === targetSum){
        //push all current integers into our solution array
        solution.push([element, arr[leftIndex], arr[rightIndex]]);
        //move each inside pointers toward each other
        leftIndex++
        rightIndex--
      } else if (currentSum > targetSum) {
        //if our current pointer position has given us a sum that's too large, we'll move the right pointer down to lower integers
        rightIndex--;
      } else if (currentSum < targetSum){
        //if our current position gives us a sum that's too small, we'll move the left pointer up to larger integers.
        leftIndex++;
      }
    }
  }
  //if we didn't find any, we just return that empty array we initialized!
  return solution;
}

//MEMO:
function arrayThreeSum (array, sum) {
  //initializes an empty array in which we can return our solution
  const solution = [];
  //we set up an initial loop to find our first value
  for (let i = 0; i < array.length-1; i++){
    //Okay. If we already have arr[i]; what's our new target sum?
    //aka the sum we're going to have given we already know one element
    let currentSum = sum - array[i]
    //initialize our memo!
    let memo = {};
    //same as pairSum--we never want array[i] === array[j] so we initialize it one index after i
    for (let j = i + 1; j < array.length; j++) {
      //if our memo ALREADY CONTAINS the remainder that we need to get our sum
      if (memo[currentSum-array[j]]){
        //we push integers at our locations in each loop, AND the remainder integer that we found in our memo
        solution.push(array[i], currentSum-array[j], array[j])
      } else {
        //if we don't find our remainder in the memo, we'll add the value at j into our memo for use later in our loop(s)!
        memo[array[j] = true]
      }
    }
  }
}
```
