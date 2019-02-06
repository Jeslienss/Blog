Today, I install the CUDA 10 with the NVIDIA Driver Version 410 (Which should compatiable with g++ 5.4 on ubuntu 16.04.5)
 - Install NVIDIA DRIVER 410  
	`sudo apt-get-repository ppa:graphics-drivers/ppa`  
	`sudo apt-get update`  
	`sudo apt-get install nvidia-410 nvidia-modprobe`  
   To test it, we input `nvidia-smi` into the terminal. It should display as following:  
Tue Feb  5 17:47:29 2019   
+-----------------------------------------------------------------------------+  
| NVIDIA-SMI 410.78       Driver Version: 410.78       CUDA Version: 10.0     |  
|-------------------------------+----------------------+----------------------+  
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |  
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |  
|===============================+======================+======================|  
|   0  GeForce GTX 1080    Off  | 00000000:01:00.0  On |                  N/A |  
|  0%   42C    P8    10W / 215W |    855MiB /  8118MiB |      1%      Default |  
+-------------------------------+----------------------+----------------------+  
                                                                                 
+-----------------------------------------------------------------------------+  
| Processes:                                                       GPU Memory |  
|  GPU       PID   Type   Process name                             Usage      |  
|=============================================================================|  
|    0       993      G   /usr/lib/xorg/Xorg                           607MiB |  
|    0      2089      G   compiz                                       109MiB |  
|    0      2564      G   ...uest-channel-token=14543146793354941521   135MiB |  
+-----------------------------------------------------------------------------+  

