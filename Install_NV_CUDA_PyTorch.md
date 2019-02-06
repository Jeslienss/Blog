Today, I install the CUDA 9.2 with the NVIDIA Driver Version 396
 - Install NVIDIA DRIVER 396  
----------------------------
	```
	sudo add-apt-repository ppa:graphics-drivers/ppa  
	sudo apt-get update  
	sudo apt-get install nvidia-396 nvidia-modprobe  
	```  
	Then, REBOOT!  
   To test it, we input `nvidia-smi` into the terminal. It should display as following:  
```
Tue Feb  5 17:47:29 2019   
Tue Feb  5 23:00:31 2019       
+-----------------------------------------------------------------------------+  
| NVIDIA-SMI 396.54                 Driver Version: 396.54                    |  
|-------------------------------+----------------------+----------------------+  
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |  
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |  
|===============================+======================+======================|  
|   0  GeForce GTX 1080    Off  | 00000000:01:00.0  On |                  N/A |  
|  0%   39C    P8    12W / 215W |   1555MiB /  8118MiB |      0%      Default |  
+-------------------------------+----------------------+----------------------+  
                                                                                 
+-----------------------------------------------------------------------------+  
| Processes:                                                       GPU Memory |  
|  GPU       PID   Type   Process name                             Usage      |  
|=============================================================================|  
|    0      1009      G   /usr/lib/xorg/Xorg                           732MiB |  
|    0      2071      G   compiz                                       468MiB |  
|    0      2461      G   ...uest-channel-token=14032021669303589656   351MiB |  
+-----------------------------------------------------------------------------+  
```  
NVIDIA Official Compatibility `https://docs.nvidia.com/deploy/cuda-compatibility/index.html`  
 - Install [NVIDIA CUDA 9.2](https://developer.nvidia.com/cuda-92-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal)  
 Here, we choose the runfile. In runfile, we can choose which components we want to install. The runfile contains three parts:
 	- an NVIDIA driver installer, but usually of stale version;  
 	- the actual CUDA installer;  
 	- the CUDA samples installer;  
 During the installation, we only need to install the CUDA and samples, without the stale NVIDIA driver.  
 After Installation finished, you should configure the runtime library.  
 ```
 sudo bash -c "echo /usr/local/cuda/lib64/ > /etc/ld.so.conf.d/cuda.conf"  
 sudo ldconfig  
 ```  
 Also, you need to include `nvcc` to the $PATH.  
 ```
 sudo vim /etcenvironment  
 ```  
 Then, add the `:/usr/local/cuda/bin` to the end of the string.  
 Then, REBOOT!  
 To test your CUDA working,
 ```
 cd /usr/local/cuda-9.2/samples/bin/x86_64/linux/release  
 ./deviceQuery  
 ```
 The result is  
 ```
 ./deviceQuery Starting...  

 CUDA Device Query (Runtime API) version (CUDART static linking)  

Detected 1 CUDA Capable device(s)  

Device 0: "GeForce GTX 1080"  
  CUDA Driver Version / Runtime Version          9.2 / 9.2  
  CUDA Capability Major/Minor version number:    6.1  
  Total amount of global memory:                 8118 MBytes (8512602112 bytes)  
  (20) Multiprocessors, (128) CUDA Cores/MP:     2560 CUDA Cores  
  GPU Max Clock rate:                            1734 MHz (1.73 GHz)  
  Memory Clock rate:                             5005 Mhz  
  Memory Bus Width:                              256-bit  
  L2 Cache Size:                                 2097152 bytes  
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)  
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers  
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers  
  Total amount of constant memory:               65536 bytes  
  Total amount of shared memory per block:       49152 bytes  
  Total number of registers available per block: 65536  
  Warp size:                                     32  
  Maximum number of threads per multiprocessor:  2048  
  Maximum number of threads per block:           1024  
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)  
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)  
  Maximum memory pitch:                          2147483647 bytes  
  Texture alignment:                             512 bytes  
  Concurrent copy and kernel execution:          Yes with 2 copy engine(s)  
  Run time limit on kernels:                     Yes  
  Integrated GPU sharing Host Memory:            No  
  Support host page-locked memory mapping:       Yes  
  Alignment requirement for Surfaces:            Yes  
  Device has ECC support:                        Disabled  
  Device supports Unified Addressing (UVA):      Yes  
  Device supports Compute Preemption:            Yes  
  Supports Cooperative Kernel Launch:            Yes  
  Supports MultiDevice Co-op Kernel Launch:      Yes  
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0  
  Compute Mode:  
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >  

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 9.2, CUDA Runtime Version = 9.2, NumDevs = 1  
Result = PASS  
 ```  


To uninstall the CUDA Toolkit, run the uninstall script in /usr/local/cuda-9.2/bin