
<!-- A-series, Av2-series, D-series, Dv2-series, DS-series*, DSv2-series* -->

- hello A-series och Av2-serien virtuella datorer kan distribueras på olika maskinvarutyper av och processorer. hello storleken begränsas baserat på hello maskinvara, toooffer konsekvent processorprestanda för hello körs oavsett hello maskinvara som den har distribuerats på-instansen. toodetermine hello fysisk maskinvara som den här storleken är distribuerad, fråga hello virtuell maskinvara från inom hello virtuell dator.

- D-serien är utformad toorun program som kräver högre datorkraft och tillfällig diskprestanda. D-serien VMs ger snabbare processorer och minne till vCPU förhållandet SSD (SSD) för hello diskutrymme. Mer information finns i hello-meddelande på hello Azure blogg [nya storlekar för virtuella datorer av D-serien](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

- Dv2-serien, produkter toohello ursprungliga D-serien, har en kraftfullare processor. Hej Dv2-serien CPU är ungefär 35% snabbare än hello D-serien CPU. Den är baserad på hello senaste generationens 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) processor, och med hello Intel Turbo förstärkningen Technology 2.0, gå upp too3.1 GHz. Hej Dv2-serien har hello samma minne och konfigurationer som hello D-serien.

- hello grundläggande nivåstorlekarna är främst för arbetsbelastningar för utveckling och andra program som inte kräver belastningsutjämning, automatisk skalning eller minnesintensiva virtuella datorer. Information om VM-storlekar som passar bättre för produktionsprogram finns i (Storlekar för virtuella datorer)[virtual-machines-size-specs.md]. Prisinformation för virtuella datorer finns i [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/) (Priser för virtuella datorer).

## <a name="dsv3-series"></a>Dsv3-serien

ACU: 160–190

Dsv3-serien storlekar baseras på hello 2,3 GHz Intel XEON® E5-2673 v4-processor (Broadwell) och uppnå 3.5GHz med Intel Turbo förstärkningen Technology 2.0 och använda premium-lagring. Hej Dsv3-serien storlekar erbjuder en kombination av vCPU, minne och tillfällig lagring för de flesta produktionsarbetsbelastningar.


| Storlek             | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_D2s_v3  | 2      | 8           | 16             | 4              | 4 000/32 (50)                                                       | 3,200 / 48                                | 2 / måttlig                                   |
| Standard_D4s_v3  | 4      | 16          | 32             | 8              | 8 000/64 (100)                                                      | 6,400 / 96                                | 2 / måttlig                                   |
| Standard_D8s_v3  | 8      | 32          | 64             | 16             | 16 000/128 (200)                                                    | 12,800 / 192                              | 4 / hög                                       |
| Standard_D16s_v3 | 16     | 64          | 128            | 32             | 32 000/256 (400)                                                    | 25,600 / 384                              | 8 / hög                                       |


## <a name="dv3-series"></a>Dv3-serien

ACU: 160–190

Dv3-serien storlekar baseras på hello 2,3 GHz Intel XEON® E5-2673 v4 processor (Broadwell) och kan uppnå 3.5GHz med Intel Turbo förstärkningen Technology 2.0. Hej Dv3-serien storlekar erbjuder en kombination av vCPU, minne och tillfällig lagring för de flesta produktionsarbetsbelastningar.

Datadisklagring faktureras separat från virtuella datorer. toouse för lagring premiumdiskar använda hello Dsv3. hello priser och fakturering mätare för Dsv3 storlekar är hello samma som Dv3-serien. 


| Storlek            | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal nätverkskort/nätverksbandbredd |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_D2_v3  | 2         | 8           | 50             | 4              | 3 000/46/23                                               | 2 / måttlig                 |
| Standard_D4_v3  | 4         | 16          | 100            | 8              | 6 000/93/46                                               | 2 / måttlig                 |
| Standard_D8_v3  | 8         | 32          | 200            | 16             | 12 000/187/93                                             | 4 / hög                     |
| Standard_D16_v3 | 16        | 64          | 400            | 32             | 24 000/375/187                                            | 8 / hög                     |


## <a name="dsv2-series"></a>DSv2-serien

ACU: 210–250

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1_v2 |1 |3.5 |7 |2 |4,000 / 32 (43) |3,200 / 48 |2/750 |
| Standard_DS2_v2 |2 |7 |14 |4 |8,000 / 64 (86) |6,400 / 96 |2/1 500 |
| Standard_DS3_v2 |4 |14 |28 |8 |16,000 / 128 (172) |12,800 / 192 |4/3 000 |
| Standard_DS4_v2 |8 |28 |56 |16 |32,000 / 256 (344) |25,600 / 384 |8/6 000 |
| Standard_DS5_v2 |16 |56 |112 |32 |64,000 / 512 (688) |51,200 / 768 |8/6 000–12 000 &#8224;|



## <a name="dv2-series"></a>Dv2-serien

ACU: 210–250

| Storlek              | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal datadiskar/dataflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1_v2    | 1         | 3.5         | 50             | 3 000 / 46 / 23                                           | 2 / 2 x 500                         | 2/750                 |
| Standard_D2_v2    | 2         | 7           | 100            | 6 000 / 93 / 46                                           | 4 / 4 x 500                         | 2/1 500                     |
| Standard_D3_v2    | 4         | 14          | 200            | 12 000 / 187 / 93                                         | 8 / 8 x 500                         | 4/3 000                     |
| Standard_D4_v2    | 8         | 28          | 400            | 24 000 / 375 / 187                                        | 16 / 16 x 500                       | 8/6 000                     |
| Standard_D5_v2    | 16        | 56          | 800            | 48 000 / 750 / 375                                        | 32 / 32 x 500                       | 8/6 000–12 000 &#8224;          |


<br>

## <a name="ds-series"></a>DS-serien

ACU: 160

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS1 |1 |3.5 |7 |2 |4,000 / 32 (43) |3,200 / 32 |2/500 |
| Standard_DS2 |2 |7 |14 |4 |8,000 / 64 (86) |6,400 / 64 |2/1 000 |
| Standard_DS3 |4 |14 |28 |8 |16,000 / 128 (172) |12,800 / 128 |4/2 000 |
| Standard_DS4 |8 |28 |56 |16 |32,000 / 256 (344) |25,600 / 256 |8/4 000 |

<br>

## <a name="d-series"></a>D-serien 

ACU: 160

| Storlek         | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal datadiskar/dataflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D1  | 1         | 3.5         | 50             | 3 000 / 46 / 23                                           | 2 / 2 x 500                         | 2/500                 |
| Standard_D2  | 2         | 7           | 100            | 6 000 / 93 / 46                                           | 4 / 4 x 500                         | 2/1 000                     |
| Standard_D3  | 4         | 14          | 200            | 12 000 / 187 / 93                                         | 8 / 8 x 500                         | 4/2 000                     |
| Standard_D4  | 8         | 28          | 400            | 24 000 / 375 / 187                                        | 16 / 16 x 500                       | 8/4 000                     |

<br>


## <a name="av2-series"></a>Av2-serien

ACU: 100

| Storlek            | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal datadiskar/dataflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) | 
|-----------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_A1_v2  | 1         | 2           | 10             | 1 000 / 20 / 10                                           | 2 / 2 x 500               | 2/250                 |
| Standard_A2_v2  | 2         | 4           | 20             | 2 000 / 40 / 20                                           | 4 / 4 x 500               | 2/500                 |
| Standard_A4_v2  | 4         | 8           | 40             | 4 000 / 80 / 40                                           | 8 / 8 x 500               | 4/1 000                     |
| Standard_A8_v2  | 8         | 16          | 80             | 8 000 / 160 / 80                                          | 16 / 16 x 500             | 8/2 000                     |
| Standard_A2m_v2 | 2         | 16          | 20             | 2 000 / 40 / 20                                           | 4 / 4 x 500               | 2/500                 |
| Standard_A4m_v2 | 4         | 32          | 40             | 4 000 / 80 / 40                                           | 8 / 8 x 500               | 4/1 000                     |
| Standard_A8m_v2 | 8         | 64          | 80             | 8 000 / 160 / 80                                          | 16 / 16 x 500             | 8/2 000                     |

<br>

## <a name="a-series"></a>A-serien

ACU: 50–100

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (HDD): GiB | Maximalt antal datadiskar | Maximalt diskgenomflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s)  |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0* |1 |0.768 |20 |1 |1 × 500 |2/100 |
| Standard_A1 |1 |1.75 |70 |2 |2 × 500 |2/500  |
| Standard_A2 |2 |3.5 |135 |4 |4 × 500 |2/500 |
| Standard_A3 |4 |7 |285 |8 |8 × 500 |2/1 000 |
| Standard_A4 |8 |14 |605 |16 |16 × 500 |4/2 000 |
| Standard_A5 |2 |14 |135 |4 |4 × 500 |2/500 |
| Standard_A6 |4 |28 |285 |8 |8 × 500 |2/1 000 |
| Standard_A7 |8 |56 |605 |16 |16 × 500 |4/2 000 |
<br>

* hello A0 storlek är över prenumererade på hello fysisk maskinvara. För den här specifika storleken kan andra kunddistributioner påverka hello prestandan för din arbetsbelastning. nedan beskrivs hello relativa prestanda som hello förväntades baslinje, ämne tooan ungefärliga variationen 15 procent.

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0–A4 med CLI och PowerShell
I hello klassiska distributionsmodellen är vissa namn för VM-storlek något annorlunda i CLI och PowerShell:

* Standard_A0 är ExtraSmall 
* Standard_A1 är Small
* Standard_A2 är Medium
* Standard_A3 är Large
* Standard_A4 är ExtraLarge

## <a name="basic-a"></a>Basic A

|Storlek – Storlek\namn | Virtuell processor |Minne|Nätverkskort (max.)|Högsta temporär diskstorlek |Max. datadiskar (1 023 GB var)|Max. IOPS (300 per disk)|
|---|---|---|---|---|---|---|
|A0\Basic_A0|1|768 MB|2| 20 GB|1|1 × 300|
|A1\Basic_A1|1|1,75 GB|2| 40 GB |2|2 × 300|
|A2\Basic_A2|2|3,5 GB|2| 60 GB|4|4 × 300|
|A3\Basic_A3|4|7 GB|2| 120 GB |8|8 × 300|
|A4\Basic_A4|8|14 GB|2| 240 GB |16|16 × 300|
