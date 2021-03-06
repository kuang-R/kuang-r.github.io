---
layout: post
title:  "Decoy-Enhanced Seamless IP Randomization阅读笔记"
category: paper
---
这边论文的basic idea比较简单，就是放出诱饵和不断改变网络位置（IP地址）来缓解网络侦察(reconnaissance)攻击。我认为它的实验很有意思，本来我以为会在路由器层面进行实现，通过改变IP包的方式对上层透明化。结果后来一看，发现竟然是在一台电脑上搭建虚拟机来模拟全部环境。这就是乞丐版实现吗？不过仔细一想，其实这种实验的可行性高而且也可信，简直就是最好方案啊。

还有就是这篇论文在实现它的协议时用到了很多的知识，对这些知识进行简单的了解会对网络空间安全方向有一个大致的了解。我认为这个还是很值的注意的。

### Problem
网络攻击者会对IP地址进行探测，为了找出现有系统的缺点或者是监控这个系统。

### Basic Idea
动态改变IP地址＋大量释放诱饵(decoy)缓解网络探测攻击。而且在改变IP地址的时候还不能中断现有服务。

### 收获
- [ZMap]（https://www.usenix.org/system/files/conference/usenixsecurity13/sec13-paper_durumeric.pdf）能花45分钟把整个IPv4的地址探测完。
- NMap是一个网络探测工具
- honypots原来是用于吸引攻击者和学习攻击者模式的，在这篇论文中用来延长其扫描时间
- 用常用服务：ssh, ftps, sftp, udpchar, tftp来做实验

### Thinking
这个方法需要大量的IP地址来释放诱饵，如果把迁移往协议栈挪上一层，到TCP/UDP层面会不会降低开销呢？但是端口号只有65535个，如果只在这么多个端口号中进行randomization会不会显得有些不够用？考虑到吞吐量，应该只是需要迁移端口就足够了。

