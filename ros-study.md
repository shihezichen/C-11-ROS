### ros-study

1. 安装   
    ```python
    # 安装ROS
    # 安装vscode
    # 安装 gazebo-ros
     sudo apt-get install ros-kinetic-gazebo-ros-pkgs ros-kinetic-gazebo-ros-control
    ```
2.  配置vscode
    ```python
    # 安装插件:
    #  chinese   :  vscode中文菜单
    #  c++: 支持C++代码智能补齐
    #  ROS: 支持ROS命令
    #  Txt Syntas:  txt, ini文件展示
    #  Msg Language Support: 
    ```
3.  在 NodeHandle.subscribe() 中使用C++ lambda函数


```c++
 using Float64 = std_msgs::Float64;
 Float64 g_force;

 //roscpp14::NodeHandle nh;
 ros::NodeHandle nh;

// 转换为boost:function
auto func = [&g_force](const Float64 & msg) {        
    ROS_INFO("received:%f", msg.data);
    g_force.data = msg.data;
};
auto callbackFunc = static_cast<boost::function<void(const Float64&)>>(func);

auto subscriber = nh.subscribe<Float64>("force_cmd", 1,  callbackFunc);
```


4. 配置vscode
```shell
# 
```

