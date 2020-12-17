---
layout:     post
title:      Verilog ~ From Entry to Abandonment
subtitle:   Verilog is too hard!
date:       2020-12-11
author:     A-LinCui
header-img: img/post/night.png
catalog: true
tags:
    - Verilog
---


#### **2020.12.10**
- Quartus 2，modelsim和Altera联合仿真时，记得``Settings/EDA Tol Settings/Simulation``填写testbench的时候，Name和Top Level Module都写testbench里面的最顶层module名，如``standard_7448_tb``。
- Testbench名称改过来之后，AmountManger还是无法进入仿真，显示没有读取模型。无语…… Verilog太难了，幸好大作业的DDL还有两周。明天就去找EDA老师询问一下。
- 破案了。当Testbench里面存在Bugs的时候，就会显示``No Design Loaded``。Bugs在生成clock信号的位置。但是这个位置我认为根本不存在问题。真是玄学。早知道当初应该选Vivado的。
- 存在问题的代码段如下。处于精神崩溃状态中……
  ```
  //Generate 50MHz clock signal
  parameter ClockPeriod = 20;
  initial begin
      clk = 0;  
      //TODO: Are you kidding me? There's no bug at all!
      always #(ClockPeriod / 2) clk = ~clk;
  end
  ```
- 早上起来继续做project。

#### **2020.12.11**
- **踩坑记录：** 
  - Altera与Modelsim联合仿真的时候，后台运行的爱奇艺与华为电脑管家会使Modelsim发生混乱，具体现象为不停地弹出窗口。
  - 解决方案：卸载爱奇艺、关闭华为电脑管家的开机自启动选项，并重启计算机。
  - 卸载爱奇艺，并且电脑管家关闭退出后，发现问题完全解决。估计是自带的电脑管家导致modelsim运行时有文件或者信息丢失导致。在此建议在使用华为笔记本上安装的modelsim时，将电脑管家退出后使用，或者卸载后重新安装其他杀毒软件。
- Test Bench Debug
  - 昨天的bug得以解决，原因是``inital begin``与``always``为两个并列的模块，不能将后者放在前者中使用。
  - 如果一定要在``initial begin``模块中使用，应该使用``forever``。
  - 更新后的代码如下:
    ```
    //Generate 50MHz clock signal
    parameter ClockPeriod = 20;
    always #(ClockPeriod / 2) clk = ~ clk;
    ```
  - Testbench的文件名最好与内部的顶层模块名一致。
  
#### **2020.12.12**
- Amount Manager Debug。current_state和next_state在S2处的跳转异常。待处理。
- Testbench 在主initial函数内部，一定要先将CLK赋值，否则无法正常生成时钟信号。

#### ***2020.12.17*
- Amount Manger finished. 最后还剩键盘扫描电路和控制器待完成。