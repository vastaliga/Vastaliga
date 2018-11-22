---
layout: post
title: Teljes hónap lekérdezése
date: 2012-11-17  11:05:00
category: Dev
tags: [T-SQL]
author: Luc
lang: hu
ref: query-full-month
---
Ha egy táblából szeretnénk egy teljes hónap eseményeit megszerezni, de úgy, hogy az eseménytelen napok is kerüljenek kilistázásra, arra itt egy módszer: <br />
```sql
with nums (i) as ( select i = 0 union all select i + 1 from nums where i < 100)
  select a.dte, COUNT (d.[Date]) from 
   (select dte = dateadd(dd,nums.i,@StartDate) from nums) a
   join [valamilyentabla] d on
     DATEADD(DAY, 0, DATEDIFF(DAY, 0, d.[Date])) = DATEADD(DAY, 0, DATEDIFF(DAY, 0, a.dte))
     where a.dte <= @EndDate
     group by a.dte
     order by a.dte
```
Természetesen ez nem csak egyetlen hónapra alkalmas, illetve nem csak napokra, hanem akár órákra, vagy hónapokra is lehet bontani a kívánt időszakot. A JOIN feltételben ekkor nem DAY intervallumot kell használni, hanem azt, ami éppen tetszik, YEAR, MINUTE, akármi. <br />
A megoldás ötletét innen vettem: [http://www.sqlteam.com/](http://www.sqlteam.com/forums/topic.asp?TOPIC_ID=68067)