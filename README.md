# deep_learning 환경 구축

ubuntu 환경에서 opencv 4.4.0, cuda 10.2, cudnn 8.03 설치하여 gpu 활용하기
ubuntu 환경에서 tesseract 최신 버전 활용하기

# NVIDIA 드라이버 설치
설치 명령어  
$ sudo apt install nvidia-driver-440

확인 명령어  
$ nvidia-smi

# CUDA 10.2 설치  
$ wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run  
$ sudo sh cuda_10.2.89_440.33.01_linux.run  

![cuda](https://user-images.githubusercontent.com/54671310/96948805-6c4cab00-1521-11eb-9791-1c2c88a2cd1e.JPG)  
설치시 드라이버를 제외하고 CUDA TOOL KIT만 설치한다.

이미 설치되어 있는 경우 삭제 방법  
CUDA 삭제  
$ sudo apt-get --purge remove 'cuda*'  
$ sudo apt-get autoremove --purge 'cuda*'  

CUDA 폴더 삭제  
$ sudo rm -rf /usr/local/cuda-10.2  
$ sudo rm -rf /usr/local/cuda  

확인 명령어  
$ nvcc --version | nvcc -V  

# CUDNN 8.03 설치  
cudnn 설치 위치(nvidia 로그인 필요) : https://developer.nvidia.com/rdp/cudnn-archive  

받은 파일 압축 해제  
$ sudo tar -xzvf cudnn-10.2-linux-x64-v8.0.3.33.tgz  
  
압축 해제된 파일 설치된 CUDA로 옮기기(CUDNN의 파일들을 CUDA로 카피)  
$ sudo cp cuda/include/cudnn*.h /usr/local/cuda/include  
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64  
$ sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*  

확인 명령어 - 8버전부터는 확인 위치가 변경  
$ cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2  
8이전 버전 확인 명령어  
$ cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2  

# OPENCV 4.4.0 컴파일  
$ wget -O opencv.zip https://github.com/opencv/opencv/arhive/4.4.0.zip  
$ unzip opencv.zip  

$ wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.4.0.zip  
$ unzip opencv_contrib.zip  

$ cd opencv-4.4.0  
$ mkdir build  
$ cd build  

cmake -D CMAKE_BUILD_TYPE=RELEASE \
	-D CMAKE_INSTALL_PREFIX=/usr/local \
	-D INSTALL_PYTHON_EXAMPLES=ON \
	-D INSTALL_C_EXAMPLES=OFF \
	-D OPENCV_ENABLE_NONFREE=ON \
	-D WITH_CUDA=ON \
	-D WITH_CUDNN=ON \
	-D OPENCV_DNN_CUDA=ON \
	-D ENABLE_FAST_MATH=1 \
	-D CUDA_FAST_MATH=1 \
	-D CUDA_ARCH_BIN=7.5 \
	-D WITH_CUBLAS=1 \
	-D OPENCV_EXTRA_MODULES_PATH=/home/ict2/installers/opencv_contrib-4.4.0/modules \
	-D HAVE_opencv_python3=ON \
	-D BUILD_EXAMPLES=ON ..

-D OPENCV_EXTRA_MODULES_PATH=/home/ict2/installers/opencv_contrib-4.4.0/modules 
path는 opencv-contrib-4.4.0의 위치로 지정  

OPENCV 4.4.0 컴파일  
$ make -j4  

![opencv_compile](https://user-images.githubusercontent.com/54671310/96948809-6ce54180-1521-11eb-967e-cd82abbba37d.JPG)
완료 후 CUDA, CUDNN이 YES와 버전명이 올바르게 나와야하며, PYTHON3 설정이 잡혀 있어야 정상적인 설치상태이다

OPENCV 4.4.0 설치, 및 심볼릭 링크  
$ sudo make install  
$ sudo ldconfig  

# Teeseract v4.1.1 설치하기  

최신 버전 설치를 위해서 PPA를 통해서 설치해야한다.

PPA에 저장소 추가
$ sudo add-apt-repository ppa:alex-p/tesseract-ocr  
 -> sudo add-apt-repository ppa:user/ppa-name  
 정식 릴리즈가 아닌 알파버전 설치(v5.0)
 sudo add-apt-repository ppa:alex-p/tesseract-ocr-devel

추가한 저장소에서 tesseract-ocr 설치
$ sudo apt-get update  
$ sudo apt-get install tesseract-ocr

$ tesseract -v


$ vi ~/.bashrc

최 하단에 환경변수 추가
export TESSDATA_PREFIX="/usr/share/tesseract-ocr/4.00/tessdata/"

$ source ~/.bashrc

언어팩은 아래 링크에서 다운로드 후 /usr/share/tesseract-ocr/4.00/tessdata/ 안에 넣어주면 된다.
https://github.com/tesseract-ocr/tessdoc/blob/master/Data-Files.md
