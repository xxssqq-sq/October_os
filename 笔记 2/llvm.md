### LLVM、clang、IR概述

对于LLVM，个人认为可以将它理解为是一个编译器，或者是一个完整的编译架构。它将源代码(.c或者.cpp或者.m等文件代码)生成与机器无关的中间代码，称之为IR。然后对产生的IR进行优化，生成对应的机器汇编语言。这和传统编译器前端，中间优化，后端的设计模式很相似。而不同之处在于，可以通过自定制前端或者后端来使之支持编译你的语言，对应的就是将源码转为中间IR代码，或者中间IR代码转为指定的机器代码，即只需要实现指定的前端后者后端即可。这就是LLVM强大的可扩展性。

对于LLVM来说，其前端是clang，在编译源码文件的时候使用的编译工具也是clang。而生成中间IR代码后需要对IR代码进行一些操作，例如添加一些代码混淆功能。LLVM的做法是通过编写Pass(其实就是对应的一个个类，每个类实现不同的功能)来实现混淆的功能。所以实现混淆，其实就是编写功能性的Pass。怎样编写pass在之前的[文章](https://www.jianshu.com/p/51a9c54f9bc6)可以找到。

### 加固与保护

如果你是安卓开发从业者，那么我相信你应该听说过VMP保护，VMP（虚拟软件保护技术）的思路是自定义一套虚拟机指令和对应的解释器，并将标准的指令转换成自己的指令，然后由解释器将自己的指令给对应的解释器。由于安卓系统后端使用了LLVM，并且smali2c的技术已经渐渐成熟，所以OLLVM(一个开源的代码混淆器)变成了一个可选项，但是对于加固来说，它的保护是基于代码级别的，需要提供源码或者编译的中间代码。

这当然不是企业能接受的事情，所以需要做二进制加固。但是二进制加固离不开反汇编解析引擎(capstone)，它可以将指令抽出来，然后转为自己的虚拟指令，例如将LLVM IR虚拟为自己的虚拟指令，但是这种方法难度较大。对于代码混淆来说，只用对IR代码进行处理就可以了。

### 如何开始

当然首先想到的是Google，但是Google出来的文章对于真正想做一个有意义的项目的人来说意义并不大。对于本人而言，目前学习了LLVM，了解了其架构与简单的实现，下面要学习的自然是该如何仿照[OLLVM](https://github.com/obfuscator-llvm/obfuscator)或者是[Hikari](https://github.com/HikariObfuscator/Hikari)或者上交的[Armariris(孤挺花)](https://github.com/GoSSIP-SJTU/Armariris)等一些开源项目来实现一些自己的混淆功能。感谢这些开源作者。

### 从熟悉项目开始

下载以上的项目中的一个，用CLion或者其他IDE打开项目查看项目结构(以OLLVM为例)：

 

![1460317-416ddcaaf7081b36.png](https://img-blog.csdnimg.cn/img_convert/1d93402b92d394512d840fbba4627a62.png)

OLLVM项目结构

让我们只关注如下的文件夹，其它的暂且不管：
**include**文件夹

![1460317-8c5f93ba5ae02a94.png](https://img-blog.csdnimg.cn/img_convert/54c027ee66fdaad4f39d4246b4649156.png)

include文件夹

 

其实从文件夹名称就能判断include文件夹是头文件所在的地方，include文件夹之下包含两个文件夹：llvm和llvm-c。
llvm文件夹下有如下目录：`llvm\Transforms\Obfuscation`，可以看到此文件夹下有一些头文件：

![1460317-3aefa6dec250099e.png](https://img-blog.csdnimg.cn/img_convert/635cbfb1efbab0c52a283e4887c27dee.png)

Obfuscation头文件

 

此处是存放OLLVM项目中自己写的pass的头文件的地方，由此可知，如果我们需要些自己的pass的话，那么对应的pass类的头文件也需要在`include\llvm\Transforms`新建一个文件夹专门用来存放头文件。头文件的具体内容暂且不管，接下来再去看看实现文件在哪里。

打开与`include`文件夹平行的`lib`文件夹并进入`lib\Transforms\Obfuscation`目录：

![1460317-90b137286a43df0a.png](https://img-blog.csdnimg.cn/img_convert/aa77b25f5a2981db6a331ae7d578af15.png)

Obfuscation所在目录


打开`Obfuscation`目录，可以看到与之前的头文件一一对应的实现文件：

![1460317-e63d8aa8d6cd1777.png](https://img-blog.csdnimg.cn/img_convert/3940f970eade1914fe32e0c322959655.png)

实现文件


至此，与我们编写自己的pass一样，在`include\llvm\Transforms\Obfuscation`定义头文件，在`lib\Transforms\Obfuscation`写实现文件。这样，我们就明白了该如何开始写自己的项目。不过要注意的是，不管是LLVM还是OLLVM，它们都是通过编写makefile来实现项目的运行的，所以我们得熟练掌握makefile的编写与依赖，才能玩转自己的项目。

 

### OLLVM简单源码分析

在分析源码之前，首先介绍一下IR的基本结构：
IR代码是由一个个Module组成的，每个Module之间互相联系，而Module又是由一个个Function组成，Function又是由一个个BasicBlock组成，在BasicBlock中又包含了一条条Instruction。

 

![1460317-e7593ef3362b5f55.png](https://img-blog.csdnimg.cn/img_convert/63b65e116c901d9d670b6892a39034a9.png)

IR代码结构

以基本块的分割为例

对于OLLVM的每个pass，其主要的工作继承对应的pass类，就是对相应的方法进行重写，例如SplitBasicBlock的实现，它继承自FunctionPass，并重写了runOnFunction方法：

```cpp
bool SplitBasicBlock::runOnFunction(Function &F) {



  // Check if the number of applications is correct



  if (!((SplitNum > 1) && (SplitNum <= 10))) {



    errs()<<"Split application basic block percentage\



            -split_num=x must be 1 < x <= 10";



    return false;



  }



 



  Function *tmp = &F;



 



  // Do we obfuscate



  if (toObfuscate(flag, tmp, "split")) {



    split(tmp);



    ++Split;



  }



 



  return false;



}
```

**1、**SplitBasicBlock 首先对`SplitNum`进行判断，`SplitNum`定义如下：

```cpp
static cl::opt<int> SplitNum("split_num", cl::init(2),



                             cl::desc("Split <split_num> time each BB"));
```

此处是对用clang编译源文件的时候选用的参数split做的定义：

```bash
clang -mllvm -split test.c



clang -mllvm -split_num=3 test.c
```

第一条命令表示启用对基本block的分割，使之扁平化。
第二条命令表示对基本block分割次数为3次(前提是必须已启用split)，默认是1次。
**2、**对于splitNum在1~10 之外的情况，提示分割次数错误，即分割次数必须在1~10次之内。
**3、**对于符合要求的splitNum，调用`toObfuscate`函数进行处理，处理方式如下(该函数在`Utils.h`文件中)：

```cpp
bool toObfuscate(bool flag, Function *f, std::string attribute) {



  std::string attr = attribute;



  std::string attrNo = "no" + attr;



 



  // Check if declaration



  if (f->isDeclaration()) {



    return false;



  }



 



  // Check external linkage



  if(f->hasAvailableExternallyLinkage() != 0) {



    return false;



  }



 



  // We have to check the nofla flag first



  // Because .find("fla") is true for a string like "fla" or



  // "nofla"



  if (readAnnotate(f).find(attrNo) != std::string::npos) {



    return false;



  }



 



  // If fla annotations



  if (readAnnotate(f).find(attr) != std::string::npos) {



    return true;



  }



 



  // If fla flag is set



  if (flag == true) {



    /* Check if the number of applications is correct



    if (!((Percentage > 0) && (Percentage <= 100))) {



      LLVMContext &ctx = llvm::getGlobalContext();



      ctx.emitError(Twine("Flattening application function\



              percentage -perFLA=x must be 0 < x <= 100"));



    }



    // Check name



    else if (func.size() != 0 && func.find(f->getName()) != std::string::npos) {



      return true;



    }



    if ((((int)llvm::cryptoutils->get_range(100))) < Percentage) {



      return true;



    }



    */



    return true;



  }



 



  return false;



}
```

可以看到该函数主要是各种检查以及判断是否启用了split功能，判断依据就是`Functions annotations`和`flag`。关于`Functions annotations`的介绍请看[这里](https://www.jianshu.com/p/0567346fd5e8)。

接下来看分割处理的函数`split`：

```cpp
void SplitBasicBlock::split(Function *f) {



  std::vector<BasicBlock *> origBB;



  int splitN = SplitNum;



 



  // Save all basic blocks



  for (Function::iterator I = f->begin(), IE = f->end(); I != IE; ++I) {



    origBB.push_back(&*I);



  }



 



  for (std::vector<BasicBlock *>::iterator I = origBB.begin(),



                                           IE = origBB.end();



       I != IE; ++I) {



    BasicBlock *curr = *I;



 



    // No need to split a 1 inst bb



    // Or ones containing a PHI node



    if (curr->size() < 2 || containsPHI(curr)) {



      continue;



    }



 



    // Check splitN and current BB size



    if ((size_t)splitN > curr->size()) {



      splitN = curr->size() - 1;



    }



 



    // Generate splits point



    std::vector<int> test;



    for (unsigned i = 1; i < curr->size(); ++i) {



      test.push_back(i);



    }



 



    // Shuffle



    if (test.size() != 1) {



      shuffle(test);



      std::sort(test.begin(), test.begin() + splitN);



    }



 



    // Split



    BasicBlock::iterator it = curr->begin();



    BasicBlock *toSplit = curr;



    int last = 0;



    for (int i = 0; i < splitN; ++i) {



      for (int j = 0; j < test[i] - last; ++j) {



        ++it;



      }



      last = test[i];



      if(toSplit->size() < 2)



        continue;



      toSplit = toSplit->splitBasicBlock(it, toSplit->getName() + ".split");



    }



 



    ++Split;



  }



}
```

该函数首先定义了一个`vector`数组`origBB`用于保存所有的block块，然后遍历`origBB`，对每一个block`curr`，如果它的`size`(即包含的指令数)只有1个或者包含PHI节点，则不分割该block。
对于待分割的block，首先生成分割点，用`test`数组存放分割点，用`shuffle`打乱指令的顺序，使`sort`函数排序前splitN个数能尽量随机。
最后分割block是调用`splitBasicBlock`函数分割基本块。

以上就是对分割基本块的一个简单介绍。OLLVM还有控制流平坦化，虚假控制流、指令替换、字符串加密等功能，对于这些内容还需要进一步的研究。

版权声明：本文为博主原创文章，遵循[ CC 4.0 BY-SA ](https://creativecommons.org/licenses/by-sa/4.0/)版权协议，转载请附上原文出处链接和本声明。