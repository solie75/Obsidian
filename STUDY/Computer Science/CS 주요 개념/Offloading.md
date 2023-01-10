Computation offloading은 집약적인 컴퓨터적 task 인 resource 를 hardware accelerator 또는 external platform(cluster, grid, cloud) 와 같은 개별적인 프로세서 로 이동시키는 것이다. coprocessor에 offloading 하는 것은 이미지 렌더링과 수학적인 계산 과 같은 응용프로그램을 가속화 하는 데 사용될 수 있다. network를 통해 external platform 에 offloading computing 하는 것은  컴퓨터의 연산력을 제공 할 수 있고 기기의 하드웨어적 한계(연산능력, 저장, 전력 과 같은)를 넘어 설 수 있다.

## 개념

Computational task 는 기초적인 연산, control logic, input/output 작업 등을 수행함으로써 지시(명령)들을 실행하는 central processor 이 다룬다. computational task 의 효율성은 CPU 가 수행할 수 있는 초당 지시(명령)에 따른다. 특정한 응용프로그램 프로세스는 메인 프로세서에서 보조 프로세서로 task 를 offloading 함으로써 가속화 될 수 있다. 

## Hardware Acceleration

Hardware offers greater possible performance for certain tasks when compared to software. The use of specialized hardware can perform functions faster than software processed on a CPU. Hardware has the benefit of customization which allows for dedicated technologies to be used for distinct functions. For example, a [graphics processing unit (GPU)](https://en.wikipedia.org/wiki/Graphics_processing_unit "Graphics processing unit"), which consists of numerous low-performance cores, is more efficient at graphical computation than a CPU which features fewer, high power, cores.[[4]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-:0-4) Hardware accelerators, however, are less versatile when compared to a CPU.

## Cloud Computing

[Cloud Computing](https://en.wikipedia.org/wiki/Cloud_computing "Cloud computing") refers to both the applications transported over the Internet and the hardware and software in the data centers that provide services; which include data storage and computing.[[5]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-:2-5) This form of computing is reliant on high-speed internet access and infrastructure investment.[[6]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-:7-6) Through network access, a computer can migrate part of its computing to the cloud. This process involves sending data to a network of data centers that have access to computing power needed for computation.

## Cluster Computing

[Cluster computing](https://en.wikipedia.org/wiki/Computer_cluster "Computer cluster") is a type of parallel processing system which combines interconnected stand-alone computers to work as a single computing resource.[[7]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-:6-7) Clusters employ a [parallel programming](https://en.wikipedia.org/wiki/Parallel_computing "Parallel computing") model which requires fast interconnection technologies to support high-[bandwidth](https://en.wikipedia.org/wiki/Bandwidth_(computing) "Bandwidth (computing)") and low [latency](https://en.wikipedia.org/wiki/Latency_(engineering) "Latency (engineering)") for communication between nodes.[[2]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-:5-2) In a shared memory model, parallel processes have access to all memory as a global address space. Multiple processors have the ability to operate independently but they share the same memory, therefore, changes in memory by one processor are reflected on all other processors.[[7]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-:6-7)

## Grid Computing

[Grid computing](https://en.wikipedia.org/wiki/Grid_computing "Grid computing") is a group of networked computers that work together as a virtual supercomputer to perform intensive computational tasks, such as analyzing huge sets of data. Through the cloud, it is possible to create and use computer grids for purposes and specific periods. Splitting computational tasks over multiple machines significantly reduces processing time to increase efficiency and minimize wasted resources. Unlike with parallel computing, grid computing tasks usually have no time dependency associated with them, but instead, they use computers that are part of the grid only when idle, and users can perform tasks unrelated to the grid at any time.[[8]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-8)


## 장점

There are several limitations in offloading computation to an external processor:

-   Cloud computing services have issues regarding downtime, as outages can occur when service providers are overloaded with tasks.
-   External computing requires network dependency which could lead to downtime if a network connection runs into issues.
-   Computing over a network introduces latency which is not desired for latency-sensitive applications, including autonomous driving and video analytics.[[10]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-10)
-   Cloud Providers are not always secure, and in an event of a security breach, important information could be disclosed.
-   Modern computers integrate various technologies including; graphics, sound hardware, and networking within the main circuit board which makes many hardware accelerators irrelevant.
-   Using cloud services removes individual control over the hardware and forces users to trust that cloud providers will maintain infrastructure and properly secure their data.
-   Application software may not be available on the target processor or operating system. Software may need to be rewritten for the target environment. This can lead to additional software development, testing and costs.

## Application

- **video gaming**
Video games are electronic games that involve input, an interaction with a user interface, and generate output, usually visual feedback on a video display device. These input/output operations rely on a computer and its components, including the CPU, GPU, RAM, and storage. Game files are stored in a form of [secondary memory](https://en.wikipedia.org/wiki/Secondary_memory "Secondary memory") which is then loaded into the [main memory](https://en.wikipedia.org/wiki/Main_memory "Main memory") when executed. The CPU is responsible for processing input from the user and passing information to the GPU. The GPU does not have access to a computer's main memory so instead, graphical assets have to be loaded into [VRAM](https://en.wikipedia.org/wiki/Video_RAM_(dual-ported_DRAM) "Video RAM (dual-ported DRAM)") which is the GPU's memory. The CPU is responsible for instructing the GPU while the GPU uses the information to render an image on to an output device. CPU's are able to run games without a GPU through software rendering, however, offloading rendering to a GPU which has specialized hardware results in improved performance.[[13]](https://en.wikipedia.org/wiki/Computation_offloading#cite_note-13)


## Reference

https://en.wikipedia.org/wiki/Computation_offloading


