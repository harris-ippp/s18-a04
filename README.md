# Assignment 4: Matching, Simulation, and Plotting

In this assignment you will build up the Gale-Shapely matching algorithm presented in class to be a reusable function and use simulation to investigate its properties.

## Part A: Reusability
1. In a file called `helper.py` write a function called `get_ranks(prefs)` that takes a dictionary of hospital preference lists and returns a dictionary of rank dictionaries. (4 points)

    You can test your function as following:

    ```python
    >>> import helper
    >>> hospital_prefs = prefs = {
    ... 'X': ['B', 'A', 'C'],
    ... 'Y': ['A', 'B', 'C'],
    ... 'Z': ['A', 'B', 'C']
    ... }
    >>> helper.get_ranks(prefs)
    {'Y': {'A': 1, 'C': 3, 'B': 2}, 'X': {'A': 2, 'C': 3, 'B': 1}, 'Z': {'A': 1, 'C': 3, 'B': 2}}
    ```
    (Remember that dictionaries are unordered so the order of your output may differ.)
    
    Hint: Iterate over each hospital `h` in `prefs`. Then iterate `i` from 0 to `len(prefs[h])` (use the `range` function). Now you know that `h` ranks `prefs[h][i]` with `i+1`.
    
2. In `helper.py` write a function `match(hospital_prefs, student_prefs, students)` that returns a stable match dictionary. (4 points)

    The included `test_match.py` tests your `match()` function using the same example from class. You can run it at the command line using `$ python test_match.py`.

    Hint: take the code included `lecture_example.py` and remove the hard-coded data (students, hospitals, student_prefs, hospital_prefs, hospital_ranks). Call your `get_ranks` function to create the `hospital_ranks` dictionary. Return the resulting match instead of printing it.
    
3. Add code to `match.py` to validate `student_prefs`, `hospital_prefs`, and `students` have the following properties:

    - The sudents specified in `students` are the same as those in `student_prefs`.
    - The number of students and hospitals are the same
    - Each preference list (both student and hospital) is the right length
    - Each student preference list contains all hospitals
    - Each hospital preference list contains all students
    
    If any of these conditions is false raise a `ValueError` with an informative message.
    
## Part B: Simulation

1. Write a function `generate_preferences(n)` in `helper.py` that generates a preference dictionary of length `n` where the keys are the numbers `0` to `n` and the values are i.i.d. random preferences. (4 points)

  The included `test_match_random.py` runs `match()` with random preferences generated by your `random_preferences()` function with `n=100`. You can run it at the command line using `$ python test_match_random.py`.

  Hint: use `range(n)` to iterate and use `list(np.random.permutation(n))` to generate a random preference list.

2. Write a function `get_hospital_match_ranks(M, hospital_prefs)` that, given a matching `M` and hospital preferences dictionary `hospital_prefs` returns a dictionary whose keys are hospitals and values are the rank of the hospital's match. Write an analogous function `get_student_match-ranks(M, student_prefs)`. (4 points)

    For example, you can test `get_hospital_match_ranks` in the `n=3` example from class as follows:
   
    ```python
    >>> import helper
    >>> helper.get_hospital_match_ranks({'Y': 'B', 'X': 'A', 'Z': 'C'},
                                        {'X': ['B', 'A', 'C'],
                                         'Y': ['A', 'B', 'C'],
                                         'Z': ['A', 'B', 'C']})
    {'Y': 2, 'X': 2, 'Z': 3}
    ```
3. Write a script called `simulate.py` that simulates the the matching problem one thousand times with `n=100`. For each one store the average hospital and student match ranks. Print the average hospital match rank and student match rank across all simulations.

    For example, your script might look like this:
    ```bash
    $ python simulate.py
    Average hospital match rank: ???
    Average student match rank: ???
    ```
    
    Hint: Initialize empty lists `student_match_avg_rank` and `hospital_match_avg_rank`. Iterate over `i=0` to `1000`. Simulate the matching problem (see `test_match_random.py`). Get the hospital and student rank dictionaries using the functions you wrote in B.3. Calculate the *average* hospital and student ranks by taking the mean over the `values()` of each dictionary. Append the averages to the respective lists (`student_match_avg_rank` or `hospital_match_avg_rank`). Finally, take the average over each list.
