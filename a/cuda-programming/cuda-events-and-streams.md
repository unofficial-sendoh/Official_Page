# CUDA Events and Streams

[Explanation Video Link on Youtube](https://youtu.be/iHK9o26EOSQ)

[B站中文解说视频链接](https://www.bilibili.com/video/BV1v34y1i75Q?share\_source=copy\_web)

```shell
!apt-get --purge remove cuda nvidia* libnvidia-*
!dpkg -l | grep cuda- | awk '{print $2}' | xargs -n1 dpkg --purge
!apt-get remove cuda-*
!apt autoremove
!apt-get update

!wget https://developer.nvidia.com/compute/cuda/9.2/Prod/local_installers/cuda-repo-ubuntu1604-9-2-local_9.2.88-1_amd64 -O cuda-repo-ubuntu1604-9-2-local_9.2.88-1_amd64.deb
!dpkg -i cuda-repo-ubuntu1604-9-2-local_9.2.88-1_amd64.deb
!apt-key add /var/cuda-repo-9-2-local/7fa2af80.pub
!apt-get update
!apt-get install cuda-9.2

!pip install git+git://github.com/andreinechaev/nvcc4jupyter.git
%load_ext nvcc_plugin
```

Use CUDA Events to profiling GPU run-time performance:

```c
%%cu
#include <stdio.h>
#define N (1024)
#define FULL_DATA_SIZE (N*500)
__global__ void add( int *a, int *b, int *c ) {
    int tid = threadIdx.x + blockIdx.x * blockDim.x;
    c[tid] = a[tid] + b[tid];
}
int main(void) {
  cudaEvent_t start, stop;
  float elapsedTime;
  cudaEventCreate(&start);
  cudaEventCreate(&stop);
  cudaEventRecord(start, 0);
  int host_a[FULL_DATA_SIZE], host_b[FULL_DATA_SIZE], host_c[FULL_DATA_SIZE];
  int *dev_a, *dev_b, *dev_c;
  cudaMalloc((void**)&dev_a, N * sizeof(int));
  cudaMalloc((void**)&dev_b, N * sizeof(int));
  cudaMalloc((void**)&dev_c, N * sizeof(int));

  for (int i = 0; i < FULL_DATA_SIZE; i++) {
    host_a[i] = rand();
    host_b[i] = rand();
  }

  for (int i = 0; i < FULL_DATA_SIZE; i+=N) {
    cudaMemcpy(dev_a, host_a + i, N * sizeof(int), cudaMemcpyHostToDevice);
    cudaMemcpy(dev_b, host_b + i, N * sizeof(int), cudaMemcpyHostToDevice);
    add<<<256,256>>>(dev_a, dev_b, dev_c);
    cudaMemcpy(host_c + i, dev_c, N * sizeof(int), cudaMemcpyDeviceToHost);
  }

  cudaEventRecord(stop, 0);
  cudaEventSynchronize(stop);
  cudaEventElapsedTime(&elapsedTime, start, stop);
  printf("Time taken: %3.1f ms\n", elapsedTime);
  cudaFree(dev_a);
  cudaFree(dev_b);
  cudaFree(dev_c);
  return 0;
}
```

Use two CUDA Streams to improve above application:

```c
%%cu
#include <stdio.h>
#define N (1024)
#define FULL_DATA_SIZE (N*500)
__global__ void add( int *a, int *b, int *c ) {
    int tid = threadIdx.x + blockIdx.x * blockDim.x;
    c[tid] = a[tid] + b[tid];
}
int main(void) {
  cudaEvent_t start, stop;
  float elapsedTime;
  cudaEventCreate(&start);
  cudaEventCreate(&stop);
  cudaEventRecord(start, 0);
  cudaStream_t stream0, stream1;
  cudaStreamCreate(&stream0);
  cudaStreamCreate(&stream1);
  int *host_a, *host_b, *host_c;
  int *dev_a0, *dev_b0, *dev_c0;
  int *dev_a1, *dev_b1, *dev_c1;
  cudaMalloc((void**)&dev_a0, N * sizeof(int));
  cudaMalloc((void**)&dev_b0, N * sizeof(int));
  cudaMalloc((void**)&dev_c0, N * sizeof(int));
  cudaMalloc((void**)&dev_a1, N * sizeof(int));
  cudaMalloc((void**)&dev_b1, N * sizeof(int));
  cudaMalloc((void**)&dev_c1, N * sizeof(int));
  cudaHostAlloc((void**)&host_a, FULL_DATA_SIZE * sizeof(int), cudaHostAllocDefault);
  cudaHostAlloc((void**)&host_b, FULL_DATA_SIZE * sizeof(int), cudaHostAllocDefault);
  cudaHostAlloc((void**)&host_c, FULL_DATA_SIZE * sizeof(int), cudaHostAllocDefault);

  for (int i = 0; i < FULL_DATA_SIZE; i++) {
    host_a[i] = rand();
    host_b[i] = rand();
  }

  for (int i = 0; i < FULL_DATA_SIZE; i+=2*N) {
    cudaMemcpyAsync(dev_a0, host_a + i, N * sizeof(int), cudaMemcpyHostToDevice, stream0);
    cudaMemcpyAsync(dev_b0, host_b + i, N * sizeof(int), cudaMemcpyHostToDevice, stream0);
    add<<<256,256, 0, stream0>>>(dev_a0, dev_b0, dev_c0);
    cudaMemcpyAsync(host_c + i, dev_c0, N * sizeof(int), cudaMemcpyDeviceToHost, stream0); 
    cudaMemcpyAsync(dev_a1, host_a + i + N, N * sizeof(int), cudaMemcpyHostToDevice, stream1);
    cudaMemcpyAsync(dev_b1, host_b + i + N, N * sizeof(int), cudaMemcpyHostToDevice, stream1);
    add<<<256,256, 0, stream1>>>(dev_a1, dev_b1, dev_c1);
    cudaMemcpyAsync(host_c + i + N, dev_c1, N * sizeof(int), cudaMemcpyDeviceToHost, stream1); 
  }

  cudaStreamSynchronize(stream0);
  cudaStreamSynchronize(stream1);
  cudaEventRecord(stop, 0);
  cudaEventSynchronize(stop);
  cudaEventElapsedTime(&elapsedTime, start, stop);
  printf("Time taken: %3.1f ms\n", elapsedTime);
  cudaFree(dev_a0);
  cudaFree(dev_b0);
  cudaFree(dev_c0);
  cudaFree(dev_a1);
  cudaFree(dev_b1);
  cudaFree(dev_c1);
  cudaFreeHost(host_a);
  cudaFreeHost(host_b);
  cudaFreeHost(host_c);
  cudaStreamDestroy(stream0);
  cudaStreamDestroy(stream1);
  return 0;
}
```
