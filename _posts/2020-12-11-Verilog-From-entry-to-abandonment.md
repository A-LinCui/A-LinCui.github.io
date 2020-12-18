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

#### **2020.12.18**
- 昨晚发现键盘扫描电路在功能仿真下正常，但是在门级仿真下异常。在除`S5`状态下一切正常。但是当状态机跳转至`S5`时，`col`发生不明原因的抖动，联动外部信号`row`也发生变化，使得状态混乱，无法正常计时输出。百思不得其解，因为`col`本应仅在`current state`触发下时变化，在固定`S5`下不存在任何竞争冒险、过渡过程，而`next state`也不应变化。
- 今天和助教debug一下午，将`next state`的触发换为clock前沿触发。代码如下。
  ```
  always@(posedge clk)
  begin
      case(current_state)
          S0: next_state = (row == no_press)? S0:S1;
          S1: next_state = (row == no_press)? S2:S5;
          S2: next_state = (row == no_press)? S3:S5;
          S3: next_state = (row == no_press)? S4:S5;
          S4: next_state = (row == no_press)? S0:S5;
          S5: next_state = (row == no_press)? S0:S5;
      endcase
  end
  ```
  但是在`confirm`输出下仍然存在问题，提示根本原因并未解决。意外获得助教特赦，验收时写死即可，因为他也弄不出来。
- 晚上不甘心，继续一个人debug。尝试精简扫描电路逻辑，仅以行列BCD输出，意外通过时序仿真。功夫不负有心人。
- 现在的正常时序仿真如下：
  ![](https://github.com/A-LinCui/A-LinCui.github.io/raw/master//img/post/time_sim.png)

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

#### **2020.12.17**
- Amount Manger finished. 最后还剩键盘扫描电路和控制器待完成。