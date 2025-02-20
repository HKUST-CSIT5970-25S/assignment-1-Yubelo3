[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: ZHOU, Yu
### Student Id: 21134995
### Email: yzhoufv@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

> I use phoronix as described in the lab slides, but with different test suite.  
> For cpu test, I have tried running `phoronix-test-suite benchmark cpu` but that takes too long, so I choose the `smallpt` suite.  
> For memory test, I tried `phoronix-test-suite benchmark ramspeed` and choose to run `average` in `Floating-point` so that the time used in testing is acceptable.  
> The followings are main commands used in my test:  
```Bash
sudo apt update
sudo apt install php-zip -y
wget https://github.com/phoronix-test-suite/phoronix-test-suite/releases/download/v10.8.4/phoronix-test-suite_10.8.4_all.deb
sudo dpkg -i phoronix-test-suite_10.8.4_all.deb
sudo apt --fix-broken install -y
phoronix-test-suite benchmark smallpt -y
phoronix-test-suite benchmark ramspeed -y
```


1. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    > Detailed logs can be found at `logs.txt`.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 176.290 Seconds, 0.09% | 10488.06 MB/s, 0.37% |
    | `t2.medium`  | 91.674 Seconds, 0.13% | 19303.35 MB/s, 0.35% |
    | `c5d.large` | 113.399 Seconds, 0.10% | 13696.61 MB/s, 0.28% |
    

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.  

    

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.  
    > Detailed logs can be found at `logs.txt`.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 3.88 Gbits/sec | 0.256/0.282/0.334/0.031 ms |
    | `m5.large` - `m5.large`   | 4.95 Gbits/sec | 0.186/0.195/0.203/0.007 ms |
    | `c5n.large` - `c5n.large` | 4.95 Gbits/sec | 0.149/0.192/0.317/0.072 ms |
    | `t3.medium` - `c5n.large` | 2.09 Gbits/sec | 0.696/0.866/1.124/0.158 ms |
    | `m5.large` - `c5n.large`  | 2.40 Gbits/sec | 0.705/0.721/0.742/0.013 ms |
    | `m5.large` - `t3.medium`  | 4.07 Gbits/sec | 0.235/0.251/0.282/0.018 ms |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    > Detailed logs can be found at `logs.txt`.
    
    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 34.5 Mbits/sec | 58.208/58.220/58.240/0.012 ms |
    | N. Virginia - N. Virginia | 1.02 Gbits/sec | 0.285/0.617/0.917/0.224 ms |
    | Oregon - Oregon           | 9.30 Gbits/sec | 0.095/0.105/0.126/0.012 ms |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
