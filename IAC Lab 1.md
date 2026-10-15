
counter.sv
![375](Pasted%20image%2020261015104029.png)
![Pasted image 20261015104114](Pasted%20image%2020261015104114.png)
The counter counts on the positive edge of clk if enable is 1. It is synchronously reset to 0 if rst is asserted. 
Note the following:
1. The file name and the module name must be the same.
2. The number of bits in the counter is specified with the parameter WIDTH. It is currently set to 8-bit.
3. The always_ff @ (posedge clk) is the way that one specifies a clocked circuit.
4. ‘<=’ in line 12 and 13 are non-block assignments which should be used within an always_ff block.
5. {WIDTH{1’b0}} in line 12 uses the concatenation operator { } to form WIDTH bits of ‘0’. (Can you explain the construct in line 13?)

Here is the mapping between System Verilog and the counter circuit "synthesized" via Verilator:

![Pasted image 20261015100014](Pasted%20image%2020261015100014.png)



![Pasted image 20261015101255](Pasted%20image%2020261015101255.png)



![Pasted image 20261015103039](Pasted%20image%2020261015103039.png)

