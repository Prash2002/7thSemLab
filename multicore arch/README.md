# MPI 

The Message Passing Interface Standard (MPI) is a message passing library standard. 
MPI primarily addresses the message-passing parallel programming model: data is moved from the address space of one process to that of another process through cooperative operations on each process.

## Reasons for Using MPI

 - Standardization - MPI is the only message passing library that can be considered a standard. It is supported on virtually all HPC platforms. Practically, it has replaced all previous message passing libraries.
 - Portability - There is little or no need to modify your source code when you port your application to a different platform that supports (and is compliant with) the MPI standard.
 - Performance Opportunities - Vendor implementations should be able to exploit native hardware features to optimize performance. Any implementation is free to develop optimized algorithms.
 - Functionality - There are over 430 routines defined in MPI-3, which includes the majority of those in MPI-2 and MPI-1.
        NOTE: Most MPI programs can be written using a dozen or less routines
 - Availability - A variety of implementations are available, both vendor and public domain.


## General MPI Program Structure
![image](https://user-images.githubusercontent.com/56592188/201576299-518c1cce-ebb9-4eb9-9bb6-dfe764c059fa.png)

### Header file:
```cpp
  #include "mpi.h"
 ```
 
 ### MPI Calls
 
    Format:	rc = MPI_Xxxxx(parameter, ... )
    Example: rc = MPI_Bsend(&buf,count,type,dest,tag,comm)
    Error code: Returned as "rc". MPI_SUCCESS if successful
    
 ### Communicators and Groups
 
 MPI uses objects called communicators and groups to define which collection of processes may communicate with each other.
 
 ### Rank
 
 Within a communicator, every process has its own unique, integer identifier assigned by the system when the process initializes. A rank is sometimes also called a “task ID”. Ranks are contiguous and begin at zero.
 Used by the programmer to specify the source and destination of messages.

## Commands
``` mpicc 3.c 
mpirun -np 4 ./a.out 
```


## Functions

### Init:
Initializes the MPI execution environment. This function must be called in every MPI program, must be called before any other MPI functions and must be called only once in an MPI program.
```c
MPI_Init (&argc,&argv)
```

### MPI_Comm_size

Returns the total number of MPI processes in the specified communicator, such as MPI_COMM_WORLD.
```c
MPI_Comm_size (comm,&size)
```

### MPI_Comm_rank

Returns the rank of the calling MPI process within the specified communicator. 
```c
MPI_Comm_rank (comm,&rank)
```

### MPI_Finalize

Terminates the MPI execution environment. This function should be the last MPI routine called in every MPI program - no other MPI routines may be called after it.
```c
MPI_Finalize ()
```

## Sending and receiving with MPI
```c
MPI_Send(
    void* data,
    int count,
    MPI_Datatype datatype,
    int destination,
    int tag,
    MPI_Comm communicator);

MPI_Recv(
    void* data,
    int count,
    MPI_Datatype datatype,
    int source,
    int tag,
    MPI_Comm communicator,
    MPI_Status* status)

```

### message tags
A might have to send many different types of messages to B. Instead of B having to go through extra measures to differentiate all these messages, MPI allows senders and receivers to also specify message IDs with the message (known as tags).

## Broadcast
 During a broadcast, one process sends the same data to all processes in a communicator. One of the main uses of broadcasting is to send out user input to a parallel program, or send out configuration parameters to all processes.
 ![image](https://user-images.githubusercontent.com/56592188/201580389-b6351944-bb07-4fd1-9e62-3b4c58783ecc.png)
 
 ```c
 MPI_Bcast(
    void* data,
    int count,
    MPI_Datatype datatype,
    int root,
    MPI_Comm communicator)

 ```
 
 ## Gather Scatter Reduce Barrier

## References
https://mpitutorial.com/tutorials/mpi-broadcast-and-collective-communication/
https://hpc-tutorials.llnl.gov/mpi/

