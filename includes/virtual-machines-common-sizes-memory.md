
* hello M-serien erbjuder hello högsta vCPU antal (upp too128 vCPUs) och största minne (upp too2.0 TiB) på alla VM i hello molnet.  Serien är det perfekta valet för mycket stora databaser eller andra program som har nytta av många virtuella processorer och stora mängder minne.

* Dv2-serien, D-serien, G-serien, och hello DS/GS motsvarigheter lämpar sig för program som kräver snabbare vCPUs, bättre prestanda för tillfällig lagring, eller ha högre krav på minne.  De utgör en kraftfull kombination för många program i företagsklass.

* D-serien är utformad toorun program som kräver högre datorkraft och tillfällig diskprestanda. Virtuella datorer i D-serien erbjuder snabbare processorer, högre ”minne till virtuell processor”-förhållande och en Solid State-hårddisk (SSD) för temporär lagring. Mer information finns i hello-meddelande på hello Azure blogg [nya storlekar för virtuella datorer av D-serien](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

* Dv2-serien, produkter toohello ursprungliga D-serien, har en kraftfullare processor. Hej Dv2-serien CPU är ungefär 35% snabbare än hello D-serien CPU. Den är baserad på hello senaste generationens 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) processor, och med hello Intel Turbo förstärkningen Technology 2.0, gå upp too3.1 GHz. Hej Dv2-serien har hello samma minne och konfigurationer som hello D-serien.


## <a name="esv3-series"></a>ESv3-serien

ACU: 160–190

ESv3-serien instanser som är baserade på hello 2,3 GHz Intel XEON® E5-2673 v4-processor (Broadwell) och uppnå 3.5GHz med Intel Turbo förstärkningen Technology 2.0 och använda premium-lagring. Instanserna i Ev3-serien är idealiska för minnesintensiva företagsprogram.


| Storlek             | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|------------------|--------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------------------------|
| Standard_E2s_v3  | 2      | 16          | 32             | 4              | 4 000/32 (50)                                                       | 3,200 / 48                                | 2 / måttlig                                   |
| Standard_E4s_v3  | 4      | 32          | 64             | 8              | 8 000/64 (100)                                                      | 6,400 / 96                                | 2 / måttlig                                   |
| Standard_E8s_v3  | 8      | 64          | 128            | 16             | 16 000/128 (200)                                                    | 12,800 / 192                              | 4 / hög                                       |
| Standard_E16s_v3 | 16     | 128         | 256            | 32             | 32 000/256 (400)                                                    | 25,600 / 384                              | 8 / hög                                       |
| Standard_E32s_v3 | 32     | 256         | 512            | 32             | 64 000/512 (800)                                                    | 51,200 / 768                              | 8 / extremt hög                             |
| Standard_E64s_v3 | 64     | 432         | 864            | 32             | 128 000/1024 (1 600)                                                   | 80 000/1 200                             | 8 / extremt hög                             |



## <a name="ev3-series"></a>Ev3-serien

ACU: 160–190 

Ev3-serien instanser som är baserade på hello 2,3 GHz Intel XEON® E5-2673 v4 processor (Broadwell) och kan uppnå 3.5GHz med Intel Turbo förstärkningen Technology 2.0. Instanserna i Ev3-serien är idealiska för minnesintensiva företagsprogram.

Datadisklagring faktureras separat från virtuella datorer. toouse för lagring premiumdiskar använda hello ESv3. hello priser och fakturering mätare för ESv3 storlekar är hello samma som Ev3-serien. 


| Storlek            | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal nätverkskort/nätverksbandbredd |
|-----------------|-----------|-------------|----------------|----------------|----------------------------------------------------------|------------------------------|
| Standard_E2_v3  | 2         | 16          | 50             | 4              | 3 000/46/23                                               | 2 / måttlig                 |
| Standard_E4_v3  | 4         | 32          | 100            | 8              | 6 000/93/46                                               | 2 / måttlig                 |
| Standard_E8_v3  | 8         | 64          | 200            | 16             | 12 000/187/93                                             | 4 / hög                     |
| Standard_E16_v3 | 16        | 128         | 400            | 32             | 24 000/375/187                                            | 8 / hög                     |
| Standard_E32_v3 | 32        | 256         | 800            | 32             | 48 000/750/375                                            | 8 / extremt hög           |
| Standard_E64_v3 | 64        | 432         | 1600           | 32             | 96 000/1 000/500                                           | 8 / extremt hög           |


## <a name="m-series"></a>M-serien*

ACU: 160–180

| Storlek            | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|-----------------|------|-------------|----------------|----------------|-----------------------------------------------------------------------|-------------------------------------------|------------------------------|
| Standard_M64ms  | 64   | 1792        | 2048           | 32             | 80 000/800 (6 348)       | 40,000 / 1,000                            | 8/16 000          |
| Standard_M128s** | 128  | 2048        | 4096           | 64             | 160 000/1 600 (12 696) | 80,000 / 2,000                            | 8/25 000          |



*Virtuella datorer i M-serien är utrustade med Intel® Hyper-Threading Technology

**Fler än 64 virtuella processorer kräver något av följande gästoperativsystem som stöds: Windows Server 2016, Ubuntu 16.04 LTS, SLES 12 SP2 och Red Hat Enterprise Linux eller CentOS 7.3 med LIS 4.2.1 

<br>

## <a name="gs-series"></a>GS-serien*

ACU: 180–240

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_GS1 |2 |28 |56 |4 |10,000 / 100 (264) |5,000 / 125 |2/2 000 |
| Standard_GS2 |4 |56 |112 |8 |20,000 / 200 (528) |10,000 / 250 |2/4 000 |
| Standard_GS3 |8 |112 |224 |16 |40,000 / 400 (1,056) |20,000 / 500 |4/8 000 |
| Standard_GS4 |16 |224 |448 |32 |80,000 / 800 (2,112) |40,000 / 1,000 |8/6 000–16 000 &#8224; |
| Standard_GS5** |32 |448 |896 |64 |160,000 / 1,600 (4,224) |80,000 / 2,000 |8/20 000 |

* hello maximal disk-genomströmning (IOPS eller Mbit/s) kan användas med GS-serien VM kan begränsas av hello antal, storlek och striping av hello anslutna diskarna. Mer information finns i [Premium Storage: Lagring med höga prestanda för arbetsbelastningar på virtuella datorer i Azure](../articles/storage/common/storage-premium-storage.md). 

** Instans är isolerad toohardware dedikerade tooa kund.


<br>

## <a name="g-series"></a>G-serien

ACU: 180–240

| Storlek         | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal datadiskar/dataflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_G1  | 2         | 28          | 384            | 6 000 / 93 / 46                                           | 4 / 4 x 500                       | 2/2 000                     |
| Standard_G2  | 4         | 56          | 768            | 12 000 / 187 / 93                                         | 8 / 8 x 500                       | 2/4 000                     |
| Standard_G3  | 8         | 112         | 1,536          | 24 000 / 375 / 187                                        | 16 / 16 x 500                     | 4/8 000                |
| Standard_G4  | 16        | 224         | 3,072          | 48 000 / 750 / 375                                        | 32 / 32 x 500                     | 8/6 000–16 000 &#8224;          |
| Standard_G5* | 32        | 448         | 6,144          | 96 000 / 1500 / 750                                       | 64 / 64 x 500                     | 8/20 000           |

* Instansen är isolerad toohardware dedikerade tooa kund.
<br>


## <a name="dsv2-series"></a>DSv2-serien*

ACU: 210–250

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11_v2 |2 |14 |28 |4 |8,000 / 64 (72) |6,400 / 96 |2/1 500 |
| Standard_DS12_v2 |4 |28 |56 |8 |16,000 / 128 (144) |12,800 / 192 |4/3 000 |
| Standard_DS13_v2 |8 |56 |112 |16 |32,000 / 256 (288) |25,600 / 384 |8/6 000 |
| Standard_DS14_v2 |16 |112 |224 |32 |64,000 / 512 (576) |51,200 / 768 |8/6 000–12 000 &#8224; |
| Standard_DS15_v2** |20 |140 |280 |40 |80,000 / 640 (720) |64,000 / 960 |8/20 000***

* hello maximal disk-genomströmning (IOPS eller Mbit/s) kan användas med DSv2-serien VM kan begränsas av hello antal, storlek och striping av hello anslutna diskarna.  Mer information finns i [Premium Storage: Lagring med höga prestanda för arbetsbelastningar på virtuella datorer i Azure](../articles/storage/common/storage-premium-storage.md).

** Instans är en isolerad nod att garanterar att den virtuella datorn är hello virtuell dator på våra Intel Haswell-nod.

***25 000 Mbit/s med Accelererat nätverk.

<br>

## <a name="dv2-series"></a>Dv2-serien

ACU: 210–250

| Storlek              | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal datadiskar/dataflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|-------------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11_v2   | 2         | 14          | 100            | 6 000 / 93 / 46                                           | 4 / 4 x 500                         | 2/1 500                     |
| Standard_D12_v2   | 4         | 28          | 200            | 12 000 / 187 / 93                                         | 8 / 8 x 500                         | 4/3 000                     |
| Standard_D13_v2   | 8         | 56          | 400            | 24 000 / 375 / 187                                        | 16 / 16 x 500                       | 8/6 000                     |
| Standard_D14_v2   | 16        | 112         | 800            | 48 000 / 750 / 375                                        | 32 / 32 x 500                       | 8/6 000–12 000 &#8224;          |
| Standard_D15_v2* | 20        | 140         | 1,000          | 60 000 / 937 / 468                                        | 40 / 40 x 500                       | 8/20 000** |

* Instans är en isolerad nod att garanterar att den virtuella datorn är hello virtuell dator på våra Intel Haswell-nod.

**25 000 Mbit/s med Accelererat nätverk.

<br>

## <a name="ds-series"></a>DS-serien*

ACU: 160

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS11 |2 |14 |28 |4 |8,000 / 64 (72) |6,400 / 64 |2/1 000 |
| Standard_DS12 |4 |28 |56 |8 |16,000 / 128 (144) |12,800 / 128 |4/2 000 |
| Standard_DS13 |8 |56 |112 |16 |32,000 / 256 (288) |25,600 / 256 |8/4 000 |
| Standard_DS14 |16 |112 |224 |32 |64,000 / 512 (576) |51,200 / 512 |8/6 000–8 000 &#8224; |

* hello maximal disk-genomströmning (IOPS eller Mbit/s) kan användas med en DS-serien VM kan begränsas av hello antal, storlek och striping av hello anslutna diskarna.  Mer information finns i [Premium Storage: Lagring med höga prestanda för arbetsbelastningar på virtuella datorer i Azure](../articles/storage/common/storage-premium-storage.md).


## <a name="d-series"></a>D-serien

ACU: 160

| Storlek         | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal datadiskar/dataflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_D11 | 2         | 14          | 100            | 6 000 / 93 / 46                                           | 4 / 4 x 500                         | 2/1 000                     |
| Standard_D12 | 4         | 28          | 200            | 12 000 / 187 / 93                                         | 8 / 8 x 500                         | 4/2 000                     |
| Standard_D13 | 8         | 56          | 400            | 24 000 / 375 / 187                                        | 16 / 16 x 500                       | 8/4 000                     |
| Standard_D14 | 16        | 112         | 800            | 48 000 / 750 / 375                                        | 32 / 32 x 500                       | 8/6 000–8 000 &#8224;                |

<br>

