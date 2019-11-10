
      .. title: 时间管理的三件武器
   .. slug: shi-jian-guan-li-de-san-jian-wu-qi
   .. date: 2019-05-15 20:53:29 UTC+08:00
   .. tags: 时间管理, time, python, bash, emacs
   .. category: programming
   .. link: 
   .. description: 时间专题之三
   .. type: text


前情概要
--------

| “时间变量的顺序与时间表示的格式”
  总结了时间的10个变量及其直觉解析的顺序
| ``Weekday Hour:Minute Month-Day WeekNumber Year``
| “时间的感知与分析”阐述对时间的认知方法论以奠定高效利用和有效掌控时间的基础。
| “工欲善其事必先利其器”，接下来的问题便是时间管理的工具。

时间管理的三件武器
------------------

无论是手机的android系统还是Linux系统，都提供两项基本的时间管理工具日历(calendar)
and 时钟(clock). Calendar是date地图，提供全景式的鸟瞰图；
clock是指南针，指导具体的每一步应该迈向何处。
二者的结合形成第三个工具datetime。

工具虽然简单，但如果不能抽象到认知的层面，则不能为己所用。试问，谁手机里没个日历，每个钟表呢。认知层面的结论是需要且仅需要这三件工具O(∩_∩)O。

-  日历提供全景式鸟瞰

   .. code:: shell

      $ ncal -B 3  #ncal中的n竟然是new这个单词
          February 2019     March 2019        April 2019        May 2019          
      Su      3 10 17 24        3 10 17 24 31     7 14 21 28        5 12 19 26   
      Mo     4 11 18 25        4 11 18 25      1  8 15 22 29        6 13 20 27   
      Tu      5 12 19 26        5 12 19 26      2  9 16 23 30        7 14 21 28   
      We     6 13 20 27        6 13 20 27      3 10 17 24        1  8 15 22 29   
      Th      7 14 21 28        7 14 21 28      4 11 18 25        2  9 16 23 30   
      Fr       1  8  15   22     1  8 15 22 29       5 12 19 26        3 10 17 24 31   
      Sa  2  9 16 23        2  9 16 23 30       6 13 20 27       4  11 18 25

-  时钟实施具体的测量

   .. code:: bash

      $ time sleep 10real    0m10.011suser    0m0.002ssys     0m0.001s

-  datetime整合二者，提供更加实用的功能

   .. code:: bash

      #贸易战倒计时
      $ TZ="America/New_York" date #美东夏季时间， 第十个变量时区。
      Thu May  9 20:04:30 EDT 2019  #Eatern Daylight Timer

Python中的时间管理
------------------

以calendar,
clock(time),datetime与十个时间变量为基础，审视python的\ ``calendar``,
``time``, and ``datetime``

1. Calender
~~~~~~~~~~~

`calendar — General calendar-related
functions <https://docs.python.org/3/library/calendar.html>`__ `calendar
— Work with Dates — PyMOTW
3 <https://pymotw.com/3/calendar/index.html>`__

Python中的calendar十个无关紧要的工具，没有人会费力不讨好的到这里查看日期和规划日程。

::

   class TextCalendar(Calendar)
    |  TextCalendar(firstweekday=0)
   In [326]: cal = calendar.TextCalendar(calendar.MONDAY)       #周一作为一周的起始点                                                                
   In [327]: cal.prmonth(2019, 5)                                                                                                
         May 2019
   Mo Tu We Th Fr Sa Su
          1  2  3  4  5
    6  7  8  9 10 11 12
   13 14 15 16 17 18 19
   20 21 22 23 24 25 26
   27 28 29 30 31
   In [328]: cal.pryear(2019)                                                                                                    
                                     2019
         January                                   February                                    March
   Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su      Mo Tu We Th Fr Sa Su
       1  2  3  4  5  6                   1  2  3                                           1  2  3
    7  8  9 10 11 12 13           4  5  6  7  8  9 10                            4  5  6  7  8  9 10
   14 15 16 17 18 19 20      11 12 13 14 15 16 17              11 12 13 14 15 16 17
   21 22 23 24 25 26 27      18 19 20 21 22 23 24             18 19 20 21 22 23 24
   28 29 30 31                       25 26 27 28                                  25 26 27 28 29 30 31
   # 只了解下TextCalendar, pryear, prmonth

2. Time
~~~~~~~

`time — Time access and
conversions <https://docs.python.org/3/library/time.html#module-time>`__
`calendar — Work with Dates — PyMOTW
3 <https://pymotw.com/3/calendar/index.html>`__

-  与epoch time的相互转换

   Use the following functions to convert between time representations:
   人以minutes计时， 机器以second计时。

   +-----------------------+-----------------------+-----------------------+
   | From                  | To                    | Use                   |
   +=======================+=======================+=======================+
   | seconds since the     | ```struct_time`` <htt | ```gmtime()`` <https: |
   | epoch                 | ps://docs.python.org/ | //docs.python.org/3/l |
   |                       | 3/library/time.html#t | ibrary/time.html#time |
   |                       | ime.struct_time>`__   | .gmtime>`__           |
   |                       | in UTC                |                       |
   +-----------------------+-----------------------+-----------------------+
   | seconds since the     | ```struct_time`` <htt | ```localtime()`` <htt |
   | epoch                 | ps://docs.python.org/ | ps://docs.python.org/ |
   |                       | 3/library/time.html#t | 3/library/time.html#t |
   |                       | ime.struct_time>`__   | ime.localtime>`__     |
   |                       | in local time         |                       |
   +-----------------------+-----------------------+-----------------------+
   | ```struct_time`` <htt | seconds since the     | ```calendar.timegm()` |
   | ps://docs.python.org/ | epoch                 | ` <https://docs.pytho |
   | 3/library/time.html#t |                       | n.org/3/library/calen |
   | ime.struct_time>`__   |                       | dar.html#calendar.tim |
   | in UTC                |                       | egm>`__               |
   +-----------------------+-----------------------+-----------------------+
   | ```struct_time`` <htt | seconds since the     | ```mktime()`` <https: |
   | ps://docs.python.org/ | epoch                 | //docs.python.org/3/l |
   | 3/library/time.html#t |                       | ibrary/time.html#time |
   | ime.struct_time>`__   |                       | .mktime>`__           |
   | in local time         |                       |                       |
   +-----------------------+-----------------------+-----------------------+

   ::

      #seconds to  timetuple
      In [8]: time.localtime() #local time
      Out[8]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=15, tm_hour=20, tm_min=4, tm_sec=17, tm_wday=2, tm_yday=135, tm_isdst=0)

      In [9]: time.gmtime() #UTC time
      Out[9]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=15, tm_hour=12, tm_min=4, tm_sec=31, tm_wday=2, tm_yday=135, tm_isdst=0)

      In [11]: time.gmtime(100)  #
      Out[11]: time.struct_time(tm_year=1970, tm_mon=1, tm_mday=1, tm_hour=0, tm_min=1, tm_sec=40, tm_wday=3, tm_yday=1, tm_isdst=0)
      #timetuple to seconds 
      In [17]: calendar.timegm(time.gmtime()) #UTC time 
      Out[17]: 1557922318
      In [18]: time.mktime(time.localtime()) #Local time 
      Out[18]: 1557922368.0

-  第十一个-时间变量 ``tm_isdst`` daylight saving time夏季时间

   .. code:: bash

      gmtime(...) ->time.struct_time,  (strptime, localtime)
          gmtime([seconds]) -> (tm_year, tm_mon, tm_mday, tm_hour, tm_min,
                                 tm_sec, tm_wday, tm_yday, tm_isdst)

-  Processor Time and Timer

   .. code:: bash

      time.process_time() #cpu time  of kernel and user-space
      time.process_time_ns() ->int #not inclue sleep time 
      time.perf_counter() ->float
      time.perf_counter_ns() -> int
      time.thread_time() ->float
      time.thread_time_ns() ->int
      time.time()
      time.time_ns()
      time.sleep
      time.monotonic()
      time.monotonic_ns()

-  Timezone Constants

   .. code:: bash

      In [9]: time.daylight
      Out[9]: 0
      In [10]: time.tzname
      Out[10]: ('CST', 'CST')
      In [11]: time.altzone
      Out[11]: -28800

-  两个重要的methods

   .. code:: bash

      time.strptime(string, format)
      time.strftime(format)

-  ctime

   .. code:: bash

      In [20]: time.asctime(time.localtime())
      Out[20]: 'Wed May 15 21:05:27 2019'
      In [21]: time.ctime()
      Out[21]: 'Wed May 15 21:05:32 2019'

3. Datetime
~~~~~~~~~~~

复习calendar(date), clock(time)的逻辑，先看两个没用的函数。

`datetime — Date and Time Value Manipulation — PyMOTW
3 <https://pymotw.com/3/datetime/index.html>`__

1. datetime.date()

   .. code:: python

      class date(builtins.object)  就是符号%x返回的内容
       |  date(year, month, day) --> date object
      In [135]: datetime.date.today().year                                                                                          
      Out[135]: 2019
      In [136]: datetime.date.today().month                                                                                         
      Out[136]: 5
      In [137]: datetime.date.today().day                                                                                           
      Out[137]: 9

2. datetime.time()

   .. code:: python

      class time(builtins.object) 符号%X返回的内容
       |  time([hour[, minute[, second[, microsecond[, tzinfo]]]]]) --> a time object
        #5个参数,由大到小排列
      In [104]:  t = datetime.time(6, 15, 30, 999999, dateutil.tz.tzutc())                                                          
      In [105]: t.strftime("%f:%S:%M:%H %Z")                                                                                        
      Out[105]: '999999:30:15:06 UTC'
      In [106]: t.min                                                                                                               
      Out[106]: datetime.time(0, 0)
      In [107]: t.max                                                                                                               
      Out[107]: datetime.time(23, 59, 59, 999999)
      In 108]: t.resolution                                                                                                        
      Out[108]: datetime.timedelta(microseconds=1) #精确度

3. datetime.datetime

   .. code:: python

      #前面两个datetime.time and datetime.date没啥用.
      #这里的关键是第十个时间变量tzinfo
      class datetime(date)
       |  datetime(year, month, day[, hour[, minute[, second[, microsecond[,tzinfo]]]]])
      Help on built-in function weekday:

      weekday(...) method of datetime.datetime instance
          Return the day of the week represented by the date.
          Monday == 0 ... Sunday == 6   
      In [200]: dt = datetime.datetime.now().replace(tzinfo=dateutil.tz.gettz("Asia/Shanghai"))                                     
      In [201]: dt                                                                                                                  
      Out[201]: datetime.datetime(2019, 5, 9, 17, 47, 10, 421561, tzinfo=tzfile('/usr/share/zoneinfo/Asia/Shanghai'))
       In [208]: datetime.datetime.utcnow().timetuple()                                                                              
      Out[208]: time.struct_time(tm_year=2019, tm_mon=5, tm_mday=9, tm_hour=9, tm_min=49, tm_sec=24, tm_wday=3, tm_yday=129, tm_isdst=-1)

4. datetime.timedelta and Arithmetic Opertions

   .. code:: python

      class datetime.timedelta(days=0, seconds=0, microseconds=0, milliseconds=0, minutes=0, hours=0, weeks=0)
      In [219]: datetime.datetime.now() + datetime.timedelta(days=-1)                                                               
      Out[219]: datetime.datetime(2019, 5, 8, 17, 57, 40, 910880)
      In [220]: !date -d "1 days ago"                                                                                               
      Wed May  8 17:57:52 CST 2019
       #转换为seconds的另外一种写法
      In [25]: str(datetime.timedelta(seconds=100))
      Out[25]: '0:01:40'
      In [29]: time.strftime("%H:%M:%S", time.gmtime(3000))
      Out[29]: '00:50:00'

Shell的时间管理工具
-------------------

1.Calendar
~~~~~~~~~~

::

   $ ncal -1
   $ ncal -M #Monday as the first day 
   $ ncal -w #weeknumber
   $ncal -y -m; 
   $ncal -d yyyy-mm 
   $ncal yyyy-mm-dd#highlight the current date 
   $ ncal -A3 -B4
   #就只有这么多操作

2.Clock(Time)
~~~~~~~~~~~~~

-  hwclock

   .. code:: bash

      In [31]: !sudo hwclock
      2019-05-15 21:25:29.514803+08:00
      ---------------------------------------------------
      NAME
             hwclock - time clocks utility
      FILES
             /etc/adjtime
                    The configuration and state file for hwclock.
             /etc/localtime
                    The system timezone file
             /usr/share/zoneinfo/
                    The system timezone database directory.
             Device files hwclock may try for Hardware Clock access:
             /dev/rtc0
             /dev/rtc
             /dev/misc/rtc
             /dev/efirtc
             /dev/misc/efirtc

-  time (processing profile)

   .. code:: bash

          $ time (tree  /usr/share/zoneinfo | grep -i "prc")
          │   ├── Chongqing -> ../PRC
          │   ├── Chungking -> ../PRC
          │   ├── Harbin -> ../PRC
          │   ├── Shanghai -> ../PRC
          │   │   ├── Chongqing -> ../../PRC
          │   │   ├── Chungking -> ../../PRC
          │   │   ├── Harbin -> ../../PRC
          │   │   ├── Shanghai -> ../../PRC
          │   ├── PRC -> ../PRC
          ├── PRC
          │   │   ├── Chongqing -> ../PRC
          │   │   ├── Chungking -> ../PRC
          │   │   ├── Harbin -> ../PRC
          │   │   ├── Shanghai -> ../PRC
          │   ├── PRC

          real    0m0.017s
          user    0m0.015s
          sys     0m0.004s

      3.Datetime

.. _datetime-1:

3.Datetime
~~~~~~~~~~

-  timedatectl

   .. code:: bash

      #systemd的service
      $ timedatectl
                     Local time: Thu 2019-05-09 21:30:27 CST
                 Universal time: Thu 2019-05-09 13:30:27 UTC
                       RTC time: Thu 2019-05-09 13:30:27
                      Time zone: Asia/Shanghai (CST, +0800)
      System clock synchronized: yes
                    NTP service: active

       - Check the current system clock time:
         timedatectl
       - Set the local time of the system clock directly:
         timedatectl set-time {{"yyyy-MM-dd hh:mm:ss"}}

       - List available timezones:
         timedatectl list-timezones

       - Set the system timezone:
         timedatectl set-timezone {{timezone}}

       - Enable Network Time Protocol (NTP) synchronization:
         timedatectl set-ntp on

-  date (最趁手的一个工具)

   .. code:: bash

      # 日常应用date作为思考工具.
      In [9]: time.time()                                                                            
      Out[9]: 1557406444.1336956
      In [10]: !date +%s                                                                             
      1557406449
      $ date -d @$(date +%s)　#date -d @1557406449
      Thu May  9 21:11:15 CST 2019

      #基本的操作
      $ date -u +"%Y-%m-%dT%H:%M:%SZ"
      2019-05-09T12:55:31Z

      #Future date and time 
      $ date -d " two weeks"
      date: invalid date ‘ two weeks’
      $ date -d "2 weeks"
      Thu May 23 21:20:17 CST 2019
      $ date -d "next fri" #只有两个变量能够以文字表述 Month and Weekday
      Fri May 10 00:00:00 CST 2019

      #the elpased date and time 
      $ date -d "last thursday"
      Thu May  2 00:00:00 CST 2019
      $ date -d "2 days ago"
      Tue May  7 21:22:28 CST 2019
      $ date -d "last month"
      Tue Apr  9 21:22:39 CST 2019

      #贸易战倒计时
      $ TZ="America/New_York" date
      Thu May  9 20:04:30 EDT 2019

Emacs中的时间管理
-----------------

….

总结
----

时间管理的三个工具, time, datetime, calendar(从微观到宏观处理5, 8,
10个时间变量)

   Our units of temporal measurement, from seconds on up to months, are
   so complicated, asymmetrical and disjunctive so as to make coherent
   mental reckoning in time all but impossible. Indeed, had some
   tyrannical god contrived to enslave our minds to time, to make it all
   but impossible for us to escape subjection to sodden routines and
   unpleasant surprises, he could hardly have done better than handing
   down our present system. It is like a set of trapezoidal building
   blocks, with no vertical or horizontal surfaces, like a language in
   which the simplest thought demands constructions, useless particles
   and lengthy circumlocutions. Unlike the more successful patterns of
   language and science, which enable us to face experience boldly or at
   least level-headedly, our system of temporal calculation silently and
   persistently encourages our terror of time.

   … It is as though architects had to measure length in feet, width in
   meters and height in ells; as though basic instruction manuals
   demanded a knowledge of five different languages. It is no wonder
   then that we often look into our own immediate past or future, last
   Tuesday or a week from Sunday, with feelings of helpless confusion. …

   —Robert Grudin, Time and the Art of Living.

哈哈哈～，神吐槽。因此需要将so complicated, asymmetrical and disjunctive
抽象为calender, time, datetime。

参考资料
--------

`GNU Coreutils: Date input
formats <https://www.gnu.org/software/coreutils/manual/html_node/Date-input-formats.html#Date-input-formats>`
