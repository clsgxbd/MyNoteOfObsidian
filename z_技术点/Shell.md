#shell #脚本

#### scp
#scp
SCP是Secure Copy的缩写，它是一个可以安全地将文件在不同计算机之间传输的命令，可以在终端中使用。scp命令提供了一种安全的方式，可以在计算机和服务器之间进行文件传输，而不必担心文件内容被窃取或被篡改。下面是一些scp命令的用法：

1.传输本地文件到远程服务器：
scp [local_file] [user]@[remote_host]:[remote_folder]
例如：
scp ./test.txt user@example.com:/var/www/html/

2.从远程服务器下载文件到本地：
scp [user]@[remote_host]:[remote_folder]/[remote_file] [local_folder]
例如：
scp user@example.com:/var/www/html/test.txt ./downloads/

3.复制一个文件夹到服务器：
scp -r [local_folder] [user]@[remote_host]:[remote_folder]
例如：
scp -r ./webapp user@example.com:/var/www/html/

4.从服务器复制文件夹到本地：

scp -r [user]@[remote_host]:[remote_folder]/[remote_folder] [local_folder]
例如：
scp -r user@example.com:/var/www/html/public ./downloads/

#### ssh
#ssh
SSH是一个用于远程登录和安全文件传输的协议。以下是在脚本中使用SSH的一些用法：

1. 远程执行命令：使用ssh user@host 'command'，可以在远程服务器上执行指定的命令。例如：
ssh user@host 'ls /var/log/'

2. 远程拷贝文件：使用scp命令可以在本地和远程服务器之间拷贝文件。例如：
scp localfile user@host:/path/to/remote/dir/

3. 免密登录：使用SSH密钥对可以在不输入密码的情况下进行SSH登录。例如：
ssh-copy-id user@host

4. 批量执行命令：使用SSH的for循环可以批量执行命令。例如：
for host in $(cat hosts.txt); do
    ssh user@$host 'command'
done

#### zsh
Linux的终端，oh my zsh 有界面美化代码预测提示补全功能 
[zsh](zsh.md)

