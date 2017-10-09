
>[!NOTE]
>Log Analytics kallades tidigare för Operational Insights.
>
>

hello följande begränsningar gäller tooLog Analytics resurser per prenumeration:

| Resurs | Standardgräns | Kommentarer
| --- | --- | --- |
| Antal lediga arbetsytor per prenumeration | 10 | Den här gränsen kan inte höjas. |
| Antal betalda arbetsytor per prenumeration | Saknas | Du är begränsad hello antalet resurser inom en resursgrupp och antalet resursgrupper per prenumeration | 


hello följande begränsningar gäller tooeach logganalys-arbetsyta:

|  | Kostnadsfri | Standard | Premium | Fristående | OMS |
| --- | --- | --- | --- | --- | --- |
| Datavolym som samlas in per dag |500 MB<sup>1</sup> |Ingen |Ingen | Ingen | Ingen
| Datakvarhållningstid |7 dagar |1 månad |12 månader | 1 månad <sup>2</sup> | 1 månad <sup>2</sup>|

<sup>1</sup> när kunder nå sina 500 MB dagliga dataöverföringsgränsen, dataanalys stoppar och återupptar hello början av hello nästa dag. En dag baseras på UTC.

<sup>2</sup> hello Datalagringsperiod för hello fristående och OMS prissättningar kan vara bättre too730 dagar.

| Kategori | Begränsningar | Kommentarer
| --- | --- | --- |
| API för datainsamling | Maximal storlek för ett enskilt inlägg är 30 MB<br>Maximal storlek för fältvärden är 32 kB | Dela större volymer i flera olika inlägg<br>Fält som är längre än 32 kB trunkeras. |
| Sök-API | 5 000 poster returneras för ej sammanräknade data<br>500 000 poster för sammanräknade data | Sammanställda data är en sökning som omfattar hello `measure` kommando
 
