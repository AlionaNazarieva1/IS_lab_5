---
# Front matter
lang: ru-RU
title: "Oтчёт по лабораторной работе"
subtitle: "Дискреционное разграничение прав в Linux. Исследование влияния дополнительных атрибутов"
author: "Назарьева Алена Игоревна НФИбд-03-18"

# Formatting
toc-title: "Содержание"
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4paper
documentclass: scrreprt
polyglossia-lang: russian
polyglossia-otherlangs: english
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase
indent: true
pdf-engine: lualatex
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Изучение механизмов изменения идентификаторов, применения
SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов

# Выполнение лабораторной работы

Создание программы

1. Вошла в систему от имени пользователя guest.
2. Создала программу simpleid.c (рис. -@fig:001)

![simpleid.c](1.jpg){ #fig:001 width=70% }

3. Скомплилировала программу и убедилась, что файл программы создан
4. Выполнила программу simpleid
5. Выполнила системную программу id
и сравнила полученный результат с данными предыдущего пункта задания. Данные совпадают (рис. -@fig:002)

![пункты 3-5](2.jpg){ #fig:002 width=70% }

6. Усложнила программу, добавив вывод действительных идентификаторов. Получившуюся программу назвала simpleid2.c.
(рис. -@fig:003)

![simpleid2.c](3.jpg){ #fig:003 width=70% }

7. Скомпилировала и запустила simpleid2.c
8. От имени суперпользователя сменила у файла владельца и установила установите SetU’D-бит
9. Использовала sudo или повысила временно свои права с помощью su.(рис. -@fig:004)

![пункты 7-9](4.jpg){ #fig:004 width=70% }

10. Выполнила проверку правильности установки новых атрибутов и смены
владельца файла simpleid2
11. Запустила simpleid2 и id. Результаты совпадают. (рис. -@fig:005)

![пункты 10-11](5.jpg){ #fig:005 width=70% }

12. Проделала тоже самое относительно SetGID-бита. (рис. -@fig:006)

![пункты 12-1](6.jpg){ #fig:006 width=70% }

(рис. -@fig:007)

![пункты 12-2](7.jpg){ #fig:007 width=70% }

13. Создала программу readfile.c (рис. -@fig:008)

![readfile.c](8.jpg){ #fig:008 width=70% }

14. Откомпилировала её.
15. Сменила владельца у файла readfile.c и изменила права так, чтобы только суперпользователь мог прочитать его, a guest не мог. (рис. -@fig:009)

![пункты 14-15](9.jpg){ #fig:009 width=70% }

16. Проверила, что пользователь guest не может прочитать файл readfile.c. (рис. -@fig:010)

![пункт 16](10.jpg){ #fig:010 width=70% }

17. Сменила у программы readfile владельца и установила SetU’D-бит.
18. Проверила, может ли программа readfile прочитать файл readfile.c (рис. -@fig:011)

![пункты 17-18](11.jpg){ #fig:011 width=70% }
19. Проверила, может ли программа readfile прочитать файл /etc/shadow (рис. -@fig:012)

![пункт 19](12.jpg){ #fig:012 width=70% }

Так как владелец файла readfile root, а setuid являются флагами прав доступа в Unix, которые разрешают пользователям запускать исполняемые файлы с правами владельца исполняемого файла, то readfile смог прочитать и readfile.c и /etc/shadow

Исследование Sticky-бита

1. Выяснила, что установлен атрибут Sticky на директории /tmp
2. От имени пользователя guest создала файл file01.txt в директории /tmp
со словом test
3. Просмотрела атрибуты у только что созданного файла и разрешила чтение и запись для категории пользователей «все остальные»
(рис. -@fig:013)

![пункт 1-3](13.jpg){ #fig:013 width=70% }

4. От пользователя guest2 (не являющегося владельцем) попробовала прочитать файл /tmp/file01.txt
5. От пользователя guest2 не смогла дозаписать в файл.
6. Проверила содержимое файла
7. От пользователя guest2 записала в файл /tmp/file01.txt
слово test3, стерев при этом всю имеющуюся в файле информацию
8. Проверила содержимое файла
9. От пользователя guest2 не смогла удалить файл /tmp/file01.txt
10. Повысила свои права до суперпользователя следующей
и выполнила после этого команду, снимающую атрибут t (Sticky-бит) с
директории /tmp
11. Покинула режим суперпользователя
12. От пользователя guest2 проверила, что атрибута t у директории /tmp
нет (рис. -@fig:014)

![пункт 4-12](14.jpg){ #fig:014 width=70% }

13. Повторила предыдущие шаги.
14. Мне удалось удалить файл от имени пользователя, не являющегося
его владельцем
Благодаря Sticky bit пользователи могут создавать файлы, читать и выполнять их, принадлежащие другим пользователям, но не могут удалять файлы, принадлежащие другим пользователям, даже если в каталоге есть разрешение 777. Если sticky bit не установлен, то юзер может удалить файл, так как он наследует разрешения родительского каталога.
15. Повысила свои права до суперпользователя и вернула атрибут t на директорию /tmp (рис. -@fig:015)

![пункты 13-15](15.jpg){ #fig:015 width=70% }


# Выводы

В результате выполнения работы я Изучила механизмы изменения идентификаторов, применения
SetUID- и Sticky-битов. Получила практические навыки работы в консоли с дополнительными атрибутами. Рассмотрела работы механизма смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов
