# 常用文件管理命令

## 路径:
- 绝对路径: 从根目录开始描述
- 相对路径: 从当前位置开始描述的路径
- . 当前目录
- .. 上级目录
- ~/ 等价于 /home/acs 家目录

## 基本命令
- ctrl c: 取消命令，并且换行
- ctrl u: 清空本行命令
- tab键：可以补全命令和文件名，如果补全不了快速按两下tab键，可以显示备选选项
- 方向键上下：切换成上一个或者下一个命令
- ls: 列出当前目录下所有文件，蓝色的是文件夹，白色的是普通文件，绿色的是可执行文件
    - -l : 展示详细信息
    - -h : 人性化的显示详细信息
    - -a : 显示所有的文件(包括被隐藏的文件, 所有被隐藏的文件都是以.开头的)
    - ll 等价于ls -la
- pwd: 显示当前路径
- cd XXX: 进入XXX目录下, cd .. 返回上层目录
    - cd 返回家目录
    - cd - 返回上一个所在的目录，不是上一级目录
- cp XXX YYY: 将XXX文件复制成YYY，XXX和YYY可以是一个路径，比如../dir_c/a.txt，表示上层目录下的dir_c文件夹下的文件a.txt
    - 功能：复制 + 粘贴 + 重命名
    - cp a/tmp.txt b (复制一份到粘贴到b里面)
    - cp a/tmp.txt b/tmp2.txt; (复制一份到粘贴到b里面,并重命名)
    - cp a b -r; (将a复制一份粘贴到b里面，如果复制文件夹后面加-r)
- * 表示本文件夹里所有文件
- mkdir XXX: 创建目录XXX
    - mkdir a/b/c -p (创建a里有b，b里有c的文件夹组合，加-p创建一系列的文件夹)
- history: 显示历史用过的指令
- find：显示本目录下的文件
- rm:
    - rm XXX： 删除普通文件
    - rm XXX -r：删除文件夹
    - rm tmp.txt tmp2.txt：删除tmp.txt和tmp2.txt
    - rm *.txt：删除本目录下所有txt文件
    - rm a/*：删除a里面所有东西，但保留a这个空文件夹
    - rm /* -rf：删除所有文件 【千万不要用！！！】
    - rm -f：删除被保护的文件
- mv XXX YYY: 将XXX文件移动到YYY，和cp命令一样，XXX和YYY可以是一个路径；重命名也是用这个命令
    - 功能：剪切 + 粘贴
    - mv a/tmp.txt b/ (将a中tmp.txt文件挪到b文件夹里面)
    - mv b/tmp.txt a/tmp2.txt (将b中tmp.txt文件挪到a文件夹里面并重命名为tmp2.txt)
    - mv tmp2.txt tmp.txt (将tmp2.txt重命名为tmp.txt)
- touch XXX: 创建一个文件
    - mkdir与touch区别:mkdir(创建文件夹), touch(创建文件)
- cat XXX: 展示文件XXX中的内容
- 复制文本
    - windows/Linux下：Ctrl + insert，Mac下：command + c
- 粘贴文本
    - windows/Linux下：Shift + insert，Mac下：command + v

作者：yxc <br>
链接：https://www.acwing.com/file_system/file/content/whole/index/content/2855530/， https://www.acwing.com/solution/content/101474/ <br>
来源：AcWing <br>
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。