# PCA-EXP-3-PARALLEL-REDUCTION-USING-UNROLLING-TECHNIQUES AY 23-24
<h3>AIM:</h3>
<h3>ENTER YOUR NAME</h3>
SANTHIYA G
<h3>ENTER YOUR REGISTER NO</h3>
212225230248
<h3>EX. NO</h3>
<h3>DATE</h3>
<h1> <align=center> PARALLEL REDUCTION USING UNROLLING TECHNIQUES </h3>
  Refer to the kernel reduceUnrolling8 and implement the kernel reduceUnrolling16, in which each thread handles 16 data blocks. Compare kernel performance with reduceUnrolling8 and use the proper metrics and events with nvprof to explain any difference in performance.</h3>

## AIM:
To implement the kernel reduceUnrolling16 and comapare the performance of kernal reduceUnrolling16 with kernal reduceUnrolling8 using nvprof.
## EQUIPMENTS REQUIRED:
Hardware – PCs with NVIDIA GPU & CUDA NVCC
Google Colab with NVCC Compiler
## PROCEDURE:
1.	Initialization and Memory Allocation
2.	Define the input size n.
3.	Allocate host memory (h_idata and h_odata) for input and output data.
Input Data Initialization
4.	Initialize the input data on the host (h_idata) by assigning a value of 1 to each element.
Device Memory Allocation
5.	Allocate device memory (d_idata and d_odata) for input and output data on the GPU.
Data Transfer: Host to Device
6.	Copy the input data from the host (h_idata) to the device (d_idata) using cudaMemcpy.
Grid and Block Configuration
7.	Define the grid and block dimensions for the kernel launch:
8.	Each block consists of 256 threads.
9.	Calculate the grid size based on the input size n and block size.
10.	Start CPU Timer
11.	Initialize a CPU timer to measure the CPU execution time.
12.	Compute CPU Sum
13.	Calculate the sum of the input data on the CPU using a for loop and store the result in sum_cpu.
14.	Stop CPU Timer
15.	Record the elapsed CPU time.
16.	Start GPU Timer
17.	Initialize a GPU timer to measure the GPU execution time.
Kernel Execution
18.	Launch the reduceUnrolling16 kernel on the GPU with the specified grid and block dimensions.
Data Transfer: Device to Host
19.	Copy the result data from the device (d_odata) to the host (h_odata) using cudaMemcpy.
20.	Compute GPU Sum
21.	Calculate the final sum on the GPU by summing the elements in h_odata and store the result in sum_gpu.
22.	Stop GPU Timer
23.	Record the elapsed GPU time.
24.	Print Results
25.	Display the computed CPU sum, GPU sum, CPU elapsed time, and GPU elapsed time.
Memory Deallocation
26.	Free the allocated host and device memory using free and cudaFree.
27.	Exit
28.	Return from the main function.

## PROGRAM:
TYPE YOUR CODE HERE

%%cuda

#include <stdio.h> #include <cuda_runtime.h>

#define N 3

global void matrixAdd(int *A, int *B, int *C) { int row = threadIdx.y; int col = threadIdx.x;
```
int index = row * N + col;

C[index] = A[index] + B[index];
```
}

int main() { int A[N][N] = { {1, 2, 3}, {4, 5, 6}, {7, 8, 9} };

```
int B[N][N] = {
    {9, 8, 7},
    {6, 5, 4},
    {3, 2, 1}
};

int C[N][N];

int *d_A, *d_B, *d_C;

cudaMalloc(&d_A, N*N*sizeof(int));
cudaMalloc(&d_B, N*N*sizeof(int));
cudaMalloc(&d_C, N*N*sizeof(int));

cudaMemcpy(d_A, A, N*N*sizeof(int), cudaMemcpyHostToDevice);
cudaMemcpy(d_B, B, N*N*sizeof(int), cudaMemcpyHostToDevice);

dim3 block(N, N);
dim3 grid(1, 1);

matrixAdd<<<grid, block>>>(d_A, d_B, d_C);

cudaDeviceSynchronize();

cudaMemcpy(C, d_C, N*N*sizeof(int), cudaMemcpyDeviceToHost);

printf("Matrix A:\n");

for(int i = 0; i < N; i++)
{
    for(int j = 0; j < N; j++)
        printf("%d ", A[i][j]);

    printf("\n");
}

printf("\nMatrix B:\n");

for(int i = 0; i < N; i++)
{
    for(int j = 0; j < N; j++)
        printf("%d ", B[i][j]);

    printf("\n");
}

printf("\nMatrix Addition (A + B):\n");

for(int i = 0; i < N; i++)
{
    for(int j = 0; j < N; j++)
        printf("%d ", C[i][j]);

    printf("\n");
}

cudaFree(d_A);
cudaFree(d_B);
cudaFree(d_C);

return 0;
```

## OUTPUT:
SHOW YOUR OUTPUT HERE

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e1c35019-1715-4c6a-a776-29de61c8b055" />


## RESULT:
Thus the program has been executed by unrolling by 8 and unrolling by 16. It is observed that _________ has executed with less elapsed time than _____________ with blocks_____,______.
