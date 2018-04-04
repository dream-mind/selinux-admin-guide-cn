## 1.2 示例

&emsp;&emsp;以下示例演示了SELinux如何提高安全性：

* 默认操作是拒绝。如果SELinux策略规则不存在以允许访问，例如打开文件的进程，则拒绝访问。
* SELinux可以限制Linux用户。 SELinux策略中存在一些受限制的SELinux用户。 Linux用户可以映射到受限制的SELinux用户，以利用应用于他们的安全规则和机制。例如，将Linux用户映射到SELinux user_u用户会导致Linux用户无法运行（除非另行配置），否则会设置用户ID（setuid）应用程序（如sudo和su），并阻止它们执行文件和应用程序在他们的主目录中。如果进行了配置，这可以防止用户从主目录执行恶意文件。
* 使用过程分离。进程在其自己的域中运行，阻止进程访问其他进程使用的文件，并阻止进程访问其他进程。例如，运行SELinux时，除非另行配置，否则攻击者不能危害Samba服务器，然后使用该Samba服务器作为攻击媒介来读取和写入由其他进程使用的文件，例如MariaDB使用的数据库。
* SELinux有助于限制配置错误造成的损害。域名系统（DNS）服务器通常在所谓的区域传输中在彼此之间复制信息。攻击者可以使用区域传输来使用虚假信息更新DNS服务器。在红帽企业版Linux中运行伯克利互联网名称域（BIND）作为DNS服务器时，即使管理员忘记限制哪些服务器可以执行区域传输，默认的SELinux策略可防止使用区域更新区域文件^[3]^由BIND命名的守护进程本身和其他进程传输。
* 请参阅 [NetworkWorld.com](http://www.networkworld.com/)文章：[服务器软件的安全带：SELinux阻止真实世界的漏洞利用](http://www.networkworld.com/article/2283723/lan-wan/a-seatbelt-for-server-software--selinux-blocks-real-world-exploits.html)^[4]^，了解有关SELinux的背景信息以及SELinux阻止的各种漏洞利用信息