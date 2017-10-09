hello i den följande tabellen beskrivs hello större kvoter, gränser, standarder och begränsningar i Azure Schemaläggaren.

| Resurs | Beskrivning av gränsen |
| --- | --- |
| **Jobbstorlek** |Maximala jobbstorlek är 16 kB. Om en PUT- eller KORRIGERINGSFIL resulterar i ett jobb som är större än dessa gränser, returnerade statuskoden 400 Felaktig begäran. |
| **Begäran om URL-storlek** |Maximal storlek för hello URL: en är 2048 tecken. |
| **Sammanställd huvudstorlek** |Maximal sammanställd huvudstorlek är 4096 tecken. |
| **Antal sidhuvud** |Maximal sidhuvud antalet är 50 huvuden. |
| **Textstorleken** |Maximal textstorleken är 8192 tecken. |
| **Återkommande intervall** |Största antal upprepningar är 18 månader. |
| **Tid toostart tid** |Största ”tid toostart time” är 18 månader. |
| **Jobbhistorik** |Maximal svarstexten lagras i jobbhistoriken är 2 048 byte. |
| **Frekvens** |Hej max frekvens standardkvot är 1 timme i en fri jobbsamling och 1 minut i en standard jobbsamlingen. Hej max replikeringsfrekvensen kan konfigureras på ett jobb samling toobe lägre än hello maximalt. Alla jobb i jobbsamlingen hello är begränsad hello-värde som anges i hello jobbsamlingen. Om du försöker toocreate ett jobb med en frekvens som är högre än hello maximala frekvensen på hello jobbsamlingen misslyckas begäran med statuskod 409 konflikt. |
| **Jobb** |Hej max jobb standardkvot är 5 jobb i en fri jobbsamling och 50 jobb i en standard jobbsamling. hello maximalt antal jobb kan konfigureras på en jobbsamling. Alla jobb i jobbsamlingen hello är begränsad hello-värde som anges i hello jobbsamlingen. Om du försöker toocreate misslyckas fler jobb än hello kvoten för maximal jobb och sedan hello begäran statuskod 409 konflikt. |
| **Jobbsamlingar** |Maximalt antal jobbsamlingen per prenumeration är 200 000. |
| **Kvarhållning av jobb historik** |Jobbhistorik sparas i in too2 månader eller in toohello senaste 1000 körningar. |
| **Kvarhållning av slutfördes och en felaktig jobb** |Slutfördes och en felaktig jobb finns kvar i 60 dagar. |
| **Timeout** |Det finns en statisk (kan inte konfigureras) begäran-timeout 60 sekunder för HTTP-åtgärder. För längre körs, följ asynkron HTTP-protokoll; till exempel returnera en 202 omedelbart men fortsätta arbeta i hello bakgrund. |

