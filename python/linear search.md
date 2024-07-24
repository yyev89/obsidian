```python
def linear_search(list, target):
	for i in range(0, len(list)):
		if list[i] == target:
			return i
	return None

def verify(index):
	if index is not None:
		print(f'Target found at index: {index}')
	else:
		print('Target not found in list')
```