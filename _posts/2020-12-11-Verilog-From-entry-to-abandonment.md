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


## TestBench
### 2020.12.11
- Quartus 2，modelsim和Altera联合仿真时，记得Settings/EDA Tol Settings/Simulation，填写testbench的时候，Name和Top Level Module都写testbench里面的最顶层module名，如"standard_7448_tb"。
- Testbench名称改过来之后，AmountManger还是无法进入仿真，显示没有读取模型。无语…… Verilog太难了，幸好大作业的DDL还有两周。明天就去找EDA老师询问一下。
- 破案了。当Testbench里面存在Bugs的时候，就会显示"No Design Loaded"。Bugs在生成clock信号的位置。但是这个位置我认为根本不存在问题。真是玄学。早知道当初应该选Vivado的。
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
