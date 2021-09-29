1.点击soap新建连接任务

![image-20210929103144559](C:\Users\yesen\AppData\Roaming\Typora\typora-user-images\image-20210929103144559.png)

2.输入服务运行的地址

​	C#示例格式："http://127.0.0.1:10010/EdgeWcf/METoPower?SingleWsdl"

![image-20210929103351897](C:\Users\yesen\AppData\Roaming\Typora\typora-user-images\image-20210929103351897.png)

3.点击运行

![image-20210929103623886](C:\Users\yesen\AppData\Roaming\Typora\typora-user-images\image-20210929103623886.png)

4.无回应，检查服务运行状态，使用services.msc

5.与visual studio联调

- 生成后台程序，在后台中开启服务可以使用bat，或者sc指令进行测试
- 保证生成版本与代码版本一致后，选择调式->附加进程，选择目标服务器，
- 如果服务运行在远程需要将远端调试工具复制到目标服务器进行配置，此软件在visualstudio安装目录下能找到，注意连接目标服务器时如果存在域需加’./‘
-  即可进行断点调试，SOAP点击运行后即可测试服务运行状况