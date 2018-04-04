## 1.4  状态和模式

&emsp;&emsp;SELinux可以处于启用或禁用状态。禁用时，只使用DAC规则。启用时，SELinux可以以下列模式之一运行：

* **执行**(`Enforcing`)：SELinux策略被强制执行。 SELinux拒绝基于SELinux策略规则的访问。
* **宽容**(`Permissive`)：SELinux政策没有执行。 SELinux不会拒绝访问，但是如果在强制模式下运行，拒绝操作将被拒绝。

&emsp;&emsp;使用`setenforce`实用程序在强制模式和宽容模式之间切换。使用`setenforce`所做的更改不会在重新启动时持续存在。要更改为强制模式，请以Linux root用户身份运行`setenforce 1`命令。要更改为宽容模式，请运行`setenforce 0`命令。使用`getenforce`实用程序查看当前的SELinux模式：

```shell
＃getenforce
Enforcing
```

```shell
＃setenforce 0
＃getenforce
Permissive
```

```shell
＃setenforce 1
＃getenforce
Enforcing
```

第4.4节“SELinux状态和模式中的永久更改”介绍了持久状态和模式更改。