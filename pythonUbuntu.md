# ubuntu中的python
<p>简介：python是解释型脚本语言，开发高效，语法简练，常用于数据分析（加上numpy等库，就是免费版matlab）、不同语言的接口编写，缺点是代码速度较慢（解释过程）以及难以操纵底层硬件。linux自带双版本python，不过2.7比较成熟，用的更多。</p>

### 1，推荐IDE环境
<p>经验丰富的程序员也难以保证写的代码没有错误，常用python IDE有很多，ipython、Anaconda、jupyter等，anaconda自带了众多常用py包，方便快速上手使用，并且相对独立与外部系统环境，但偶尔有与系统安装的库冲突的情况。联网时推荐使用<code>sudo pip install "ipython[all]"</code>安装同样的功能到系统中。然后就可以使用<```jupyter notebook```>启动基于浏览器的IDE环境了，比较方便。一般的数据可视化，分段调试都可行，作图大小不好控制，动画更难实现（需要额外插件，而且不一定好用）。</p>

### 2，为python添加模块儿搜索路径
<p>自己开发的py包或者封装的so库，都需要额外设置，python才能找到路径，否则提示<```no module named ...```>，有三种设置方案：<br>
（1）函数添加。在py脚本中<br>
<code>
import sys<br>
sys.path.append("/home/usrname/mysolib")</code><br>
（2）修改环境变量PYTHONPATH<br>
（3）添加.pth文件。在.../python2.7/dist-packages文件夹中添加mysolib.pth文件，内容是所要添加的目录。</p>
