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

## Code proof of a statement that if an array a is Fortran-contiguous, it's stored in column-major order in memory
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

### Output
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
