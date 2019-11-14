.. title: Emacs中的datetime与clock
.. slug: emacszhong-de-datetimeyu-clock
.. date: 2019-05-18 20:35:15 UTC+08:00
.. tags: emacs, time
.. category: programming
.. link: 
.. description:  时间专题之四
.. type: text

.. contents::
 

前文概要:
---------

时间管理的三件工具 (clock, datetime, calendar)
时间变量的顺序与时间表示的格式 (%a %b-d %H:%M %Y)

Emacs中的时间格式与任务的时间属性
---------------------------------

时间管理的三件工具: 钟表, 日历, 以及二者结合的便利工具datetime.

`The Org Manual: Dates and
Times <https://orgmode.org/manual/Dates-and-Times.html#Dates-and-Times>`__\ 主要应用datetime和clock两个工具管理时间.

-  时间戳的格式 （Datetime)

   -  datetime格式 <2019-05-18 sat 09:52> C-u C-c . 6个时间变量 %Y-%m-%d
      %a %H:%M
   -  date格式 <2019-05-18 sat> C-c . 4个时间变量

Datetime时间戳与任务的时间属性
------------------------------

-  时间(戳)到底什么? 时间戳从概念上辨析为两个类别 1) appointment 2)
   schedule. 可以将主动安排的时间理解为schedule,
   被动参与的时间为appointment
-  事件(任务)的四个时间属性

   -  Repeater 三种repeater,

      #. standard 时间戳后面 +1w day, week, month, year
      #. 过期后只需要补上一次, ++1w(比如每周给父母打电话)
      #. 以档次完成时间为下一个interval的开始点,比如3个周理一次发. 理发
         .+3w; 任何时间点上理完发,自动从该时间点上启动下一轮repeat.

   -  Time Span 会议的时间 <2019-05-18 sat 10:04>–<2019-05-18 sat 10:05>
   -  Scheduled 任务的开始时间, SCHEDULED: <2019-05-18 sat>
   -  Deadline 任务的截止时间 DEADLINE: <2019-05-18 sat>

-  任务的监控与跟踪 Check

   整合管理deadline与schedule.

   -  ``org-check-deadlines``, C-7 C-c / d
      #7天内将要截止的任务.变量org-deadline-warning-days
   -  ``org-check-before-date``, given-date前的schedule and appointments
   -  ``org-check-after-date``, given-date后的schedule and appointments.

(这些功能并不好用)

Clock计时与中断处理
-------------------

#. 基本计时功能, ``org-clock-in``, ``org-clock-out`` 可以手动修改计时,
   然后调用=org-evaluate-time-range=或者=update-maybe=

#. 任务的第五个属性, efforts 总结任务的5个属性, 1)schedule 2)deadline
   3)repeater 4)time span 5)预测的time span(efforts).
   efforts是核心参数,逐步提高对时间和任务的掌控.

#. Resolve idle time and continuous clocking
   中断计时的问题,=调出来org-resolve-clocks=重新设置.k(keep),
   s(substract),只用keep便可.

总结
----

#. 计时与计算
#. 任务的5个时间属性
#. idle中断计时的处理.
