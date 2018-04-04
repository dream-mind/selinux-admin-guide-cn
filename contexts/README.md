# 2.SELINUX上下文 

&emsp;&emsp;进程和文件标有SELinux上下文，其中包含其他信息，如SELinux用户，角色，类型和可选的级别。运行SELinux时，所有这些信息都用于制定访问控制决策。在红帽企业Linux中，SELinux提供基于角色的访问控制（RBAC），类型执行（TE）和可选的多级安全（MLS）的组合。

&emsp;&emsp;以下是显示SELinux上下文的示例。 SELinux上下文用于运行SELinux的Linux操作系统上的进程，Linux用户和文件。使用以下命令查看文件和目录的SELinux上下文：
```shell
$ ls -Z file1
-rwxrw -r-- user1 group1 unconfined_u：object_r：user_home_t：s0 file1
```

SELinux上下文遵循SELinux用户：role：type：level语法。这些字段如下所示：
***SELinux用户***
&emsp;&emsp;SELinux用户身份是授权一组特定角色以及特定MLS / MCS范围的策略的已知身份。每个Linux用户都使用SELinux策略映射到SELinux用户。这允许Linux用户继承对SELinux用户的限制。映射的SELinux用户身份在SELinux上下文中用于该会话中的进程，以便定义他们可以输入的角色和级别。以root身份输入以下命令以查看SELinux和Linux用户帐户之间的映射列表（您需要安装policycoreutils-python软件包）：
```shell
＃semanage login -l
Login Name           SELinux User         MLS/MCS Range        Service

__default__          unconfined_u         s0-s0:c0.c1023       *
root                 unconfined_u         s0-s0:c0.c1023       *
system_u             system_u             s0-s0:c0.c1023       *
```

&emsp;&emsp;每个系统的输出可能会有所不同：
  * `Login Name`中列出了Linux用户。
  * `SELinux User`列出了Linux用户映射到哪个SELinux用户。对于进程，SELinux用户限制哪些角色和级别可以访问。
  * `MLS/MCS Range`列是多级安全（MLS）和多分类安全（MCS）使用的级别。
  * `Service`是确定正确的SELinux上下文，Linux用户应该在该上下文中登录系统。默认情况下，使用星号（ `*`）字符，表示任何服务。

***角色***
&emsp;&emsp;SELinux的一部分是基于角色的访问控制（RBAC）安全模型。角色是RBAC的一个属性。 SELinux用户被授权进行角色授权，角色授权进行域授权。该角色充当域和SELinux用户之间的中介。可以输入的角色决定可以输入哪些域;最终，这控制着哪些对象类型可以被访问。这有助于减少特权升级攻击的漏洞。
***类型***
&emsp;&emsp;该类型是Type Enforcement的一个属性。该类型为进程定义了一个域，并为文件定义了一个类型。 SELinux策略规则定义类型如何访问对方，无论是访问类型的域还是访问其他域的域。只有存在特定的SELinux策略规则才允许访问。
***水平***
&emsp;&emsp;该级别是MLS和MCS的一个属性。 MLS范围是一对水平，如果水平不同，则写为低水平高水平;如果水平相同（`s0-s0`与`s0`相同），则为低水平。每个级别都是灵敏度类别对，分类是可选的。如果有类别，则级别被写为敏感度：类别集。如果没有类别，则将其写为敏感度。

如果类别集是连续的系列，则可以缩写。例如，`c0.c3`与`c0，c1，c2，c3`相同。 `/etc/selinux/targeted/setrans.conf`文件将级别（`s0:c0`）映射为人类可读形式（即`CompanyConfidential`）。在红帽企业Linux中，目标策略强制MCS，而在MCS中，只有一个灵敏度`s0`。红帽企业Linux中的MCS支持1024种不同的类别：`c0`到`c1023`。 `s0-s0：c0.c1023`的灵敏度为s0，并授权所有类别。

MLS执行Bell-La Padula强制访问模型，并用于Labeled Security Protection Profile（LSPP）环境。要使用MLS限制，请安装selinux-policy-mls软件包，并将MLS配置为默认的SELinux策略。红帽企业Linux附带的MLS策略省略了许多不属于评估配置一部分的程序域，因此桌面工作站上的MLS不可用（不支持X Window系统）;但是，可以构建包含所有程序域的上游SELinux参考策略的MLS策略。有关MLS配置的更多信息，请参见第4.13节“多级安全性（MLS）”。