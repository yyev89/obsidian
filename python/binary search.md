ver 1:

```python
def binary_search(list, target):
    first = 0
    last = len(list) - 1
    while first <= last:
        midpoint = (first + last)//2
        if list[midpoint] == target:
            return midpoint
        elif list[midpoint] < target:
            first = midpoint + 1
        else:
            last = midpoint - 1
    return None

def verify(index):
    if index is not None:
        print(f'Target found at index: {index}')
    else:
        print('Target not found in list')
```

ver 2:

```python
def recursive_bs(list, target):
    if len(list) == 0:
        return False
    else:
        midpoint = (len(list))//2
        if list[midpoint] == target:
            return True
        else:
            if list[midpoint] < target:
                return recursive_bs(list[midpoint+1:], target)
            else:
                return recursive_bs(list[:midpoint], target)

def verify(result):
    print(f'Target found: {result}')
```