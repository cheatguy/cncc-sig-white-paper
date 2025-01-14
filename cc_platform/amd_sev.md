# AMD SEV: AMD安全加密虚拟化技术

## 项目位置链接

https://github.com/AMDESE/AMDSEV

## 项目归属SIG

云原生机密计算SIG

## 技术自身介绍

### 背景

机密计算指的是在Secure or Trusted执行环境中进行计算操作，以防止正在做计算的敏感数据被泄露或者修改。

机密计算的核心是Trusted Execution Environment (TEE)，TEE既可基于硬件实现、也可基于软件实现，本文主要专注于基于硬件和密码学原理的实现，相比于纯软件解决方案，具有较高的通用性、易用性、可靠性、较优的性能以及更小的TCB。其缺点是需要引入可信方，即信任芯片厂商。此外由于CPU相关实现属于TCB，侧信道攻击也成为不可忽视的攻击向量，需要关注相关漏洞和研究进展。

### 问题&挑战

随着越来越多的业务上云，端到端的全链路可信或机密正在慢慢成为公有云基础设施的默认要求而不再是一个特性，需要综合利用加密存储、安全网络传输、机密计算等技术来实现对用户敏感数据全生命周期的保护。

机密计算是当前业界正在补齐的环节，主流的硬件平台已经部分提供或正在实现对机密计算的支持，如AMD SEV，Intel TDX和SGX，Arm CCA，IBM SE和RISC-V的KeyStone等。

### 解决方案

AMD SEV技术基于AMD EPYC CPU，将物理机密计算能力传导至虚拟机实例，在公有云上打造一个立体化可信加密环境。

SEV可保证单个虚拟机实例使用独立的硬件密钥对内存加密，同时提供高性能支持。密钥由AMD平台安全处理器 (PSP)在实例创建期间生成，而且仅位于处理器中，云厂商无法访问这些密钥。

AMD SEV提供了硬件级的内存加密方案, 用以实现安全加密的虚拟化：

内存控制器中集成了AES-128硬件加速引擎: 客户操作系统通过页表选择要加密的页, 对终端用户的应用程序没有任何改变。

AMD安全内存加密（SME）: 所有内存由单一的密钥进行加密, 仅仅在BIOS中开启就可以实现（TSME）。

AMD安全加密虚拟化（SEV）: 每台虚拟机都会被分配自己的独立加密密钥, 宿主机和客户虚拟机之间相互加密隔离。

![image.png](../materials/imgs/amd_sev.png)


Confidential Virtual Machine也可以称作Secure Virtual Machine (SVM)，是最终实现机密计算的实体，本身作为一个可信域存在，SVM自身和在其中运行的用户程序可以不受来自SVM之外的非可信特权软件和硬件的的攻击，从而实现对用户的机密数据和代码的保护。

因为是整个虚拟机作为一个可信域，所以对运行于其中的用户程序可以做到透明，无需重构用户程序，对最终用户而言可以实现零成本可信 （Trust Native）.


## 用户情况

目前AMD SEV 已经在主流的云平台上都已经提供相关的实例和部署：
1. 阿里云G8ae实例内测中。
2. Google公有云已经提供2代AMD SEV的机密计算实例。
3. Microsoft Azure公有云已经提供了AMD SEV的机密计算实例和机密计算容器方案。
