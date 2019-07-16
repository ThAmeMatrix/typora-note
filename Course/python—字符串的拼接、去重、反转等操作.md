python—字符串的拼接、去重、反转、字母花样排序、单词出现判断、统计文件单词频率、硬盘容量、列表转字符串

1. 已知字符串 a = “aAsmr3idd4bgs7Dlsf9eAF”,要求如下 
   1.1 请将a字符串的大写改为小写，小写改为大写。

```
#使用字符串的内置方法a.swapcase()：



>>> a = 'aAsmr3idd4bgs7Dlsf9eAF'



>>> a



'aAsmr3idd4bgs7Dlsf9eAF'



>>> print a.swapcase()



AaSMR3IDD4BGS7dLSF9Eaf



>>> 1234567
```

1.2 请将a字符串的数字取出，并输出成一个新的字符串。

```
#字符串有个内置判断函数，如果字符是纯数字返回True,否则返回False。



>>> a = '2345'



>>> a



'2345'



>>> a.isdigit()



True



>>> b = 'we34234'



>>> b.isdigit()



False



>>> 12345678910
>>> a = 'aAsmr3idd4bgs7Dlsf9eAF'



>>> a



'aAsmr3idd4bgs7Dlsf9eAF'



>>> print [s for s in a if s.isdigit()]



['3', '4', '7', '9']



>>> print ''.join([s for s in a if s.isdigit()])



3479



>>> 12345678
''.join([s for s in a if s.isdigit()])



表达的意思为：



列表l = []



for s in a:#遍历a字符串里面所有字符



    if s.isdigit():



        l.append(s)



''.join() 把所有数字去除任何字符进行组装起来1234567
```

读取一个Log1.txt中的所有数字，并拼接在一起

```
#系统测试log1.txt文件



root@kali:~/python/laowangpy# cat log1.txt 



2017-09-18:xuweibo login in pmsweb at 3:10:45 pm



2017-09-18:xuweibo del admin235 in pmsweb at 4:23:59 pm



2017-09-19:xuweibo mod xwb21 in pmsweb at 6:18:34 pm



2017-09-19:xuweibo login out pmsweb at 19:01:34 pm



root@kali:~/python/laowangpy# 



 



#脚本执行文件:



root@kali:~/python/laowangpy# ls



log1.txt  tokenno.py



root@kali:~/python/laowangpy# cat tokenno.py 



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



f1 = open('log1.txt','r')



str = ''.join([n for n in f1.read(1000)])



print str



print ''.join([x for x in str if x.isdigit()])



f1.close()



 



root@kali:~/python/laowangpy# 



 



#脚本执行情况：



root@kali:~/python/laowangpy# python tokenno.py



2017-09-18:xuweibo login in pmsweb at 3:10:45 pm



2017-09-18:xuweibo del admin235 in pmsweb at 4:23:59 pm



2017-09-19:xuweibo mod xwb21 in pmsweb at 6:18:34 pm



2017-09-19:xuweibo login out pmsweb at 19:01:34 pm



 



2017091831045201709182354235920170919216183420170919190134



root@kali:~/python/laowangpy# 1234567891011121314151617181920212223242526272829303132
```

1.3 请统计a字符串出现的每个字母的出现次数（忽略大小写，a与A是同一个字母），并输出成一个字典。 例 {‘a’:4,’b’:2}

```
#内置方法统计次数



>>> c = 'aasdebbcaa'



>>> c.count('a')



4



>>> 12345
>>> a



'aAsmr3idd4bgs7Dlsf9eAF'



>>> a = a.lower()#所有字母都小写



>>> a



'aasmr3idd4bgs7dlsf9eaf'



>>> print [(x,a.count(x)) for x in set(a)]#首先把a的所有字符串一个一个的拆成列表形式，并遍历每个字符串，且计算每个字符串出现次数。



[('a', 3), ('b', 1), ('e', 1), ('d', 3), ('g', 1), ('f', 2), ('i', 1), ('m', 1), ('3', 1), ('l', 1), ('s', 3), ('r', 1), ('4', 1), ('7', 1), ('9', 1)]



>>> print dict([(x,a.count(x)) for x in set(a)])#把计算出的列表转化成字典形式



{'a': 3, 'b': 1, 'e': 1, 'd': 3, 'g': 1, 'f': 2, 'i': 1, 's': 3, 'm': 1, 'l': 1, '3': 1, 'r': 1, '4': 1, '7': 1, '9': 1}



>>> 12345678910
```

1.4 请去除a字符串多次出现的字母，仅留最先出现的一个。例 ‘abcabb’，经过去除后，输出 ‘abc’

```
root@kali:~/python/laowangpy# cat sortzimu.py 



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



a = 'aAsmr3idd4bugs7Dlsf9eAF'



print "----原始字符串----"



print a



 



a_list = list(a)



print "----转换列表----"



print a_list



 



set_list = list(set(a_list))



print '-----去重列表字符串后，变成新的列表-----'



print set_list



 



set_list.sort(key=a_list.index)



print "----拼接原先位置字符串并去重后组成新的字符串------"



print ''.join(set_list)



 



#a_list = list(a)#转换成list列表



#set_list = list(set(a_list))#对列表内容去重，然后再转换回list。set表示去重



#set_list.sort(key=a_list.index)#对去重以后的list进行原先的排序



#print ''.join(set_list)#拼接成字符串



root@kali:~/python/laowangpy# 



root@kali:~/python/laowangpy# python sortzimu.py



----原始字符串----



aAsmr3idd4bugs7Dlsf9eAF



----转换列表----



['a', 'A', 's', 'm', 'r', '3', 'i', 'd', 'd', '4', 'b', 'u', 'g', 's', '7', 'D', 'l', 's', 'f', '9', 'e', 'A', 'F']



-----去重列表字符串后，变成新的列表-----



['a', 'A', 'b', 'e', 'd', 'g', 'f', 'i', 'F', 'm', '3', 'l', 's', 'r', 'u', '4', '7', '9', 'D']



----拼接原先位置字符串并去重后组成新的字符串------



aAsmr3id4bug7Dlf9eF



root@kali:~/python/laowangpy# 123456789101112131415161718192021222324252627282930313233343536
```

1.5 请将a字符串反转并输出。例：’abc’的反转是’cba’ 
第一种方式：

```
>>> a



'aAsmr3idd4bugs7Dlsf9eAF'



>>> print a[::-1]#存在步长为-1说明该字符串为倒叙输出排序的



FAe9fslD7sgub4ddi3rmsAa



>>> 12345
```

第二种方式:

```
>>> a = 'aAsmr3idd4bugs7Dlsf9eAF'



>>> a



'aAsmr3idd4bugs7Dlsf9eAF'



>>> alist = list(a)



>>> alist



['a', 'A', 's', 'm', 'r', '3', 'i', 'd', 'd', '4', 'b', 'u', 'g', 's', '7', 'D', 'l', 's', 'f', '9', 'e', 'A', 'F']



>>> alist.reverse()



>>> alist



['F', 'A', 'e', '9', 'f', 's', 'l', 'D', '7', 's', 'g', 'u', 'b', '4', 'd', 'd', 'i', '3', 'r', 'm', 's', 'A', 'a']



>>> print ''.join(alist)



FAe9fslD7sgub4ddi3rmsAa



>>> 12345678910111213
```

1.6 去除a字符串内的数字后，请将该字符串里的单词重新排序（a-z），并且重新输出一个排序后的字符 串。（保留大小写,a与A的顺序关系为：A在a前面。例：AaBb）

```
#伪代码



1、要有小写字母从a-z的排序



2、大小写不同，但值相同的字母，大写在小写前面



 



#脚本如下：



root@kali:~/python/laowangpy# vi sortzimu2.py



root@kali:~/python/laowangpy# python sortzimu2.py



['3', '4', '7', '9', 'A', 'A', 'D', 'F', 'a', 'b', 'd', 'd', 'e', 'f', 'g', 'i', 'l', 'm', 'r', 's', 's', 's']



root@kali:~/python/laowangpy# vi sortzimu2.py



root@kali:~/python/laowangpy# cat sortzimu2.py 



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



'''



#伪代码



1、要有小写字母从a-z的排序



2、大小写不同，但值相同的字母，大写在小写前面



'''



 



a = 'aAsmr3idd4bgs7Dlsf9eAF'



 



l = sorted(a)#该方法意思是把数字0-9从小到大排序，再对大写字母A-Z从小到大排序，然后对小写字母a-z从小到大排序



print l



 



root@kali:~/python/laowangpy# 12345678910111213141516171819202122232425
root@kali:~/python/laowangpy# 



root@kali:~/python/laowangpy# cat sortzimu2.py 



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



'''



#伪代码



1、要有小写字母从a-z的排序



2、大小写不同，但值相同的字母，大写在小写前面



'''



 



a = 'aAsmr3idd4bgs7Dlsf9eAF'



 



l = sorted(a)#该方法意思是把数字0-9从小到大排序，再对大写字母A-Z从小到大排序，然后对小写字母a-z从小到大排序



print l#运行后l的列表为['3', '4', '7', '9', 'A', 'A', 'D', 'F', 'a', 'b', 'd', 'd', 'e', 'f', 'g', 'i', 'l', 'm', 'r', 's', 's', 's']



 



a_upper_list = []#存放大写字母列表



a_lower_list = []#存放小写字母列表



 



for x in l:#遍历列表l



    if x.isupper():#如果x元素是大写字母则进入



        a_upper_list.append(x)



    elif x.islower():#如果x元素是大写字母则进入



        a_lower_list.append(x)



    else:



        pass



 



for y in a_upper_list:#在大写的列表中迭代，如果小写列表中有大写字母，则把大写字母插入小写字母前面



    y_lower = y.lower()



    print '---y_lower---'



    print y_lower



    if y_lower in a_lower_list:



        a_lower_list.insert(a_lower_list.index(y_lower),y)



        print '---a_lower_list----'



        print a_lower_list



 



print ''.join(a_lower_list)



 



root@kali:~/python/laowangpy# 



root@kali:~/python/laowangpy# 



root@kali:~/python/laowangpy# 



root@kali:~/python/laowangpy# python sortzimu2.py



['3', '4', '7', '9', 'A', 'A', 'D', 'F', 'a', 'b', 'd', 'd', 'e', 'f', 'g', 'i', 'l', 'm', 'r', 's', 's', 's']



---y_lower---



a



---a_lower_list----



['A', 'a', 'b', 'd', 'd', 'e', 'f', 'g', 'i', 'l', 'm', 'r', 's', 's', 's']



---y_lower---



a



---a_lower_list----



['A', 'A', 'a', 'b', 'd', 'd', 'e', 'f', 'g', 'i', 'l', 'm', 'r', 's', 's', 's']



---y_lower---



d



---a_lower_list----



['A', 'A', 'a', 'b', 'D', 'd', 'd', 'e', 'f', 'g', 'i', 'l', 'm', 'r', 's', 's', 's']



---y_lower---



f



---a_lower_list----



['A', 'A', 'a', 'b', 'D', 'd', 'd', 'e', 'F', 'f', 'g', 'i', 'l', 'm', 'r', 's', 's', 's']



AAabDddeFfgilmrsss



root@kali:~/python/laowangpy# 12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061
```

1.7 请判断 ‘boy’里出现的每一个字母，是否都出现在a字符串里。如果出现，则输出True，否则，则输 出False.

解题思路

```
root@kali:~/python/laowangpy# python



Python 2.7.3 (default, Mar 14 2014, 11:57:14) 



[GCC 4.7.2] on linux2



Type "help", "copyright", "credits" or "license" for more information.



>>> a = [1,2,3,4,5]



>>> d = set(a)#把a列表转成集合d



>>> d



set([1, 2, 3, 4, 5])



>>> len(d)



5



>>> d



set([1, 2, 3, 4, 5])



>>> d.update([4])#对集合d更新元素，如果存在就不会更新元素，因此总长度不变，否则则变化总长度



>>> d



set([1, 2, 3, 4, 5])



>>> 1234567891011121314151617
root@kali:~/python/laowangpy# vi searchzimu.py



root@kali:~/python/laowangpy# cat searchzimu.py



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



a = 'AaAsmr3idd4bgsboy7Dlsf9eAF'



search = 'boy'



 



u = set(a)



print u



u.update(list(search))#把要查找的字符串更新到a的列表中，如果存在相同顺序的字符串的则返回true，否则返回False



print len(set(a)) == len(u)



 



root@kali:~/python/laowangpy# python searchzimu.py



set(['A', 'a', 'b', 'e', 'd', 'g', 'f', 'i', 'F', 'm', '3', 'o', 'l', '9', 's', 'r', '4', '7', 'y', 'D'])



True



root@kali:~/python/laowangpy# 123456789101112131415161718
```

1.8 要求如1.7，此时的单词判断，由’boy’改为四个，分别是 ‘boy’,’girl’,’bird’,’dirty’，请判断如上这4个字符串里的每个字母，是否都出现在a字符串里。

```
root@kali:~/python/laowangpy# cat searchzimu2.py



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



a = 'aAsrm3idd4boygs7Dlsf9eAF'



 



 



search = ['boy','girl','brird','dirty']



 



b = set(a)#把a列表变成集合



print b



 



for i in search:



    b.update(list(i))



 



print len(b) == len(set(a))



 



root@kali:~/python/laowangpy# python searchzimu2.py



set(['a', 'A', 'b', 'e', 'd', 'g', 'f', 'i', 'F', 'm', '3', 'o', 'l', '9', 's', 'r', '4', '7', 'y', 'D'])



False



root@kali:~/python/laowangpy# 12345678910111213141516171819202122
```

1.9 输出a字符串出现频率最高的字母

```
root@kali:~/python/laowangpy# vi nocount.py



root@kali:~/python/laowangpy# cat  nocount.py



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



a = 'AaAsmr3idd4bgsboy7Dlsf9eAF'



 



l = ([(x,a.count(x)) for x in set(a)])#作为列表形式统计每个字符出现次数



print '------未排序的列表-------'



print l



 



l.sort(key = lambda k:k[1],reverse=True)#取出列表中每个元素的第二个键值，并使用从大到小的排序



print '-------字母出现的频率从大到小排序的---------'



print l



print '-------第一个元素-------'



print l[0]



print '----第一个元素的第一个键值-----'



print l[0][0]



 



root@kali:~/python/laowangpy# python nocount.py



------未排序的列表-------



[('A', 3), ('a', 1), ('b', 2), ('e', 1), ('d', 2), ('g', 1), ('f', 1), ('i', 1), ('F', 1), ('m', 1), ('3', 1), ('o', 1), ('l', 1), ('9', 1), ('s', 3), ('r', 1), ('4', 1), ('7', 1), ('y', 1), ('D', 1)]



-------字母出现的频率从大到小排序的---------



[('A', 3), ('s', 3), ('b', 2), ('d', 2), ('a', 1), ('e', 1), ('g', 1), ('f', 1), ('i', 1), ('F', 1), ('m', 1), ('3', 1), ('o', 1), ('l', 1), ('9', 1), ('r', 1), ('4', 1), ('7', 1), ('y', 1), ('D', 1)]



-------第一个元素-------



('A', 3)



----第一个元素的第一个键值-----



A



root@kali:~/python/laowangpy# 123456789101112131415161718192021222324252627282930
```

2.在python命令行里，输入import this 以后出现的文档，统计该文档中，”be” “is” “than” 的出现次数。

```
root@kali:~/python/laowangpy# python -m this



The Zen of Python, by Tim Peters



 



Beautiful is better than ugly.



Explicit is better than implicit.



Simple is better than complex.



Complex is better than complicated.



Flat is better than nested.



Sparse is better than dense.



Readability counts.



Special cases aren't special enough to break the rules.



Although practicality beats purity.



Errors should never pass silently.



Unless explicitly silenced.



In the face of ambiguity, refuse the temptation to guess.



There should be one-- and preferably only one --obvious way to do it.



Although that way may not be obvious at first unless you're Dutch.



Now is better than never.



Although never is often better than *right* now.



If the implementation is hard to explain, it's a bad idea.



If the implementation is easy to explain, it may be a good idea.



Namespaces are one honking great idea -- let's do more of those!



root@kali:~/python/laowangpy# 



 



root@kali:~/python/laowangpy# python -m this



The Zen of Python, by Tim Peters



 



Beautiful is better than ugly.



Explicit is better than implicit.



Simple is better than complex.



Complex is better than complicated.



Flat is better than nested.



Sparse is better than dense.



Readability counts.



Special cases aren't special enough to break the rules.



Although practicality beats purity.



Errors should never pass silently.



Unless explicitly silenced.



In the face of ambiguity, refuse the temptation to guess.



There should be one-- and preferably only one --obvious way to do it.



Although that way may not be obvious at first unless you're Dutch.



Now is better than never.



Although never is often better than *right* now.



If the implementation is hard to explain, it's a bad idea.



If the implementation is easy to explain, it may be a good idea.



Namespaces are one honking great idea -- let's do more of those!



 



 



root@kali:~/python/laowangpy# 



root@kali:~/python/laowangpy# vi beiscount.py



root@kali:~/python/laowangpy# cat beiscount.py



#!/usr/bin/python



# --*-- coding:utf-8 --*--



 



import os



m = os.popen('python -m this').read()#使用os命令运行python -m this



 



m = m.replace('\n','')#对换行符进行替换



 



l = m.split(' ')#使用空格分割



 



print [(x,l.count(x)) for x in ['be','is','than']]#统计多个单词在文件中出现频率



 



root@kali:~/python/laowangpy# python beiscount.py



[('be', 3), ('is', 10), ('than', 8)]



root@kali:~/python/laowangpy# 12345678910111213141516171819202122232425262728293031323334353637383940414243444546474849505152535455565758596061626364656667
```

3.一文件的字节数为 102324123499123，请计算该文件按照kb与mb计算得到的大小。

```
>>> size = 102324123499123



>>> print "%s kb" % (size >> 10)



99925901854 kb



>>> print "%s mb" % (size >> 20)



97583888 mb



>>> print "%s gb" % (size >> 30)



95296 gb



>>> 123456789
```

4.已知 a = [1,2,3,6,8,9,10,14,17],请将该list转换为字符串，例如 ‘123689101417’.

```
>>> a = [1,2,3,6,8,9,10,14,17]



>>> a



[1, 2, 3, 6, 8, 9, 10, 14, 17]



>>> b = str(a)#转换成字符串



>>> print b



[1, 2, 3, 6, 8, 9, 10, 14, 17]



>>> c = str(a)[1:-1]#去除方括号



>>> print c



1, 2, 3, 6, 8, 9, 10, 14, 17



>>> 



>>> d = str(a)[1:-1].replace(', ','')#对, 进行替换空白



>>> print d



123689101417



>>>
```