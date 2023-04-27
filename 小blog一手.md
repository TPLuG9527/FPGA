```
2023 4 15（more verilog features - basic gate
hdl bits 刷题(more features 和basic gates)中一些惊艳到我的方法
1： 题目："A "population count" circuit counts the number of '1's in an input vector. Build a population count circuit for a 3-bit input vector.
答案的做法是用case语法 罗列出8种不同从输入， 然后一一对应输出；而我的做法是希望用for循环，再加上if判断， 一开始给Out赋值为0， 用if语句 在每一位上看到1， 就给out加上1，但这样显然不对， 因为verilog的语句是同时执行，我这样做的结果是只会加上一次，做不到计数的效果。
而csdn上有一个很好做法：assign out = in[0] + in[1] + in[2];这样的好处是，把对应的1都加起来，从而达到计数的目的。
2. 题目： out_both: Each bit of this output vector should indicate whether both the corresponding input bit and its neighbour to the left are '1'. For example, out_both[98] should indicate if in[98] and in[99] are both 1. Since in[99] has no neighbour to the left, the answer is obvious so we don't need to know out_both[99].
out_any: Each bit of this output vector should indicate whether any of the corresponding input bit and its neighbour to the right are '1'. For example, out_any[2] should indicate if either in[2] or in[1] are 1. Since in[0] has no neighbour to the right, the answer is obvious so we don't need to know out_any[0].
out_different: Each bit of this output vector should indicate whether the corresponding input bit is different from its neighbour to the left. For example, out_different[98] should indicate if in[98] is different from in[99]. For this part, treat the vector as wrapping around, so in[99]'s neighbour to the left is in[0].
简单来说就是 out_both 实现的是当前位置数字与上一位信号相与， out_any是当前位置信号与下一位信号相或， out_different是当前位置信号与上一位异或，最高一位就和最低位异或
我的做法还是for 套if，写下来也还行， 但答案的做法是：
    assign out_both = in[99:1] & in[98:0];
    assign out_any = in[99:1] | in[98:0];
    assign out_different = in ^ {in[0], in[99:1]};
    着实惊艳到我了。
```

```
2023 4 16
verilog 刷题 mutiplexers - arithnmetic circuit
1：Create a 4-bit wide, 256-to-1 multiplexer. The 256 4-bit inputs are all packed into a single 1024-bit input vector. sel=0 should select bits in[3:0], sel=1 selects bits in[7:4], sel=2 selects bits in[11:8], etc.
变量(index)的进阶用法：
assign out = [4*sel +: 4];  //意思是[4*sel + 3 : 4 * sel] 这样用Index是因为如果我写成我希望的样子， 程序会报错。
assign out = [4*sel -: 4];
2： Arithmetic Circuits 的Adder有疑惑（构造全加器）
3.在100-bit binary adder中， 给定的output顺序是output cout, outpust sum;而我在写代码时发现我用{cout, sum}和{sum, cout}出来的结果不一样（一个对一个错），我就发现， 如果想用{}输出， 那顺序一定要按照top_module中给定的来。
4. 关于全加器
```

```
2022 4 17 kmap - 
1. pos 指或与表达式， sop指与或表达式

```


```
4.23
好久没blog了 自从题刷到时序电路，就一直狂补数电，没码就没blog
flip-flop高电平有效是指 高电平的时候复位为0;

检测一个输入信号何时从一个时钟周期变化到下一个时钟周期（检测脉冲边缘）:
in    输入信号
in_d  输入信号下一拍（与in隔了一个变化周期）
上升沿检测：out = in & ~in_d
下降沿检测： out = ~in & in _d
双边检测（同时检测上升沿和下降沿）： out = in ^ in_d;
```

```
verilog能自动识别同一数字不同进制 比如 4'd9 = d'b1001;
```