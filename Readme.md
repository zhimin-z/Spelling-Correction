# [KenLM](https://github.com/kpu/kenlm)统计语言模型构建与应用

我们将学习到如何使用KenLM工具构建语言模型，并使用它完成一个典型的“智能纠错”文本任务。

### 安装依赖
```shell
!apt install libboost-all-dev
!apt install libbz2-dev
!apt install libeigen3-dev
```

### 下载KenLM编译
```shell
!wget -O - https://kheafield.com/code/kenlm.tar.gz | tar xz
!mkdir kenlm/build
!cd kenlm/build && cmake .. && make -j8
```

### 安装kenLM
```shell
!cd kenlm/build && make install
```

### 准备训练数据
我们准备好了一份英文的训练数据(已经对内容做过Normalization了)，内容如下：
```shell
!head -5 /data/NLP/Language_Models/lm_train_data
```

### 语言模型训练
我们通过命令行的方式使用kenlm，在我们的训练集语料上训练语言模型，命令为
```shell
lmplz -o 5 <text > text.arpa
```
-o 后面的数字5代表使用N-gram的N取值为5

text.arpa 表示kenlm训练得到的文件格式为.arpa格式，名字为text

我们这里训练一个简单的2-gram语言模型
```shell
!lmplz -o 2 </data/NLP/Language_Models/lm_train_data> /data/NLP/Language_Models/lm.arpa
```

### 模型压缩
对训练得到的文件进行压缩：将arpa文件转换为binary文件，这样可以对arpa文件进行压缩和序列化，提高后续在python中加载的速度。针对我们训练的到的 lm.arpa 文件其转换命令为：
```shell
!build_binary -s /data/NLP/Language_Models/lm.arpa /data/NLP/Language_Models/lm.bin
!ls -l /data/NLP/Language_Models/lm.arpa
!ls -l /data/NLP/Language_Models/lm.bin
```

### 安装KenLM接口
```shell
!pip install -i https://pypi.tuna.tsinghua.edu.cn/simple kenlm
```
