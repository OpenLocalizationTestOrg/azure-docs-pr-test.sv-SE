<!--author=alkohli last changed: 12/15/15-->

| Gräns för identifierare | Gräns | Kommentarer |
| --- | --- | --- |
| Maximalt antal lagringskontouppgifter |64 | |
| Maximalt antal volymbehållare |64 | |
| Maximalt antal volymer |255 | |
| Maximalt antal scheman per bandbreddsmall |168 |Ett schema för varje timme, varje dag i veckan hello (24 * 7). |
| Maximal storlek för en nivåindelad volym på fysiska enheter |64 TB för 8100 och 8600 |8100 och 8600 är fysiska enheter. |
| Maximal storlek för en nivåindelad volym på virtuella enheter i Azure |30 TB för 8010 <br></br> 64 TB för 8020 |8010 och 8020 är virtuella enheter i Azure som använder standardlagring respektive Premium-lagring. |
| Maximal storlek för en lokalt Fäst volym på fysiska enheter |9 TB för 8100 <br></br> 24 TB för 8600 |8100 och 8600 är fysiska enheter. |
| Maximalt antal iSCSI-anslutningar |512 | |
| Maximalt antal iSCSI klientanslutningar från initierarna |512 | |
| Högsta antal access control poster per enhet |64 | |
| Maximalt antal volymer per princip för säkerhetskopiering |24 | |
| Maximalt antal säkerhetskopior behålls per princip för säkerhetskopiering |64 | |
| Maximalt antal scheman per princip för säkerhetskopiering |10 | |
| Maximalt antal ögonblicksbilder av typer som kan behållas per volym |256 |Detta inkluderar lokala ögonblicksbilder och molnbaserade ögonblicksbilder. |
| Maximalt antal ögonblicksbilder som kan finnas i valfri enhet |10 000 | |
| Maximalt antal volymer som kan bearbetas parallellt för säkerhetskopiering, återställning eller klona |16 |<ul><li>Om det finns fler än 16 volymer kan bearbetas de sekventiellt när bearbetningen fack blir tillgängliga.</li><li>Nya säkerhetskopior av en klonade eller en återställd nivåindelad volym kan inte ske förrän hello-åtgärden har slutförts. Men för en lokal volym är säkerhetskopior tillåtna när hello volymen är online.</li></ul> |
| Återställa och klona Återställ tid för nivåindelade volymer |< 2 minuter |<ul><li>hello volym görs tillgänglig inom 2 minuter för återställning eller klona igen, oavsett hello volymens storlek.</li><li>hello gå volymen först långsammare än normalt som de flesta hello data och metadata finns fortfarande i hello molnet. Prestanda kan öka som data flödar från hello molnet toohello StorSimple-enhet.</li><li>hello total tid toodownload metadata är beroende av hello allokerade volymens storlek. Metadata hämtas automatiskt till hello-enhet i hello bakgrunden i hello frekvens på 5 minuter per TB allokerade volymdata. Detta värde kan påverkas av Internet bandbredd toohello moln.</li><li>Hej återställning eller kopieringen är klar när alla hello metadata är på hello enhet.</li><li>Säkerhetskopieringsåtgärder kan inte utföras förrän hello återställning eller kopieringen är helt klar. |
| Återställning återställa tid för lokalt fästa volymer |< 2 minuter |<ul><li>hello volym görs tillgänglig inom 2 minuter efter hello återställningen oavsett hello volymens storlek.</li><li>hello gå volymen först långsammare än normalt som de flesta hello data och metadata finns fortfarande i hello molnet. Prestanda kan öka som data flödar från hello molnet toohello StorSimple-enhet.</li><li>hello total tid toodownload metadata är beroende av hello allokerade volymens storlek. Metadata hämtas automatiskt till hello-enhet i hello bakgrunden i hello frekvens på 5 minuter per TB allokerade volymdata. Detta värde kan påverkas av Internet bandbredd toohello moln.</li><li>Till skillnad från nivåindelade volymer hello gäller lokalt fästa volymer hämtas även hello volymdata lokalt på hello enhet. hello återställningen är klar när alla hello volymdata har trätt toohello enhet.</li><li>hello återställningsåtgärderna kan bli långa och hello total tid toocomplete hello återställning beror på hello storleken på hello etablerats lokal volym och din Internet-bandbredd och hello befintliga data på hello enhet. Säkerhetskopieringsåtgärder på hello lokalt Fäst volym är tillåtna när hello återställningen pågår. |
| Tunn-återställning tillgänglighet |Senaste redundans | |
| Maximalt antal klienter läsning och skrivning genomströmning (när hanteras från hello SSD-nivån) * |920/720 MB/s med en enda 10GbE nätverksgränssnittet |Konfigurera too2x med MPIO och två nätverksgränssnitt. |
| Maximalt antal klienter läsning och skrivning genomströmning (när hanteras från hello hårddisknivå) * |120/250 MB/s | |
| Maximalt antal klienter läsning och skrivning genomströmning (när hanteras från hello molnnivån) * |11/41 MB/s |Läs genomströmning beroende klienter skapa och upprätthålla tillräcklig i/o-ködjup. |

&#42; Maximalt dataflöde per i/o-typ har mätt med 100 procent Läs- och 100 procent skrivåtgärder scenarier. Faktiska genomflöde kan vara lägre och beror på i/o blanda och nätverk villkor.

