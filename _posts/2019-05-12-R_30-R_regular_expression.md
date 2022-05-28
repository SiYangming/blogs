---
layout: post
title: "R语言30-R语法扩展: 正则表达式"
date:   2019-05-12
tags: [R语言]
comments: true
author: Si Yangming
toc : true
---

## 正则表达式

*   正则表达式可以被认为是普通字符常量和*元字符*的组合
*   与自然语言相比，普通字符常量相当于语言中的字词，元字符就是定义词句的语法
*   正则表达式有丰富的元字符

## 普通字符常量

最简单的匹配只包含普通字符常量。 “nuclear” 会匹配以下行：

```source-gfm
Ooh. I just learned that to keep myself alive after a nuclear blast! All I have to do is milk some rats then drink the milk. Aweosme. :}

Laozi says nuclear weapons are mas macho

Chaos in a country that has nuclear weapons -- not good.

my nephew is trying to teach me nuclear physics, or possibly just trying to show me how smart he is. so I’ll be proud of him [which I am].

lol if you ever say "nuclear" people immediately think DEATH by radiation LOL
```

* * *

 “Obama” 会匹配以下行：
```source-gfm
Politics r dum. Not 2 long ago Clinton was sayin Obama was crap n now she sez vote 4 him n unite? WTF? Screw em both + Mcain. Go Ron Paul!

Clinton conceeds to Obama but will her followers listen??

Are we sure Chelsea didn’t vote for Obama?

thinking ... Michelle Obama is terrific!

jetlag..no sleep...early mornig to starbux..Ms. Obama was moving
```

* * *

## 正则表达式

*   最简单的模式只包含普通字符常量，匹配文本中的任意位置包含该字符的文本
*   如果只想匹配单词 “Obama” 的时候，或者以 “Clinton”、“clinton”、“clinto”为结尾的句子

需要用一种方式来表达

*   词之间的空白边界
*   普通字符集
*   一行的开始和结束
*   可选字符 (如“war”或“peace”)

元字符可以解决这些问题

* * *

## 元字符

一些元字符代表一行的开始
```source-gfm
^i think
```

会匹配以下内容

```source-gfm
i think we all rule for participating
i think i have been outed
i think this will be quite fun actually
i think i need to go to work
i think i first saw zombo in 1999.
```

* * *

## 元字符

`$` 代表一行的末尾

```source-gfm
morning$
```

会匹配以下内容

```source-gfm
well they had something this morning
then had to catch a tram home in the morning
dog obedience school in the morning
and yes happy birthday i forgot to say it earlier this morning
I walked in the rain this morning
good morning
```

* * *

## 字符串类 []

通过设计一组可选的字符，来匹配单词

```source-gfm
[Bb][Uu][Ss][Hh]
```

会匹配以下行

```source-gfm
The democrats are playing, "Name the worst thing about Bush!"
I smelled the desert creosote bush, brownies, BBQ chicken
BBQ and bushwalking at Molonglo Gorge
Bush TOLD you that North Korea is part of the Axis of Evil
I’m listening to Bush - Hurricane (Album Version)
```

* * *

```source-gfm
^[Ii] am
```
会匹配

```source-gfm
i am so angry at my boyfriend i can’t even bear to look at him

i am boycotting the apple store

I am twittering from iPhone

I am a very vengeful person when you ruin my sweetheart.

I am so over this. I need food. Mmmm bacon...
```

* * *

类似的，可以设置一系列的字母 [a-z] 或 [a-zA-Z]；注意顺序无关紧要

```source-gfm
^[0-9][a-zA-Z]
```

会匹配以下行

```source-gfm
7th inning stretch
2nd half soon to begin. OSU did just win something
3am - cant sleep - too hot still.. :(
5ft 7 sent from heaven
1st sign of starvagtion
```

* * *

当在[]中使用 “^” 字符，表示进行逆向匹配

```source-gfm
[^?.]$
```

会匹配以下行

```source-gfm
i like basketballs
6 and 9
dont worry... we all die anyway!
Not in Baghdad
helicopter under water? hmmm
```

* * *

## 更多的元字符

“.” 用来匹配任意字符
```source-gfm
9.11
```

会匹配以下行

```source-gfm
its stupid the post 9-11 rules
if any 1 of us did 9/11 we would have been caught in days.
NetBios: scanning ip 203.169.114.66
Front Door 9:11:46 AM
Sings: 0118999881999119725...3 !
```

* * *



`|` 在正则表达式中不代表管道符，代表逻辑或，可以用来链接两个表达式，前后表达式都是可选项

```source-gfm
flood|fire
```

会匹配以下行

```source-gfm
is firewire like usb on none macs?
the global flood makes sense within the context of the bible
yeah ive had the fire on tonight
... and the floods, hurricanes, killer heatwaves, rednecks, gun nuts, etc.
￼
```

可以设置任意数量的可选项

```source-gfm
flood|earthquake|hurricane|coldfire
```
会匹配以下行

```source-gfm
Not a whole lot of hurricanes in the Arctic.
We do have earthquakes nearly every day somewhere in our State
hurricanes swirl in the other direction
coldfire is STRAIGHT!
’cause we keep getting earthquakes
```

也可以用于连接两个表达式

```source-gfm
^[Gg]ood|[Bb]ad
```
匹配以下行

```source-gfm
good to hear some good knews from someone here
Good afternoon fellow american infidels!
good on you-what do you drive?
Katie... guess they had bad experiences...
my middle name is trouble, Miss Bad News
```

* * *
`^` 与 `|` 字符联合使用时，需要加括号，否则只会匹配第一个表达式的值

```source-gfm
^([Gg]ood|[Bb]ad)
```

会匹配以下行

```source-gfm
bad habbit
bad coordination today
good, becuase there is nothing worse than a man in kinky underwear
Badcop, its because people want to use drugs
Good Monday Holiday
Good riddance to Limey
```

* * *
`?` 表示可选匹配，1个或0个

```source-gfm
[Gg]eorge( [Ww]\.)? [Bb]ush
```

会匹配以下行

```source-gfm
i bet i can spell better than you and george bush combined
BBC reported that President George W. Bush claimed God told him to invade I
a bird in the hand is worth two george bushes
```

* * *

## 需要注意的是
`.` 是一个元字符，如果相匹配它，需要通过`\`转义
```source-gfm
[Gg]eorge( [Ww]\.)? [Bb]ush
```

* * *

## `*` 和 `+`

`*` 和 `+`是用来显示重复的元字符，`*` 表示任意数量，包括0，`+`表示匹配至少一个

```source-gfm
(.*)
```

会匹配以下行

```source-gfm
anyone wanna chat? (24, m, germany)
hello, 20.m here... ( east area + drives + webcam )
(he means older men)
()
```

* * *
匹配两个数字之间任意长度的行
```source-gfm
[0-9]+ (.*)[0-9]+
```

会匹配以下行

```source-gfm
working as MP here 720 MP battallion, 42nd birgade
so say 2 or 3 years at colleage and 4 at uni makes us 23 when and if we fin
it went down on several occasions for like, 3 or 4 *days*
Mmmm its time 4 me 2 go 2 bed
```

* * *

##  元字符-`{ }`

`{ }` 代表一个区间，设置表达式匹配的最小和最大数量

```source-gfm
[Bb]ush( +[^ ]+ +){1,5} debate
```

会匹配以下行

```source-gfm
Bush has historically won all major debates he’s done.
in my view, Bush doesn’t need these debates..
bush doesn’t need the debates? maybe you are right
That’s what Bush supporters are doing about the debate.
Felix, I don’t disagree that Bush was poorly prepared for the debate.
indeed, but still, Bush should have taken the debate more seriously.
Keep repeating that Bush smirked and scowled during the debate
```
`{m,n}` 表示至少精确匹配m个，但不超过n个

* * *

## 元字符--`()`

*   正则表达式中，括号不仅仅限制可选表达式的范围，还可以用来记录匹配的文本
*   用类似\1, \2等表示

* * *

```source-gfm
+([a-zA-Z]+) +\1 +
```
会匹配以下行

```source-gfm
time for bed, night night twitter!
blah blah blah blah
my tattoo is so so itchy today
i was standing all all alone against the world outside...
hi anybody anybody at home
estudiando css css css css.... que desastritooooo
```

* * *


 `*` 是贪婪匹配，会匹配满足正则表达式的尽可能长的字符串

```source-gfm
^s(.*)s
```

匹配以下结果

```source-gfm
sitting at starbucks
setting up mysql and rails
studying stuff for the exams
spaghetti with marshmallows
stop fighting with crackers
sore shoulders, stupid ergonomics
```

* * *

 `*` 的贪婪匹配可以通过 `?` 关闭

```source-gfm
^s(.*?)s$
```

* * *

## 小结

*   正则表达式在许多不同的编程语言中使用，不只是R。
*   正则表达式由普通字符和元字符组成，代表一组或一类字符串或词句
*   通过正则表达式提取数据是一种很便捷的方式

课程分享
生信技能树全球公益巡讲
（https://mp.weixin.qq.com/s/E9ykuIbc-2Ja9HOY0bn_6g）
B站公益74小时生信工程师教学视频合辑
（https://mp.weixin.qq.com/s/IyFK7l_WBAiUgqQi8O7Hxw）
招学徒：
（https://mp.weixin.qq.com/s/KgbilzXnFjbKKunuw7NVfw）
