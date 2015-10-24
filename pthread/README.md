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
3. 3s-02.c file
    Include the code sentences used to estimate how much time takes to each thread to count the number of 3s in a given array.
        Thread [3] elapsed time: 13043.142578 
        Thread [5] elapsed time: 13301.642578 
        Thread [2] elapsed time: 13759.101562 
        Thread [7] elapsed time: 13777.224609 
        Thread [1] elapsed time: 14092.099609 
        Thread [6] elapsed time: 14111.096680 
        Thread [4] elapsed time: 14151.321289 
        Thread [0] elapsed time: 14173.210938 
    What is wrong with this code this time?
        This code allows the access to multiple threads to the count variable, this is going to result in serious issues related to the non-determinism of the operations done over the count variable.
4. 3s-03.c file
    Identify why this program is now doing right.
    
        This program is now doing right because it has a mutex (Mutual Exclusion) strategy implemented. The segments of code that affects shared resources between threads are preceded by a lock statement (pthread_mutex_lock(&mutex);) and suceded by a unlock statement (pthread_mutex_unlock(&mutex);). This way the resource is locked by the thread using it until it finish the processing. In this particular case the mutex strategy implemented lock the code segment where the array is accessed and the count variable is modified.
    
    Include the code sentences used to estimate how much time takes to each thread to count the number of 3s in a segment of the full array.

        Thread [0] elapsed time: 1280.357910 
        Thread [0] exited with status [38]
        Thread [1] elapsed time: 2564.146973 
        Thread [1] exited with status [38]
        Thread [2] elapsed time: 3824.508057 
        Thread [2] exited with status [38]
        Thread [3] elapsed time: 5124.394043 
        Thread [3] exited with status [38]
        Thread [4] elapsed time: 6472.359375 
        Thread [4] exited with status [38]
        Thread [5] elapsed time: 7786.957031 
        Thread [5] exited with status [38]
        Thread [6] elapsed time: 9089.784180 
        Thread [6] exited with status [38]
        Thread [7] elapsed time: 10385.375000 
        Thread [7] exited with status [39]
        [3s-03] Count by threads 49991499
        [3s-03] Double check 49991499
        [[3s-03] Elapsed time 10388.507812
    
    How much time took to obtain the total count of 3s in the whole array.
    
    4154.7355954 average time for 5 executions
5. 3s-04.c file
    What is the difference between 3s-03.c and 3s-04.c?
        In this case the mutex only includes the variable count, this is much more effective than the 3s-03 strategy. The count variable is the only one that needs to be updated by several threads concurrently, the vector only has to be accessed but not modified by the threads so it can be accessed safely by several threads in parallel, that is not the case of the count variable, that has to be accessed by one thread at a time. In this implementation the lock and unlock functions are called in every position of the vector.

    Compare elapsed time per thread during the counting process and the total time that all threads took for counting the number of 3s in the whole array. Run each program (3s-03 and 3s-04) three times and compute the average time per program. Present your results and explain them.
        3s-03: 3981.39803033 vs. 3s-04: 269316.333333

        In the program 3s-03 the time elapsed per thread during the counting process is the result of waiting for all the predecessors threads to finish their counting and the time each thread takes in count its segment of the vector. This is because the mutex strategy prevents that several threads access the array in parallel. 

        The execution of the program 3s-04 takes several times the time it takes the execution of the program 3s-03. There is two main reasons for this issue the overhead of using the lock and unlock functions every time and the  massive interruptions that have to make each thread waiting the release of the count variable. Each thread no matter when it starts have to wait almost all the execution to finish the counting. 




6. 3s-05.c file
    9211.3268554 in average
    Where the success of this program resides? Compare this program execution and compare its performance with previous instances and write your observations.

        This program has lesser lock and unlocks operations than the 3s-03 and much lesser that the 3s-04. In this implementation every thread has a private count that prevents the issues of accessing the count variable in parallel. The count variable is only accessed every time each thread finish the counting of its segment. This way the array can be accessed in parallel and there is not overhead of using the lock and unlock functions in every iteration of the counting loop. It also prevents the massive interruptions presented in the 3s-04 program.

        The elapsed time peer each thread for the counting process doesn't depend on the other threads processing. That is a huge difference with the previous implementation of the mutex strategy. The independence of the threads in the counting process give us the possibility of exploit all the potential of the parallel processing.
        
7. The code was modified to keep count in independents cache lines as the cache coherence is maintained per line. A data structure is included to separate caches lines between private[1] and private[0].

    The solution behaoviur is similar when one thread is used but is worst as thread is increased. The result suggest that more research is necessary to adapt algorithm to computed architecture.

    - Testing Data Times
        - Execution 1 (1 Theread) : 2.770608
        - Execution 2 (2 Thread): 3.537544
        - Execution 3 (3 Thread): 3.984579
