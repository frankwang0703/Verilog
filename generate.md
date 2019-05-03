# Verilog

参考文章：  
verilog语法之generate语句的基本认识: https://blog.csdn.net/qq_38428056/article/details/84821982  
  
一、为什么学习generate？  
在设计中，很多情况下需要编写很多结构相同但是参数不同的赋值语句或者逻辑语句，如果在参数量很大的的情况下，原本的列举就会显得心有余而力不足。c语言中常用for语句来解决此类问题，verilog则为我们提供了generate语句。  
  
二、generate的基本概念及语法  
generate语句的最主要功能就是对module、reg、assign、always、task等语句或者模块进行复制。  
generate语句有generate_for、generate_if、generate_case三种语句。  
下面开来看看这三种语句有什么不同？  
1、generate_for语句  
注：  
（1）、必须使用genvar申明一个正整数变量，用作for循环的判断。（genvar是generate语句中的一种变量类型，用在generate_for中声明正整数变量。）  
（2）、需要复制的语句必须写到begin_end语句里面。就算只有一句！！！！！！  
（3）、begin_end需要有一个类似于模块名的名字。  
  
	例1、利用generate_for来复制assign语句：  
  
module	generate_for(  
	input	[7:0]          data_in,  
	output	[1:0]				 t0,  
	output	[1:0]				 t1,  
	output	[1:0]				 t2,  
	output	[1:0]				 t3  										
);  

wire[1:0]   temp [3:0];       //定义位宽为2，深度为4的temp  
  
genvar    i;								  //利用genvar声明正整数变量  
generate for(i=0;i<4;i=i+1)		//复制模块  
	begin : gfor						    //begi_end的名字  
		assign temp[i] = data_in[2*i+1:2*i];   
	end  
endgenerate  
  
assign t0 = temp[0];    //assign temp[0] = data_in[1:0];  
assign t1 = temp[1];    //assign temp[1] = data_in[3:2];  
assign t2 = temp[2];    //assign temp[2] = data_in[5:4];  
assign t3 = temp[3];    //assign temp[3] = data_in[7:6];  
  
endmodule    
  

2、generate_if语句  
generate_for用于复制模块，而generate_if则是根据模块的参数（必须是常量）作为条件判断，来产生满足条件的电路。相当于判断语句。  
  
例2、  

module	generate_if(  
	input					 t0					,  
	input					 t1					,  
	input					 t2					,  
	output 			        	 d		  	
);

localparam    S = 6;	            //定义模块所需参数,用于判断产生电路  

generate   
	if(S < 7)		  
		assign d = t0 | t1 | t2;  
	else  
		assign d = t0 & t1 & t2;  
endgenerate  
  
endmodule  
  
3、generate_case语句  
generate_case其实跟generate_if一样，都是根据参数（都必须为常量）作为判断条件，来产生满足条件的电路，不同于使用了case语法而已。  
  
例、3  
  
module	generate_case(  
	input					 t0					,  
	input					 t1					,  
	input					 t2					,  
	output 				        d			  
);  
  
localparam    S = 8;	            //定义模块所需参数,用于判断产生电路  
  
generate   
	case(S)  
	0:  
		assign d = t0 | t1 | t2;  
	1:  
		assign d = t0 & t1 & t2;  
	default:  
		assign d = t0 & t1 | t2;  
	endcase  
endgenerate  
  
endmodule  

由上面3副图其实就可以看出generate_if和generate_case功能其实是一样的。但是比较常用的就是generat_for语句。  
