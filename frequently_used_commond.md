
# linux常用命令

|用途|命令|详细说明|
|----|-----|----|
|查找|grep -rn --color funcName * | |
|查找|find -name "*.[c.h]" \| xargs grep funcName | |
|增强版查找|http://3ms.huawei.com/hi/group/2027319/blog_2268929.html?mapId=3605023 ||
|查找并替换|find -name "*.lua" \| xargs sed -i 's/require("FeiBaseComm")/require("nphal_base")/g' | 将所有lua文件中的require("FeiBaseComm")替换为require("nphal_base") |
|文件传输|scp /home/mydirecoty/log.txt root@10.137.81.26:/home/remotedirecoty|将本地文件拷贝到远程机器 |
|文件传输|scp 10.137.81.26:/home/remotedirecoty/log.txt /home/mydirecoty|将远程服务器上的文件复制到本机 |
|文件传输|scp -r |拷贝整个目录 |
|文件ftp|mget *.log.zip | 拷贝多个以log.zip为后缀的文件|
|so依赖|objdump -x libfeilfeagt_cortexa15le.so \| grep NEED ||
|so依赖|readelf -d xxx.so ||
|so依赖|ldd /usr/local/php/bin/php |利用ldd查看可执行程序的依赖库|
|反汇编|objdump -lG libfei_drv_srv_nphal_588x.dbg  -S libfei_drv_srv_nphal_588x_arm64.so -m arm > disammble.txt | |
|反汇编|objdump -d -l -S xxx.so  | |
|格式转换|find -type f \| xargs dos2unix | winddows文件格式批量转换为linux格式|
|查看磁盘空间|du -s * \|sort -rn \|head -10|“-a” ：显示每一个文件的磁盘使用量;“-s”：仅显示汇总的使用量;“-k”：报告结果以K字节为单位 |
|创建快捷方式|ln -s /usr1/code/release release|usr1下为源目录， 第二个参数为新建的快捷方式目录|
|统计文件计数|ls -l \| grep "^-" \| wc -l|ls -l长列表输出该目录下文件信息(注意这里的文件，不同于一般的文件，可能是目录、链接、设备文件等)；grep "^-"将长列表输出信息过滤一部分，只保留一般文件，如果只保留目录就是 ^d； wc -l统计输出信息的行数，因为已经过滤得只剩一般文件了，所以统计结果就是一般文件信息的行数，又由于一行信息对应一个文件，所以也就是文件的个数。|

# VIM常用命令
VIM常用 http://3ms.huawei.com/hi/wiki/11298219.html

|用途|命令格式|详细说明|
|----|-----|----|
|启动编辑|vi xxx.x||
|修改|i|在光标位置输入i，开始修改|
|退出|Esc; :q||
|强制退出|Esc; :q!|无修改后强制退出|
|保存退出|Esc; :wq||
|查找|/xxx||
|查找|8302G|跳至8302行|
|查找|gg|跳至文件最前|
|查找|G|跳至文件最后|
|粘贴不自动对齐|：set paste|vim中使用鼠标中键粘贴文件时，经常遇到一个恼火的问题，被贴的东西会往右下飞。set paste解决|
|列编辑|ctrl+v d  shitf+i |http://3ms.huawei.com/km/blogs/details/1737561|
|复制一段|v--->移动--->y  新位置：p||
|复制一行|yy，新位置：p |（注: p在行后插入；P在行前插入）|
|剪切一段|v--->移动--->d 新位置：p||
|剪切一行|dd，新位置：p||
|翻屏|屏中：M；屏上：H；屏下：L||

# GIT常用命令
git中文社区 http://gitbook.liuhui998.com/index.html

|用途|命令格式|详细说明|
|----|-----|----|
|下载代码|git clone ssh://git@source.huawei.com:2222/Router_V8/nw_router_v8_main.git| NA|
|子仓下载|git submodule update --init --recursive --remote --merge| NA|
|子仓更新|git submodule update --init --“repoName”| 指定某个子仓|
|分支切换|git checkout -b branchName remotes/origin/branchName | NA|
|放弃修改|git checkout -- xxxx | 放弃本地修改的文件或目录|
|撤销文件|git rm -r --cached fileName | 撤销git add的文件|
|强制同步|git fetch origin; git reset --hard origin/master; git push -f origin br_lxy_0709 | 在本地主仓目录下依次执行三个命令，清除本地残留的主子仓关系，强制与master同步。|
|分支删除|git branch -d branchName | 删除本地分支|
|同步主线|git merge origin/OrigName | 先切换到本地分支，执行merge命令，同步OrigName后提交|
|同步xxx分支代码|git submodule foreach git pull origin 被同步分支名；git submodule foreach git push origin 自己的分支名 | |
|本地撤销上次提交|git reset --soft HEAD^ ||
|挑合|git cherry-pick 38361a68 |将本地分支A的38361a68提交同步到本地分支B，先checkout到B，再执行cherry-pick|
|修改记录查询|git blame -L 100,200 xxx.c |查看100~200行该文件的修改记录|
|更新提交log|git reset xxxx; git commit --amend; |先根据hash值xxx复位此次提交记录， 使用新log commit重新提交|

# GDB常用命令

|用途|命令格式|详细说明|
|----|-----|----|
|条件断点|b FEI_TBLM_ADD if $r0==0x55e| 函数第一个入参uiTblId为0x55e时断点|
|指定线程断点|b osm_chrparse.cpp thread 69| 默认断点所有线程均设置， 多线程调用同一函数可能会在其它线程断住。需要指定线程断点跟踪。|
|寄存器显示|info reg| 函数断点后查看函数入参， R0对应第一个参数; finish退出函数时， R0为函数返回值|
|寄存器显示|x /4x 0xb037899| 十六进制显示指定地址中的4个word|
|变量监控|(gdb) watch *(int*)0x3786aa30| 监控0x3786aa30地址的内容变化|
|watch查看与删除|info watchpoints ； 删除del break| 没有删除watchpoint，删除断点即可|


# python常用命令

|用途|命令|详细说明|
|----|-----|----|
|断点|python -m pdb test.py | |
|| | |
