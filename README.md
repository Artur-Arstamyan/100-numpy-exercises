## 100 numpy exercises

[![Binder](http://mybinder.org/badge.svg)](http://mybinder.org:/repo/rougier/numpy-100/notebooks/100%20Numpy%20exercises.ipynb)

This is a collection of numpy exercises from numpy mailing list, stack overflow, and numpy documentation. I've also created some problems myself to reach the 100 limit. The goal of this collection is to offer a quick reference for both old and new users but also to provide a set of exercises for those who teach. For extended exercises, make sure to read [From Python to NumPy](http://www.labri.fr/perso/nrougier/from-python-to-numpy/).

→ [Test them on Binder](http://mybinder.org:/repo/rougier/numpy-100/notebooks/100_Numpy_exercises.ipynb)  
→ [Read them on GitHub](100_Numpy_exercises.md)  

Note: markdown and ipython notebook are created programmatically from the source data in `source/exercises.ktx`.
To modify the content of these files, please change the text in the source and run the `generators.py` module with a python
interpreter with the libraries under `requirements.txt` installed.

The keyed text format (`ktx`) is a minimal human readable key-values to store text (markdown or others) indexed by keys. 

This work is licensed under the MIT license.  
[![DOI](https://zenodo.org/badge/10173/rougier/numpy-100.svg)](https://zenodo.org/badge/latestdoi/10173/rougier/numpy-100)


### Variants in Other Languages

 - **Julia**: [100 Julia Exercises](https://github.com/RoyiAvital/Julia100Exercises).

# More knowledge from exercises  
## Exercise N9 - Code proof of a statement that if an array a is Fortran-contiguous, it's stored in column-major order in memory
```python
import ctypes  
import numpy as np  

arr = np.arange(9).reshape(3, 3, order='F')  
print(arr, arr.dtype)  

for i in range(9):  
    # Get the memory address of i_th element  
    memory_address = arr.ctypes.data + i * arr.itemsize  

    # Access the value at the given memory address using ctypes  
    value = ctypes.cast(memory_address, ctypes.POINTER(ctypes.c_int32)).contents.value  
    
    print(f"Memory address of {value}: {memory_address}")  
```

### Output
```
[[0 3 6]  
 [1 4 7]  
 [2 5 8]] int32  
Memory address of 0: 2426869300128  
Memory address of 1: 2426869300132  
Memory address of 2: 2426869300136  
Memory address of 3: 2426869300140  
Memory address of 4: 2426869300144  
Memory address of 5: 2426869300148  
Memory address of 6: 2426869300152  
Memory address of 7: 2426869300156  
Memory address of 8: 2426869300160
```
## Exercise N12 - np.random.rand vs np.random.random  
Syntax:  
- np.random.rand takes separate arguments for each dimension (e.g., np.random.rand(2, 3)).    
- np.random.random takes a tuple as its size argument (e.g., np.random.random((2, 3))).  

Flexibility:  
- np.random.rand is slightly more convenient for quick use when you want to specify dimensions without needing to wrap them in a tuple.  
- np.random.random is more flexible if you are dynamically setting the shape using a tuple, which might be useful in certain programming contexts.
## Exercise N17 - Floating-point numbers in Python  
1. **`0 * np.nan`**:
   - **Result**: `nan`
   - **Explanation**: In floating-point arithmetic, any operation involving `NaN` results in `NaN`, regardless of the other operand. So multiplying `0` by `NaN` results in `NaN`.

   ```python
   import numpy as np
   print(0 * np.nan)  # nan
   ```
2. **`np.nan == np.nan`**
   - **Result**: `False`
   - **Explanation**: According to the IEEE 754 standard for floating-point arithmetic, `NaN` is **not equal** to anything, including itself. Therefore, the expression `np.nan == np.nan` evaluates to `False`.

   ```python
   import numpy as np
   print(np.nan == np.nan)  # False
   ```
3. **`np.inf > np.nan`**
   - **Result**: `False`
   - **Explanation**: Comparisons involving `NaN` always return `False`. Even though `np.inf` represents infinity, any comparison with `NaN` results in `False` because `NaN` is not comparable.

   ```python
   import numpy as np
   print(np.inf > np.nan)  # False
   ```
4. **`np.nan - np.nan`**
   - **Result**: `nan`
   - **Explanation**: Any arithmetic operation involving `NaN` results in `NaN`. Therefore, subtracting `NaN` from `NaN` still gives `NaN`.

   ```python
   import numpy as np
   print(np.nan - np.nan)  # nan
   ```
5. **`np.nan in set([np.nan])`**
   - **Result**: `True`
   - **Explanation**: Although `np.nan != np.nan`, Python's `set` checks membership using object identity (the `is` operator). Since the `NaN` in the set is the same object as the one being checked, the membership test returns `True`.

   ```python
   import numpy as np
   print(np.nan in set([np.nan]))  # True
   ```
6. **`0.3 == 3 * 0.1`**
   - **Result**: `False`
   - **Explanation**: Due to floating-point precision errors, the expression `3 * 0.1` does not exactly equal `0.3`. Instead, it evaluates to `0.30000000000000004`, which is not equal to `0.3`. This is a common issue in floating-point arithmetic which can be mitigated by using functions like ```math.isclose()``` or ```numpy.isclose()``` to compare floating-point numbers with a tolerance.
   ```python
   print(0.3 == 3 * 0.1)
   # Output: False
   ```
   ```python
   import math  
   print(math.isclose(0.3, 3 * 0.1))  
   # Output: True
   ```
## Exercise N26 - Python sum(range(5), -1) vs NumPy's sum(range(5), -1)
1. **Using the built-in Python `sum` function:**
   ```python
   print(sum(range(5), -1))
   ```  
   - `range(5)` generates the numbers `[0, 1, 2, 3, 4]`.  
   - The `sum` function adds these numbers together, starting with the initial value of `-1` (the second argument).  
   - Calculation: `0 + 1 + 2 + 3 + 4 + (-1) = 10 - 1 = 9`.  
2. **Using NumPy's `sum` function:**
   ```python
   from numpy import *  
   print(sum(range(5), -1))
   ```  
   - `range(5)` still generates `[0, 1, 2, 3, 4]`.  
   - When using NumPy's `sum`, the second argument (`-1`) is treated as the `axis` parameter instead of an initial value.
   - Since `range(5)` is a one-dimensional iterable, the `sum` will compute the total of the elements without any influence from the `-1` parameter.
   - Calculation: `0 + 1 + 2 + 3 + 4 = 10`.
## Exercise N27 
```python
import numpy as np
Z = np.array([4, 3, 10, 2], dtype=np.int64)
```
1. **Element-wise Exponentiation**
   ```python
   Z ** Z
   # Output(Each element raised to the power of itself):
   array([         256,          27, 10000000000,           4])
   ```
2. **Bitwise Left Shift and Right Shift**
   ```python
   2 << Z >> 2
   # Output: 
   array([  8,  4, 512,  2])
   ```
   - The left shift operation `<<` shifts the bits of the number to the left, effectively multiplying by 2^n, where 𝑛 is the number of positions to shift.
   - Here, `2 << Z` shifts `2` left by each value in `Z`, and then `>> 2` shifts the result back right by 2.    
   Calculating:
   - `2 << 4` is 2×16=32 then 32>>2=8
   - `2 << 3` is 2×8=16 then 16>>2=4
   - `2 << 10` is 2×1024=2048 then 2048>>2=512
   - `2 << 2` is 2×4=8 then 8>>2=2
3. **In-Place Negation Comparison**
   ```python
   Z <- Z
   # Output: 
   array([False, False, False, False])
   ```
   The expression `Z < -Z` checks each element in `Z` to see if it is less than its negation. Since no positive number is less than its negative counterpart, the result is a boolean array of `False` values.
4. **Complex Numbers**
   ```python
   1j * Z
   # Output: 
   array([0.+4.j, 0.+3.j, 0.+10.j, 0.+2.j])
   ```
   This operation creates a complex array with the imaginary unit `1j` multiplied by each element in `Z`.
5. **Division**
   ```python
   Z / 1 / 1
   # Output: 
   array([4., 3., 10., 2.])
   ```
   This operation divides each element in `Z` by `1` twice, which effectively leaves `Z` unchanged.
6. **Comparison and Logical Operations**
   ```python
   Z < Z > Z
   # Output: 
   ValueError: The truth value of an array with more than one element is ambiguous. Use a.any() or a.all()
   ```
   To compare values in NumPy arrays, check each condition separately or use logical operators for combined checks.
   You cannot directly chain comparisons on NumPy arrays like you can with scalars.
## Exercise N46 - Structured Arrays  
**Key Features of a Structured Array:**  
- **Fields:** A structured array can have named fields (like columns in a table), and each field can store data of different types.
- **Heterogeneous Data:** The fields can have different types, such as integers, floats, strings, etc., in the same array.
- **Accessing Fields:** You can access individual fields by their names, which makes the data easy to work with.
Structured arrays are defined by specifying a `dtype` that lists the field names and corresponding data types.
```python
import numpy as np

# Define the dtype with field names and types
dtype = [('name', 'U10'), ('age', 'i4'), ('weight', 'f4')]

# Create a structured array with that dtype
data = np.array([('Alice', 25, 55.0), 
                 ('Bob', 30, 85.5), 
                 ('Charlie', 35, 77.2)], dtype=dtype)
**data[1][1] = 11**
# Print the structured array
print(data) 
print(data.dtype)
**print(data[1][1].flags.writeable)**
```
### Output
```
[('Alice', 25, 55. ) ('Bob', **11**, 85.5) ('Charlie', 35, 77.2)]
[('name', '<U10'), ('age', '<i4'), ('weight', '<f4')]
**False**
'''
