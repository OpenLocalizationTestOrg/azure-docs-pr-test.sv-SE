<!-- F-series, Fs-series* -->

F-serien är baserad på hello 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) processor, vilket kan uppnå högre klockfrekvens lika hög som 3.1 GHz med hello Intel Turbo förstärkningen Technology 2.0. Detta är hello samma CPU-prestanda som hello Dv2 antal virtuella datorer.  Till ett lägre per timme listan pris är hello F-serien hello bästa värde i pris prestanda i hello Azure portfölj baserat på hello Azure Compute-enhet (ACU) per vCPU. 

De virtuella datorerna i F-serien är ett utmärkt alternativ för arbetsbelastningar som kräver snabbare processorer, men som inte behöver så mycket minne eller temporär lagring per virtuell processor.  Arbetsbelastningar som till exempel analytics, spel-servrar, webbservrar och batchbearbetning drar nytta av hello värdet för hello F-serien.

hello Fs-serien ger alla hello fördelarna med hello F-serien i tillägg tooPremium lagring.

## <a name="fs-series"></a>Fs-serien*

ACU: 210–250

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_F1s |1 |2 |4 |2 |4,000 / 32 (12) |3,200 / 48 |2/750 |
| Standard_F2s |2 |4 |8 |4 |8,000 / 64 (24) |6,400 / 96 |2/1 500 |
| Standard_F4s |4 |8 |16 |8 |16,000 / 128 (48) |12,800 / 192 |4/3 000 |
| Standard_F8s |8 |16 |32 |16 |32,000 / 256 (96) |25,600 / 384 |8/6 000 |
| Standard_F16s |16 |32 |64 |32 |64,000 / 512 (192) |51,200 / 768 |8/6 000–12 000 &#8224; |

Mbit/s = 10^6 byte per sekund och GiB = 1 024^3 byte.

* hello maximal disk-genomströmning (IOPS eller Mbit/s) kan användas med en Fs serie VM kan begränsas av hello antal, storlek och striping av hello anslutna diskarna.  Mer information finns i [Premium Storage: Lagring med höga prestanda för arbetsbelastningar på virtuella datorer i Azure](../articles/storage/common/storage-premium-storage.md).


<br>

## <a name="f-series"></a>F-serien

ACU: 210–250

| Storlek         | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt genomflöde för temporär lagring: IOPS / Mbit/s för läsning / M/bit/s för skrivning | Maximalt antal datadiskar/dataflöde: IOPS | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) |
|--------------|-----------|-------------|----------------|----------------------------------------------------------|-----------------------------------|------------------------------|
| Standard_F1  | 1         | 2           | 16             | 3 000 / 46 / 23                                           | 2 / 2 x 500                         | 2/750                 |
| Standard_F2  | 2         | 4           | 32             | 6 000 / 93 / 46                                           | 4 / 4 x 500                         | 2/1 500                     |
| Standard_F4  | 4         | 8           | 64             | 12 000 / 187 / 93                                         | 8 / 8 x 500                         | 4/3 000                     |
| Standard_F8  | 8         | 16          | 128            | 24 000 / 375 / 187                                        | 16 / 16 x 500                       | 8/6 000                     |
| Standard_F16 | 16        | 32          | 256            | 48 000 / 750 / 375                                        | 32 / 32 x 500                       | 8/6 000–12 000 &#8224;           |


<br>


