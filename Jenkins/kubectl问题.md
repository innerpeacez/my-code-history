1，k8s调度的pod跑构建任务，无法使用kubectl命令问题

解决方法：挂载kubectl的.kube文件

![image](https://note.youdao.com/yws/res/1237/2C7FE46A7ED1442B9BD668F9906B6AE2)

2，调用apiserver报错
![image](https://note.youdao.com/yws/res/1232/8AA58CCD90D34437B1C238FC19D9C165)

jenkins默认工作目录是/home/jenkins，.kube目录挂载错误，可以在执行kubectl命令时指定.kube文件目录，或者更换卷路径

![image](https://note.youdao.com/yws/res/1246/B54744F73F0A4CC4A9BE4D3906B1A7EB)

