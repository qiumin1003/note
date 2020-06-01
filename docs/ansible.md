## --ask-su-pass
	ask for su password (deprecated, use become)

## --ask-sudo-pass
      ask for sudo password (deprecated, use become)

## --ask-vault-pass
      ask for vault password

## --become-method 'BECOME_METHOD'
      privilege  escalation  method  to  use (default=sudo), valid choices: [
      sudo | su | pbrun | pfexec | doas | dzdo | ksu | runas | pmrun | enable
      | machinectl ]

## --become-user 'BECOME_USER'
      run operations as this user (default=root)

## --list-hosts
      outputs a list of matching hosts; does not execute anything else

## --playbook-dir 'BASEDIR'
      Since  this  tool does not use playbooks, use this as a subsitute play-
      book directory.This sets the relative path for many features  including
      roles/ group_vars/ etc.

## --private-key, --key-file
      use this file to authenticate the connection

## --scp-extra-args 'SCP_EXTRA_ARGS'
      specify extra arguments to pass to scp only (e.g. -l)

## --sftp-extra-args 'SFTP_EXTRA_ARGS'
      specify extra arguments to pass to sftp only (e.g. -f, -l)

## --ssh-common-args 'SSH_COMMON_ARGS'
      specify common arguments to pass to sftp/scp/ssh (e.g. ProxyCommand)

## --ssh-extra-args 'SSH_EXTRA_ARGS'
      specify extra arguments to pass to ssh only (e.g. -R)

## --syntax-check
      perform a syntax check on the playbook, but do not execute it

## --vault-id
      the vault identity to use

## --vault-password-file
      vault password file

## --version
      show program's version number and exit

##   -B 后台运行多少秒，结合-P
     'SECONDS', --background 'SECONDS', run asynchronously, failing after X seconds (default=N/A)

##   -C, --check
      don't  make  any  changes;  instead, try to predict some of the changes
      that may occur

##   -D, --diff
      when changing (small) files and  templates,  show  the  differences  in
      those files; works great with --check

##   -K, --ask-become-pass
      ask for privilege escalation password

##   -M, --module-path
      prepend      colon-separated      path(s)     to     module     library
      (default=[u'/home/jenkins/.ansible/plugins/modules', u'/usr/share/ansi-
      ble/plugins/modules'])

##   -P 每隔多少秒获取信息，结合-B
      'POLL_INTERVAL', --poll 'POLL_INTERVAL'，set the poll interval if using -B (default=15)

##   -R 'SU_USER', --su-user 'SU_USER'

      run  operations  with  su  as this user (default=None) (deprecated, use
      become)

##   -S, --su
      run operations with su (deprecated, use become)

##   -T 'TIMEOUT', --timeout 'TIMEOUT'
      override the connection timeout in seconds (default=10)

##   -U 'SUDO_USER', --sudo-user 'SUDO_USER'
      desired sudo user (default=root) (deprecated, use become)

##   -a 'MODULE_ARGS', --args 'MODULE_ARGS'
      module arguments

##   -b, --become
      run operations with become (does not imply password prompting)

##   -c 'CONNECTION', --connection 'CONNECTION'
      connection type to use (default=smart)
##   -e, --extra-vars 定义变量
      set additional variables as key=value or YAML/JSON, if filename prepend
      with @

##   -f 'FORKS', --forks 'FORKS' 子进程
      specify number of parallel processes to use (default=5)

##   -h, --help
      show this help message and exit

##   -i, --inventory, --inventory-file
      specify  inventory  host  path  or  comma separated host list. --inven-
      tory-file is deprecated

##  -k, --ask-pass 询问密码
      ask for connection password

##  -l 'SUBSET', --limit 'SUBSET'
      further limit selected hosts to an additional pattern

##   -m 'MODULE_NAME', --module-name 'MODULE_NAME'
      module name to execute (default=command)
      condense output

##   -s, --sudo
      run operations with sudo (nopasswd) (deprecated, use become)

##   -t 'TREE', --tree 'TREE'
      log output to this directory

##   -u 'REMOTE_USER', --user 'REMOTE_USER'
      connect as this user (default=None)

##   -v, --verbose
      verbose mode (-vvv for more, -vvvv to enable connection debugging)!>  ansible的配置文件在/etc/ansible/ansible.cfg or ~/.ansible.cfg

?> ansible配置文件默认采用公钥认证，可以关闭

## defaults段
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/knowledge/ansible   master ●  cat ~/.ansible.cfg
[defaults]
host_key_checking = False
remote_user = root
inventory = /Users/qiumin/.qiumin/ansible/hosts
```

### ask_pass
ask_pass即playbook 是否会自动默认弹出弹出密码.默认为no，一般无需打开
```
ask_pass=True
```

### ask_sudo_pass
类似ask_pass,用来控制Ansible playbook 在执行sudo之前是否询问sudo密码.默认为no
```
ask_sudo_pass=True
```

### _forks
设置与主机通信时的默认并行进程数
```
_forks=5
```

### gather_facts
控制默认facts收集（远程系统变量）,默认收集
```
gather_facts: False
```

### host_key_checking
检测主机密钥
```
host_key_checking=True
```

### inventory
默认主机文件
```
inventory = /etc/ansible/hosts
```

### log_path
默认日志目录，默认不开启
```
log_path=/var/log/ansible.log
```

### module_name
默认模块，默认为command,建议使用shell作为默认
```
module_name = command
```

### hosts
playbook要通信的默认主机组
```
hosts=*
```
!> /usr/bin/ansible 一直需要一个host pattern,并且不使用这个选项.这个选项只作用于/usr/bin/ansible-playbook

### remote_port
默认的远程SSH端口，默认为22
```
remote_port = 22
```

### remote_tmp
默认的执行路径，默认路径是在用户家目录下属的目录.Ansible 会在这个目录中使用一个随机的文件夹名称
```
remote_tmp = $HOME/.ansible/tmp
```

### remote_user
默认的远程用户，如果不指定，默认使用当前用户
```
remote_user = root
```

### timeout
默认SSH链接尝试超时时间
```
timeout = 10
```

## paramiko段
### record_host_keys
默认设置会记录并验证通过在用户hostfile中新发现的的主机（如果host key checking 被激活的话）. 这个选项在有很多主机的时候将会性能很差.在 这种情况下,建议使用SSH传输代替. 当设置为False时, 性能将会提升.
```
record_host_keys=True
```

## ssh_connection段

```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/knowledge/ansible   master ●  cat ~/.ansible.cfg
[defaults]
host_key_checking = False
remote_user = root
inventory = /Users/qiumin/.qiumin/ansible/hosts
[ssh_connection]

```

### ssh_args
用来调整SSH的通信连接
```
ssh_args = -o ControlMaster=auto -o ControlPersist=60s
```

### pipelining
在不通过实际文件传输的情况下执行ansible模块来使用管道特性,从而减少执行远程模块SSH操作次数.如果开启这个设置,将显著提高性能. 然而当使用”sudo:”操作的时候, 你必须在所有管理的主机的/etc/sudoers中禁用’requiretty’.默认关闭
```
pipelining=False
```














!> ansible的默认读取的host文件在/etc/ansible/hosts or ~/.qiumin/ansible/hosts

```
 qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
192.168.1.105
192.168.1.106
192.168.1.107
```
## 在本机操作
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
local ansible_connection=local
```
## 调整端口
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
192.168.1.105:33
192.168.1.106
192.168.1.107
```

## 设置主机别名
```host文件
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
lip ansible_ssh_port=5555 ansible_ssh_host=192.168.1.105
192.168.1.106
192.168.1.107
```

```执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m ping
lip | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
## 设置用户名
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
lip ansible_ssh_port=22 ansible_ssh_host=192.168.1.105 ansible_ssh_user=qiumin
192.168.1.106
192.168.1.107
```
## ip连续简写
```
 qiumin@QiuMin-MacBook-Pro  ~  cat ~/.qiumin/ansible/hosts
[session]
192.168.1.10[5:7]
```

## 通过host文件设置变量

```host文件
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  cat ~/.qiumin/ansible/hosts
[session]
lip ansible_ssh_host=192.168.1.127
comp1 ansible_ssh_host=192.168.1.105
comp2 ansible_ssh_host=192.168.1.106
comp3 ansible_ssh_host=192.168.1.107

[session:vars]
redis_log="/data"
```

```yml文件
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  cat a.yaml
- hosts: session
  tasks:
    - debug:
        msg: "My location is {{ redis_log }} "
```

```执行结果
✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [session] ***************************************************************************

TASK [Gathering Facts] *******************************************************************
ok: [comp2]
ok: [comp3]
ok: [lip]
ok: [comp1]

TASK [debug] *****************************************************************************
ok: [lip] => {
    "msg": "My location is /data "
}
ok: [comp1] => {
    "msg": "My location is /data "
}
ok: [comp2] => {
    "msg": "My location is /data "
}
ok: [comp3] => {
    "msg": "My location is /data "
}

PLAY RECAP *******************************************************************************
comp1                      : ok=2    changed=0    unreachable=0    failed=0
comp2                      : ok=2    changed=0    unreachable=0    failed=0
comp3                      : ok=2    changed=0    unreachable=0    failed=0
lip                        : ok=2    changed=0    unreachable=0    failed=0

```

## ansible_ssh_host
      将要连接的远程主机名.与你想要设定的主机的别名不同的话,可通过此变量设置.

## ansible_ssh_port
      ssh端口号.如果不是默认的端口号,通过此变量设置.

## ansible_ssh_user
      默认的 ssh 用户名

## ansible_ssh_pass
      ssh 密码(这种方式并不安全,我们强烈建议使用 --ask-pass 或 SSH 密钥)

## ansible_sudo_pass
      sudo 密码(这种方式并不安全,我们强烈建议使用 --ask-sudo-pass)

## ansible_sudo_exe 
	  (new in version 1.8)
      sudo 命令路径(适用于1.8及以上版本)

## ansible_connection
      与主机的连接类型.比如:local, ssh 或者 paramiko. Ansible 1.2 以前默认使用 paramiko.1.2 以后默认使用 'smart','smart' 方式会根据是否支持 ControlPersist, 来判断'ssh' 方式是否可行.

## ansible_ssh_private_key_file
      ssh 使用的私钥文件.适用于有多个密钥,而你不想使用 SSH 代理的情况.

## ansible_shell_type
      目标系统的shell类型.默认情况下,命令的执行使用 'sh' 语法,可设置为 'csh' 或 'fish'.

## ansible_python_interpreter
      目标主机的 python 路径.适用于的情况: 系统中有多个 Python, 或者命令路径不是"/usr/bin/python",比如  \*BSD, 或者 /usr/bin/python
      不是 2.X 版本的 Python.我们不使用 "/usr/bin/env" 机制,因为这要求远程用户的路径设置正确,且要求 "python" 可执行程序名不可为 python以外的名字(实际有可能名为python26).

      与 ansible_python_interpreter 的工作方式相同,可设定如 ruby 或 perl 的路径....

## ping 模块
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible all -m ping
192.168.1.105 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
192.168.1.107 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
192.168.1.106 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## get_url
```yml
  get_url:
    url: "{{ item.url }}"
    dest: "{{ item.dest }}"
    timeout: 100
  with_items:
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-client-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-common-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-devel-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-libs-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-libs-compat-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }
    - { url: "http://uni.mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-community-server-5.7.29-1.el7.x86_64.rpm",dest: "/tmp" }

```

## command模块

```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible all -m command -a 'echo `hostname`'
192.168.1.106 | SUCCESS | rc=0 >>
`hostname`

192.168.1.107 | SUCCESS | rc=0 >>
`hostname`

192.168.1.105 | SUCCESS | rc=0 >>
`hostname`
```

!> command模块和shell模块的差异见上下文，shell模块会传递变量或者管道到服务器运行，command模块不会，而ansible的默认模块为command模块

## shell模块
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible all -m shell -a 'echo `hostname`'
192.168.1.105 | SUCCESS | rc=0 >>
do1cloud00

192.168.1.106 | SUCCESS | rc=0 >>
do1cloud01

192.168.1.107 | SUCCESS | rc=0 >>
do1cloud02

```

## copy模块
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m copy -a 'src=/Users/qiumin/Downloads/a.yaml dest=/root'
lip | SUCCESS => {
    "changed": true,
    "checksum": "52c9503bc9e3b8f5333ddd79e6b28930192a5450",
    "dest": "/root/a.yaml",
    "gid": 0,
    "group": "root",
    "md5sum": "67c1f4dedb8821df825877d139ae5b55",
    "mode": "0644",
    "owner": "root",
    "size": 86,
    "src": "/root/.ansible/tmp/ansible-tmp-1585120813.75-68485718758798/source",
    "state": "file",
    "uid": 0
}
```
## fetch模块
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m fetch -a 'src=/root/anaconda-ks.cfg dest=/Users/qiumin/Downloads'
lip | SUCCESS => {
    "changed": true,
    "checksum": "625ea4372b3bcf85f3568481c8b01c0d4fc5a35c",
    "dest": "/Users/qiumin/Downloads/lip/root/anaconda-ks.cfg",
    "md5sum": "b5759680e7e8e051c75cee85e0e73e50",
    "remote_checksum": "625ea4372b3bcf85f3568481c8b01c0d4fc5a35c",
    "remote_md5sum": null
}
```
## file模块
### 创建目录
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'dest=/root/qiumin mode=755 state=directory'
lip | SUCCESS => {
    "changed": true,
    "gid": 0,
    "group": "root",
    "mode": "0755",
    "owner": "root",
    "path": "/root/qiumin",
    "size": 6,
    "state": "directory",
    "uid": 0
}
```
### 创建文件
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'path=/root/qiufile111 mode=755 state=touch'
lip | SUCCESS => {
    "changed": true,
    "dest": "/root/qiufile111",
    "gid": 0,
    "group": "root",
    "mode": "0755",
    "owner": "root",
    "size": 0,
    "state": "file",
    "uid": 0
}
```

### 修改文件权限
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'path=/root/qiufile111 mode=600'
lip | SUCCESS => {
    "changed": true,
    "gid": 0,
    "group": "root",
    "mode": "0600",
    "owner": "root",
    "path": "/root/qiufile111",
    "size": 0,
    "state": "file",
    "uid": 0
}
```
### 删除文件或目录
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m file -a 'path=/root/qiufile111 state=absent'
lip | SUCCESS => {
    "changed": true,
    "path": "/root/qiufile111",
    "state": "absent"
}
```
## yum模块
!> 另外还有`disable_gpg_check`和`enablerepo`的常用参数可选
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m yum -a 'name=telnet state=latest'
lip | SUCCESS => {
    "changed": true,
    "msg": "",
    "rc": 0,
    "results": [
        "Loaded plugins: fastestmirror\nLoading mirror speeds from cached hostfile\n * base: mirrors.163.com\n * extras: mirrors.163.com\n * updates: mirrors.163.com\nResolving Dependencies\n--> Running transaction check\n---> Package telnet.x86_64 1:0.17-64.el7 will be installed\n--> Finished Dependency Resolution\n\nDependencies Resolved\n\n================================================================================\n Package          Arch             Version                 Repository      Size\n================================================================================\nInstalling:\n telnet           x86_64           1:0.17-64.el7           base            64 k\n\nTransaction Summary\n================================================================================\nInstall  1 Package\n\nTotal download size: 64 k\nInstalled size: 113 k\nDownloading packages:\nRunning transaction check\nRunning transaction test\nTransaction test succeeded\nRunning transaction\n  Installing : 1:telnet-0.17-64.el7.x86_64                                  1/1 \n  Verifying  : 1:telnet-0.17-64.el7.x86_64                                  1/1 \n\nInstalled:\n  telnet.x86_64 1:0.17-64.el7                                                   \n\nComplete!\n"
    ]
}
```

## user模块
```添加用户
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m user -a 'name=qiu'
lip | SUCCESS => {
    "changed": true,
    "comment": "",
    "create_home": true,
    "group": 1000,
    "home": "/home/qiu",
    "name": "qiu",
    "shell": "/bin/bash",
    "state": "present",
    "system": false,
    "uid": 1000
}
```

```删除用户
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m user -a 'name=qiu state=absent'
lip | SUCCESS => {
    "changed": true,
    "force": false,
    "name": "qiu",
    "remove": false,
    "state": "absent"
}
```
## service模块
```
qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m service -a 'name=rsyslog state=started'
lip | SUCCESS => {
    "changed": true,
    "name": "rsyslog",
    "state": "started",
    "status": {
        "ActiveEnterTimestamp": "Fri 2020-03-13 13:24:17 CST",
        "ActiveEnterTimestampMonotonic": "16282710",
        "ActiveExitTimestamp": "Wed 2020-03-25 15:48:19 CST",
        "ActiveExitTimestampMonotonic": "1045458324290",
        "ActiveState": "inactive",
        "After": "network-online.target basic.target network.target system.slice",
        "AllowIsolate": "no",
        "AmbientCapabilities": "0",
        "AssertResult": "yes",
        "AssertTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "AssertTimestampMonotonic": "9479947",
        "Before": "shutdown.target multi-user.target",
        "BlockIOAccounting": "no",
        "BlockIOWeight": "18446744073709551615",
        "CPUAccounting": "no",
        "CPUQuotaPerSecUSec": "infinity",
        "CPUSchedulingPolicy": "0",
        "CPUSchedulingPriority": "0",
        "CPUSchedulingResetOnFork": "no",
        "CPUShares": "18446744073709551615",
        "CanIsolate": "no",
        "CanReload": "no",
        "CanStart": "yes",
        "CanStop": "yes",
        "CapabilityBoundingSet": "18446744073709551615",
        "ConditionResult": "yes",
        "ConditionTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "ConditionTimestampMonotonic": "9479946",
        "Conflicts": "shutdown.target",
        "ControlPID": "0",
        "DefaultDependencies": "yes",
        "Delegate": "no",
        "Description": "System Logging Service",
        "DevicePolicy": "auto",
        "Documentation": "man:rsyslogd(8) http://www.rsyslog.com/doc/",
        "EnvironmentFile": "/etc/sysconfig/rsyslog (ignore_errors=yes)",
        "ExecMainCode": "1",
        "ExecMainExitTimestamp": "Wed 2020-03-25 15:48:19 CST",
        "ExecMainExitTimestampMonotonic": "1045458331466",
        "ExecMainPID": "858",
        "ExecMainStartTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "ExecMainStartTimestampMonotonic": "9490815",
        "ExecMainStatus": "0",
        "ExecStart": "{ path=/usr/sbin/rsyslogd ; argv[]=/usr/sbin/rsyslogd -n $SYSLOGD_OPTIONS ; ignore_errors=no ; start_time=[Fri 2020-03-13 13:24:10 CST] ; stop_time=[Wed 2020-03-25 15:48:19 CST] ; pid=858 ; code=exited ; status=0 }",
        "FailureAction": "none",
        "FileDescriptorStoreMax": "0",
        "FragmentPath": "/usr/lib/systemd/system/rsyslog.service",
        "GuessMainPID": "yes",
        "IOScheduling": "0",
        "Id": "rsyslog.service",
        "IgnoreOnIsolate": "no",
        "IgnoreOnSnapshot": "no",
        "IgnoreSIGPIPE": "yes",
        "InactiveEnterTimestamp": "Wed 2020-03-25 15:48:19 CST",
        "InactiveEnterTimestampMonotonic": "1045458331893",
        "InactiveExitTimestamp": "Fri 2020-03-13 13:24:10 CST",
        "InactiveExitTimestampMonotonic": "9490900",
        "JobTimeoutAction": "none",
        "JobTimeoutUSec": "0",
        "KillMode": "control-group",
        "KillSignal": "15",
        "LimitAS": "18446744073709551615",
        "LimitCORE": "18446744073709551615",
        "LimitCPU": "18446744073709551615",
        "LimitDATA": "18446744073709551615",
        "LimitFSIZE": "18446744073709551615",
        "LimitLOCKS": "18446744073709551615",
        "LimitMEMLOCK": "65536",
        "LimitMSGQUEUE": "819200",
        "LimitNICE": "0",
        "LimitNOFILE": "4096",
        "LimitNPROC": "63449",
        "LimitRSS": "18446744073709551615",
        "LimitRTPRIO": "0",
        "LimitRTTIME": "18446744073709551615",
        "LimitSIGPENDING": "63449",
        "LimitSTACK": "18446744073709551615",
        "LoadState": "loaded",
        "MainPID": "0",
        "MemoryAccounting": "no",
        "MemoryCurrent": "18446744073709551615",
        "MemoryLimit": "18446744073709551615",
        "MountFlags": "0",
        "Names": "rsyslog.service",
        "NeedDaemonReload": "no",
        "Nice": "0",
        "NoNewPrivileges": "no",
        "NonBlocking": "no",
        "NotifyAccess": "main",
        "OOMScoreAdjust": "0",
        "OnFailureJobMode": "replace",
        "PermissionsStartOnly": "no",
        "PrivateDevices": "no",
        "PrivateNetwork": "no",
        "PrivateTmp": "no",
        "ProtectHome": "no",
        "ProtectSystem": "no",
        "RefuseManualStart": "no",
        "RefuseManualStop": "no",
        "RemainAfterExit": "no",
        "Requires": "basic.target",
        "Restart": "on-failure",
        "RestartUSec": "100ms",
        "Result": "success",
        "RootDirectoryStartOnly": "no",
        "RuntimeDirectoryMode": "0755",
        "SameProcessGroup": "no",
        "SecureBits": "0",
        "SendSIGHUP": "no",
        "SendSIGKILL": "yes",
        "Slice": "system.slice",
        "StandardError": "inherit",
        "StandardInput": "null",
        "StandardOutput": "null",
        "StartLimitAction": "none",
        "StartLimitBurst": "5",
        "StartLimitInterval": "10000000",
        "StartupBlockIOWeight": "18446744073709551615",
        "StartupCPUShares": "18446744073709551615",
        "StatusErrno": "0",
        "StopWhenUnneeded": "no",
        "SubState": "dead",
        "SyslogLevelPrefix": "yes",
        "SyslogPriority": "30",
        "SystemCallErrorNumber": "0",
        "TTYReset": "no",
        "TTYVHangup": "no",
        "TTYVTDisallocate": "no",
        "TasksAccounting": "no",
        "TasksCurrent": "18446744073709551615",
        "TasksMax": "18446744073709551615",
        "TimeoutStartUSec": "1min 30s",
        "TimeoutStopUSec": "1min 30s",
        "TimerSlackNSec": "50000",
        "Transient": "no",
        "Type": "notify",
        "UMask": "0066",
        "UnitFilePreset": "enabled",
        "UnitFileState": "enabled",
        "WantedBy": "multi-user.target",
        "Wants": "network-online.target network.target system.slice",
        "WatchdogTimestampMonotonic": "0",
        "WatchdogUSec": "0"
    }
}
```

## cron模块
```脚本
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m cron -a 'name=test minute=0 hour=1 job=ls'
lip | SUCCESS => {
    "changed": true,
    "envs": [],
    "jobs": [
        "test"
    ]
}
```
```结果
[root@lip1 .ssh]# crontab -l
#Ansible: test
0 1 * * * ls
```
## synchronize模块
```前提是先在目标端安装rsync
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m yum -a 'name=rsync'
lip | SUCCESS => {
    "changed": true,
    "msg": "",
    "rc": 0,
```
```syschronize模块用法
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m synchronize -a 'src=/Users/qiumin/Downloads/  dest=/root/'
lip | SUCCESS => {
    "changed": true,
    "cmd": "/usr/bin/rsync --delay-updates -F --compress --archive --rsh=/usr/local/bin/ssh -S none -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null --out-format=<<CHANGED>>%i %n%L /Users/qiumin/Downloads/ root@192.168.1.127:/root/",
```

## script模块
```sh脚本
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  cat a.sh
echo "dsfsfsfsfsfsdfsf"
```
```script执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible lip -m script -a 'a.sh'
lip | SUCCESS => {
    "changed": true,
    "rc": 0,
    "stderr": "Shared connection to 192.168.1.127 closed.\r\n",
    "stderr_lines": [
        "Shared connection to 192.168.1.127 closed."
    ],
    "stdout": "dsfsfsfsfsfsdfsf\r\n",
    "stdout_lines": [
        "dsfsfsfsfsfsdfsf"
    ]
}
```
?> `playbook`由一个或者多个`play`组成，每个`play`必须有`hosts`和`tasks`,`name`可以省略，`var`可以不定义

## name
	即play名称
## hosts
	即需要执行的host组，可使用单独某台机器的别名
## remote_user
	即定义用于远程的用户，也可以仅在某个task中指定
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: qiumin
  tasks:
    - name: "测试remote_user"
      command: id
      register: output
    - 
      debug: var=output
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试remote_user] *********************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.093959",
        "end": "2020-03-30 09:14:14.572240",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 09:14:14.478281",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)",
        "stdout_lines": [
            "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```

## sudo
	即确认是否需要先sudo后再执行，也可以仅在某个task中指定
```代码
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: qiumin
  sudo: yes
  tasks:
    - name: "测试sudo"
      command: id
      register: output
    - 
      debug: var=output
```
```结果
PLAY [测试play] ****************************************************************************************

TASK [测试sudo] ****************************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.112487",
        "end": "2020-03-30 09:21:02.615402",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 09:21:02.502915",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=0(root) gid=0(root) groups=0(root)",
        "stdout_lines": [
            "uid=0(root) gid=0(root) groups=0(root)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```
## su_user
	如确认需要sudo，确认sudo后的用户,比如登陆到root后切换成elk
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: root
  su: true
  su_user: qiumin
  tasks:
    - name: "测试sudo_user"
      command: id
      register: output
    - 
      debug: var=output
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml
[DEPRECATION WARNING]: Instead of su/su_user, use become/become_user and set become_method to 'su'
(default is sudo). This feature will be removed in version 2.6. Deprecation warnings can be disabled
by setting deprecation_warnings=False in ansible.cfg.

PLAY [测试play] ****************************************************************************************

TASK [测试sudo_user] ***********************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.108443",
        "end": "2020-03-30 16:00:47.404187",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 16:00:47.295744",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)",
        "stdout_lines": [
            "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```
## vars
	即定义变量
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  vars:
      redis_log: "qiumin"
  tasks:
    - name: "测试sudo_user"
      debug: msg="输出redis_log:{{ redis_log }}" 
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试sudo_user] ***********************************************************************************
ok: [lip] => {
    "msg": "输出redis_log:qiumin"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=0    unreachable=0    failed=0
```
## handlers
	触发器，假如上一步执行后是changed=1状态，则触发handlers，第二次触发则没有效果
!> 注意触发器不是按照在task中触发的顺序执行，而是在task执行完成后依次按照handlers中的触发
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  vars:
      redis_log: "qiumin"
  tasks:
    - name: "测试handlers"
      template: src=/Users/qiumin/Downloads/a.yaml dest=/root/minnnn
      notify:
        - test handlers
  handlers:
    - name: test handlers
      debug: msg="成功触发handlers"
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试handlers] ************************************************************************************
changed: [lip]

RUNNING HANDLER [test handlers] **********************************************************************
ok: [lip] => {
    "msg": "成功触发handlers"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0

 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试handlers] ************************************************************************************
ok: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=0    unreachable=0    failed=0
```
## ignore_errors
	即是否忽略错误，执行下一步，True或者False,也可在task中指定
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  ignore_errors: true
  tasks:
    - name: "关闭selinux"
      command: /sbin/setenforce 0
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [关闭selinux] *************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.109399", "end": "2020-03-30 12:13:36.316263", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 12:13:36.206864", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
...ignoring

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=1    unreachable=0    failed=0

```
## tasks
	即任务，每一个play下都有无数的tasks
### name
	即任务名称
### command
	即command模块
```yml代码
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "关闭selinux"
      command: /sbin/setenforce 0
```
```执行结果
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [关闭selinux] *************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.083810", "end": "2020-03-30 08:50:23.092366", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 08:50:23.008556", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
	to retry, use: --limit @/Users/qiumin/Downloads/a.retry

PLAY RECAP *******************************************************************************************
lip                        : ok=0    changed=0    unreachable=0    failed=1
```

### shell
	即shell模块
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  vars:
      redis_log: "qiumin"
  tasks:
    - name: "测试tasks"
      shell: "echo $LANG"
      register: output
    - name: "打印输出"
      debug: msg={{ output }}

```
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试tasks] ***************************************************************************************
changed: [lip]

TASK [打印输出] ******************************************************************************************
ok: [lip] => {
    "msg": {
        "changed": true,
        "cmd": "echo $LANG",
        "delta": "0:00:00.107680",
        "end": "2020-03-30 11:53:28.842424",
        "failed": false,
        "rc": 0,
        "start": "2020-03-30 11:53:28.734744",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "en_US.UTF-8",
        "stdout_lines": [
            "en_US.UTF-8"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```	
### service
	即service模块	
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "测试tasks"
      service: 
         name: firewalld
         state: started
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试tasks] ***************************************************************************************
ok: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=0    unreachable=0    failed=0
```
### copy
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "测试tasks"
      copy: 
         src: /Users/qiumin/Downloads/a.yaml
         dest: /root/b.yaml
```
### template
	template和copy的不同之处在于，如果copy一个yaml，template可包含变量
```
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "测试tasks"
      template: 
         src: /Users/qiumin/Downloads/a.yaml
         dest: /root/c.yaml
```
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [测试tasks] ***************************************************************************************
changed: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=1    unreachable=0    failed=0
```
### meta
  主要是在roles中使用，这里不阐述
### notify
	结合Handlers使用，最佳的应用场景是用来重启服务,查看hadlers具体使用方法
### create
	这个是tag，但未发现文档说明和解析
### register
	用于装command或者shell返回的结果，查看【示例：打印命令返回值】看具体使用方法
### delegate_to 关键字
  任务委派，即假设有一个play中有3个task，中间的task需要在指定机器执行
```yml
- hosts: test70,test71
  gather_facts: no
  tasks:
  - file:
      path: "/tmp/ttt"
      state: touch
 
  - file:
      path: "/tmp/delegate"
      state: touch
    delegate_to: test61
 
  - file:
      path: "/tmp/ttt1"
      state: touch
```

### connection关键字
    当很多任务时，有部分任务需要在本机执行的使用方法
```yaml
- hosts: test70,test61
  gather_facts: no
  tasks:
  - file:
      path: "/tmp/inAnsible"
      state: touch
    connection: local
 
  - file:
      path: "/tmp/test"
      state: touch
```
### run_once 关键字
    用于部分task只需要运行一次的场景
```yaml
- hosts: A,B,C,D,E
  gather_facts: no
  tasks:
  - get_url:
      url: "http://nexus.zsythink.net/repository/testraw/testfile/test.tar"
      dest: /tmp/
    connection: local
    run_once: true
 
  - copy:
      src: "/tmp/test.tar"
      dest: "/tmp"

```
### wait_for
	等待一个端口监听或者等待文件创建才开始下一个动作
```yaml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  ignore_errors: true
  tasks:
    - name: "关闭selinux"
      command: /sbin/setenforce 0
    - name: "检查端口是否监听"
      wait_for:
       host: 0.0.0.0
       delay: 2
       port: 80
       state: started
    - name: "执行"
      command: /sbin/setenforce 0
```
```
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试play] ****************************************************************************************

TASK [关闭selinux] *************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.099897", "end": "2020-03-30 14:49:27.122632", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 14:49:27.022735", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
...ignoring

TASK [检查端口是否监听] **************************************************************************************
ok: [lip]

TASK [执行] ********************************************************************************************
fatal: [lip]: FAILED! => {"changed": true, "cmd": ["/sbin/setenforce", "0"], "delta": "0:00:00.103046", "end": "2020-03-30 14:49:50.963744", "msg": "non-zero return code", "rc": 1, "start": "2020-03-30 14:49:50.860698", "stderr": "/sbin/setenforce: SELinux is disabled", "stderr_lines": ["/sbin/setenforce: SELinux is disabled"], "stdout": "", "stdout_lines": []}
...ignoring

PLAY RECAP *******************************************************************************************
lip                        : ok=3    changed=2    unreachable=0    failed=0
```

### vars_prompt
```yml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  ignore_errors: true
  vars_prompt:
    - name: "First"
      prompt: "please input your username"
      private: no
    - name: "Second"
      prompt: "please input your passwd"
      default: 'good'  # 默认显示一个值
      private: yes  #置为yes的话，那么就是看不见自己输入的什么了
  tasks:
    - name: "打印用户名"
      debug: msg="您输入的用户名为：{{ First }}"
    - name: "打印密码"
      debug: msg="您输入的密码为：{{ Second }}"
```
```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml
please input your username: qiu
please input your passwd [good]:

PLAY [测试play] ****************************************************************************************

TASK [打印用户名] *****************************************************************************************
ok: [lip] => {
    "msg": "您输入的用户名为：qiu"
}

TASK [打印密码] ******************************************************************************************
ok: [lip] => {
    "msg": "您输入的密码为：min"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=0    unreachable=0    failed=0
```

### authorized_key
```host文件
[root@lip1 ~]# cat  /etc/ansible/hosts
[session]
lip ansible_ssh_host=192.168.1.127 ansible_ssh_pass=qiumin520
local ansible_connection=local
```
```yaml文件
[root@lip1 ~]# cat tian.yaml
-
  name: "测试play"
  hosts: local
  gather_facts: false
  ignore_errors: true
  tasks:
    - name: "设置root用户的sshkeys"
      user:
        name: root
        generate_ssh_key: yes
        ssh_key_bits: 2048
        ssh_key_file: .ssh/id_rsa
-
  name: "测试play2"
  hosts: lip
  gather_facts: false
  tasks:
    - name: "将root用户的公钥传递到本机"
      authorized_key:
        user: root
        state: present
        key: "{{ lookup('file', '/root/.ssh/id_rsa.pub') }}"
```
```执行结果
[root@lip1 ~]# ansible-playbook tian.yaml

PLAY [测试play] ****************************************************************************************

TASK [设置root用户的sshkeys] ******************************************************************************
ok: [local]

PLAY [测试play2] ***************************************************************************************

TASK [将root用户的公钥传递到本机] *******************************************************************************
changed: [lip]

PLAY RECAP *******************************************************************************************
lip                        : ok=1    changed=1    unreachable=0    failed=0
local                      : ok=1    changed=0    unreachable=0    failed=0
```
```查看是否已经添加
[root@lip1 ~]# cat ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDLtoGq+GoK2maLUkxp9ns6Sk7GqUDpN8Tb2jrFx2RyqPwBNs9/lgGX52SzM6IUZ4TmWjMxTWOcGVxtWTeOU7pkY3eHXhPvn8KFvc4POFL1J6XepIH5S6deovs4G+w+1tUfWuIzcs+lHjqO2BKT/tB3HH7PZiBJ1A7+JXqkm3SFa+HLHaisuePauI5UVluJZP01JsQ6zYf7fvgBWyJ2pBMZZGGjlXIxAR+ySoRiu7hUK2Te1yVVRSlHHSE5AvmEH2ESW9DJO43CzDCKe59xacqM/tUgj72h5rbIP7f67M2uDEB56M5oX+NAkqBO8+36rJBt/LINfi/HB7kus41rd/Ph ansible-generated on lip1
```


## 示例：打印变量

```
- 
  name: “第一个play”
  hosts: lip
  gather_facts: false
  vars: 
     redis_log: "qiumin"
  tasks:
     - 
        debug: msg="输出redis_log变量的值：{{ redis_log }}"
     - 
        debug: var=redis_log
```

```
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [“第一个play”] *************************************************************************************

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "msg": "输出redis_log变量的值：qiumin"
}

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "redis_log": "qiumin"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=0    unreachable=0    failed=0

```

## 示例：打印命令返回值
```yaml
- 
  name: "打印命令返回"
  hosts: lip
  gather_facts: false
  tasks:
    - 
      command: "w"
      register: ouput
    - 
      debug: var=ouput
```

```rezult
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [打印命令返回] ****************************************************************************************

TASK [command] ***************************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "ouput": {
        "changed": true,
        "cmd": [
            "w"
        ],
        "delta": "0:00:00.122231",
        "end": "2020-03-26 15:03:48.545926",
        "failed": false,
        "rc": 0,
        "start": "2020-03-26 15:03:48.423695",
        "stderr": "",
        "stderr_lines": [],
        "stdout": " 15:03:48 up 13 days,  1:39,  2 users,  load average: 0.00, 0.01, 0.05\nUSER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT\nroot     pts/0    192.168.23.54    07:37   28.00s  0.31s  0.31s -bash\nroot     pts/1    192.168.23.54    15:03    0.00s  0.37s  0.11s w",
        "stdout_lines": [
            " 15:03:48 up 13 days,  1:39,  2 users,  load average: 0.00, 0.01, 0.05",
            "USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT",
            "root     pts/0    192.168.23.54    07:37   28.00s  0.31s  0.31s -bash",
            "root     pts/1    192.168.23.54    15:03    0.00s  0.37s  0.11s w"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```## include tasks 语句
```shell
# 源文件
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: root
  su: true
  su_user: qiumin
  tasks:
    - name: "测试sudo_user"
      command: id
      register: output
    - 
      debug: var=output
```

```yaml
# 拆分后的主文件a.yaml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: root
  su: true
  su_user: qiumin
  tasks:
    - include: b.yaml
    - 
      debug: var=output
```

```yaml
# 拆分后的附加文件b.yaml
- name: "测试sudo_user"
  command: id
  register: output

```

```shell
# 执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml
[DEPRECATION WARNING]: Instead of su/su_user, use become/become_user and set become_method to 'su'
(default is sudo). This feature will be removed in version 2.6. Deprecation warnings can be disabled
by setting deprecation_warnings=False in ansible.cfg.

PLAY [测试play] ****************************************************************************************

TASK [测试sudo_user] ***********************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.108341",
        "end": "2020-03-31 11:10:53.289308",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 11:10:53.180967",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)",
        "stdout_lines": [
            "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0

```
## include tasks 附加变量

```yaml
# 源文件a.yaml
- 
  name: "测试play"
  hosts: lip
  gather_facts: false
  remote_user: root
  su: true
  su_user: qiumin
  tasks:
    - include: b.yaml yourname=qiumin
    - 
      debug: var=output

```

```yaml
# 源文件b.yaml
- name: "测试sudo_user"
  command: id
  register: output
- name: "打印传递后的变量"
  debug: msg="传递进来的变量结果为：{{ yourname }}"
```

```shell
# 执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml
[DEPRECATION WARNING]: Instead of su/su_user, use become/become_user and set become_method to 'su'
(default is sudo). This feature will be removed in version 2.6. Deprecation warnings can be disabled
by setting deprecation_warnings=False in ansible.cfg.

PLAY [测试play] ****************************************************************************************

TASK [测试sudo_user] ***********************************************************************************
changed: [lip]

TASK [打印传递后的变量] **************************************************************************************
ok: [lip] => {
    "msg": "传递进来的变量结果为：qiumin"
}

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.105138",
        "end": "2020-03-31 11:17:10.813778",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 11:17:10.708640",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)",
        "stdout_lines": [
            "uid=1001(qiumin) gid=1001(qiumin) groups=1001(qiumin)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=3    changed=1    unreachable=0    failed=0
```

## import_playbook 语法

```yaml
# 主playbook[a.yml]
- 
  name: "测试主play"
  hosts: lip
  gather_facts: false
  remote_user: root
  tasks:
    - include: b.yaml
    - 
      debug: var=output
- import_playbook: c.yaml
```
```yaml
# task引用的yaml[b.yaml]
- name: "测试task的import"
  command: id
  register: output
```
```yaml
# 主yaml引入的附yaml[c.yaml]
- 
  name: "测试import_playbook"
  hosts: lip
  gather_facts: false
  remote_user: root
  vars:
    yourname: qiumin
  tasks:
     - name: "打印变量"
       debug: msg="传递进来的变量结果为：{{ yourname }}"

```

```shell
# 执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads  ansible-playbook a.yaml

PLAY [测试主play] ***************************************************************************************

TASK [测试task的import] *********************************************************************************
changed: [lip]

TASK [debug] *****************************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.106674",
        "end": "2020-03-31 12:43:47.510809",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 12:43:47.404135",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=0(root) gid=0(root) groups=0(root)",
        "stdout_lines": [
            "uid=0(root) gid=0(root) groups=0(root)"
        ]
    }
}

PLAY [测试import_playbook] *****************************************************************************

TASK [打印变量] ******************************************************************************************
ok: [lip] => {
    "msg": "传递进来的变量结果为：qiumin"
}

PLAY RECAP *******************************************************************************************
lip                        : ok=3    changed=1    unreachable=0    failed=0
```
## 结构化playbook
```shell
# 目录
.
├── main.retry
├── main.yml
└── roles
    └── ceshi
        ├── README.md
        ├── defaults
        │   └── main.yml
        ├── files
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   ├── b.yml
        │   └── main.yml
        ├── templates
        ├── tests
        │   ├── inventory
        │   └── test.yml
        └── vars
            └── main.yml
```

```yaml
# 主main.yml
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat ~/Downloads/test_roles/main.yml
-
 hosts: lip
 gather_facts: false
 remote_user: root
 roles:
    - ceshi
```

```yaml
# ceshi的main.yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat roles/ceshi/tasks/main.yml
    - include: b.yml
    - name: "打印输出"
      debug: var=output
```
```yaml
# ceshi的b.yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat roles/ceshi/tasks/b.yml
- name: "测试task的import"
  command: id
  register: output
```
```yaml
# 执行结果
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  ansible-playbook main.yml

PLAY [lip] *******************************************************************************************

TASK [ceshi : 测试task的import] *************************************************************************
changed: [lip]

TASK [ceshi : 打印输出] **********************************************************************************
ok: [lip] => {
    "output": {
        "changed": true,
        "cmd": [
            "id"
        ],
        "delta": "0:00:00.110835",
        "end": "2020-03-31 13:19:43.892485",
        "failed": false,
        "rc": 0,
        "start": "2020-03-31 13:19:43.781650",
        "stderr": "",
        "stderr_lines": [],
        "stdout": "uid=0(root) gid=0(root) groups=0(root)",
        "stdout_lines": [
            "uid=0(root) gid=0(root) groups=0(root)"
        ]
    }
}

PLAY RECAP *******************************************************************************************
lip                        : ok=2    changed=1    unreachable=0    failed=0
```
### defaults
	默认变量目录，优先级最低，理论上不会用，不做解释
### files
	角色可能会用到的文件，比如nginx的证书
```yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles/roles/test  cat tasks/main.yml
    - name: "测试file"
      copy:
         src: file.sh
         dest: /root/file.sh
```
### handlers
	可以被notify触发，按顺序执行
```yml
#task文件 
- name: "测试handlers"
  template: src=/Users/qiumin/Downloads/a.yaml dest=/root/minnnn
  notify:
    - test handlers
```
```yml
# handle文件
- name: test handlers
      debug: msg="成功触发handlers"
```
### meta
	元数据，另外可以添加依赖，添加dependencies后可自动触发其它roles中的task
```yaml
dependencies: 
  - { role: test, test_value: 3 }
```
### tasks
	即任务
```yaml
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat roles/ceshi/tasks/main.yml
    - include: b.yml
    - name: "打印输出"
      debug: var=output
```
### templates
	模版相关的文件，里面带变量替换用
```yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles/roles/test  cat tasks/main.yml
    - name: "测试template"
      template:
         src: a.sh
         dest: /root/abbbb.sh
```
### vars
	定义变量的目录
```yml
 qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles/roles/test  cat vars/main.yml
---
# vars file for test
yourname: qiumin
```
##  更丰富的结构化
```yml
 ✘ qiumin@QiuMin-MacBook-Pro  ~/Downloads/test_roles  cat main.yml
-
 hosts: lip
 gather_facts: false
 remote_user: root
 pre_tasks:
    - name: "第一个task"
      shell: echo 'hello'
    - name: "第二个task"
      shell: echo 'hello1'
 roles:
    - ceshi
 tasks:
    - name: "第三个task"
      shell: echo 'hello2'
 post_tasks:
    - name: "第四个task"
      shell: echo 'hello3'
```


## yml语法

	1. 大小写敏感
	2. 使用缩进代表层级关系
	3. 缩进不允许使用tab，只能空格
	4. 缩进需注意左侧对齐

### 对象

```
msg: 您好
```
### 数组

```
- ip
- mac
- dns
```

### 复合使用
#### 对象+对象
```
msg: 您好
  ip: 1.1.1.1
  mac: "00:50:56:aa:b3:4d"
  dns: 114.114.114.114
```

#### 对象+数组
```
msg:
  - 
    name: qiumin
  - 
    name1: qiumin1
  - 
    name2: qiumin2
```

#### 数组+数组
```
- ip
  - ip1
  - ip2
- mac
  - mac1
  - mac2
- dns
  - dns1
  - dns2
```
#### 数组+对象
```
-
  ip1: 1.1.1.2
  ip2: 1.1.1.3
-
  mac1: 1.1.1.2
  mac2: 1.1.1.3
-
  dns1: 4.4.4.4
  dns2: 8.8.8.8
```
