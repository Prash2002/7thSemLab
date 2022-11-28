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
 
 ## Scatter 
 MPI_Scatter is a collective routine that is very similar to MPI_Bcast. MPI_Scatter involves a designated root process sending data to all processes in a communicator. MPI_Bcast sends the same piece of data to all processes while MPI_Scatter sends chunks of an array to different processes.
 
![image](https://user-images.githubusercontent.com/56592188/204192115-d38df44e-f14a-4a96-88e9-05a050af284d.png)

 ```c
 MPI_Scatter(
    void* send_data,
    int send_count,
    MPI_Datatype send_datatype,
    void* recv_data,
    int recv_count,
    MPI_Datatype recv_datatype,
    int root,
    MPI_Comm communicator)

 ```
 ## Gather
  MPI_Gather is the inverse of MPI_Scatter. Instead of spreading elements from one process to many processes, MPI_Gather takes elements from many processes and gathers them to one single process. Used in parallel sorting and searching.
 
 ![image](https://user-images.githubusercontent.com/56592188/204192520-f58f2bf9-54c1-4cc4-a328-449103294a2e.png)

```c
MPI_Gather(
    void* send_data,
    int send_count,
    MPI_Datatype send_datatype,
    void* recv_data,
    int recv_count,
    MPI_Datatype recv_datatype,
    int root,
    MPI_Comm communicator)

```
 
 ## Reduce 
 Similar to MPI_Gather, MPI_Reduce takes an array of input elements on each process and returns an array of output elements to the root process. The output elements contain the reduced result.
 
 ```c
 MPI_Reduce(
    void* send_data,
    void* recv_data,
    int count,
    MPI_Datatype datatype,
    MPI_Op op,
    int root,
    MPI_Comm communicator)
 ```
 
 ![image](https://user-images.githubusercontent.com/56592188/204193140-80122188-3a69-480c-82d1-011e17ad2ff9.png)

 
 ## Barrier
 Blocks until all processes in the communicator have reached this routine. 
 
 ```c
 int MPI_Barrier( MPI_Comm comm )
 ```

## References
https://mpitutorial.com/tutorials/mpi-broadcast-and-collective-communication/
https://hpc-tutorials.llnl.gov/mpi/

