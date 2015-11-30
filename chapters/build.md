#构建篇：Build

构建是一个很大的话题，特别是对于传统软件来说，对于Web应用也是相当重要的。

在构建上，Ruby比Python会强大些。
Ruby用的是Rake，Python兴许是scons，如果是用于python的话可以用shovel，这个Python就没有和一个好的标准，

Rakefile算是Ruby的一个标准。

##Rake简介##

> Make 是一个 UNIX® 的本机实用程序，是为管理软件编译过程而设计的。它十分通用，足以用于许多其他环境中，即使它已用于将文档编译成书，维护 Web 站点以及裁减发行版。但是，make 也有自身的约束。它具有自己的语法，这取决于制表符的（tabbed）和非制表符的（nontabbed）空白空间。许多其他工具已经进行了扩展，可以弥 补 make 的一些不足，如 Aegis 和 Ant，但这两者也都具有自己的问题。

> Make 以及类似的工具都有改进的余地，但是它们都不可能让 Ruby 黑客十分开心。您从这里要去哪里？幸好，可以使用一些 Ruby 选项。Rant 是一个由 Stefan Lang 编写的工具（请参阅 参考资料）。Rant 仍处于开发周期的初级阶段，因此它可能还没有成熟到足以适用于每个人。Jim Weirich 编写的 Rake 是一个在 Ruby 社区中广泛使用的成熟系统。

> Rake 是用 Ruby 编写的，并使用 Ruby 作为它的语法，因此学习曲线很短。Rake 使用 Ruby 的元编程功能来扩展语言，使之更利落地适应自动化任务。Rake 附带的 rdoc 中列出了一些优点（请注意，前两个是诸如 make 的其他任务自动化工具所共有的）：

 - 用户可以用先决条件指定任务。
 - Rake 支持规则模式来合并隐式任务。
 - Rake 是轻量级的。它可以用其他项目发布为单个文件。依靠 Rake 的项目不需要在目标系统上安装 Rake。

###简单的Rakefile

    task :default do
      puts "Simple Rakefile Example"
    end

运行结果

    Simple Rakefile Example
    [Finished in 0.2s]

##Shovel##

官方是这么介绍的

> Shovel is like Rake for python. Turn python functions into tasks simply, and access and invoke them from the command line. 'Nuff said. New Shovel also now has support for invoking the same tasks in the browser you'd normally run from the command line, without any modification to your shovel scripts.

那么就

    git clone https://github.com/seomoz/shovel.git
    cd shovel
    python setup.py install 

与用官方的示例，有一个foo.py

    from shovel import task
    
    @task
    def howdy(times=1):
        '''Just prints "Howdy" as many times as requests.
        
        Examples:
            shovel foo.howdy 10
            http://localhost:3000/foo.howdy?15'''
        print('\n'.join(['Howdy'] * int(times)))

shovel一下
        shovel foo.howdy 10

###构建C语言的Hello,World: Makefile

C代码

    #include
    
    int main(){
    	printf("Hello,world\n");
    	return 0;
    }

一个简单的makefile示例

    hello:c 
    	gcc hello.c -o hello
    clean:
    	rm hello

执行：

    make

就会生成hello的可执行文件，再执行

    make clean

清理。

###Rakefile

    task :default =&gt; :make
    
    file 'hello.o' =&gt; 'hello.c' do  
        `gcc -c hello.c`  
    end  
      
    task :make =&gt; 'hello.o' do  
        `gcc hello.o -o hello`  
    end  
      
    task :clean do  
        `rm -f *.o hello`  
    end  

再Rake一下，似乎Ruby中的 Rake用来作构建工具很强大，当然还有其他语言的也可以，旨在可以替代Makefile

###Scons

新建一个SConstruct

    Program('hello.c')

Program('hello.c')

    scons

，过程如下

    phodal@linux-dlkp:~/helloworld&gt; scons
    scons: Reading SConscript files ...
    scons: done reading SConscript files.
    scons: Building targets ...
    gcc -o hello.o -c hello.c
    gcc -o hello hello.o
    scons: done building targets.

总结

    Rakefile
