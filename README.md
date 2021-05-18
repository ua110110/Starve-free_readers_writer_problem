# Starve-Free Readers Writers Problem
## Problem Statement
We have a "critical section" where multiple writers are allowed, as well as multiple readers, but at no time should readers and writers be allowed in at the same time. We want a deadlock free, starvation free solution.

## Solution 
Let's see a writer process. First the process will check the number of current and queued readers. If both, the number of queued readers and number of current readers are zero or their sum is zero, then the it will read directly. Otherwise, it will wait until a reader process release and allows that writer to enter. Once the writer process is done, if it is the last writer, then it will release and allow all the writers "queued" readers process to enter. Similarily, the last reader process will release and allow the "queued" writers processes to enter.

Now let's visualize the problem by imagining a room and two benches outside the room for waiting/queued reader and writer process. Suppose you are a writer, if there is no reader inside and no reader on the bench, you can enter. If there are writers inside, the last writer to leave the room will allow all the readers on the bench to enter. Similar in case of readers inside, then the last reader before leaving will allow all the writers on the bench to enter in the room.

## Semaphores
Three semaphores are used as explained below:

1. reading_semaphore : it is a semaphore that reading processes sleep upon. A Reading process starts reading in the critical section only when read_sem releases, until then it queued.
2. writing_semaphore : it is a semaphore that writing processes sleep upon. A writing process starts reading in the critical section only when write-sem releases, until then it queued.
3. mutex : it a mutex semaphore for controlled access to the following variables --> current_writers, queued_writers, current_readers, queued_readers.

## Variables Used
1. current_readers : It is the number of readers who are currently in the CRITICAL section and reading the data.
2. queued_readers : It is the number of readers that are waiting for access to the critical section. It gains access after group of writers that came before are completed.
3. current_writers : It is the number of writers who are currently in the CRITICAL section and writing the data.
4. queued_writers : It is the count of writers which are waiting for access to the critical section. It gains access only when group of readers that came before it completes their execution.

## Psuedo code
In file [starve-free_reader_writer_problem.txt](https://github.com/ua110110/Starve-free_readers_writer_problem/blob/main/starve-free_reader_writer_problem.txt) is the code for the Reader's writers problem that is starvation free and deadlock-free.
Here is the psuedo code :-
```
// for reading 
if(active + waiting writers are zero)
	release and enter the process
else 
	wait for release
	
//Read DATA

if( this is the last reader)
	release the queued writers
  
// for writing 
if(active and waiting readers are zero)
	release and enter the process
else 
	wait for release
	
//Write DATA

if( this is the last writer)
	release the queued readers
```
