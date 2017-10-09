
## <a name="size-table-definitions"></a>Definitioner för storlekstabellen

- Lagringskapaciteten visas i GiB, eller 1 024^3 byte. När jämföra diskar mätt i GB (1000 ^ 3 byte) toodisks mätt i GiB (1024 ^ 3) Kom ihåg att kapacitet siffrorna i GiB visas mindre. Exempel: 1 023 GiB = 1 098,4 GB
- Diskgenomflödet mäts i indata-/utdataåtgärder per sekund (IOPS) och Mbit/s där Mbit/s = 10^6 byte/sek.
- Datadiskar kan köras i cachelagrat eller icke cachelagrat läge. För cachelagrade data disk åtgärden hello värden cacheläge har angetts för**ReadOnly** eller **ReadWrite**.  För cachelagrade data disk åtgärden hello värden cacheläge har angetts för**ingen**.
- **Förväntat nätverksprestanda** är hello högsta sammanlagda bandbredden fördelas per VM typen på alla nätverkskort för alla mål. Övre gräns är inte garanterat, men är avsedda tooprovide riktlinjer för att välja hello rätt VM-typ för hello avsedd program. Faktiska nätverksprestanda beror på flera faktorer, t.ex. nätverksbelastning, programinläsningar och nätverksinställningar. Information om hur du optimerar dataflödet i nätverket finns i [Optimizing network throughput for Windows and Linux](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md) (Optimera nätverksgenomflödet för Windows och Linux). tooachieve hello förväntades nätverksprestanda i Linux eller Windows, den kan vara nödvändiga tooselect en specifik version eller optimera din virtuella dator. Mer information finns i [hur tooreliably testa för virtuell dator genomströmning](../articles/virtual-network/virtual-network-bandwidth-testing.md).

- &#8224; 16 vCPU prestanda når konsekvent hello övre gräns i en kommande version.


