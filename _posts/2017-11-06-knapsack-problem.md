---
layout: post
date: 2017-11-05
feelings: happy
tags: firebase algods
title: knapsack problem
---

went to a firebase meetup today and got sold on all the firebase things. maybe the ONE weakness is the nosql database.

---

why you cannot solve the knapsack problem with a value per weight thing

```js
function knapsackProblem(items, capacity) {
  // Write your code here.
	let	arr = items.map((item, i) => ({valueperweight: item[0]/item[1], value: item[0], weight: item[1], idx: i}))
								.sort((a, b) => b.valueperweight - a.valueperweight === 0 ? a.weight - b.weight : b.valueperweight - a.valueperweight)
	let finalValue = 0
	let finalPicks = []
	for (let i = 0; i< arr.length; i++) {
		if (arr[i].weight <= capacity) {
			capacity -= arr[i].weight
			finalValue += arr[i].value
			finalPicks.push(arr[i].idx)
		}
	}
	return [finalValue, finalPicks.sort((a,b) => a - b)]
}

// Do not edit the line below.
exports.knapsackProblem = knapsackProblem;
```

this looks fine except for this test case:

```js

it('Test Case #5', function() {
  chai.expect(program.knapsackProblem([[1, 2], [70, 70], [30, 30], [69, 69], [100, 100]], 100)).to.deep.equal([100, [1, 2]]);
});
```

which gets
```js
AssertionError: expected [ 99, [ 2, 3 ] ] to deeply equal [ 100, [ 1, 2 ] ]
```

this is because the opportunity set is not continuous and the holes mean some individually selections are globally maximizing
