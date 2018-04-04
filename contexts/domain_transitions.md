#2.1 域转换

通过执行具有新域的`entrypoint`类型的应用程序，一个域中的进程转换到另一个域。`entrypoint`权限用于SELinux策略，并控制哪些应用程序可用于输入域。以下示例演示了域转换：

*程序2.1。一个域转换的例子*

1. 用户想要更改密码。为此，他们运行`passwd`实用程序。`/usr/bin/passwd`可执行文件标有`passwd_exec_t`类型：

   ```shell
$ ls -Z / usr / bin / passwd
-rwsr-xr-x root root system_u：object_r：passwd_exec_t：s0 / usr / bin / passwd
   ```
   `passwd`实用程序访问用`shadow_t`类型标记的`/etc/shadow`：
   ```shell
$ ls -Z / etc / shadow
-r --------。 root root system_u：object_r：shadow_t：s0 / etc / shadow
   ```
2. SELinux策略规则指出，允许在`passwd_t`域中运行的进程读取和写入标有`shadow_t`类型的文件。 `shadow_t`类型仅适用于密码更改所需的文件。这包括`/etc/gshadow`，`/etc/shadow`及其备份文件。
3. SELinux策略规则指出`passwd_t`域的入口点权限设置为`passwd_exec_t`类型。
4. 当用户运行`passwd`实用程序时，用户的shell进程转换到`passwd_t`域。在SELinux中，由于缺省操作是拒绝的，并且存在允许（除其他之外）在`passwd_t`域中运行的应用程序访问标有`shadow_t`类型的文件的规则，允许`passwd`应用程序访问`/etc/shadow`，并且更新用户的密码。

这个例子并不详尽，并且被用作解释域转换的基本例子。虽然有一个实际的规则允许在`passwd_t`域中运行的主体访问标有`shadow_t`文件类型的对象，但在主题转换到新域之前必须满足其他SELinux策略规则。在这个例子中，Type Enforcement确保：
* `passwd_t`域只能通过执行标有`passwd_exec_t`类型的应用程序来输入;只能从授权的共享库中执行，例如`lib_t`类型;并且不能执行任何其他应用程序。
* 只有经过授权的域（如`passwd_t`）才能写入标有`shadow_t`类型的文件。即使其他进程以超级用户权限运行，这些进程也不能写入标有shadow_t类型的文件，因为它们不在`passwd_t`域中运行。
* 只有授权的域可以转换到`passwd_t`域。例如，运行在`sendmail_t`域中的`sendmail`进程没有合法的理由来执行`passwd`;因此，它不能转换到`passwd_t`域。
* 在`passwd_t`域中运行的进程只能读取和写入授权类型，例如标有`etc_t`或`shadow_t`类型的文件。这可以防止`passwd`应用程序被骗入读取或写入任意文件。