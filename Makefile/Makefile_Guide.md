## Makefile

### 基本结构
Makefile是Make读入的唯一配置文件

由make工具创建的目标体(target)，通常是目标文件或可执行文件

要创建的目标体所依赖的文件(dependency_file)

创建每个目标体时需要运行的命令(command)

注意: 命令行前面必须是一个”TAB 键”,否则编译错误为:*** missing separator. Stop.

![Alt text](image.png)
```
Makefile格式：

target : dependcy_files    
<TAB>command
```

target //目标 : target也就是一个目标文件，可以是Object File，也可以是执行文件。还可以是一个标签（Label）

dependcy_files //生成目标所要的目标文件: dependcy_files 就是，要生成那个target所需要的文件或是目标。

command也就是make需要执行的命令。（任意的Shell命令）

这是一个文件的依赖关系，也就是说，target这一个或多个的目标文件依赖于dependcy_files中的文件，其生成规则定义在command中。说白一点就是说，dependcy_files中如果有一个以上的文件比target文件要新的话，command所定义的命令就会被执行。这就是Makefile的规则。也就是Makefile中最核心的内容。