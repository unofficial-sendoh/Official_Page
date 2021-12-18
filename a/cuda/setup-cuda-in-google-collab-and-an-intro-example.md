# Setup CUDA in google collab and an intro example

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

```cpp
%%cu

#include <stdio.h>
#define N (2)

static void HandleError(cudaError_t err,
                         const char *file,
                         int line) {
    if (err != cudaSuccess) {
        printf( "%s in %s at line %d\n", cudaGetErrorString( err ),
                file, line );
        exit( EXIT_FAILURE );
    }
}
#define HANDLE_ERROR( err ) (HandleError( err, __FILE__, __LINE__ ))

__global__ void add(int a, int b, int *c) {
  *c = a + b;
}

int main(void) {
  cudaDeviceProp prop;
  cudaGetDeviceProperties(&prop, 0);
  printf("  Device name: %s\n", prop.name);
  int c;
  int *dev_c;
  HANDLE_ERROR(cudaMalloc( (void**)&dev_c, sizeof(int)));
  add<<<1,1>>>(2, 7, dev_c);
  HANDLE_ERROR(cudaMemcpy(&c,dev_c,sizeof(int),cudaMemcpyDeviceToHost));
  printf("2 + 7 = %d\n", c);
  cudaFree(dev_c);
  return 0;
}
```
