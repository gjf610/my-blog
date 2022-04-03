```js
const numbers = [1, 6, 7, 9, 10]
const target = 8
const twoSum = (numbers, target) => {
  const map = {}
  for (let i = 0; i < numbers.length; i++) {
    const num1 = numbers[i]
    const num2 = target - num1
    if (num2 in map) {
      const num2Index = map[num2]
      return [i, num2Index]
    } else {
      map[num1] = i
    }
  }
  return []
}

console.log(twoSum(numbers, target))
```