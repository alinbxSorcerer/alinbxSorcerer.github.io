
.. title: 时间变量的顺序与格式
.. slug: shi-jian-bian-liang-de-shun-xu-yu-ge-shi
.. date: 2019-05-13 17:38:19 UTC+08:00
.. tags: python, time, bash,  
.. category: programming
.. link: 
.. description: 时间专题之一
.. type: text
.. toc: true


.. contents::

时间的感知
==========

直觉对时间的感知, 是按照怎么的顺序解析的呢?
从早晨起床这个场景中,分析大脑解析时间的顺序.

似乎理所当然的一点,Hour(时)排在最前面,Hour:Minute的组合是醒来后第一时间想要获得的信息,以判断上班是否迟到,是否有时间吃早餐?但是再想一层,上班是否迟到?
在处理Hour:Minute之前,潜意识已经先获取并处理完了一个信息:今天是周几?(Weekday).如果是周末的话,几点几分起床并不特别重要.
因此,最先处理的三个变量是Weekday Hour:Minutes

再看日期,Month-Day,哪个变量在前呢?
如果day在前,则是冗余信息,因为已经weekday在先.
解析日期,首先获取Month,以判断一年的进度,获得当前时间在一年中所处的位置,比如May的含义是一年已经将半.
日期的顺序是Month-Day

还有变量week number,是对一年进度的另外一种表示.

最后是Year

综上, 直觉解析时间的顺序为:
``Weekday Hour:Minute Month-Day WeekNumber Year``,
要事第一,最重要的信息是Weekday. 合计8个变量, 命名为intuitive time(or
intelligent time)简写为itime

这也是CTIME时间格式的理念.

::

   In [2]: time.ctime()
   Out[2]: 'Wed May 15 08:29:38
   [3]:!date
   Wed May 15 08:31:45 CST 2019

ctime将weekday安排在最前面,然后遵循这个先处理日期的逻辑,May:15跟在后面提高日期的精确颗粒度.但是对大脑的瞬间思考而言,则是冗余信息.因此以itime作为基石,推导拓展到其他时间顺序.

Cron中时间变量的顺序
====================

The sequence matters. 时间变量顺序的第一个应用场景是cron.

Cron的顺序是Minute:Hour Day:Month Weekday

::

   # ┌───────────── minute (0 - 59)
   # │ ┌───────────── hour (0 - 23)
   # │ │ ┌───────────── day of the month (1 - 31)
   # │ │ │ ┌───────────── month (1 - 12)
   # │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
   # │ │ │ │ │                                   7 is also Sunday on some systems)
   # │ │ │ │ │
   # │ │ │ │ │
   # * * * * * command to execute

大脑长于处理模糊宏观的信息,而机器专于精确的信息,将最小的时间单位排在前面,是倒排的ctime.应用的时候也只需要将思考倒叙.

cron将minute作为最小单位和第一顺序是因为crond按分钟扫描,可见从性能经济性的角度考虑,对大脑和对机器,
秒都是可以忽略的,不然会浪费大量的资源．

接下来讨论一个问题: 在感知上, minutes:hours 与hours:minutes有何差异呢？

minutes:hours的顺序，是将精力关注到一个小时之内，比如六点十五=15:06=表达出来是，在从6点到７点这一个小时的时间段内，已经过去了15分钟，知道现在已经消费了一个小时的四分之一(1/4)，接下来就会自然的发问，按照这个进度，等一个小时结束能完成多少工作．
而hours:minutes的顺序，是关注在一天之内，比如=06:15=表达的是，现在是早上时间(6/24)，一天之计在于晨，后面的分钟15只是作为辅助．
总结：hour:minutes(6:15)关注的是一天之内，而minute:hour关注的是一个小时之内，以一个小时为单位做计划和考核产出．

在直觉上采用哪一个作为基础,等更多的应用总结后,再行确定.暂时采用Hour:Minute的格式,因为minute:hour消耗更多大脑内存和计算资源.

总结, cron是写给机器读的,对象是机器,因此将ctime倒叙.

需要记住时间顺序的另外一个应用场景是=journalctl=

::


   journalctl --since  “YYYY-MM-DD HH:MM:SS” --until “YYYY-MM-DD HH:MM:SS”

从宏观到微观,从模糊到精确,这是写给人用的时间格式,也是ISO的标准时间格式.

引用一段话:

   Your computer is not a lifeless piece of machinery. It is a dynamic
   tool that interacts with your very thought processes. Whenever you
   use a computer it becomes, for better or for worse, an extension of
   your mind.

   This means that, whenever you use a Unix or Linux system, you are
   forging a mental relationship with some of the smartest, most
   accomplished (and satisfied) programmers and computer scientists who
   ever lived. Such a partnership can't help but have a positive effect
   on you.

   As you do, your mind will change for the better, your thought
   processes will improve, and your way of looking at the world and at
   yourself will change.

时间的格式
==========

以Cron的5个时间变量顺序为基础分析时间表示的格式, 前面加上second+(minute,
hour day, month weekday)后面补充year and weeknumber.

 时间的格式以C, python采用的unix strftime格式为准.

::

|-------------+----------+----------+---------------------+-----------------+-------------------+---------------------+---------------+---------|
| 时间变量    | 1.second | 2.minute | 3.hour              | 4.day           | 5.month           | 6.weekday           | 7.week number | 8.year  |
|-------------+----------+----------+---------------------+-----------------+-------------------+---------------------+---------------+---------|
| 常规符号    | %S 01    | %M 01    | %H 23               | %d 01           | %m 01             | %w (0-6)            | %W            | %y 19   |
| 简单扩展    |          |          | %I (12-hour), %p AM |                 |                   |                     |               | %Y 2019 |
| 文字描述    |          |          |                     |                 | %b Oct, %BOctober | %a Thu, %A Thursday |               |         |
| Obscure扩展 | %f 微秒  |          |                     | % e 1~31 %j 366 |                   | %u(1~7)             | %V %U         | %G 2019 |
|-------------+----------+----------+---------------------+-----------------+-------------------+---------------------+---------------+---------|


分析:

第一行, 钟表时间全部为大写, 日期时间全部为小写,
WeekNumber大写%W与weekday %w相区分; 第二行, 简单扩展, %H是24小时制,
H后面的字母是I,因此用%I表示12小时制,同时%p(postnoon)标注上下午;
大写的%Y表示四位数字的年2019; 第三行, 文字描述,只有两个变量能以文字描述,
weekday和month, 分别用A, B表示.weekday %A还是第一重要的.

前三行是常规的表示.

第四行, obscure扩展, 微秒的表示应该是从剩余可选字母中随机确定的,
%f(fly飞逝),和数日子day 366 %j , %e空格padding. weekday与weekNumber在u,
v, w这三个字母上打转, 小写是weekday大写是weekNumber.

总结: 按照表格前三行可以在一秒钟内永久性记忆, 日常的书写也采用该格式.
日如ctime ``%a %b %d %H:%M:%S %Y``

Timezone的表示

::

   In [7]: !date +"%z"   #UTC offset
   +0800
   In [8]: !date +"%Z"
   CST #China Standard Time

快捷方式的format



   In [10]: datetime.now().strftime("%c")      #Locale’s appropriate date and time representation.
   Out[10]: 'Thu May  9 11:48:33 2019 #这个格式将weekday放在了前面, 也就是 %a %b的形式.
   In [11]: datetime.now().strftime("%x")      #Locale’s appropriate date representation
   Out[11]: '05/09/19'
   In [12]: datetime.now().strftime("%X")
   Out[12]: '11:49:06'

总结
====

10个时间变量,将时区放置在最后.

   %f:%S:%M:%H %d-%m %w %W %Y %Z 日常的书写,使用符号替代minute, hour等

时间格式的的标准：

#. `ISO 8601 - Wikipedia <https://en.wikipedia.org/wiki/ISO_8601>`
#. `W3C DTF <https://www.w3.org/TR/NOTE-datetime>`
#. RFC 822(as updated by RFC 1123)
#. `RFC 2822 - Internet Message Format <https://tools.ietf.org/html/rfc2822.html>`
