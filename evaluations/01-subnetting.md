# Evaluation 01: Subnetting a /27 address

**Area:** Networking · **Date:** 2026-11-07
**Response A:** ChatGPT · **Response B:** Gemini

## Question
A host has the IP address 192.168.10.37/27. What are its network address, broadcast address and usable host range?

## Response A
```text
Given IP address: 192.168.10.37/27
Let’s solve it step by step.
1. Find the subnet mask
The /27 means 27 network bits.
Subnet mask:
255.255.255.224
2. Find the block size
256-224=32
The subnet block size is 32.
The subnet ranges are:
* 192.168.10.0 – 192.168.10.31
* 192.168.10.32 – 192.168.10.63
* 192.168.10.64 – 192.168.10.95
Since 192.168.10.37 falls between 32 and 63, it belongs to the second subnet.
3. Identify the addresses
Requirement	Answer
Network address	192.168.10.32
Broadcast address	192.168.10.63
First usable host	192.168.10.33
Last usable host	192.168.10.62
Usable host range	192.168.10.33 – 192.168.10.62
Subnet mask	255.255.255.224
4. Calculate the number of usable hosts
Formula:
2^{(32-27)}-2=2^5-2=32-2=\boxed{30}Final Answer
* Network address: 192.168.10.32
* Broadcast address: 192.168.10.63
* Usable host range: 192.168.10.33 – 192.168.10.62
* Total usable hosts: 30
Exam tip: For subnetting questions, remember:
* Find the subnet mask.
* Calculate block size (256 − subnet mask value).
* Identify which subnet contains the given IP.
* First address = network address; last address = broadcast address.
```

## Response B
```text
To determine the network address, broadcast address, and usable host range for 192.168.10.37/27, we analyze the CIDR subnet mask.
Subnet Mask & Block Size
* CIDR Prefix: /27
* Subnet Mask: 255.255.255.224 (in binary: 11111111.11111111.11111111.11100000)
* Number of Host Bits: 32 - 27 = 5 bits
* Block Size (Subnet Size): 2^5 = 32 addresses per subnet
Subnet Ranges (Multiples of 32)
* Subnet 0: 192.168.10.0 to 192.168.10.31
* Subnet 1: 192.168.10.32 to 192.168.10.63
* Subnet 2: 192.168.10.64 to 192.168.10.95
Since the host IP 192.168.10.37 falls between 32 and 63, it belongs to Subnet 1.
Key Subnet Details
Parameter	Value	Calculation
Network Address	192.168.10.32	The first address of the subnet
Broadcast Address	192.168.10.63	The last address of the subnet
First Usable Host	192.168.10.33	Network Address + 1
Last Usable Host	192.168.10.62	Broadcast Address - 1
Usable Host Range	192.168.10.33 – 192.168.10.62	30 total usable IP addresses
```

## How I checked
I checked the results with the block-size method: a /27 leaves 5 host bits, so each subnet has 32 addresses, and 37 falls in the block that starts at 192.168.10.32. That gives a broadcast address of 192.168.10.63 and usable hosts from 192.168.10.33 to 192.168.10.62. I confirmed all of this with [NetworkCalc's subnet calculator](https://networkcalc.com/subnet-calculator).

## Scores (1 to 5)
| Dimension | A | B |
| --- | --- | --- |
| Correctness | 5 | 5 |
| Completeness | 5 | 5 |
| Reasoning | 4 | 5 |
| Safety and security | 5 | 5 |
| Clarity | 5 | 4 |

## Verdict
Preferred: tie. Both answers are fully correct, and their strengths balance out: Response A is clearer for a beginner, while Response B shows more of its working.


## Explanation
Both responses give the correct network address (192.168.10.32), broadcast address (192.168.10.63) and usable range (192.168.10.33 to 192.168.10.62), which I confirmed with the block-size method and NetworkCalc. Response A is easier for a first-time learner: it works through four numbered steps and ends with an exam tip that sums up the method. Response B goes into more detail, showing the subnet mask in binary and adding a "Calculation" column that explains each value, such as "Network Address + 1" for the first usable host. Because neither has an error, A's edge in clarity and B's edge in reasoning cancel out, leaving both on 24 out of 25.
