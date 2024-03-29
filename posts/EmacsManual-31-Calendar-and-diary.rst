   .. title: 评注Eamcs: 31.The Calendar and the Diary
   .. slug: emacs-manual-calendar-and-the-diary
   .. date: 2019-05-16 09:53:29 UTC+08:00
   .. tags: emacs, time, 评注笔记
   .. category: programming
   .. link:
   .. description:
   .. type: text


.. contents::

总结:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
三个部分:   

1) 基本的操作
2) 与社会的互动, holidays and calendars
3) 与天地四时的互动, sunrise-sunset 因此需要再加上两个时间点, sunrise and sunset

.. image:: /images/emacs-calendars-structure.jpeg
   :width: 700
   
Emacs provides the functions of a desk calendar, with a diary of planned
or past events. It also has facilities for managing your appointments,
and keeping track of how much time you spend working on certain
projects.

To enter the calendar, type M-x calendar. This displays a three-month
calendar centered on the current month, with point on the current date.
With a numeric argument, as in C-u M-x calendar, it prompts you for the
month and year to be the center of the three-month calendar. The
calendar uses its own buffer, whose major mode is Calendar mode.

mouse-3 in the calendar brings up a menu of operations on a particular
date; mouse-2 brings up a menu of commonly used calendar features that
are independent of any particular date. To exit the calendar, type q.

我想要一个类似在桌面操作系统中, 可以操作任务的日历.

31.1 Movement in the Calendar
-----------------------------

Calendar mode provides commands to move through the calendar
``in logical units``

of time such as days, weeks, months, and years. If you move outside the
three months originally displayed, the calendar display scrolls
automatically through time to make the selected date visible. Moving to
a date lets you view its holidays or diary entries, or convert it to
other calendars; moving by long time periods is also useful simply to
scroll the calendar.

.. code:: elisp

   ;;修改calendar-week-start-day
   (setq calendar-week-start-day 1)

对movement的总结, span, 按照四个logical unit, day, week, month, year

#. backwords and forwards with specified lenghts
#. selector, begin and end
#. specified dates

31.1.1 Motion by Standard Lengths of Time
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The commands for movement in the calendar buffer parallel the commands
for movement in text. You can move forward and backward by days, weeks,
months, and years.

-  C-f

   Move point one day forward (``calendar-forward-day``).

-  C-b

   Move point one day backward (``calendar-backward-day``).

-  C-n

   Move point one week forward (``calendar-forward-week``).

-  C-p

   Move point one week backward (``calendar-backward-week``).

-  M-}
   Move point one month forward (``calendar-forward-month``).

-  M-{

   Move point one month backward (``calendar-backward-month``).

in logical unit- C-x ]

Move point one year forward (``calendar-forward-year``).

-  C-x [

   Move point one year backward (``calendar-backward-year``).

The day and week commands are natural analogues of the usual Emacs
commands for moving by characters and by lines. Just as C-n usually
moves to the same column in the following line, in Calendar mode it
moves to the same day in the following week. And C-p moves to the same
day in the previous week.

The arrow keys are equivalent to C-f, C-b, C-n and C-p, just as they
normally are in other modes.

The commands for motion by months and years work like those for weeks,
but move a larger distance. The month commands M-} and M-{ move forward
or backward by an entire month. The year commands C-x ] and C-x [ move
forward or backward a whole year.

The easiest way to remember these commands is to consider months and
years analogous to paragraphs and pages of text, respectively. But the
calendar movement commands themselves do not quite parallel those for
movement through text: the ordinary Emacs paragraph commands move to the
beginning or end of a paragraph, whereas these month and year commands
move by an entire month or an entire year, keeping the same date within
the month or year.

All these commands accept a numeric argument as a repeat count. For
convenience, the digit keys and the minus sign specify numeric arguments
in Calendar mode even without the Meta modifier. For example, 100 C-f
moves point 100 days forward from its present location.

31.1.2 Beginning or End of Week, Month or Year
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A week (or month, or year) is not just a quantity of days; we think of
weeks (months, years) as starting on particular dates. So Calendar mode
provides commands to move to the start or end of a week, month or year:

-  C-a

   Move point to start of week (``calendar-beginning-of-week``).

-  C-e

   Move point to end of week (``calendar-end-of-week``).

-  M-a

   Move point to start of month (``calendar-beginning-of-month``).

-  M-e

   Move point to end of month (``calendar-end-of-month``).

-  M-<

   Move point to start of year (``calendar-beginning-of-year``).

-  M->

   Move point to end of year (``calendar-end-of-year``).

These commands also take numeric arguments as repeat counts, with the
repeat count indicating how many weeks, months, or years to move
backward or forward.

By default, weeks begin on Sunday. To make them begin on Monday instead,
set the variable ``calendar-week-start-day`` to 1. To change which day
headers are highlighted as weekend days, set the variable
``calendar-weekend-days``.

31.1.3 Specified Dates
~~~~~~~~~~~~~~~~~~~~~~

Calendar mode provides commands for moving to a particular date
specified in various ways.

-  g d

   Move point to specified date (``calendar-goto-date``).

-  g D

   Move point to specified day of year (``calendar-goto-day-of-year``).

-  g w

   Move point to specified week of year (``calendar-iso-goto-week``).

-  o

   Center calendar around specified month (``calendar-other-month``).

-  .

   Move point to today's date (``calendar-goto-today``).

   g d (``calendar-goto-date``) prompts for a year, a month, and a day
   of the month, and then moves to that date. Because the calendar
   includes all dates from the beginning of the current era, you must
   type the year in its entirety; that is, type '2010', not '10'.

   g D (``calendar-goto-day-of-year``) prompts for a year and day
   number, and moves to that date. Negative day numbers count backward
   from the end of the year. g w (``calendar-iso-goto-week``) prompts
   for a year and week number, and moves to that week.

   o (``calendar-other-month``) prompts for a month and year, then
   centers the three-month calendar around that month.

   You can return to today's date with . (``calendar-goto-today``).

31.2 Scrolling in the Calendar
------------------------------

The calendar display scrolls automatically through time when you move
out of the visible portion. You can also scroll it manually. Imagine
that the calendar window contains a long strip of paper with the months
on it. Scrolling the calendar means moving the strip horizontally, so
that new months become visible in the window.

-  >

   Scroll calendar one month forward (``calendar-scroll-left``).

-  <

   Scroll calendar one month backward (``calendar-scroll-right``).

-  C-v Scroll forward by three months
   (``calendar-scroll-left-three-months``).

-  M-v Scroll backward by three months
   (``calendar-scroll-right-three-months``).

The most basic calendar scroll commands scroll by one month at a time.
This means that there are two months of overlap between the display
before the command and the display after. > scrolls the calendar
contents one month forward in time. < scrolls the contents one month
backwards in time.

The commands C-v and M-v scroll the calendar by an entire
screenful—three months—in analogy with the usual meaning of these
commands. C-v makes later dates visible and M-v makes earlier dates
visible. These commands take a numeric argument as a repeat count; in
particular, since C-u multiplies the next command by four, typing C-u
C-v scrolls the calendar forward by a year and typing C-u M-v scrolls
the calendar backward by a year.

The function keys (or ) and (or ) are equivalent to C-v and M-v, just as
they are in other modes.

31.3 Counting Days
------------------

-  M-=

   Display the number of days in the current region
   (``calendar-count-days-region``).

   To determine the number of days in a range, set the mark on one date
   using C-, move point to another date, and type M-=
   (``calendar-count-days-region``). The numbers of days shown is
   *inclusive*; that is, it includes the days specified by mark and
   point.

31.4 Miscellaneous Calendar Commands
------------------------------------

Display day-in-year (``calendar-print-day-of-year``).

-  C-c C-l

   Regenerate the calendar window (``calendar-redraw``).

-  <SPC>

   Scroll the next window up (``scroll-other-window``).

-  <DEL>

   Scroll the next window down (``scroll-other-window-down``).

-  q

   Exit from calendar (``calendar-exit``).

#

To display the number of days elapsed since the start of the year, or
the number of days remaining in the year, type the p d command
(``calendar-print-day-of-year``). This displays both of those numbers in
the echo area. The count of days elapsed includes the selected date. The
count of days remaining does not include that date.

If the calendar window text gets corrupted, type C-c C-l
(``calendar-redraw``) to redraw it. (This can only happen if you use
non-Calendar-mode editing commands.)

In Calendar mode, you can use (``scroll-other-window``) and
(``scroll-other-window-down``) to scroll the other window (if there is
one) up or down, respectively. This is handy when you display a list of
holidays or diary entries in another window.

To exit from the calendar, type q (``calendar-exit``). This buries all
buffers related to the calendar, selecting other buffers. (If a frame
contains a dedicated calendar window, exiting from the calendar deletes
or iconifies that frame depending on the value of
``calendar-remove-frame-by-deleting``.)

31.5 Writing Calendar Files
---------------------------

You can write calendars and diary entries to HTML and LaTeX files.

The Calendar HTML commands produce files of HTML code that contain
calendar, holiday, and diary entries. Each file applies to one month,
and has a name of the format yyyy-mm.html, where yyyy and mm are the
four-digit year and two-digit month, respectively. The variable
``cal-html-directory`` specifies the default output directory for the
HTML files. To prevent holidays from being shown, customize
``cal-html-holidays``.

Diary entries enclosed by ``<`` and ``>`` are interpreted as HTML tags
(for example: this is a diary entry with some red text). You can change
the overall appearance of the displayed HTML pages (for example, the
color of various page elements, header styles) via a stylesheet cal.css
in the directory containing the HTML files (see the value of the
variable ``cal-html-css-default`` for relevant style settings).

-  H m

   Generate a one-month calendar (``cal-html-cursor-month``).

-  H y

   Generate a calendar file for each month of a year, as well as an
   index page (``cal-html-cursor-year``). By default, this command
   writes files to a yyyy subdirectory—if this is altered some
   hyperlinks between years will not work.

If the variable ``cal-html-print-day-number-flag`` is non-\ ``nil``,
then the monthly calendars show the day-of-the-year number. The variable
``cal-html-year-index-cols`` specifies the number of columns in the
yearly index page.

The Calendar LaTeX commands produce a buffer of LaTeX code that prints
as a calendar. Depending on the command you use, the printed calendar
covers the day, week, month or year that point is in.

​

-  t m

   Generate a one-month calendar (``cal-tex-cursor-month``).

-  t M

   Generate a sideways-printing one-month calendar
   (``cal-tex-cursor-month-landscape``).

-  t d

   Generate a one-day calendar (``cal-tex-cursor-day``).

-  t w 1

   Generate a one-page calendar for one week, with hours
   (``cal-tex-cursor-week``).

-  t w 2

   Generate a two-page calendar for one week, with hours
   (``cal-tex-cursor-week2``).

-  t w 3

   Generate an ISO-style calendar for one week, without hours
   (``cal-tex-cursor-week-iso``).

-  t w 4

   Generate a calendar for one Monday-starting week, with hours
   (``cal-tex-cursor-week-monday``).

-  t w W

   Generate a two-page calendar for one week, without hours
   (``cal-tex-cursor-week2-summary``).

-  t f w

   Generate a Filofax-style two-weeks-at-a-glance calendar
   (``cal-tex-cursor-filofax-2week``).

-  t f W

   Generate a Filofax-style one-week-at-a-glance calendar
   (``cal-tex-cursor-filofax-week``).

-  t y

   Generate a calendar for one year (``cal-tex-cursor-year``).

-  t Y

   Generate a sideways-printing calendar for one year
   (``cal-tex-cursor-year-landscape``).

-  t f y

   Generate a Filofax-style calendar for one year
   (``cal-tex-cursor-filofax-year``).

Some of these commands print the calendar sideways (in landscape mode),
so it can be wider than it is long. Some of them use Filofax paper size
(3.75in x 6.75in). All of these commands accept a prefix argument, which
specifies how many days, weeks, months or years to print (starting
always with the selected one).

If the variable ``cal-tex-holidays`` is non-\ ``nil`` (the default),
then the printed calendars show the holidays in ``calendar-holidays``.
If the variable ``cal-tex-diary`` is non-\ ``nil`` (the default is
``nil``), diary entries are included also (in monthly, Filofax, and
iso-week calendars only). If the variable ``cal-tex-rules`` is
non-\ ``nil`` (the default is ``nil``), the calendar displays ruled
pages in styles that have sufficient room. Consult the documentation of
the individual cal-tex functions to see which calendars support which
features.

You can use the variable ``cal-tex-preamble-extra`` to insert extra
LaTeX commands in the preamble of the generated document if you need to.

31.6 Holidays
-------------

The Emacs calendar knows about many major and minor holidays, and can
display them. You can add your own holidays to the default list.

-  mouse-3 Holidays

-  h

   Display holidays for the selected date
   (``calendar-cursor-holidays``).

-  x

   Mark holidays in the calendar window (``calendar-mark-holidays``).

-  u

   Unmark calendar window (``calendar-unmark``).

-  a

   List all holidays for the displayed three months in another window
   (``calendar-list-holidays``).

-  M-x holidays

   List all holidays for three months around today's date in another
   window.

..

   Define: Veterans' day 老兵 Etymology: Old Lithuanian vetušas "old,
   aged;" and compare wether). 助记: Veterans day

.. code:: elisp

   ;;(sunrise-sunset)
   (lunar-phases)

-  M-x list-holidays

   List holidays in another window for a specified range of years.

   To see if any holidays fall on a given date, position point on that
   date in the calendar window and use the h command. Alternatively,
   click on that date with mouse-3 and then choose Holidays from the
   menu that appears. Either way, this displays the holidays for that
   date, in the echo area if they fit there, otherwise in a separate
   window.

   To view the distribution of holidays for all the dates shown in the
   calendar, use the x command. This displays the dates that are
   holidays in a different face. See
   `calendar-holiday-marker <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Calendar-Customizing>`__.
   The command applies both to the currently visible months and to other
   months that subsequently become visible by scrolling. To turn marking
   off and erase the current marks, type u, which also erases any diary
   marks (see
   `Diary <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Diary>`__).
   If the variable ``calendar-mark-holidays-flag`` is non-\ ``nil``,
   creating or updating the calendar marks holidays automatically.

   To get even more detailed information, use the a command, which
   displays a separate buffer containing a list of all holidays in the
   current three-month range. You can use and in the calendar window to
   scroll that list up and down, respectively.

   The command M-x holidays displays the list of holidays for the
   current month and the preceding and succeeding months; this works
   even if you don't have a calendar window. If the variable
   ``calendar-view-holidays-initially-flag`` is non-\ ``nil``, creating
   the calendar displays holidays in this way. If you want the list of
   holidays centered around a different month, use C-u M-x holidays,
   which prompts for the month and year.

The holidays known to Emacs include United States holidays and the major
Bahá'í, Chinese, Christian, Islamic, and Jewish holidays; also the
solstices and equinoxes.

   Define: solstices *ˈsɑːl.stɪs* 至日, 至点 Etymology: Middle English:
   from Old French, from Latin solstitium, from sol ‘sun’ + stit-
   ‘stopped, stationary’ (from the verb sistere). 助记:sol(sun) stice,
   stand, 停止的点.

   Define: equinox *ˈek.wə.nɑːks* Etymology: late Middle English: from
   Old French equinoxe or Latin aequinoctium, from aequi- ‘equal’ + nox,
   noct- ‘night’. 助记: equal好说,nox是night

The command M-x holiday-list displays the list of holidays for a range
of years. This function asks you for the starting and stopping years,
and allows you to choose all the holidays or one of several categories
of holidays. You can use this command even if you don't have a calendar
window.

The dates used by Emacs for holidays are based on *current practice*,
not historical fact. For example Veteran's Day began in 1919, but is
shown in earlier years.

31.7 Times of Sunrise and Sunset
--------------------------------

Special calendar commands can tell you, to within a minute or two, the
times of sunrise and sunset for any date.

-  mouse-3 Sunrise/sunset

-  S

   Display times of sunrise and sunset for the selected date
   (``calendar-sunrise-sunset``).

-  M-x sunrise-sunset

   Display times of sunrise and sunset for today's date.

-  C-u M-x sunrise-sunset

   Display times of sunrise and sunset for a specified date.

-  M-x calendar-sunrise-sunset-month

   Display times of sunrise and sunset for the selected month.

   Within the calendar, to display the *local times* of sunrise and
   sunset in the echo area, move point to the date you want, and type S.
   Alternatively, click mouse-3 on the date, then choose
   'Sunrise/sunset' from the menu that appears. The command M-x
   sunrise-sunset is available outside the calendar to display this
   information for today's date or a specified date. To specify a date
   other than today, use C-u M-x sunrise-sunset, which prompts for the
   year, month, and day.

You can display the times of sunrise and sunset for any location and any
date with C-u C-u M-x sunrise-sunset. This asks you for a longitude,
latitude, number of minutes difference from Coordinated Universal Time,
and date, and then tells you the times of sunrise and sunset for that
location on that date.

Because the times of sunrise and sunset depend on the location on earth,
you need to tell Emacs your latitude, longitude, and location name
before using these commands. Here is an example of what to set:

::

   (setq calendar-latitude 40.1)
   (setq calendar-longitude -88.2)
   (setq calendar-location-name "Urbana, IL")

Use one decimal place in the values of ``calendar-latitude`` and
``calendar-longitude``.

Your time zone also affects the local time of sunrise and sunset. Emacs
usually gets time zone information from the operating system, but if
these values are not what you want (or if the operating system does not
supply them), you must set them yourself. Here is an example:

::

   (setq calendar-time-zone -360)
   (setq calendar-standard-time-zone-name "CST")
   (setq calendar-daylight-time-zone-name "CDT")

The value of ``calendar-time-zone`` is the number of minutes difference
between your local standard time and Coordinated Universal Time
(Greenwich time). The values of ``calendar-standard-time-zone-name`` and
``calendar-daylight-time-zone-name`` are the abbreviations used in your
time zone. Emacs displays the times of sunrise and sunset *corrected for
daylight saving time*. See\ `Daylight
Saving <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Daylight-Saving>`__,
for how daylight saving time is determined.

As a user, you might find it convenient to set the calendar location
variables for your usual physical location in your .emacs file. If you
are a system administrator, you may want to set these variables for all
users in a default.el file. See `Init
File <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Init-File>`__.

31.8 Phases of the Moon
-----------------------

These calendar commands display the dates and times of the phases of the
moon (new moon, first quarter, full moon, last quarter). This feature is
useful for debugging problems that depend on the phase of the moon.

-  M Display the dates and times for all the quarters of the moon for
   the three-month period shown (``calendar-lunar-phases``).

-  M-x lunar-phases

   Display dates and times of the quarters of the moon for three months
   around today's date.

   Within the calendar, use the M command to display a separate buffer
   of the phases of the moon for the current three-month range. The
   dates and times listed are accurate to within a few minutes.

   Outside the calendar, use the command M-x lunar-phases to display the
   list of the phases of the moon for the current month and the
   preceding and succeeding months. For information about a different
   month, use C-u M-x lunar-phases, which prompts for the month and
   year.

The dates and times given for the phases of the moon are given in local
time (corrected for daylight saving, when appropriate). See the
discussion in the previous section. See
`Sunrise/Sunset <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Sunrise_002fSunset>`__.

31.9 Conversion To and From Other Calendars
-------------------------------------------

The Emacs calendar displayed is *always* the Gregorian calendar,
sometimes called the New Style calendar, which is used in most of the
world today. However, this calendar did not exist before the sixteenth
century and was not widely used before the eighteenth century; it did
not fully displace the Julian calendar and gain universal acceptance
until the early twentieth century. The Emacs calendar can display any
month since January, year 1 of the current era, but the calendar
displayed is always the Gregorian, even for a date at which the
Gregorian calendar did not exist.

While Emacs cannot display other calendars, it can convert dates to and
from several other calendars.

-  `Calendar
   Systems <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Calendar-Systems>`__:
   The calendars Emacs understands (aside from Gregorian).
-  `To Other
   Calendar <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#To-Other-Calendar>`__:
   Converting the selected date to various calendars.
-  `From Other
   Calendar <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#From-Other-Calendar>`__:
   Moving to a date specified in another calendar.

31.9.1 Supported Calendar Systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ISO commercial calendar is often used in business.

The Julian calendar, named after Julius Caesar, was the one used in
Europe throughout medieval times, and in many countries up until the
nineteenth century.

Astronomers use a simple counting of days elapsed since noon, Monday,
January 1, 4713 B.C. on the Julian calendar. The number of days elapsed
is called the Julian day number or the Astronomical day number.

The Hebrew calendar is used by tradition in the Jewish religion. The
Emacs calendar program uses the Hebrew calendar to determine the dates
of Jewish holidays. Hebrew calendar dates begin and end at sunset.

The Islamic calendar is used in many predominantly Islamic countries.
Emacs uses it to determine the dates of Islamic holidays. There is no
universal agreement in the Islamic world about the calendar; Emacs uses
a widely accepted version, but the precise dates of Islamic holidays
often depend on proclamation by religious authorities, not on
calculations. As a consequence, the actual dates of observance can vary
slightly from the dates computed by Emacs. Islamic calendar dates begin
and end at sunset.

The French Revolutionary calendar was created by the Jacobins after the
1789 revolution, to represent a more secular and nature-based view of
the annual cycle, and to install a 10-day week in a rationalization
measure similar to the metric system. The French government officially
abandoned this calendar at the end of 1805.

The Maya of Central America used three separate, overlapping calendar
systems, the *long count*, the *tzolkin*, and the *haab*. Emacs knows
about all three of these calendars. Experts dispute the exact
correlation between the Mayan calendar and our calendar; Emacs uses the
Goodman-Martinez-Thompson correlation in its calculations.

The Copts use a calendar based on the ancient Egyptian solar calendar.
Their calendar consists of twelve 30-day months followed by an extra
five-day period. Once every fourth year they add a leap day to this
extra period to make it six days. The Ethiopic calendar is identical in
structure, but has different year numbers and month names.

The Persians use a solar calendar based on a design of Omar Khayyam.
Their calendar consists of twelve months of which the first six have 31
days, the next five have 30 days, and the last has 29 in ordinary years
and 30 in leap years. Leap years occur in a complicated pattern every
four or five years. The calendar implemented here is the arithmetical
Persian calendar championed by Birashk, based on a 2,820-year cycle. It
differs from the astronomical Persian calendar, which is based on
astronomical events. As of this writing the first future discrepancy is
projected to occur on March 20, 2025. It is currently not clear what the
official calendar of Iran will be at that time.

The Chinese calendar is a complicated system of lunar months arranged
into solar years. The years go in cycles of sixty, each year containing
either twelve months in an ordinary year or thirteen months in a leap
year; each month has either 29 or 30 days. Years, ordinary months, and
days are named by combining one of ten celestial stems with one of
twelve terrestrial branches for a total of sixty names that are repeated
in a cycle of sixty.

The Bahá'í calendar system is based on a solar cycle of 19 months with
19 days each. The four remaining intercalary days are placed between the
18th and 19th months.

31.9.2 Converting To Other Calendars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following commands describe the selected date (the date at point) in
various other calendar systems:

-  mouse-3 Other calendars

-  p o

   Display the selected date in various other calendars.
   (``calendar-print-other-dates``).

-  p c

   Display ISO commercial calendar equivalent for selected day
   (``calendar-iso-print-date``).

-  p j

   Display Julian date for selected day
   (``calendar-julian-print-date``).

-  p a

   Display astronomical (Julian) day number for selected day
   (``calendar-astro-print-day-number``).

-  p h

   Display Hebrew date for selected day
   (``calendar-hebrew-print-date``).

-  p i

   Display Islamic date for selected day
   (``calendar-islamic-print-date``).

-  p f

   Display French Revolutionary date for selected day
   (``calendar-french-print-date``).

-  p b

   Display Bahá'í date for selected day (``calendar-bahai-print-date``).

-  p C

   Display Chinese date for selected day
   (``calendar-chinese-print-date``).

-  p k

   Display Coptic date for selected day
   (``calendar-coptic-print-date``).

-  p e

   Display Ethiopic date for selected day
   (``calendar-ethiopic-print-date``).

-  p p

   Display Persian date for selected day
   (``calendar-persian-print-date``).

-  p m

   Display Mayan date for selected day (``calendar-mayan-print-date``).

Otherwise, move point to the date you want to convert, then type the
appropriate command starting with p from the table above. The prefix p
is a mnemonic for "print", since Emacs "prints" the equivalent date in
the echo area. p o displays the date in all forms known to Emacs. You
can also use mouse-3 and then choose Other calendars from the menu that
appears. This displays the equivalent forms of the date in all the
calendars Emacs understands, in the form of a menu. (Choosing an
alternative from this menu doesn't actually do anything—the menu is used
only for display.)

31.9.3 Converting From Other Calendars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can use the other supported calendars to specify a date to move to.
This section describes the commands for doing this using calendars other
than Mayan; for the Mayan calendar, see the following section.

-  g c

   Move to a date specified in the ISO commercial calendar
   (``calendar-iso-goto-date``).

-  g w

   Move to a week specified in the ISO commercial calendar
   (``calendar-iso-goto-week``).

-  g j

   Move to a date specified in the Julian calendar
   (``calendar-julian-goto-date``).

-  g a

   Move to a date specified with an astronomical (Julian) day number
   (``calendar-astro-goto-day-number``).

-  g b

   Move to a date specified in the Bahá'í calendar
   (``calendar-bahai-goto-date``).

-  g h

   Move to a date specified in the Hebrew calendar
   (``calendar-hebrew-goto-date``).

-  g i

   Move to a date specified in the Islamic calendar
   (``calendar-islamic-goto-date``).

-  g f

   Move to a date specified in the French Revolutionary calendar
   (``calendar-french-goto-date``).

-  g C

   Move to a date specified in the Chinese calendar
   (``calendar-chinese-goto-date``).

-  g p

   Move to a date specified in the Persian calendar
   (``calendar-persian-goto-date``).

-  g k

   Move to a date specified in the Coptic calendar
   (``calendar-coptic-goto-date``).

-  g e

   Move to a date specified in the Ethiopic calendar
   (``calendar-ethiopic-goto-date``).

These commands ask you for a date on the other calendar, move point to
the Gregorian calendar date equivalent to that date, and display the
other calendar's date in the echo area. Emacs uses strict completion
(see `Completion
Exit <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Completion-Exit>`__)
whenever it asks you to type a month name, so you don't have to worry
about the spelling of Hebrew, Islamic, or French names.

One common issue concerning the Hebrew calendar is the computation of
the anniversary of a date of death, called a yahrzeit. The Emacs
calendar includes a facility for such calculations. If you are in the
calendar, the command M-x calendar-hebrew-list-yahrzeits asks you for a
range of years and then displays a list of the yahrzeit dates for those
years for the date given by point. If you are not in the calendar, this
command first asks you for the date of death and the range of years, and
then displays the list of yahrzeit dates.

31.10 The Diary
---------------

The Emacs diary keeps track of appointments or other events on a daily
basis, in ``conjunction`` with the calendar. To use the diary feature,
you must first create a diary file containing a list of events and their
dates. Then Emacs can automatically pick out and display the events for
today, for the immediate future, or for any specified date.

Although you probably will start by creating a diary manually, Emacs
provides a number of commands to let you view, add, and change diary
entries.

31.10.1 The Diary File
~~~~~~~~~~~~~~~~~~~~~~

Your diary file is a file that records events associated with particular
dates. The name of the diary file is specified by the variable
``diary-file``. The default is ``~/.emacs.d/diary``, though for
compatibility with older versions Emacs will use ~/diary if it exists.

Each entry in the diary file describes one event and consists of one or
more lines. An entry always begins with a date specification at the left
margin. The rest of the entry is simply text to describe the event. If
the entry has more than one line, then the lines after the first must
begin with whitespace to indicate they continue a previous entry. Lines
that do not begin with valid dates and do not continue a preceding entry
are ignored. Here's an example:

::

   12/22/2015  Twentieth wedding anniversary!
   10/22       Ruth's birthday.
   * 21, *:    Payday
   Tuesday--weekly meeting with grad students at 10am
            Supowit, Shen, Bitner, and Kapoor to attend.
   1/13/89     Friday the thirteenth!!
   thu 4pm     squash game with Lloyd.
   mar 16      Dad's birthday
   April 15, 2016 Income tax due.
   * 15        time cards due.

This example uses extra spaces to align the event descriptions of most
of the entries. Such formatting is purely a matter of taste.

You can also use a format where the first line of a diary entry consists
only of the date or day name (with no following blanks or punctuation).
For example:

::

   02/11/2012
         Bill B. visits Princeton today
         2pm Cognitive Studies Committee meeting
         2:30-5:30 Liz at Lawrenceville
         4:00pm Dentist appt
         7:30pm Dinner at George's
         8:00-10:00pm concert

This entry will have a different appearance if you use the simple diary
display (see`Diary
Display <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Diary-Display>`__).
The simple diary display omits the date line at the beginning; only the
continuation lines appear. This style of entry looks neater when you
display just a single day's entries, but can cause confusion if you ask
for more than one day's entries.

31.10.2 Displaying the Diary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once you have created a diary file, you can use the calendar to view it.
You can also view today's events outside of Calendar mode. In the
following, key bindings refer to the Calendar buffer.

-  mouse-3 Diary

-  d

   Display all diary entries for the selected date
   (``diary-view-entries``).

-  s

   Display the entire diary file (``diary-show-all-entries``).

-  m

   Mark all visible dates that have diary entries
   (``diary-mark-entries``).

-  u

   Unmark the calendar window (``calendar-unmark``).

-  M-x diary-print-entries

   Print hard copy of the diary display as it appears.

-  M-x diary

   Display all diary entries for today's date.

-  M-x diary-mail-entries

   Mail yourself email reminders about upcoming diary entries.

Displaying the diary entries with d shows in a separate buffer the diary
entries for the selected date in the calendar. The mode line of the new
buffer shows the date of the diary entries. Holidays are shown either in
the buffer or in the mode line, depending on the display method you
choose (see `Diary
Display <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Diary-Display>`__).
If you specify a numeric argument with d, it shows all the diary entries
for that many successive days. Thus, 2 d displays all the entries for
the selected date and for the following day.

Another way to display the diary entries for a date is to click mouse-3
on the date, and then choose Diary entries from the menu that appears.
If the variable ``calendar-view-diary-initially-flag`` is non-\ ``nil``,
creating the calendar lists the diary entries for the current date
(provided the current date is visible).

To get a broader view of which days are mentioned in the diary, use the
m command. This marks the dates that have diary entries in a different
face. See
`diary-entry-marker <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Calendar-Customizing>`__.

This command applies both to the months that are currently visible and
to those that subsequently become visible after scrolling. To turn
marking off and erase the current marks, type u, which also turns off
holiday marks (see
`Holidays <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Holidays>`__).
If the variable ``calendar-mark-diary-entries-flag`` is non-\ ``nil``,
creating or updating the calendar marks diary dates automatically.

To prevent an individual diary entry from being marked in the calendar,
insert the string that ``diary-nonmarking-symbol`` specifies (the
default is '&') at the beginning of the entry, before the date. This has
no effect on display of the entry in the diary buffer; it only affects
marks on dates in the calendar. Nonmarking entries can be useful for
generic entries that would otherwise mark many different dates.

To see the full diary file, rather than just some of the entries, use
the s command.

The command M-x diary displays the diary entries for the current date,
independently of the calendar display, and optionally for the next few
days as well; the variable ``diary-number-of-entries`` specifies how
many days to include. See
`diary-number-of-entries <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Diary-Customizing>`__.

If you put ``(diary)`` in your .emacs file, this automatically displays
a window with the day's diary entries when you start Emacs.

Some people like to receive email notifications of events in their
diary. To send such mail to yourself, use the command M-x
diary-mail-entries. A prefix argument specifies how many days (starting
with today) to check; otherwise, the variable ``diary-mail-days`` says
how many days.

31.10.3 Date Formats
~~~~~~~~~~~~~~~~~~~~

Here are some sample diary entries, illustrating different ways of
formatting a date. The examples all show dates in American order (month,
day, year), but Calendar mode supports European order (day, month, year)
and ISO order (year, month, day) as options.

::

   4/20/12  Switch-over to new tabulation system
   apr. 25  Start tabulating annual results
   4/30  Results for April are due
   */25  Monthly cycle finishes
   Friday  Don't leave without backing up files

The first entry appears only once, on April 20, 2012. The second and
third appear every year on the specified dates, and the fourth uses a
wildcard (asterisk) for the month, so it appears on the 25th of every
month. The final entry appears every week on Friday.

You can use just numbers to express a date, as in 'month/day' or
'month/day/year'. This must be followed by a nondigit. In the date
itself, month and day are numbers of one or two digits. The optional
year is also a number, and may be abbreviated to the last two digits;
that is, you can use '11/12/2012' or '11/12/12'.

Dates can also have the form 'monthname day' or 'monthname day, year',
where the month's name can be spelled in full or abbreviated (with or
without a period). The preferred abbreviations for month and day names
can be set using the variables ``calendar-abbrev-length``,
``calendar-month-abbrev-array``, and ``calendar-day-abbrev-array``. The
default is to use the first three letters of a name as its abbreviation.
Case is not significant.

A date may be generic,that is, partially unspecified. Then the entry
applies to all dates that match the specification. If the date does not
contain a year, it is generic and applies to any year. Alternatively,
month, day, or year can be '*'; this matches any month, day, or year,
respectively. Thus, a diary entry ‘3///*' matches any day in March of
any year; so does ‘march /'.

If you prefer the European style of writing dates (in which the day
comes before the month), or the ISO style (in which the order is year,
month, day), type M-x calendar-set-date-style while in the calendar, or
customize the variable ``calendar-date-style``. This affects how diary
dates are interpreted, date display, and the order in which some
commands expect their arguments to be given.

You can use the name of a day of the week as a generic date which
applies to any date falling on that day of the week. You can abbreviate
the day of the week as described above, or spell it in full; case is not
significant.

31.10.4 Commands to Add to the Diary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

While in the calendar, there are several commands to create diary
entries. The basic commands are listed here; more sophisticated commands
are in the next section (see `Special Diary
Entries <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Special-Diary-Entries>`__).
Entries can also be based on non-Gregorian calendars. See `Non-Gregorian
Diary <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Non_002dGregorian-Diary>`__.

-  i d

   Add a diary entry for the selected date (``diary-insert-entry``).

-  i w

   Add a diary entry for the selected day of the week
   (``diary-insert-weekly-entry``).

-  i m

   Add a diary entry for the selected day of the month
   (``diary-insert-monthly-entry``).

-  i y

   Add a diary entry for the selected day of the year
   (``diary-insert-yearly-entry``).

You can make a diary entry for a specific date by selecting that date in
the calendar window and typing the i d command. This command displays
the end of your diary file in another window and inserts the date; you
can then type the rest of the diary entry.

If you want to make a diary entry that applies to a specific day of the
week, select that day of the week (any occurrence will do) and type i w.
This inserts the day-of-week as a generic date; you can then type the
rest of the diary entry. You can make a monthly diary entry in the same
fashion: select the day of the month, use the i m command, and type the
rest of the entry. Similarly, you can insert a yearly diary entry with
the i y command.

All of the above commands make marking diary entries by default. To make
a nonmarking diary entry, give a prefix argument to the command. For
example, C-u i w makes a nonmarking weekly diary entry.

When you modify the diary file, be sure to save the file before exiting
Emacs. Saving the diary file after using any of the above insertion
commands will automatically update the diary marks in the calendar
window, if appropriate. You can use the command ``calendar-redraw`` to
force an update at any time.

31.10.5 Special Diary Entries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In addition to entries based on calendar dates, the diary file can
contain sexp entries for regular events such as anniversaries. These
entries are based on Lisp expressions (sexps) that Emacs evaluates as it
scans the diary file. Instead of a date, a sexp entry contains '%%'
followed by a Lisp expression which must begin and end with parentheses.
The Lisp expression determines which dates the entry applies to.

Calendar mode provides commands to insert certain commonly used sexp
entries:

-  i a

   Add an anniversary diary entry for the selected date
   (``diary-insert-anniversary-entry``).

-  i b

   Add a block diary entry for the current region
   (``diary-insert-block-entry``).

-  i c

   Add a cyclic diary entry starting at the date
   (``diary-insert-cyclic-entry``).

   If you want to make a diary entry that applies to the anniversary of
   a specific date, move point to that date and use the i a command.
   This displays the end of your diary file in another window and
   inserts the anniversary description; you can then type the rest of
   the diary entry. The entry looks like this:

::

   %%(diary-anniversary 10 31 1988) Arthur's birthday

This entry applies to October 31 in any year after 1988; '10 31 1988'
specifies the date. (If you are using the European or ISO calendar
style, the input order of month, day and year is different.) The reason
this expression requires a beginning year is that advanced diary
functions can use it to calculate the number of elapsed years.

A block diary entry applies to a specified range of consecutive dates.
Here is a block diary entry that applies to all dates from June 24, 2012
through July 10, 2012:

::

   %%(diary-block 6 24 2012 7 10 2012) Vacation

The '6 24 2012' indicates the starting date and the '7 10 2012'
indicates the stopping date. (Again, if you are using the European or
ISO calendar style, the input order of month, day and year is
different.)

To insert a block entry, place point and the mark on the two dates that
begin and end the range, and type i b. This command displays the end of
your diary file in another window and inserts the block description; you
can then type the diary entry.

Cyclic diary entries repeat after a fixed interval of days. To create
one, select the starting date and use the i c command. The command
prompts for the length of interval, then inserts the entry, which looks
like this:

::

   %%(diary-cyclic 50 3 1 2012) Renew medication

This entry applies to March 1, 2012 and every 50th day following; '3 1
2012' specifies the starting date. (If you are using the European or ISO
calendar style, the input order of month, day and year is different.)

All three of these commands make marking diary entries. To insert a
nonmarking entry, give a prefix argument to the command. For example,
C-u i a makes a nonmarking anniversary diary entry.

Marking sexp diary entries in the calendar can be time-consuming, since
every date visible in the calendar window must be individually checked.
So it's a good idea to make sexp diary entries nonmarking (with '&')
when possible.

Another sophisticated kind of sexp entry, a floating diary entry,
specifies a regularly occurring event by offsets specified in days,
weeks, and months. It is comparable to a crontab entry interpreted by
the ``cron`` utility. Here is a nonmarking, floating diary entry that
applies to the fourth Thursday in November:

::

   &%%(diary-float 11 4 4) American Thanksgiving

The 11 specifies November (the eleventh month), the 4 specifies Thursday
(the fourth day of the week, where Sunday is numbered zero), and the
second 4 specifies the fourth Thursday (1 would mean "first", 2 would
mean "second", −2 would mean "second-to-last", and so on). The month can
be a single month or a list of months. Thus you could change the 11
above to ‘'(1 2 3)' and have the entry apply to the last Thursday of
January, February, and March. If the month is ``t``, the entry applies
to all months of the year.

Each of the standard sexp diary entries takes an optional parameter
specifying the name of a face or a single-character string to use when
marking the entry in the calendar. Most generally, sexp diary entries
can perform arbitrary computations to determine when they apply.

31.10.6 Appointments
~~~~~~~~~~~~~~~~~~~~

If you have a diary entry for an appointment, and that diary entry
begins with a recognizable time of day, Emacs can warn you in advance
that an appointment is pending. Emacs alerts you to the appointment by
displaying a message in your chosen format, as specified by the variable
``appt-display-format``. If the value of ``appt-audible`` is
non-\ ``nil``, the warning includes an audible reminder. In addition, if
``appt-display-mode-line`` is non-\ ``nil``, Emacs displays the number
of minutes to the appointment on the mode line.

If ``appt-display-format`` has the value ``window``, then the variable
``appt-display-duration`` controls how long the reminder window is
visible for; and the variables ``appt-disp-window-function`` and
``appt-delete-window-function`` give the names of functions used to
create and destroy the window, respectively.

To enable appointment notification, type M-x appt-activate. With a
positive argument, it enables notification; with a negative argument, it
disables notification; with no argument, it toggles. Enabling
notification also sets up an appointment list for today from the diary
file, giving all diary entries found with recognizable times of day, and
reminds you just before each of them.

For example, suppose the diary file contains these lines:

::

   Monday
     9:30am Coffee break
    12:00pm Lunch

Then on Mondays, you will be reminded at around 9:20am about your coffee
break and at around 11:50am about lunch. The variable
``appt-message-warning-time`` specifies how many minutes (default 12) in
advance to warn you. This is a default warning time. Each appointment
can specify a different warning time by adding a piece matching
``appt-warning-time-regexp`` (see that variable's documentation for
details).

You can write times in am/pm style (with '12:00am' standing for midnight
and '12:00pm' standing for noon), or 24-hour European/military style.
You need not be consistent; your diary file can have a mixture of the
two styles. Times must be at the beginning of diary entries if they are
to be recognized.

Emacs updates the appointments list from the diary file automatically
just after midnight. You can force an update at any time by re-enabling
appointment notification. Both these actions also display the day's
diary buffer, unless you set ``appt-display-diary`` to ``nil``. The
appointments list is also updated whenever the diary file (or a file it
includes; see `Fancy Diary
Display <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fancy-Diary-Display>`__)
is saved. If you use the Org Mode and keep appointments in your Org
agenda files, you can add those appointments to the list using the
``org-agenda-to-appt`` command. See `Appointment
reminders <https://www.gnu.org/software/emacs/manual/html_mono/org.html#Weekly_002fdaily-agenda>`__,
for more about that command.

You can also use the appointment notification facility like an alarm
clock. The command M-x appt-add adds entries to the appointment list
without affecting your diary file. You delete entries from the
appointment list with M-x appt-delete.

31.10.7 Importing and Exporting Diary Entries
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can transfer diary entries between Emacs diary files and a variety
of other formats.

You can import diary entries from Outlook-generated appointment
messages. While viewing such a message in Rmail or Gnus, do M-x
diary-from-outlook to import the entry. You can make this command
recognize additional appointment message formats by customizing the
variable ``diary-outlook-formats``. Other mail clients can set
``diary-from-outlook-function`` to an appropriate value.

The icalendar package allows you to transfer data between your Emacs
diary file and iCalendar files, which are defined in RFC 2445—Internet
Calendaring and Scheduling Core Object Specification (iCalendar) (as
well as the earlier vCalendar format).

The command ``icalendar-import-buffer`` extracts iCalendar data from the
current buffer and adds it to your diary file. This function is also
suitable for automatic extraction of iCalendar data; for example with
the Rmail mail client one could use:

::

   (add-hook 'rmail-show-message-hook 'icalendar-import-buffer)

The command ``icalendar-import-file`` imports an iCalendar file and adds
the results to an Emacs diary file. For example:

::

   (icalendar-import-file "/here/is/calendar.ics"
                          "/there/goes/ical-diary")

You can use an ``#include`` directive to add the import file contents to
the main diary file, if these are different files. See `Fancy Diary
Display <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fancy-Diary-Display>`__.

Use ``icalendar-export-file`` to interactively export an entire Emacs
diary file to iCalendar format. To export only a part of a diary file,
mark the relevant area, and call ``icalendar-export-region``. In both
cases, Emacs appends the result to the target file.

31.11 Daylight Saving Time
--------------------------

Emacs understands the difference between standard time and daylight
saving time—the times given for sunrise, sunset, solstices, equinoxes,
and the phases of the moon take that into account. The rules for
daylight saving time vary from place to place and have also varied
historically from year to year. To do the job properly, Emacs needs to
know which rules to use.

Some operating systems keep track of the rules that apply to the place
where you are; on these systems, Emacs gets the information it needs
from the system automatically. If some or all of this information is
missing, Emacs fills in the gaps with the rules currently used in
Cambridge, Massachusetts. If the resulting rules are not what you want,
you can tell Emacs the rules to use by setting certain variables:
``calendar-daylight-savings-starts`` and
``calendar-daylight-savings-ends``.

These values should be Lisp expressions that refer to the variable
``year``, and evaluate to the Gregorian date on which daylight saving
time starts or (respectively) ends, in the form of a list ``(=month day
year``)=. The values should be ``nil`` if your area does not use
daylight saving time.

Emacs uses these expressions to determine the starting date of daylight
saving time for the holiday list and for correcting times of day in the
solar and lunar calculations.

The values for Cambridge, Massachusetts are as follows:

::

   (calendar-nth-named-day 2 0 3 year)
   (calendar-nth-named-day 1 0 11 year)

That is, the second 0th day (Sunday) of the third month (March) in the
year specified by ``year``, and the first Sunday of the eleventh month
(November) of that year. If daylight saving time were changed to start
on October 1, you would set ``calendar-daylight-savings-starts`` to
this:

::

   (list 10 1 year)

If there is no daylight saving time at your location, or if you want all
times in standard time, set ``calendar-daylight-savings-starts`` and
``calendar-daylight-savings-ends`` to ``nil``.

The variable ``calendar-daylight-time-offset`` specifies the difference
between daylight saving time and standard time, measured in minutes. The
value for Cambridge, Massachusetts is 60.

Finally, the two variables ``calendar-daylight-savings-starts-time`` and
``calendar-daylight-savings-ends-time`` specify the number of minutes
after midnight local time when the transition to and from daylight
saving time should occur. For Cambridge, Massachusetts both variables'
values are 120.

31.12 Summing Time Intervals
----------------------------

The timeclock package adds up time intervals, so you can (for instance)
keep track of how much time you spend working on particular projects. (A
more advanced alternative is to use the Org Mode's facilities for
clocking time, see `Clocking work
time <https://www.gnu.org/software/emacs/manual/html_mono/org.html#Clocking-work-time>`__).

Use the M-x timeclock-in command when you start working on a project,
and M-x timeclock-out command when you're done. Each time you do this,
it adds one time interval to the record of the project. You can change
to working on a different project with M-x timeclock-change.

Once you've collected data from a number of time intervals, you can use
M-x timeclock-workday-remaining to see how much time is left to work
today (assuming a typical average of 8 hours a day), and M-x
timeclock-when-to-leave which will calculate when you're done.

If you want Emacs to display the amount of time left of your workday in
the mode line, either customize the ``timeclock-modeline-display``
variable and set its value to ``t``, or invoke the M-x
timeclock-modeline-display command.

Terminating the current Emacs session might or might not mean that you
have stopped working on the project and, by default, Emacs asks you. You
can, however, customize the value of the variable
``timeclock-ask-before-exiting`` to ``nil`` to avoid the question; then,
only an explicit M-x timeclock-out or M-x timeclock-change will tell
Emacs that the current interval is over.

The timeclock functions work by accumulating the data in a file called
~/.emacs.d/timelog. You can specify a different name for this file by
customizing the variable ``timeclock-file``. If you edit the timeclock
file manually, or if you change the value of any of timeclock's
customizable variables, you should run the command M-x
timeclock-reread-log to update the data in Emacs from the file.

31.13 More advanced features of the Calendar and Diary
------------------------------------------------------

This section describes some of the more advanced/specialized features of
the calendar and diary. It starts with some of the many ways in which
you can customize the calendar and diary to suit your personal tastes.

31.13.1 Customizing the Calendar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The calendar display unfortunately cannot be changed from three months,
but you can customize the whitespace used by setting the variables:
``calendar-left-margin``, ``calendar-day-header-width``,
``calendar-day-digit-width``, ``calendar-column-width``, and
``calendar-intermonth-spacing``. To display text *between* the months,
for example week numbers, customize the variables
``calendar-intermonth-header`` and ``calendar-intermonth-text`` as
described in their documentation.

The variable ``calendar-month-header`` controls the text that appears
above each month in the calendar. By default, it shows the month and
year. The variable ``calendar-day-header-array`` controls the text that
appears above each day's column in every month. By default, it shows the
first two letters of each day's name.

The variable ``calendar-holiday-marker`` specifies how to mark a date
that is a holiday. Its value may be a single-character string to insert
next to the date, or a face name to use for displaying the date.
Likewise, the variable ``diary-entry-marker`` specifies how to mark a
date that has diary entries. The function ``calendar-mark-today`` uses
``calendar-today-marker`` to mark today's date. By default, the calendar
uses faces named ``holiday``, ``diary``, and ``calendar-today`` for
these purposes.

Starting the calendar runs the normal hook
``calendar-initial-window-hook``. Recomputation of the calendar display
does not run this hook. But if you leave the calendar with the q command
and reenter it, the hook runs again.

The variable ``calendar-today-visible-hook`` is a normal hook run after
the calendar buffer has been prepared with the calendar, when the
current date is visible in the window. One use of this hook is to mark
today's date; to do that use either of the functions
``calendar-mark-today`` or ``calendar-star-date``:

::

   (add-hook 'calendar-today-visible-hook 'calendar-mark-today)

A similar normal hook, ``calendar-today-invisible-hook`` is run if the
current date is *not* visible in the window.

Each of the calendar cursor motion commands runs the hook
``calendar-move-hook`` after it moves the cursor.

31.13.2 Customizing the Holidays
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are several variables listing the default holidays that Emacs
knows about. These are: ``holiday-general-holidays``,
``holiday-local-holidays``, ``holiday-solar-holidays``,
``holiday-bahai-holidays``, ``holiday-christian-holidays``,
``holiday-hebrew-holidays``, ``holiday-islamic-holidays``,
``holiday-oriental-holidays``, and ``holiday-other-holidays``. The names
should be self-explanatory; e.g., ``holiday-solar-holidays`` lists sun-
and moon-related holidays.

You can customize these lists of holidays to your own needs, deleting or
adding holidays as described below. Set any of them to ``nil`` to not
show the associated holidays.

The general holidays are, by default, holidays common throughout the
United States. In contrast, ``holiday-local-holidays`` and
``holiday-other-holidays`` are both empty by default. These are intended
for system-wide settings and your individual use, respectively.

By default, Emacs does not include all the holidays of the religions
that it knows, only those commonly found in secular calendars. For a
more extensive collection of religious holidays, you can set any (or
all) of the variables ``calendar-bahai-all-holidays-flag``,
``calendar-christian-all-holidays-flag``,
``calendar-hebrew-all-holidays-flag``, or
``calendar-islamic-all-holidays-flag`` to ``t``.

Each of the holiday variables is a list of holiday forms, each form
describing a holiday (or sometimes a list of holidays). Here is a table
of the possible kinds of holiday form. Day numbers and month numbers
count starting from 1, but dayname numbers count Sunday as 0. The
argument string is always the description of the holiday, as a string.

-  ``(holiday-fixed=month day string``)=

   A fixed date on the Gregorian calendar.

-  =(holiday-float=month dayname k string

   ​ &optional day) The kth dayname (dayname=0 for Sunday, and so on)
   after or before Gregorian date month, day. Negative k means count
   back from the end of the month. Optional day defaults to 1 if k is
   positive, and the last day of month otherwise.

-  ``(holiday-chinese=month day string``)=

   A fixed date on the Chinese calendar.

-  ``(holiday-hebrew=month day string``)=

   A fixed date on the Hebrew calendar.

-  ``(holiday-islamic=month day string``)=

   A fixed date on the Islamic calendar.

-  ``(holiday-julian=month day string``)=

   A fixed date on the Julian calendar.

-  ``(holiday-sexp=sexp string``)=

   A date calculated by the Lisp expression sexp. The expression should
   use the variable ``year`` to compute and return the date of a holiday
   in the form of a list ``(=month day year``)=, or ``nil`` if the
   holiday doesn't happen this year.

-  ``(if=condition holiday-form``)=

   A holiday that happens only if condition is true.

-  ``(=function [args]``)=

   A list of dates calculated by the function function, called with
   arguments args.

For example, suppose you want to add Bastille Day, celebrated in France
on July 14 (i.e., the fourteenth day of the seventh month). You can do
this as follows:

::

   (setq holiday-other-holidays '((holiday-fixed 7 14 "Bastille Day")))

Many holidays occur on a specific day of the week, at a specific time of
month. Here is a holiday form describing Hurricane Supplication Day,
celebrated in the Virgin Islands on the fourth Monday in July:

::

   (holiday-float 7 1 4 "Hurricane Supplication Day")

Here the 7 specifies July, the 1 specifies Monday (Sunday is 0, Tuesday
is 2, and so on), and the 4 specifies the fourth occurrence in the month
(1 specifies the first occurrence, 2 the second occurrence, −1 the last
occurrence, −2 the second-to-last occurrence, and so on).

You can specify holidays that occur on fixed days of the Bahá'í,
Chinese, Hebrew, Islamic, and Julian calendars too. For example,

::

   (setq holiday-other-holidays
         '((holiday-hebrew 10 2 "Last day of Hanukkah")
           (holiday-islamic 3 12 "Mohammed's Birthday")
           (holiday-julian 4 2 "Jefferson's Birthday")))

adds the last day of Hanukkah (since the Hebrew months are numbered with
1 starting from Nisan), the Islamic feast celebrating Mohammed's
birthday (since the Islamic months are numbered from 1 starting with
Muharram), and Thomas Jefferson's birthday, which is 2 April 1743 on the
Julian calendar.

To include a holiday conditionally, use either Emacs Lisp's ``if`` or
the ``holiday-sexp`` form. For example, American presidential elections
occur on the first Tuesday after the first Monday in November of years
divisible by 4:

::

   (holiday-sexp '(if (zerop (% year 4))
                      (calendar-gregorian-from-absolute
                       (1+ (calendar-dayname-on-or-before
                             1 (+ 6 (calendar-absolute-from-gregorian
                                     (list 11 1 year)))))))
                 "US Presidential Election")

or

::

   (if (zerop (% displayed-year 4))
       (holiday-fixed 11
              (calendar-extract-day
                (calendar-gregorian-from-absolute
                  (1+ (calendar-dayname-on-or-before
                        1 (+ 6 (calendar-absolute-from-gregorian
                                 (list 11 1 displayed-year)))))))
              "US Presidential Election"))

Some holidays just don't fit into any of these forms because special
calculations are involved in their determination. In such cases you must
write a Lisp function to do the calculation. To include eclipses, for
example, add ``(eclipses)`` to ``holiday-other-holidays`` and write an
Emacs Lisp function ``eclipses`` that returns a (possibly empty) list of
the relevant Gregorian dates among the range visible in the calendar
window, with descriptive strings, like this:

::

   (((6 4 2012) "Lunar Eclipse") ((11 13 2012) "Solar Eclipse") ... )

31.13.3 Converting from the Mayan Calendar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here are the commands to select dates based on the Mayan calendar:

-  g m l

   Move to a date specified by the long count calendar
   (``calendar-mayan-goto-long-count-date``).

-  g m n t

   Move to the next occurrence of a place in the tzolkin calendar
   (``calendar-mayan-next-tzolkin-date``).

-  g m p t

   Move to the previous occurrence of a place in the tzolkin calendar
   (``calendar-mayan-previous-tzolkin-date``).

-  g m n h

   Move to the next occurrence of a place in the haab calendar
   (``calendar-mayan-next-haab-date``).

-  g m p h

   Move to the previous occurrence of a place in the haab calendar
   (``calendar-mayan-previous-haab-date``).

-  g m n c

   Move to the next occurrence of a place in the calendar round
   (``calendar-mayan-next-calendar-round-date``).

-  g m p c

   Move to the previous occurrence of a place in the calendar round
   (``calendar-mayan-previous-calendar-round-date``).

   To understand these commands, you need to understand the Mayan
   calendars. The long count is a counting of days with these units:

::

   1 kin = 1 day   1 uinal = 20 kin   1 tun = 18 uinal
   1 katun = 20 tun   1 baktun = 20 katun

Thus, the long count date 12.16.11.16.6 means 12 baktun, 16 katun, 11
tun, 16 uinal, and 6 kin. The Emacs calendar can handle Mayan long count
dates as early as 7.17.18.13.3, but no earlier. When you use the g m l
command, type the Mayan long count date with the baktun, katun, tun,
uinal, and kin separated by periods.

The Mayan tzolkin calendar is a cycle of 260 days formed by a pair of
independent cycles of 13 and 20 days. Since this cycle repeats
endlessly, Emacs provides commands to move backward and forward to the
previous or next point in the cycle. Type g m p t to go to the previous
tzolkin date; Emacs asks you for a tzolkin date and moves point to the
previous occurrence of that date. Similarly, type g m n t to go to the
next occurrence of a tzolkin date.

The Mayan haab calendar is a cycle of 365 days arranged as 18 months of
20 days each, followed by a 5-day monthless period. Like the tzolkin
cycle, this cycle repeats endlessly, and there are commands to move
backward and forward to the previous or next point in the cycle. Type g
m p h to go to the previous haab date; Emacs asks you for a haab date
and moves point to the previous occurrence of that date. Similarly, type
g m n h to go to the next occurrence of a haab date.

The Maya also used the combination of the tzolkin date and the haab
date. This combination is a cycle of about 52 years called a *calendar
round*. If you type g m p c, Emacs asks you for both a haab and a
tzolkin date and then moves point to the previous occurrence of that
combination. Use g m n c to move point to the next occurrence of a
combination. These commands signal an error if the haab/tzolkin date
combination you have typed is impossible.

Emacs uses strict completion (see `Completion
Exit <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Completion-Exit>`__)
whenever it asks you to type a Mayan name, so you don't have to worry
about spelling.

31.13.4 Date Display Format
~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can customize the way dates are displayed in the diary, mode lines,
and messages by setting ``calendar-date-display-form``. This variable
holds a list of expressions that can involve the variables ``month``,
``day``, and ``year``, which are all numbers in string form, and
``monthname`` and ``dayname``, which are both alphabetic strings. In the
American style, the default value of this list is as follows:

::

   ((if dayname (concat dayname ", ")) monthname " " day ", " year)

while in the European style this value is the default:

::

   ((if dayname (concat dayname ", ")) day " " monthname " " year)

The default ISO date representation is:

::

   ((format "%s-%.2d-%.2d" year (string-to-number month)
            (string-to-number day)))

Another typical American format is:

::

   (month "/" day "/" (substring year -2))

31.13.5 Time Display Format
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The calendar and diary by default display times of day in the
conventional American style with the hours from 1 through 12, minutes,
and either 'am' or 'pm'. If you prefer the European style, also known in
the US as military, in which the hours go from 00 to 23, you can alter
the variable ``calendar-time-display-form``. This variable is a list of
expressions that can involve the variables ``12-hours``, ``24-hours``,
and ``minutes``, which are all numbers in string form, and ``am-pm`` and
``time-zone``, which are both alphabetic strings. The default value is:

::

   (12-hours ":" minutes am-pm
             (if time-zone " (") time-zone (if time-zone ")"))

Here is a value that provides European style times:

::

   (24-hours ":" minutes
             (if time-zone " (") time-zone (if time-zone ")"))

Note that few calendar functions return a time of day (at present, only
solar functions).

31.13.6 Customizing the Diary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ordinarily, the diary window indicates any holidays that fall on the
date of the diary entries, either in the mode line or the buffer itself.
The process of checking for holidays can be slow, depending on the
defined holidays. In that case, setting ``diary-show-holidays-flag`` to
``nil`` will speed up the diary display.

The variable ``diary-number-of-entries`` controls the number of days of
diary entries to be displayed at one time. It affects the initial
display when ``calendar-view-diary-initially-flag`` is ``t``, as well as
the command M-x diary. For example, a value of 1 (the default) displays
only the current day's diary entries, whereas a value of 2 will also
show the next day's entries. The value can also be a vector of seven
integers: for example, if the value is ``[0 2 2 2 2 4 1]`` then no diary
entries appear on Sunday, the current date's and the next day's diary
entries appear Monday through Thursday, Friday through Monday's entries
appear on Friday, while on Saturday only that day's entries appear.

You can customize the form of dates in your diary file by setting the
variable ``diary-date-forms``. This variable is a list of patterns for
recognizing a date. Each date pattern is a list whose elements may be
regular expressions (see `Regular
Expressions <https://www.gnu.org/software/emacs/manual/html_mono/elisp.html#Regular-Expressions>`__)
or the symbols ``month``, ``day``, ``year``, ``monthname``, and
``dayname``. All these elements serve as patterns that match certain
kinds of text in the diary file. In order for the date pattern as a
whole to match, all of its elements must match consecutively.

A regular expression in a date pattern matches in its usual fashion,
using the standard syntax table altered so that ‘*' is a word
constituent.

The symbols ``month``, ``day``, ``year``, ``monthname``, and ``dayname``
match the month number, day number, year number, month name, and day
name of the date being considered. The symbols that match numbers allow
leading zeros; those that match names allow capitalization and
abbreviation (as specified by ``calendar-month-abbrev-array`` and
``calendar-day-abbrev-array``). All the symbols can match '*'; since ‘*'
in a diary entry means "any day", "any month", and so on, it should
match regardless of the date being considered.

The default value of ``diary-date-forms`` in the American style is
provided by ``diary-american-date-forms``:

::

   ((month "/" day "[^/0-9]")
    (month "/" day "/" year "[^0-9]")
    (monthname " *" day "[^,0-9]")
    (monthname " *" day ", *" year "[^0-9]")
    (dayname "\\W"))

The variables ``diary-european-date-forms`` and ``diary-iso-date-forms``
provide other default styles.

The date patterns in the list must be *mutually exclusive* and must not
match any portion of the diary entry itself, just the date and one
character of whitespace. If, to be mutually exclusive, the pattern must
match a portion of the diary entry text—beyond the whitespace that ends
the date—then the first element of the date pattern *must* be
``backup``. This causes the date recognizer to back up to the beginning
of the current word of the diary entry, after finishing the match. Even
if you use ``backup``, the date pattern must absolutely not match more
than a portion of the first word of the diary entry. For example, the
default value of ``diary-european-date-forms`` is:

::

   ((day "/" month "[^/0-9]")
    (day "/" month "/" year "[^0-9]")
    (backup day " *" monthname "\\W+\\<\\([^*0-9]\\|\\([0-9]+[:aApP]\\)\\)")
    (day " *" monthname " *" year "[^0-9]")
    (dayname "\\W"))

Notice the use of ``backup`` in the third pattern, because it needs to
match part of a word beyond the date itself to distinguish it from the
fourth pattern.

31.13.7 Diary Entries Using non-Gregorian Calendars
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As well as entries based on the standard Gregorian calendar, your diary
can have entries based on Bahá'í, Chinese, Hebrew, or Islamic dates.
Recognition of such entries can be time-consuming, however, and since
most people don't use them, you must explicitly enable their use. If you
want the diary to recognize Hebrew-date diary entries, for example, you
must do this:

::

   (add-hook 'diary-nongregorian-listing-hook 'diary-hebrew-list-entries)
   (add-hook 'diary-nongregorian-marking-hook 'diary-hebrew-mark-entries)

Similarly, for Islamic, Bahá'í and Chinese entries, add
``diary-islamic-list-entries`` and ``diary-islamic-mark-entries``,
``diary-bahai-list-entries`` and ``diary-bahai-mark-entries``, or
``diary-chinese-list-entries`` and ``diary-chinese-mark-entries``.

These diary entries have the same formats as Gregorian-date diary
entries; except that ``diary-bahai-entry-symbol`` (default 'B') must
precede a Bahá'í date, ``diary-chinese-entry-symbol`` (default 'C') a
Chinese date, ``diary-hebrew-entry-symbol`` (default 'H') a Hebrew date,
and ``diary-islamic-entry-symbol`` (default 'I') an Islamic date.
Moreover, non-Gregorian month names may not be abbreviated (because the
first three letters are often not unique). (Note also that you must use
"Adar I" if you want Adar of a common Hebrew year.) For example, a diary
entry for the Hebrew date Heshvan 25 could look like this:

::

   HHeshvan 25 Happy Hebrew birthday!

and would appear in the diary for any date that corresponds to Heshvan
25 on the Hebrew calendar. And here is an Islamic-date diary entry that
matches Dhu al-Qada 25:

::

   IDhu al-Qada 25 Happy Islamic birthday!

As with Gregorian-date diary entries, non-Gregorian entries are
nonmarking if preceded by ``diary-nonmarking-symbol`` (default '&').

Here is a table of commands used in the calendar to create diary entries
that match the selected date and other dates that are similar in the
Bahá'í, Chinese, Hebrew, or Islamic calendars:

-  i h d

   ``diary-hebrew-insert-entry``

-  i h m

   ``diary-hebrew-insert-monthly-entry``

-  i h y

   ``diary-hebrew-insert-yearly-entry``

-  i i d

   ``diary-islamic-insert-entry``

-  i i m

   ``diary-islamic-insert-monthly-entry``

-  i i y

   ``diary-islamic-insert-yearly-entry``

-  i B d

   ``diary-bahai-insert-entry``

-  i B m

   ``diary-bahai-insert-monthly-entry``

-  i B y

   ``diary-bahai-insert-yearly-entry``

-  i C d

   ``diary-chinese-insert-entry``

-  i C m

   ``diary-chinese-insert-monthly-entry``

-  i C y

   ``diary-chinese-insert-yearly-entry``

-  i C a

   ``diary-chinese-insert-anniversary-entry``

   These commands work much like the corresponding commands for ordinary
   diary entries: they apply to the date that point is on in the
   calendar window, and what they do is insert just the date portion of
   a diary entry at the end of your diary file. You must then insert the
   rest of the diary entry. The basic commands add an entry for the
   specific non-Gregorian date, the 'monthly' commands for the given
   non-Gregorian day-within-month in every month, and the 'yearly'
   commands for the given non-Gregorian day and month in every year.

Next: `Fancy Diary
Display <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fancy-Diary-Display>`__,
Previous: `Non-Gregorian
Diary <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Non_002dGregorian-Diary>`__,
Up: `Advanced Calendar/Diary
Usage <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Advanced-Calendar_002fDiary-Usage>`__

31.13.8 Diary Display
~~~~~~~~~~~~~~~~~~~~~

Diary display works by preparing the list of diary entries and then
running the function specified by the variable
``diary-display-function``. The default value ``diary-fancy-display``
displays diary entries and holidays by copying them into a special
buffer that exists only for the sake of display. Copying diary entries
to a separate buffer provides an opportunity to change the displayed
text to make it prettier—for example, to sort the entries by the dates
they apply to.

Ordinarily, the fancy diary buffer does not show days for which there
are no diary entries, even if that day is a holiday. If you want such
days to be shown in the fancy diary buffer, set the variable
``diary-list-include-blanks`` to ``t``.

The fancy diary buffer enables View mode (see `View
Mode <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#View-Mode>`__).

The alternative display method ``diary-simple-display`` shows the actual
diary buffer, and uses invisible text to hide entries that don't apply.
Holidays are shown in the mode line. The advantage of this method is
that you can edit the buffer and save your changes directly to the diary
file. This method is not as flexible as the fancy method, however. For
example, it cannot sort entries. Another disadvantage is that invisible
text can be confusing. For example, if you copy a region of text in
order to paste it elsewhere, invisible text may be included. Similarly,
since the diary buffer as you see it is an illusion, simply printing the
buffer may not print what you see on your screen.

For this reason, there is a special command to print hard copy of the
diary buffer *as it appears*; this command is M-x diary-print-entries.
It works with either display method, although with the fancy display you
can also print the buffer like any other. To print a hard copy of a
day-by-day diary for a week, position point on the first day of the
week, type 7 d, and then do M-x diary-print-entries. As usual, the
inclusion of the holidays slows down the display slightly; you can speed
things up by setting the variable ``diary-show-holidays-flag`` to
``nil``.

This command prepares a temporary buffer that contains only the diary
entries currently visible in the diary buffer. Unlike with the simple
display, the other irrelevant entries are really absent, not just
hidden. After preparing the buffer, it runs the hook
``diary-print-entries-hook``. The default value of this hook sends the
data directly to the printer with the command ``lpr-buffer`` (see
`Printing <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Printing>`__).
If you want to use a different command to do the printing, just change
the value of this hook. Other uses might include, for example,
rearranging the lines into order by day and time.

You can edit the diary entries as they appear in the simple diary
window, but it is important to remember that the buffer displayed
contains the *entire* diary file, with portions of it concealed from
view. This means, for instance, that the C-f (``forward-char``) command
can put point at what appears to be the end of the line, but what is in
reality the middle of some concealed line.

*Be careful when editing the diary entries in the simple display!*
Inserting additional lines or adding/deleting characters in the middle
of a visible line cannot cause problems, but editing at the end of a
line may not do what you expect. Deleting a line may delete other
invisible entries that follow it. Before editing the simple diary
buffer, it is best to display the entire file with s
(``diary-show-all-entries``).

31.13.9 Fancy Diary Display
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following features only work with the fancy diary display.

You can use the normal hook ``diary-list-entries-hook`` to sort each
day's diary entries by their time of day. Here's how:

::

   (add-hook 'diary-list-entries-hook 'diary-sort-entries t)

For each day, this sorts diary entries that begin with a recognizable
time of day according to their times. Diary entries without times come
first within each day. Note how the sort command is placed at the end of
the hook list, in case earlier members of the list change the order of
the diary entries, or add items.

You can write 'comments' in diary entries, by setting the variables
``diary-comment-start`` and ``diary-comment-end`` to strings that
delimit comments. The fancy display does not print comments. You might
want to put meta-data for the use of other packages (e.g., the
appointment package, see
`Appointments <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Appointments>`__)
inside comments.

Your main diary file can include other files. This permits a group of
people to share a diary file for events that apply to all of them. Lines
in the diary file starting with ``diary-include-string``:

::

   #include "filename"

include the diary entries from the file filename in the fancy diary
buffer. The include mechanism is recursive, so that included files can
include other files, and so on (you must be careful not to have a cycle
of inclusions, of course). Here is how to enable the include facility:

::

   (add-hook 'diary-list-entries-hook 'diary-include-other-diary-files)
   (add-hook 'diary-mark-entries-hook 'diary-mark-included-diary-files)

The include mechanism works only with the fancy diary display, because
simple diary display shows the entries directly from your diary file.

Previous: `Fancy Diary
Display <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Fancy-Diary-Display>`__,
Up: `Advanced Calendar/Diary
Usage <https://www.gnu.org/software/emacs/manual/html_mono/emacs.html#Advanced-Calendar_002fDiary-Usage>`__

31.13.10 Sexp Entries and the Fancy Diary Display
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sexp diary entries allow you to do more than just have complicated
conditions under which a diary entry applies. Sexp entries should be
preceded by ``diary-sexp-entry-symbol`` (default '%%') in the diary
file. With the fancy diary display, sexp entries can generate the text
of the entry depending on the date itself.

For example, an anniversary diary entry can insert the number of years
since the anniversary date into the text of the diary entry. Thus the
'%d' in this diary entry:

::

   %%(diary-anniversary 10 31 1948) Arthur's birthday (%d years old)

gets replaced by the age, so on October 31, 1990 the entry appears in
the fancy diary buffer like this:

::

   Arthur's birthday (42 years old)

If the diary file instead contains this entry:

::

   %%(diary-anniversary 10 31 1948) Arthur's %d%s birthday

the entry in the fancy diary buffer for October 31, 1990 appears like
this:

::

   Arthur's 42nd birthday

Similarly, cyclic diary entries can interpolate the number of
repetitions that have occurred:

::

   %%(diary-cyclic 50 1 1 2012) Renew medication (%d%s time)

looks like this:

::

   Renew medication (5th time)

in the fancy diary display on September 7, 2012.

There is an early-reminder diary sexp that includes its entry in the
diary not only on the date of occurrence, but also on earlier dates. For
example, if you want a reminder a week before your anniversary, you can
use

::

   %%(diary-remind '(diary-anniversary 12 22 1968) 7) Ed's anniversary

and the fancy diary will show 'Ed's anniversary' both on December 15 and
on December 22.

The function ``diary-date`` applies to dates described by a month, day,
year combination, each of which can be an integer, a list of integers,
or ``t`` (meaning all values). For example,

::

   %%(diary-date '(10 11 12) 22 t) Rake leaves

causes the fancy diary to show

::

   Rake leaves

on October 22, November 22, and December 22 of every year.

The function ``diary-float`` allows you to describe diary entries that
apply to dates like the third Friday of November, or the last Tuesday in
April. The parameters are the month, dayname, and an index n. The entry
appears on the nth dayname after the first day of month, where dayname=0
means Sunday, 1 means Monday, and so on. If n is negative it counts
backward from the end of month. The value of month can be a list of
months, a single month, or ``t`` to specify all months. You can also use
an optional parameter day to specify the nth dayname on or after/before
day of month; the value of day defaults to 1 if n is positive and to the
last day of month if n is negative. For example,

::

   %%(diary-float t 1 -1) Pay rent

causes the fancy diary to show

::

   Pay rent

on the last Monday of every month.

The generality of sexp diary entries lets you specify any diary entry
that you can describe algorithmically. A sexp diary entry contains an
expression that computes whether the entry applies to any given date. If
its value is non-\ ``nil``, the entry applies to that date; otherwise,
it does not. The expression can use the variable ``date`` to find the
date being considered; its value is a list (month day year) that refers
to the Gregorian calendar.

The sexp diary entry applies to a date when the expression's value is
non-\ ``nil``, but some values have more specific meanings. If the value
is a string, that string is a description of the event which occurs on
that date. The value can also have the form ``(=mark``.=string=)=; then
mark specifies how to mark the date in the calendar, and string is the
description of the event. If mark is a single-character string, that
character appears next to the date in the calendar. If mark is a face
name, the date is displayed in that face. If mark is ``nil``, that
specifies no particular highlighting for the date.

Suppose you get paid on the 21st of the month if it is a weekday, and on
the Friday before if the 21st is on a weekend. Here is how to write a
sexp diary entry that matches those dates:

::

   &%%(let ((dayname (calendar-day-of-week date))
            (day (cadr date)))
         (or (and (= day 21) (memq dayname '(1 2 3 4 5)))
             (and (memq day '(19 20)) (= dayname 5)))
            ) Pay check deposited

The following sexp diary entries take advantage of the ability (in the
fancy diary display) to concoct diary entries whose text varies based on
the date:

​

-  ``%%(diary-sunrise-sunset)``

   Make a diary entry for today's local times of sunrise and sunset.

-  ``%%(diary-lunar-phases)``

   Make a diary entry for the phases (quarters) of the moon.

-  ``%%(diary-day-of-year)``

   Make a diary entry with today's day number in the current year and
   the number of days remaining in the current year.

-  ``%%(diary-iso-date)``

   Make a diary entry with today's equivalent ISO commercial date.

-  ``%%(diary-julian-date)``

   Make a diary entry with today's equivalent Julian calendar date.

-  ``%%(diary-astro-day-number)``

   Make a diary entry with today's equivalent astronomical (Julian) day
   number.

-  ``%%(diary-bahai-date)``

   Make a diary entry with today's equivalent Bahá'í calendar date.

-  ``%%(diary-chinese-date)``

   Make a diary entry with today's equivalent Chinese calendar date.

-  ``%%(diary-coptic-date)``

   Make a diary entry with today's equivalent Coptic calendar date.

-  ``%%(diary-ethiopic-date)``

   Make a diary entry with today's equivalent Ethiopic calendar date.

-  ``%%(diary-french-date)``

   Make a diary entry with today's equivalent date on the French
   Revolutionary calendar.

-  ``%%(diary-hebrew-date)``

   Make a diary entry with today's equivalent Hebrew calendar date.

-  ``%%(diary-islamic-date)``

   Make a diary entry with today's equivalent Islamic calendar date.

-  ``%%(diary-mayan-date)``

   Make a diary entry with today's equivalent Mayan calendar date.

-  ``%%(diary-persian-date)``

   Make a diary entry with today's equivalent Persian calendar date.

For example, including the diary entry

::

   &%%(diary-hebrew-date)

causes every day's diary display to contain the equivalent date on the
Hebrew calendar, if you are using the fancy diary display. (With simple
diary display, the literal line '&%%(diary-hebrew-date)' appears in the
diary for any date.)

This function has been used to construct certain standard Hebrew sexp
diary entries:

​

-  ``%%(diary-hebrew-rosh-hodesh)``

   Make a diary entry that tells the occurrence and ritual announcement
   of each new Hebrew month.

-  ``%%(diary-hebrew-parasha)``

   Make a Saturday diary entry that tells the weekly synagogue scripture
   reading.

-  ``%%(diary-hebrew-sabbath-candles)``

   Make a Friday diary entry that tells the *local time* of Sabbath
   candle lighting.

-  ``%%(diary-hebrew-omer)``

   Make a diary entry that gives the omer count, when appropriate.

-  ``%%(diary-hebrew-yahrzeit=month day year``)=name

   Make a diary entry marking the anniversary of a date of death. The
   date is the *Gregorian* (civil) date of death. The diary entry
   appears on the proper Hebrew calendar anniversary and on the day
   before. (The order of the parameters changes according to the
   calendar date style; for example in the European style to day, month,
   year.)

-  ``%%(diary-hebrew-birthday=month day year``)=

   Make a diary entry for a birthday on the Hebrew calendar.

All the functions documented above take an optional argument mark which
specifies how to mark the date in the calendar display. If one of these
functions decides that it applies to a certain date, it returns a value
that contains mark, as described above.


