<!-- A-series - compute-intensive instances, H-series -->

hello A8 A11 och H-serien storlek kallas även *beräkningsintensiva instanser*. hello-maskinvara som kör dessa storlekar har utformats och optimerats för beräkningsintensiva och nätverks-intensiva program, inklusive högpresterande Datorsystem kluster program, modellering och simulering. hello A8 A11 serien använder Intel Xeon E5-2670 @ 2.6 GHZ och hello H-serien använder Intel Xeon E5-2667 v3 @ 3,2 GHz. 

Virtuella datorer i Azure H-serien är hello nästa generation med höga prestanda VMs syftar till att avancerad beräkningar behov som molekylär utformning och beräkningsprogram. Dessa 8 och 16 vCPU VMs bygger på hello Intel Haswell E5 2667 V3 processor technology med DDR4 minne och tillfällig lagring för SSD-baserad. 

Dessutom toohello betydande CPU-ström hello H-serien finns olika alternativ för låg latens RDMA nätverk med hjälp av FDR InfiniBand och flera konfigurationer toosupport minne beräkningsintensiva beräkningar minneskrav.



## <a name="h-series"></a>H-serien

ACU: 290–300

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (SSD) GiB | Maximalt antal datadiskar | Maximalt diskgenomflöde: IOPS | Maximalt antal nätverkskort |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_H8 |8 |56 |1000 |16 |16 × 500 |2  |
| Standard_H16 |16 |112 |2000 |32 |32 × 500 |4 |
| Standard_H8m |8 |112 |1000 |16 |16 × 500 |2  |
| Standard_H16m |16 |224 |2000 |32 |32 × 500 |4  |
| Standard_H16r* |16 |112 |2000 |32 |32 × 500 |4  |
| Standard_H16mr* |16 |224 |2000 |32 |32 × 500 |4 |

*För MPI-program används FDR InfiniBand-nätverk, som har extremt korta svarstider och hög bandbredd, för att upprätta dedikerade RDMA-servernätverk.

<br>



## <a name="a-series---compute-intensive-instances"></a>A-serien – beräkningsintensiva instanser

ACU: 225

| Storlek | Virtuell processor | Minne: GiB | Temporär lagring (HDD): GiB | Maximalt antal datadiskar | Maximalt diskgenomflöde: IOPS | Maximalt antal nätverkskort|
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A8* |8 |56 |382 |16 |16 × 500 |2 |
| Standard_A9* |16 |112 |382 |16 |16 × 500 |4 |
| Standard_A10 |8 |56 |382 |16 |16 × 500 |2  |
| Standard_A11 |16 |112 |382 |16 |16 × 500 |4 |

*För MPI-program används FDR InfiniBand-nätverk, som har extremt korta svarstider och hög bandbredd, för att upprätta dedikerade RDMA-servernätverk.

<br>



