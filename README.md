
// Vector addition in C language

#include<stdio.h>
 
int main()
    {
                   int N = 1024*1024,i,j;
		   float t1,t2,t3;
                   size_t size = N * sizeof(float);
                   // Allocate input vectors h_A and h_B in host memory
                   float* h_A = (float*)malloc(size);
                   float* h_B = (float*)malloc(size);
		   float* h_C = (float*)malloc(size);
		   float* h_D = (float*)malloc(size);
                   // Initialize input vectors

                   for(i=0; i<N; i++)
			{
			h_A[i]=  (i*2);
			h_B[i]=  (i*3);
			}	
                   t1= clock();
                   for(i=0; i<N; i++)
			{
			for( j=0;j<1024;j++)
			{h_C[i]= h_A[i] + h_B[i];
			 h_D[i]=h_A[i]*h_B[i];}
			}	
                    t2=clock();
                    t3= ((t2-t1)/CLOCK_PER_SECONDS);
                    
		  // for(i=0; i<N; i++)
			{
			printf("\n%f",h_C[100]);
			}
                   printf("\ntime in cpu= %f",t3);

       return 0;              
     }
