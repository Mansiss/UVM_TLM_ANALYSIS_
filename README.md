# UVM_TLM_ANALYSIS_
```sh

`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 08/02/2022 12:11:30 AM
// Design Name: 
// Module Name: TB
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
///////////////////////////////////////////////////////////////////////////////////////////////////////
// Assume you have producer consisting of three data members
// ( reg [1:0] a = 2'b11, reg [3:0] b = 4'b0100 and reg c = 1'b1 ). 
//Communicate this data to three subscribers with the help of analysis port. 
//Print the data sent by the producer as well as received by all the subscribers for verification.
//////////////////////////////////////////////////////////////////////////////////
`include "uvm_macros.svh"
import uvm_pkg::*;

class producer extends uvm_component;
`uvm_component_utils(producer)
reg [1:0] a = 2'b11;
reg [3:0] b = 4'b0100;
 reg c = 1'b1 ;
integer i; 
 
 uvm_analysis_port#(reg [1:0] )send1;
 uvm_analysis_port#(reg [3:0] )send2;
 uvm_analysis_port#(reg )send3;
 
 function new(input string inst="PROD",uvm_component c);
super.new(inst,c);
send1=new("SEND1",this);
send2=new("SEND2",this);
send3=new("SEND3",this);
endfunction

virtual task run_phase(uvm_phase phase);
phase.raise_objection(phase);

`uvm_info("prod",$sformatf("value of a,b,c in producer class is %0b,%0b,%0b",a,b,c),UVM_NONE)
send1.write(a);
send2.write(b);
send3.write(c);


phase.drop_objection(phase);
endtask
endclass

class subscriber1 extends uvm_component;
`uvm_component_utils(subscriber1)

integer i; 
reg [1:0] x;
reg [3:0]y;
 reg z;
 `uvm_analysis_imp_decl(_porta)
 `uvm_analysis_imp_decl(_portb)
 `uvm_analysis_imp_decl(_portc)
 uvm_analysis_imp_porta#(reg [1:0],subscriber1 )recv1;
 uvm_analysis_imp_portb#(reg [3:0] ,subscriber1)recv2;
 uvm_analysis_imp_portc#(reg,subscriber1 )recv3;
 
 function new(input string inst="s1",uvm_component c);
super.new(inst,c);
recv1=new("SEND1",this);
recv2=new("SEND2",this);
recv3=new("SEND3",this);
endfunction

virtual function void write_porta(input reg [1:0] x);
 `uvm_info("S1",$sformatf("value of x %0b",x),UVM_NONE)
 endfunction
 
 virtual function void write_portb(input reg [3:0] y);
 `uvm_info("S1",$sformatf("value of y %0b",y),UVM_NONE)
 endfunction
 
 virtual function void write_portc(input reg z);
 `uvm_info("S1",$sformatf("value of z %0b",z),UVM_NONE)
 endfunction
endclass

class subscriber2 extends uvm_component;
`uvm_component_utils(subscriber2)

integer i; 
reg [1:0] x;
reg [3:0]y;
 reg z;
 `uvm_analysis_imp_decl(_porta)
 `uvm_analysis_imp_decl(_portb)
 `uvm_analysis_imp_decl(_portc)
 uvm_analysis_imp_porta#(reg [1:0],subscriber2 )recv1;
 uvm_analysis_imp_portb#(reg [3:0] ,subscriber2)recv2;
 uvm_analysis_imp_portc#(reg,subscriber2 )recv3;
 
 function new(input string inst="s2",uvm_component c);
super.new(inst,c);
recv1=new("SEND1",this);
recv2=new("SEND2",this);
recv3=new("SEND3",this);
endfunction

virtual function void write_porta(input reg [1:0] x);
 `uvm_info("S2",$sformatf("value of x %0b",x),UVM_NONE)
 endfunction
 
 virtual function void write_portb(input reg [3:0] y);
 `uvm_info("S2",$sformatf("value of y %0b",y),UVM_NONE)
 endfunction
 
 virtual function void write_portc(input reg z);
 `uvm_info("S2",$sformatf("value of z %0b",z),UVM_NONE)
 endfunction
endclass

class subscriber3 extends uvm_component;
`uvm_component_utils(subscriber3)

integer i; 
reg [1:0] x;
reg [3:0]y;
 reg z;
 `uvm_analysis_imp_decl(_porta)
 `uvm_analysis_imp_decl(_portb)
 `uvm_analysis_imp_decl(_portc)
 uvm_analysis_imp_porta#(reg [1:0],subscriber3 )recv1;
 uvm_analysis_imp_portb#(reg [3:0] ,subscriber3)recv2;
 uvm_analysis_imp_portc#(reg,subscriber3 )recv3;
 
 function new(input string inst="s3",uvm_component c);
super.new(inst,c);
recv1=new("SEND1",this);
recv2=new("SEND2",this);
recv3=new("SEND3",this);
endfunction

virtual function void write_porta(input reg [1:0] x);
 `uvm_info("S3",$sformatf("value of x %0b",x),UVM_NONE)
 endfunction
 
 virtual function void write_portb(input reg [3:0] y);
 `uvm_info("S3",$sformatf("value of y %0b",y),UVM_NONE)
 endfunction
 
 virtual function void write_portc(input reg z);
 `uvm_info("S3",$sformatf("value of z %0b",z),UVM_NONE)
 endfunction
endclass

class env extends uvm_env;

`uvm_component_utils(env)

producer p;
subscriber1 s1;
subscriber2 s2;
subscriber3 s3;

function new(input string inst="env",uvm_component c);
super.new(inst,c);
endfunction

virtual function void build_phase(uvm_phase phase);
super.build_phase(phase);
p=producer::type_id::create("envp",this);
s1=subscriber1::type_id::create("envp1",this);
s2=subscriber2::type_id::create("envp2",this);
s3=subscriber3::type_id::create("envp3",this);
endfunction

virtual function void connect_phase(uvm_phase phase);
super.connect_phase(phase);
p.send1.connect(s1.recv1);
p.send2.connect(s2.recv2);
p.send3.connect(s3.recv3);

endfunction


endclass


class test extends uvm_test;

`uvm_component_utils(test)
env e;

function new(input string inst="env",uvm_component c);
super.new(inst,c);
endfunction

virtual function void build_phase(uvm_phase phase);
super.build_phase(phase);
e=env::type_id::create("test",this);

endfunction
endclass




module TB(

    );
    test t;
    initial begin
    t=new("test",null);
    run_test();
    end
endmodule

```
