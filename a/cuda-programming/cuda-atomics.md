# CUDA Atomics

```cpp
%%cu
#include <stdio.h>
#include <chrono>
#define N (1000000)
#define M (256)

__global__ void hist_kernel(unsigned int *buffer, unsigned int *hist) {
  int thread_id = threadIdx.x + blockIdx.x * blockDim.x;
  __shared__ unsigned int temp[M];
  temp[threadIdx.x] = 0;
  __syncthreads();
  int total_num_threads = blockDim.x * gridDim.x;

  while (thread_id < N) {
    atomicAdd(&temp[buffer[thread_id]], 1);
    thread_id += total_num_threads;
  }

  __syncthreads();
  atomicAdd(&(hist[threadIdx.x]), temp[threadIdx.x]); 
}

int main(void) {
  std::chrono::steady_clock::time_point cpu_start = std::chrono::steady_clock::now();
  unsigned int buffer[N];
  unsigned int cpu_hist[M] = {0};
  for (int i = 0; i < N; i++) {
      buffer[i] = rand() % M;
      cpu_hist[buffer[i]]++;
  }
  
  std::chrono::steady_clock::time_point cpu_end = std::chrono::steady_clock::now();
  double cpu_elapsed_time = std::chrono::duration_cast<std::chrono::milliseconds>(cpu_end - cpu_start).count();
  printf("CPU Time taken: %3.1f ms\n", cpu_elapsed_time);

  cudaEvent_t start, stop;
  float elapsedTime;
  cudaEventCreate(&start);
  cudaEventCreate(&stop);
  cudaEventRecord(start, 0);

  unsigned int *dev_buffer;
  unsigned int *dev_hist;
  cudaMalloc((void**)&dev_buffer, N * sizeof(int));
  cudaMemcpy(dev_buffer, buffer, N * sizeof(int), cudaMemcpyHostToDevice);
  cudaMalloc((void**)&dev_hist, M * sizeof(int));
  cudaMemset(dev_hist, 0, M * sizeof(int));

  hist_kernel<<<100, M>>>(dev_buffer, dev_hist);

  unsigned int gpu_hist[M];
  cudaMemcpy(gpu_hist, dev_hist, M * sizeof(int), cudaMemcpyDeviceToHost);
  cudaEventRecord(stop, 0);
  cudaEventSynchronize(stop);
  cudaEventElapsedTime(&elapsedTime, start, stop);
  printf("GPU Time taken: %3.1f ms\n", elapsedTime);

  for (int i = 0; i < M; i++) {
      if (gpu_hist[i] != cpu_hist[i]) {
          printf("Mistmatch index: %d CPU: %d GPU: %d \n", i, cpu_hist[i], gpu_hist[i]);
      }
  }

  cudaEventDestroy(start);
  cudaEventDestroy(stop);
  cudaFree(dev_hist);
  cudaFree(dev_buffer);
  return 0;
}
```
