原因是编译的库是Debug版本的，而在静态库工程里Build Active Architecture Only的Debug选项里为Yes，所以没有编译出armv7的库，需要改为No

