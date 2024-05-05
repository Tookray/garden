>[!info] Use case
>Finding the maximum (or minimum) sum subarray.

## Kadane's algorithm

Kadane's algorithm is used to find the maximum (or minimum) sum (non-empty) subarray ending at a particular position of an array.

```python
def kadane(A):
	best = float('-inf')
	current = 0

	for a in A:
		current = max(current + a, a)
		best = max(best, current)

	return best
```

There is a trivial extension that allows empty subarrays (whose sum defaults to `0`).

```python
def kadane(A):
	best = 0
	current = 0

	for a in A:
		current = max(current + a, 0)
		best = max(best, current)

	return best
```

## Sliding window algorithm

The <mark style="background: #FFB86CA6;">sliding window</mark> algorithm the natural extension of Kadane's algorithm, and it helps us find the actual subarray containing the maximum (or minimum) sum.

```python
def sliding_window(A):
	max_sum, current_sum = A[0], 0

	max_l, max_r = 0, 0

	l = 0

	for r, a in enumerate(A):
		if current_sum < 0:
			current_sum = 0
			l = r

		current_sum += a

		if current_sum > max_sum:
			max_sum = current_sum

			max_l, max_r = l, r

	return A[max_l:max_r + 1]
```

>[!note]
>There is a <mark style="background: #FFB86CA6;">variation of sliding window</mark> (well, more like more constraints) where you are looking for the maximum sum subarrays between size $a$ and $b$. This problem is CSES maximum subarray sum 2, and I applied the same idea to solve [[918. Maximum Sum Circular Subarray]].
