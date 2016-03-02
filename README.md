//Vector Addition in CUDA C:

#include<stdio.h>
 __global__ void VecAdd(float* A, float* B, float* C,float* D, int N)
    {
 		int j;
                int i = blockDim.x * blockIdx.x + threadIdx.x;
                if (i < N)
		for(j=0;j<1024;j++)
	        {C[i] = A[i] + B[i];
		 D[i]= A[i]*B[i];}
    }
               // Host code
int main()
    {
                   int N = 1024*1024,i;
                   size_t size = N * sizeof(float);
                   // Allocate input vectors h_A and h_B in host memory
                   float* h_A = (float*)malloc(size);
                   float* h_B = (float*)malloc(size);
		   float* h_C = (float*)malloc(size);
		   float* h_D = (float*)malloc(size);
                   // Initialize input vectors
                   for(i=0; i<N; i++)
			{
			h_A[i]=  (i*3);
			h_B[i]=  (i*3);
			}	
                   // Allocate vectors in device memory
			float t1, t2, t3;
		   t1=clock();
                                      
			float* d_A;
                   cudaMalloc((void**)&d_A, size);
                   float* d_B;
                   cudaMalloc((void**)&d_B, size);
                   float* d_C;
                   cudaMalloc((void**)&d_C, size);
		   float* d_D;
		   cudaMalloc((void**)&d_D, size);
                   // Copy vectors from host memory to device memory
                   cudaMemcpy(d_A, h_A, size, cudaMemcpyHostToDevice);
                   cudaMemcpy(d_B, h_B, size, cudaMemcpyHostToDevice);
                   // Invoke kernel
                   VecAdd<<< 16384,64>>>(d_A, d_B, d_C, d_D,N);
                  
                   // Copy result from device memory to host memory
                   // h_C contains the result in host memory
                   cudaMemcpy(h_C, d_C, size, cudaMemcpyDeviceToHost);
                   cudaMemcpy(h_D, d_D, size, cudaMemcpyDeviceToHost);
			 t2=clock();
                   t3=((t2-t1)/ CLOCKS_PER_SECONDS);
		   //for(i=0; i<N; i++)
			{
			printf("\n%f",h_C[100]);
			printf("\n%f",h_D[100]);
			}
		    printf("\n time in gpu= %f",t3);                   // Free device memory

                     cudaFree(d_A);
                     cudaFree(d_B);
                     cudaFree(d_C);
                     // Free host memory
                     
     }
