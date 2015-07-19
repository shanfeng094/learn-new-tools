## 安装
用下列命令可快速安装除CUDA/cnDNN以外**所有**依赖库：

    sudo apt-get install libatlas-dev libatlas-base-dev libboost-dev libboost-all-dev libboost-system-dev libboost-thread-dev libboost-python-dev libopencv-dev libprotobuf-dev protobuf-compiler libhdf5-dev libleveldb-dev liblmdb-dev libsnappy-dev libpython-dev libpython2.7 automake cmake python-pip
    export CAFFE\_ROOT=/path/to/your/caffe
    sudo pip install -r $CAFFE\_ROOT/python/requirements.txt

用下列命令配置编译：

    cd $CAFFE\_ROOT
    mkdir build && cd build && cmake ..

cmake最后输出含有的标题应该有：

* General
* Dependencies
* NVIDIA CUDA
* Python
* Matlab
* Documentation
* Install

如果缺少Python或者Matlab，则不会编译对应接口，这点一定要注意。

然后编译：

    make -j7

-j后面的数字代表并行编译的核数目，建议设置为你的CPU实际并行核数目-1。CPU并行核数目可在terminal中用nproc查看。

### Python接口
在用cmake配置安装过程时，如果python的包不够全会发生不自动编译pycaffe模块的现象，但并不会有任何提示。这样，在import caffe时会发生no module named \_caffe的错误。解决方法是在CAFFE\_ROOT下sudo pip install -r ./python/requirements.txt，然后再sudo apt-get install libboost-python-dev，然后删掉build文件夹，重新cmake，重新make。 

### Matlab接口
有两点需要注意：
* 在用cmake配置安装过程时，如果matlab是采用在PATH中添加/usr/local/MATLAB/R20xxx/bin这种方式，则会出现找不到matlab的情况。这一方案很不规范，不应当在PATH中直接加软件可执行文件夹，而应当在/usr/bin下建立软链。
* 如果gcc版本不正确，会无法mex。例如Matlab R2014b要求用gcc4.7.x，但笔者写此文档时Ubuntu15.04自带gcc4.9.2，因此需要安装gcc4.7和g++4.7，并设置系统默认gcc&&g++版本为4.7
* 在某些情况下mex会试图链接lpython2，而我们安装的往往是lpython2.7，因此需要创建符号链接。先找到libpython2.7.so，然后链接到/usr/lib下。在笔者的计算机上，命令如下：

    sudo ln -s /usr/lib/x86\_64-linux-gnu/libpython2.7.so /usr/lib/libpython2.so

然后重新make即可。


