# object_recognition
yolo를 이용한 객체인식

ubuntu 환경에서 opencv 4.4.0, cuda 10.2, cudnn 8.03 설치하여 gpu 활용하기

# NVIDIA 드라이버 설치
설치 명령어\n
$ sudo apt install nvidia-driver-440

확인 명령어
$ nvidia-smi

# CUDA 10.2 설치
$ wget http://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run 
$ sudo sh cuda_10.2.89_440.33.01_linux.run

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
OPENCV 4.4.0 설치, 및 심볼릭 링크
$ sudo make install
$ sudo ldconfig


