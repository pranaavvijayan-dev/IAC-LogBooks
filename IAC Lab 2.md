
## Task 1 - Simple sinewave generator
---

**Step 1 - Specifying a 256 x 8 bit ROM**

Open in VS Code the folder for this repo. In the folder **_task1_**, create the component **rom.sv** according to Lecture 4 slide 14

![](Pasted%20image%2020261022113614.png)

![](Pasted%20image%2020261022143752.png)

![](Pasted%20image%2020261022143716.png)

**Step 2 - Create the sinegen module**

This requires you to create a "top-level" module **_sinegen.sv_**, which includes two components: **_counter.sv_** and **_rom.sv_** as shown below. (See L2 slide 16).

![](Pasted%20image%2020261022135608.png)

![](Pasted%20image%2020261022135527.png)

![](Pasted%20image%2020261022143657.png)


**Step 3 - Create the testbench for sinegen**

Based on what you learned from Lab 1, try to write the testbench **_sinegen_tb.cpp_** on your own. You should initially set incr[7:0] to 1. Then use the **_vbdPlot()_** function to plot output data sample to Vbuddy. If you need help, I have included my version of **_sinegen_tb.cpp_** at the end of this file as an appendix.

Here are two things you should do:

1. Since you are generating a continuous sinewave, the number of simulation cycle should be increased to a large value, say, 1,000,000!
2. Because of this large number of simulation cycles before termination, you need to have an elegant way to exit the simulation. Use the **_vbdGetkey()_** function to check if a key has been pressed. This can be achieved with the following code snippet to exit the simulation any time you press the 'q' key:

```c
    // either simulation finished, or 'q' is pressed
    if ((Verilated::gotFinish()) || (vbdGetkey()=='q')) 
      exit(0);                // ... exit if finish OR 'q' pressed
```

Note that **_vbdGetkey()_** is non-blocking, meaning that it returns immediately with a NULL character if no key is pressed, otherwise it returns the ASCII code of the key being pressed.

![](Pasted%20image%2020261022143632.png)

**Step 4 - Modify the shell script _doit.sh_** You should know how to do this by now.

Now, make the Verilated model and test your signal generator.

![](Pasted%20image%2020261022140200.png)

### _Test-yourself Challenge_

Modify your design so that you use the **_vbdValue()_** function to chage the frequency of the sinewave generated.

![▶ Watch video](20261022_150704.mp4)


## Task 2 - Sine and Cosine Dual wave generation

Copy over to the task2 folder the main files from Task 1. Modify your design in Task 1 so that you generate **TWO** simultaenous sinusoid signals which have different phases. The phase offset between the two sinusoids is determined by the rotary encoder on Vbuddy. You need to use a dual-port ROM similar to that you used in Task 1. The first port's address **_addr1[7:0]_** is from the counter. The second port's address **_addr2]7:0]_** is offset from the first address. This offset is read from Vbuddy with the **_vbdValue()_** function.

To display two waveforms on Vbuddy, you can call the **_vbdPlot()_** function twice, first with **_data1[7:0]_**, then with **_data2[7:0]_**.

Test your design.

By setting the offset value to 64, the two waveforms will be exactly 90 degrees apart in their phases, making one a sinewave and the other a cosine wave at the same frequency. These signals, called "quadrature signals", are often used in communication systems.

![](Pasted%20image%2020261022195549.png)

![](Pasted%20image%2020261022195559.png)

![](Pasted%20image%2020261022195614.png)

![](Pasted%20image%2020261022195706.png)

## Task 3 - Capture and display audio signal in RAM
---

The goal of Task 3 is to capture real audio signal using the microphone/amplifier module on Vbuddy. These samples are written to a dual-port RAM stored in successive locations. At the same time, read back from the RAM the store signal at a different address offset from the write address. In this way, the retrieved signal is a delayed version of the original signal.

The difference between the read and write addresses is the offset. Use the **_vbdValue()_** function to vary this offset using the rotary encoder.

You can plot both the write and read data on the Vbuddy display (using **_vbdPlot()_** function).

Capturing audio signal from Vbuddy requires two steps:

1. Use the **_vbdInitMicIn()_** function to inform Vbuddy the size of audio signal buffer to reserve for storing the captured sample.
2. Use the **_vbdMicValue()_** function to return the next sample in the audio buffer. When all the samples are exhausted, Vbuddy will automatically capture another block of audio samples and store them in the audio buffer.

```c
  // intialize variables for analogue output
  vbdInitMicIn(RAM_SZ);
  
  // ask Vbuddy to return the next audio sample
  top->mic_signal = vbdMicValue();
```

To perform this task you need to first create a 512 x 8 bit dual-port RAM component. (See notes.) Combine the dual-port RAM with the counter to form a top-level module (say, **_sigdelay.sv_**). To save time, the testbench **_sigdelay_tb.cpp_** is provide for you in the Lab_2 repo.

Compile and test your design. You need an audio signal source. I suggest that you can use the following online website to generate a variety of signals at different frequencies:

[Tone Generator](https://onlinetonegenerator.com/)

![](Pasted%20image%2020261024230244.png)

![](Pasted%20image%2020261024230317.png)

![](Pasted%20image%2020261024230331.png)

![](Pasted%20image%2020261024230422.png)

![](20261024_225841.mp4)