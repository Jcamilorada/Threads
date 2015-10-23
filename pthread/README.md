This folder contains the answer to proposed questions and source code with solutions of issues and coding examples presented at https://github.com/josanabr/distributedsystems/tree/master/threads

1. 3s-00 file including modifications to determinate how much time the program spend during

    -  Vector initialization.
    -  Counting the number of 3s in a given vector.

2. 3s-01.c file

    - How many elements the large vector has?
    R/ The largest vector has length/max_threads, as max_threads = 8 and the vector size is 1000000000, that means 125000000.

    - The program is correct? What is wrong with it? What value the program gets and what is the expected value?
    R/ No, the program is wrong, as the sentence count ++ is equivalent to count = count + 1. The issue is that one thread execution can override another count value, that because race condition.
The expected value is 50010522 and the result is 1655.
