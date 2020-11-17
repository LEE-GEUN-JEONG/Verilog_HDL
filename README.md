## 자율학습으로 IDEC에서 진행한 Verilog 강의에 관한 자료와 코딩을 정리 및 나열하였음.
## 특이사항으로 IDEC 강의 수료증도 제시하였음.
## Verilog에 관한 흥미로 여러 verilog 강의를 수강하였음.
## Quartus II 13.0sp1 tool을 이용한 HDL Verilog 코딩


### IDEC Verilog HDL 수료증
![image](https://user-images.githubusercontent.com/58419421/99415652-62a93e00-293b-11eb-8077-d1f28475a630.png)
![image](https://user-images.githubusercontent.com/58419421/99415681-6a68e280-293b-11eb-8192-36e80e1eef8c.png)

![image](https://user-images.githubusercontent.com/58419421/99415838-93897300-293b-11eb-9765-e3669c6e9e18.png)


![image](https://user-images.githubusercontent.com/58419421/99415610-545b2200-293b-11eb-9fdc-47865491a4bf.png)







![image](https://user-images.githubusercontent.com/58419421/99415310-03e3c480-293b-11eb-91c5-d9e733cd188e.png)

![image](https://user-images.githubusercontent.com/58419421/99415508-35f52680-293b-11eb-9d70-9763684e95ec.png)

### 소스코드
#### 피보나치 수열을 hw적으로 
module test(clk,reset,max_number, min_number, output_valid, fibonacci_out);
// output fibonacci number between min_number and max_number)
input clk, reset;
input [15:0] max_number, min_number;
output output_valid;
output [15:0] fibonacci_out;

reg output_valid;
reg [15:0] fibonacci_value, prev_fibonacci_value1, prev_fibonacci_value2;

assign fibonacci_out = output_valid?fibonacci_value:0;



always@(posedge clk)
   begin
	    if (reset)
		   begin
			   output_valid=0;
				fibonacci_value=0;
				prev_fibonacci_value1=1;
				prev_fibonacci_value2=0;
			end
		 else
		   begin
			 fibonacci_value=prev_fibonacci_value1 + prev_fibonacci_value2;
			 prev_fibonacci_value2=prev_fibonacci_value1;
			 prev_fibonacci_value1=fibonacci_value;
			 
			 if (fibonacci_value>min_number)
			    output_valid=1;
			 else if (fibonacci_value>max_number)
			    output_valid=0;
				 			  
			end
   end

endmodule
`timescale 1ns/1ns
module test_tb;
reg clk, reset;
reg [15:0] max_number, min_number;
wire output_valid;
wire [15:0] fibonacci_out;

test i0 (clk,reset,max_number, min_number, output_valid, fibonacci_out);

always
#5 clk=~clk;

initial
  begin
    clk=0;
	 reset=1;
	 
	 #10 
	 max_number=200;
	 min_number=100;
	 reset=0;
	 
	 #1000 reset=1;
	 #10 max_number=2000;
	 min_number=500;
    reset=0;	 
  end  
endmodule
-------------------------------------
module test(clk,reset,max_number, min_number, output_valid, fibonacci_out);
// output fibonacci number between min_number and max_number)
input clk, reset;
input [15:0] max_number, min_number;
output output_valid;
output [15:0] fibonacci_out;

reg output_valid;
reg [15:0] fibonacci_value, prev_fibonacci_value1, prev_fibonacci_value2;
reg not_done;

assign fibonacci_out = output_valid?fibonacci_value:0;



always@(posedge clk)
   begin
	    if (reset)
		   begin
			   output_valid=0;
				fibonacci_value=0;
				prev_fibonacci_value1=1;
				prev_fibonacci_value2=0;
				not_done=1;
			end
		 else
		   begin
			 fibonacci_value=prev_fibonacci_value1 + prev_fibonacci_value2;
			 prev_fibonacci_value2=prev_fibonacci_value1;
			 prev_fibonacci_value1=fibonacci_value;
			 
			 if ((fibonacci_value>min_number) && not_done )
			   begin
			    output_valid=1;
				 not_done=0;		 
				end 
			 else if (fibonacci_value>max_number)
			    output_valid=0;
				 			  
			end
   end

endmodule

#### 래치 구현
module fib_latch(clk, reset, in1, out1);
input clk, reset;
input [15:0] in1;
output [15:0] out1;
reg [15:0] out1;

always@(posedge clk)
 begin
   if (reset)
	  out1=0;
	else
	   begin
		if (in1>0)
		  out1=in1;
		
		end
		
 end
 
endmodule

#### 피보나치 테스트
module test(clk,reset,max_number, min_number, output_valid, fibonacci_out);
// output fibonacci number between min_number and max_number)
input clk, reset;
input [15:0] max_number, min_number;
output output_valid;
output [15:0] fibonacci_out;

reg output_valid;
reg [15:0] fibonacci_value, prev_fibonacci_value1, prev_fibonacci_value2;
reg not_done;

assign fibonacci_out = output_valid?fibonacci_value:0;



always@(posedge clk)
   begin
	    if (reset)
		   begin
			   output_valid=0;
				fibonacci_value=0;
				prev_fibonacci_value1=1;
				prev_fibonacci_value2=0;
				not_done=1;
			end
		 else
		   begin
			 fibonacci_value=prev_fibonacci_value1 + prev_fibonacci_value2;
			 prev_fibonacci_value2=prev_fibonacci_value1;
			 prev_fibonacci_value1=fibonacci_value;
			 
			 if ((fibonacci_value>min_number) && not_done )
			   begin
			    output_valid=1;
				 not_done=0;		 
				end 
			 else if (fibonacci_value>max_number)
			    output_valid=0;
				 			  
			end
   end

endmodule

#### 테스트 벤치
`timescale 1ns/1ns
module test_tb;
reg clk, reset;
reg [15:0] max_number, min_number;
wire output_valid;
wire [15:0] fibonacci_out;
wire [15:0] fibonacci_latched_out;

test i0 (clk,reset,max_number, min_number, output_valid, fibonacci_out);
fib_latch i1 (~clk, reset, fibonacci_out, fibonacci_latched_out);


always
#5 clk=~clk;

initial
  begin
    clk=0;
	 reset=1;
	 
	 #10 
	 max_number=200;
	 min_number=100;
	 reset=0;
	 
	 #200 reset=1;
	 #10 max_number=2000;
	 min_number=500;
    reset=0;	 
  end  
endmodule

