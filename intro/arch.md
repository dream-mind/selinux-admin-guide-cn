## 1.3 SELINUX结构

&emsp;&emsp;SELinux是Linux内核中的一个Linux安全模块。 SELinux由可加载策略规则驱动。 当进行安全相关的访问时，比如当一个进程试图打开一个文件时，这个操作在SELinux内核中被拦截。 如果SELinux策略规则允许操作，则继续操作，否则操作将被阻止，并且进程接收到错误。

&emsp;&emsp;缓存SELinux决策，例如允许或禁止访问。 该缓存被称为访问矢量缓存（AVC）。 在使用这些缓存的决策时，SELinux策略规则需要检查得更少，这会提高性能。 请记住，如果DAC规则首先拒绝访问，则SELinux策略规则不起作用。