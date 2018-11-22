---
layout: post
title: Teljes hónap lekérdezése
date: 2012-11-17  11:05:00
category: Dev
tags: [T-SQL]
---
Ha egy táblából szeretnénk egy teljes hónap eseményeit megszerezni, de úgy, hogy az eseménytelen napok is kerüljenek kilistázásra, arra itt egy módszer: <br><pre>with nums (i) as ( select i = 0 union all select i + 1 from nums where i &lt; 100 )<br>&nbsp;&nbsp; &nbsp;select a.dte, COUNT (d.[Date])<br>&nbsp;&nbsp; &nbsp;from <br>&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;(select dte = dateadd(dd,nums.i,@StartDate) from nums) a <br>&nbsp;&nbsp; &nbsp;left&nbsp; join <br>&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;[valamilyentabla] d <br>&nbsp;&nbsp; &nbsp;on <br>&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;DATEADD(DAY, 0, DATEDIFF(DAY, 0, d.[Date])) = DATEADD(DAY, 0, DATEDIFF(DAY, 0, a.dte))<br>&nbsp;&nbsp; &nbsp;where a.dte &lt;= @EndDate<br>&nbsp;&nbsp; &nbsp;group by a.dte<br>&nbsp;&nbsp; &nbsp;order by a.dte</pre>Természetesen ez nem csak egyetlen hónapra alkalmas, illetve nem csak napokra, hanem akár órákra, vagy hónapokra is lehet bontani a kívánt időszakot. A JOIN feltételben ekkor nem DAY intervallumot kell használni, hanem azt, ami éppen tetszik, YEAR, MINUTE, akármi. <br>A megoldás ötletét innen vettem: <a href="http://www.sqlteam.com/forums/topic.asp?TOPIC_ID=68067">http://www.sqlteam.com/</a> <br>