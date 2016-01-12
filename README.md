# Hardware Circular Buffer Controller
## Introduction
This is a circular buffer controller used in FPGA written in verilog.

According to [wiki](https://en.wikipedia.org/wiki/Circular_buffer): A circular buffer, circular queue, cyclic buffer or ring buffer is a data structure that uses a single, fixed-size buffer as if it were connected end-to-end. This structure lends itself easily to buffering data streams.

<div  align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/f/fd/Circular_Buffer_Animation.gif" width = "400" height = "300" align=center />
</div>

Circular buffer controllers are commonly used in FPGA design. However, verilog does not provide a very flexible method to specifiy the parameters of a circular buffer easily (such as word length, buffer depth, buffer number, etc).

This code privides a easy way to specify the parameters mentioned above. And the controller fully support cross clock domain feature, i.e., data writing clock and data read clock are not the same.

This code is verified by simulation and Xilinx XST synthesizer, and verified on spartan 6 FPGAs. 

## USAGE
1. Specify the circular buffer parameters: WRITE_DATA_WIDTH, WRITE_DATA_DEPTH, READ_DATA_WIDTH, READ_DATA_DEPTH, and BUFFER_NUM.

2. Create a RAM with the same volume of WRITE_DATA_WIDTH * WRITE_DATA_DEPTH * BUFFER_NUM or READ_DATA_WIDTH * READ_DATA_DEPTH * BUFFER_NUM

3. Connect RAM to the controller.

4. When you use, the producer just needs to request the controller whether there is a empty buffer to accept new data by pulling high wr_req_i. The controller will reply as soon as possible by pulling high wr_req_ack_o, and tells the producer the result by wr_req_result_o. (HIGH for valid, LOW for all buffer full). When there is one (or more than one) empty buffer, the producer can just write the data to the buffer (address counter should be privided by the producer). And when finished, the producer just need to pulling HIGH wr_finish_i and wait for the HIGH of wr_finish_ack_o signal.

5. Vice versa operation for the consumer.
