# 仿真
## elaborate error类
### 参数名字不符
 
 ```verilog
 module clock_div(
    input clk,
    output reg clk_div = 0
    );
    // 函数定义
endmodule

// 错误调用
clock_div U1(
    .clock(clock), // 参数名应为clk，而不是clock
    .clk_div(clk_sys)
);
```
 >  [USF-XSim-62] 'elaborate' step failed with error(s). Please check the Tcl console output or 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/elaborate.log' file for more information.

### 匿名的组件调用
```verilog
// 禁止匿名调用，应该为
// clock_div U1 (
clock_div ( 
    .clk(clock),
    .clk_div(clk_sys)
);
```
 >  [USF-XSim-62] 'elaborate' step failed with error(s). Please check the Tcl console output or 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/elaborate.log' file for more information.


### 传入reg变量给reg参数
```verilog
// 模块
module led_8lights(
    input clock,
    input reset,
    output reg [7:0] Y
    );
    // 模块内容
endmodule

// 调用
reg [7:0] Y;  // 错误
wire [7:0] Y; // 正确
led_8lights uut(
    .clock(clock),
    .reset(reset),
    .Y(Y)
);
```
> [VRFC 10-529] concurrent assignment to a non-net Y is not permitted ["D:/vivado/LED_8light/LED_8light.srcs/sim_1/new/led_sim.v":56]

## complie error类

### 在错误的位置赋值或assign
```verilog
module mod(
    );
    wire w;
    reg r;
    assign w = 1;             // 正确,在initial或always之assign
    r = 0;                    // 错误，在initial或always之外赋值
    assign r = 0
    assign r = 
    initial begin
        assign w = 0;         // 错误，在initial或always之内assign
        r = 1;                // 正确，在initial或always之内赋值
    end
endmodule
```
> [USF-XSim 62] 'compile' step failed with error(s) while executing 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/compile.bat' script. Please check that the file has the correct 'read/write/execute' permissions and the Tcl console output for any other possible errors or warnings.

### 使用了未定义的变量
>  [USF-XSim-62] 'compile' step failed with error(s) while executing 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/compile.bat' script. Please check that the file has the correct 'read/write/execute' permissions and the Tcl console output for any other possible errors or warnings.

### 在always块中声明变量
```verilog
always @(count or reset) begin
    reg [7:0] Y;
end
```
> [USF-XSim 62] 'compile' step failed with error(s) while executing 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/compile.bat' script. Please check that the file has the correct 'read/write/execute' permissions and the Tcl console output for any other possible errors or warnings.

### 重复声明变量
```verilog
reg [7:0] Y;
wire Y;
```
> [USF-XSim 62] 'compile' step failed with error(s) while executing 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/compile.bat' script. Please check that the file has the correct 'read/write/execute' permissions and the Tcl console output for any other possible errors or warnings.

### 语法错误
- 参数间没有加逗号
- 参数最后加了分号
- 参数前面没加input或output
- 参数个数不匹配
> [USF-XSim 62] 'compile' step failed with error(s) while executing 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/compile.bat' script. Please check that the file has the correct 'read/write/execute' permissions and the Tcl console output for any other possible errors or warnings.

### 在错误的地方使用inut/output
```verilog
module multiplier_sim();
    input reg[31:0] multiplicand;
    input reg[31:0] multiplier;
    input wire[31:0] product;
    // 错误：input/output不能定义在模块参数列表之外
endmodule
```
> [USF-XSim 62] 'compile' step failed with error(s) while executing 'D:/vivado/LED_8light/LED_8light.sim/sim_1/behav/compile.bat' script. Please check that the file has the correct 'read/write/execute' permissions and the Tcl console output for any other possible errors or warnings.

### 在input中使用reg
```verilog
module ALU(
    input reg y
    );
endmodule
```
 > [USF-XSim-62] 'compile' step failed with error(s) while executing 'D:/vivado/SinglePeriodCPU/SinglePeriodCPU.sim/sim_1/behav/compile.bat' script. Please check that the file has the correct 'read/write/execute' permissions and the Tcl console output for any other possible errors or warnings.
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTcyMjAwMTk2OCw1Njg2MjY5MTAsLTc5Nz
g4Njg3OSw2NDE3MjYxMDUsMTMyODA3NTkxN119
-->