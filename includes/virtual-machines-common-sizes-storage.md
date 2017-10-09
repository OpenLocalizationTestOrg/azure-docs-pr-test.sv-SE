
hello Ls-serien är optimerad för arbetsbelastningar som kräver låg latens tillfällig lagring som NoSQL-databaser inklusive Cassandra, MongoDB, Cloudera och Redis. hello Ls-serien ger upp too32 vCPUs, med hjälp av hello [Intel Xeon® processor E5 v3-familjen](http://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html). hello Ls-serien hämtar hello samma CPU-prestanda som hello G-GS-serien och levereras med 8 GiB minne per vCPU.  

## <a name="ls-series"></a>Ls-serien

ACU: 180–240
 
| Storlek          | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt genomflöde för cachelagring och temporär lagring: IOPS / Mbit/s (cachestorlek i GiB) | Maximalt icke cachelagrat diskgenomflöde: IOPS / Mbit/s | Maximalt antal nätverkskort/förväntade nätverksprestanda (Mbit/s) | 
|---------------|-----------|-------------|--------------------------|----------------|-------------------------------------------------------------|-------------------------------------------|------------------------------| 
| Standard_L4s  | 4    | 32   | 678   | 8              | NA / NA (0)          | 5,000 / 125                               | 2/4 000       | 
| Standard_L8s  | 8    | 64   | 1,388 | 16             | NA / NA (0)          | 10,000 / 250                              | 4/8 000  | 
| Standard_L16s | 16   | 128  | 2,807 | 32             | NA / NA (0)          | 20,000 / 500                              | 8/6 000–16 000 &#8224; | 
| Standard_L32s* | 32 | 256  | 5,630 | 64             | NA / NA (0)          | 40,000 / 1,000                            | 8/20 000 | 
 

hello maximal disk-genomströmning möjligt med Ls-serien virtuella datorer kan vara begränsas av hello antal, storlek och striping av alla anslutna diskar. Mer information finns i [Premium Storage: Lagring med höga prestanda för arbetsbelastningar på virtuella datorer i Azure](../articles/storage/common/storage-premium-storage.md). 

* Instansen är isolerad toohardware dedikerade tooa kund.

