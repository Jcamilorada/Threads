This folder contains the answer to proposed questions and source code with solutions of issues and coding examples presented at https://github.com/josanabr/distributedsystems/tree/master/threads

1. 3s-00 file including modifications to determinate how much time the program spend during

    -  Vector initialization.
    -  Counting the number of 3s in a given vector.

    R/ We add the function clock that measures the time (check code).
    - Testing Data Times
        - Execution 1 : 2.849077
        - Execution 2 : 2.767444
        - Execution 3 : 2.840893

2. 3s-01.c file

    - How many elements the large vector has?
    R/ The largest vector has length/max_threads, as max_threads = 8 and the vector size is 1000000000, that means 125000000.

    - The program is correct? What is wrong with it? What value the program gets and what is the expected value?
    R/ No, the program is wrong, as the sentence count ++ is equivalent to count = count + 1. The issue is that one thread execution can override another count value, that because race condition. The expected value is 50010522 and the result is 1655.

    - Testing Data Times
        - Execution 1 : 0.000987
        - Execution 2 : 0.001925
        - Execution 3 : 0.000459

3. The code was modified to keep count in independents cache lines as the cache coherence is maintained per line. A data structure is included to separate caches lines between private[1] and private[0].

    The solution behaoviur is similar when one thread is used but is worst as thread is increased. The result suggest that more research is necessary to adapt algorithm to computed architecture.

    - Testing Data Times
        - Execution 1 (1 Theread) : 2.770608
        - Execution 2 (2 Thread): 3.537544
        - Execution 3 (3 Thread): 3.984579
