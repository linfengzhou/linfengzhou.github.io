---
layout: post

title: "R中的各种apply函数"

description: ""

category: Chinese
published: true

tags: 
- R 


---

  在使用解释型语言的时候，使用显式循环是一种很坑爹的事情，而利用apply类函数使代码“伪向量化”能够让你的代码更加简洁，具有可读性。
 > Notice: Pretend vectorization only happens at the R level rather than by calling internal C code, you don't get the performance benefits of the vectorization only more readable code.
  
  常用的apply函数包括apply, lapply, tapply等，他们分别用于R中不同的数据结构。
  
## apply()
  
  首先登场的是apply函数, apply()允许用户在矩阵的各行或各列调用特定的函数。以下给出apply的函数形式
     
    apply(x,dimcode,FUN，fargs)
  
 
  参数解释:
  
  - x 是一个矩阵
  - dimcode是维度编号,取值为1代表对每一行应用函数，取值为2代表对每一列利用函数
  - FUN是应用在行或者列上的函数
  - fargs是FUN的可选参数集
  
例如:
  
    z <- matrix(1:6,nrow=3,ncol=2)
    apply(z,2,mean)

上面的代码就是对于一个3*2的矩阵的每一列利用函数mean()
apply()里还可以使用用户自己定义的函数

> Notice: apply输出的结果是第一列而非第一行，如果有需要的话可以使用转置t()

## lapply()
  apply()函数主要利用在矩阵，lapply(list apply)则对列表（或强制转换成列表的向量）的每个组件执行给定的函数，并返回另一个列表。lapply其函数形式为：
  
    lapply(x,FUN...)
    
  - lapply作用于列表的每一个元素上
  - 其返回结果为另一个列表
  
> Notice: 针对返回结果,lapply有两个变种函数，vapply和sapply:
>> vapply即应用于列表而返回向量(Vector)，他在lapply的基础之上还需要第三个参数，即返回的模板。例如:```vapply(x,FUN,numeric(1))```,返回的木板可以是向量也可以是数组。如果输出不能匹配模板，则会抛出错误。

>> sapply是介于lapply和vapply之间的一种形式，即simplfy。其不需要模板，但它会尽可能的把结果简化到一个合适的向量和数组中。  
 

##Split-Apply-Combine & tapply
  This example shows a really common problem when investigating data is how to calculate some statistic 
on a variable that has been split into groups.

    (frogger_scores<-data.frame(
    players = rep(c("Tom","Dick","Harry"),times = c(2,5,3)),
    score = round(rlnorm(10,8),-1)
    ))
    ##split the dataset by players
    (scores_by_players <- with(
     frogger_scores,
     split(score,players)
    ))
    ## apply :mean function
    list_of_means_by_players <- lapply(scores_by_players,mean)
    list_of_means_by_players
    ## combine  
    mean_by_players <- unlist(list_of_means_by_players)
    mean_by_players
    ## all three steps in one  
    with(frogger_scores, tapply(score, players, mean))
    
上面这个例子即将一个已经被分好组的变量进行计算。实质上它做了以下几件事情：

  - 将Data进行分组(Split)
  - 每一个组别对应一个Factor(因子),得到Data的子向量
  - 对其使用函数mean function
  - 最后组合到一起。
  
tapply函数就是这样完成任务的，其函数形式如下：
 
   tapply(x,f,FUN)

  - x代表Data
  - f代表因子或者因子列表
  - FUN代表你所需要使用的函数

由于tapply的第一个参数必须是向量，而不能是矩阵或者数据框，因此，如果数据形式是矩阵或者数据框时，可以使用by()，其调用方式和tapply非常相似。

## matlab包和plyr包 （待更新）


> Notice: 本文为个人学习笔记心得，部分内容可参考 Learning with R 以及 The Art of R Programming。
