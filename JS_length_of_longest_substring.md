```js
const lengthOfLongestSubstring = (str) => {
  if (str.length <= 1) return str.length
  let max = 0
  let p1 = 0
  let p2 = 1
  while (p2 < str.length) {
    let sameIndex = -1
    for (let i = p1; i < p2; i++) {
      if (str[i] === str[p2]) {
        sameIndex = i
        break
      }
    }
    let tempMax
    if (sameIndex >= 0) {
      tempMax = p2 - p1
      p1 = sameIndex + 1
    } else {
      tempMax = p2 - p1 + 1
    }
    if (tempMax > max) {
      max = tempMax
    }
    p2 += 1
  }

  return max
}

console.log(lengthOfLongestSubstring("abcabcbb"))
```