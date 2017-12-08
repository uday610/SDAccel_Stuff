Q: How can I measure data transfer rate between the Host machine and FPGA device? 

A: The SDAccel tool provides a small example to measure data-transfer between Host machine and FPGA. 
     On AWS AMI, you can find the example directory inside the tool installation area: 
        /opt/Xilinx/SDx/2017.1.op/examples/host_global/

  Here are the simple steps to use the "host_global" testcase to measure data-transfer rate after copying it at your local directory. 

     1. cp -r /opt/Xilinx/SDx/2017.1.op/examples/common/  <your directory>/.
     2. cp -r /opt/Xilinx/SDx/2017.1.op/examples/host_global/  <your directory>/.
     3. cd <your directory>/host_global
     4. make -f sdaccel.mk host
     5. Execute the Host executable:
          sudo sh
          sh-4.2# source /opt/Xilinx/SDx/2017.1.rte/setup.sh 
          sh-4.2# ./host_global_bandwidth

    
Upon running, it generates a file **metric1.csv** with the detail of data-transfer rate. 
       
Q: How do I maximize data-transfer rate between Host and FPGA device? 

A: In the OpenCL Host code, using the API **clEnqueueMigrateMemObjects** is recommended API to maximize the data-transfer rate instead of clEnqueueWriteBuffer/clEnqueueReadBuffer. 

Q: What to do when Software Emulation passes, but Hardware Emulation fails? 

A: If the Host and Kernel code have potential bugs such as memory related issues, the Hardware Emulation may fail. 
One way to detect such memory issues in the code is using Software Verification tools such as Valgrind.
   
If the code is clean of memory related issues, the Hardware Emulation can be debugged using the standard debugger like GDB using from SDAccel environment.
 
Please refer to the following page in order to do application debug:
 [Application Debug: SDAccel User Guide](https://www.xilinx.com/html_docs/xilinx2017_1/sdaccel_doc/topics/application-debug/concept_application_debug.html?hl=hardware%2Cemulation)

Q: How to troubleshoot deadlock issues in the SDAccel runtime? 

A: You may use the SDAccel Application debug feature by using GDB to find the location of deadlock.   

One common reason of deadlock is the lack of synchronization among different commands executed from a single out-of-order command queue. So in case of out-of-order command queue, always make sure about event dependencies and setup proper synchronization if required. 
