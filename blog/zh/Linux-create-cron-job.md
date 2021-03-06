@def title = "Linux创建cron定时任务"
@def published = "3 December 2020"
@def tags = ["Linux", "Cron", "Command"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

Cron是Linux一个很有用的工具，也是开发人员最喜欢的工具，因为它可以让你使用通用脚本和特定于任务的脚本在特定的时间段、日期和间隔自动运行命令。有了该描述，你可以想象系统管理员如何使用它来自动执行备份任务、目录清除、通知等。

Cron作业在后台运行，并不断检查`/etc/crontab`文件，`/etc/cron.*/`和`/var/spool/cron/`目录。我们最好不要直接编辑cron文件，因为每个用户都有唯一的crontab。

那你应该如何创建和编辑cron作业？我们可以使用crontab命令。crontab是用于创建，编辑，安装，卸载和列出cron作业的方法。

创建和编辑cron作业的命令是相同而且很简单。而且更酷的是，你无需在创建新文件或编辑现有文件后重新启动cron。
```bash
$ crontab -e
```
### **Cron语法**

就像使用任何语言一样，当你了解**cron**的语法时，使用**cron**会容易得多，它的语法有两种格式：
```bash
A B C D E USERNAME /path/to/command arg1 arg2
OR
A B C D E USERNAME /root/backup.sh
```
以上cron语法的说明：

*   **A：分钟**范围：**0 – 59**
*   **B：时间**范围：**0 – 23**
*   **C：天数**范围：**1 – 31**
*   **D：月**范围：**1 – 12** or **JAN-DEC**
*   **E：星期几**：**0 – 6** or **SUN-SAT，Sunday=0 or 7**。
*   **USERNAME：** 用户名
*   **/path/to/command** – 你要计划的脚本或命令的名称

形象一点表示就是：
```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12 or JAN-DEC)
│ │ │ │ ┌───────────── day of the week (0 - 6 or SUN-SAT, Sunday=0 or 7)
│ │ │ │ │
│ │ │ │ │
│ │ │ │ │
* * * * *
```
此外，Cron使用3个运算符，可以在字段中指定多个值：

1.  **星号`(*)`**：指定字段的所有可能值，`* * * * *` 在每天的每分钟运行。
2.  **逗号`(,)`**：指定值列表，`2,10 4,5 * * *`在每天第 4 和第 5 小时的第 2 和第 10 分钟运行。
3.  **破折号`(-)`**：指定值范围，`0 4-6 * * *` 在第 4、5、6 小时的第 0 分钟运行。
4.  **分隔符`(/)`**：指定步长值，`20/15 * * * *` 从第 20 分钟到第 59 分钟每隔 15 分钟运行（第 20、35 和 50 分钟）。

Cron的语法和运算符大概就这么多，下面展示一些cron示例。

### **Cron工作示例**

运行cron命令的第一步是使用以下命令安装crontab：
```
#crontab -e
```
在每天**凌晨3点**运行`/root/backup.sh`：
```bash
0 3 * * * /root/backup.sh
```
在每个月的第二天的**下午4:30**运行`script.sh`：
```bash
30 16 2 * * /path/to/script.sh
```
在每周工作日的晚上**10点**运行`/scripts/phpscript.php`：
```bash
0 22 * * 1-5 /scripts/phpscript.php
```
在每天的午夜，凌晨2点和凌晨4点后的**23分钟**，运行`perlscript.pl`：
```bash
23 0-23 / 2 * * * /path/to/perlscript.pl
```
每个星期日的04:05运行Linux命令：
```bash
5 4 * * sun /path/to/linuxcommand
```
### **Cron选项**

列出cron作业。
```
#crontab -l
OR
#crontab -u username -l
```
删除所有crontab作业。
```
#crontab -r
```
删除特定用户的Cron作业。
```
#crontab -r -u username
```
### **Crontab中的字符串**

字符串是开发人员最喜欢的东西，因为它们通过消除重复的书写来帮助节省时间。Cron具有特定的字符串，可用于更快地创建命令：

1.  `@hourly`：每小时运行一次，即"**0 \* \* \* \***"
2.  `@midnight`：每天运行一次，即"**0 0 \* \* \***"
3.  `@daily`：与午夜相同
4.  `@weekly`：每周运行一次，即"**0 0 \* \* 0**"
5.  `@monthly`：每月运行一次，即"**0 0 1 \* \*"**
6.  `@annually`：每年运行一次，即"**0 0 1 1 \***"
7.  `@yearly`：与**@annually**相同
8.  `@reboot`：每次启动时运行一次

例如，这是每天备份系统的方法：
```bash
@daily /path/to/backup/script.sh
```

至此，你已经拥有使用**Cron**创建和管理系统任务所需的全部内容。现在，你可以开始使用计划的命令来设置和维护多个环境。

当你对Crontab的工作方式了解得足够多时，可以使用这些漂亮的[Crontab生成器实用程序](https://www.tecmint.com/online-cron-job-generator-and-tester-for-linux/)免费生成crontab行。

另外，你可以在[此处](https://help.ubuntu.com/community/CronHowto)阅读Ubuntu的有关如何使用Cron的文章。

## **REFERENCE**

[https://www.tecmint.com/create-and-manage-cron-jobs-on-linux/](https://www.tecmint.com/create-and-manage-cron-jobs-on-linux/)

[https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows](https://docs.github.com/en/free-pro-team@latest/actions/reference/events-that-trigger-workflows)
