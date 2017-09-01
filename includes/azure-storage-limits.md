| Resurs | Standardgräns |
| --- | --- |
| Antal lagringskonton per prenumeration |200<sup>1</sup> |
| Maximal kapacitet för lagringskonton |500 TB<sup>2</sup> |
| Högsta antal blob-behållare, blobbar, filresurser, tabeller, köer, entiteter eller meddelanden per storage-konto |Obegränsat |
| Maxstorlek på en enda blob-behållare, en tabell eller en kö |Samma som den maximala lagringskapaciteten för kontot |
| Max antal block i en blockblobb eller lägga till blob |50,000 |
| Maxstorlek på ett block i en blockblobb |100 MB |
| Maxstorlek på en blockblob |50 000 x 100 MB (uppskattat 4,75 TB) |
| Maxstorlek på ett block i en tilläggsblobb |4 MB |
| Maxstorlek på en tilläggsblobb |50 000 x 4 MB (uppskattat 195 GB) |
| Maxstorlek på en sidblob |8 TB |
| Maxstorlek på en tabell entitet |1 MB |
| Max antal egenskaper i en tabell entitet |252 |
| Maxstorlek på ett meddelande i en kö |64 kB |
| Maxstorlek på en filresurs |5 TB |
| Maxstorlek på en fil i en filresurs |1 TB |
| Högsta antal filer i en filresurs |Endast gränsen är 5 TB total kapacitet för filresursen |
| Max IOPS per resurs |1000 |
| Högsta antal filer i en filresurs |Endast gränsen är 5 TB total kapacitet för filresursen |
| Högsta antal lagrade åtkomstprinciper per behållare, filresurs, tabell eller kön |5 |
| Högsta överföringshastighet per lagringskonto som är begäran |BLOB: 20 000 begäranden per sekund<sup>2</sup> för BLOB för någon giltig storlek (begränsat av kontots ingång-/ utgång gränser) <br />Filer: 1000 IOPS (8 KB) per filresurs <br />Köer: 20 000 meddelanden per sekund (meddelandestorlek antagande om 1 KB)<br />Tabeller: 20 000 transaktioner per sekund (antagande om 1 KB entitet storlek) |
| Mål-genomströmning för enda blob |Upp till 60 MB per sekund, eller upp till 500 begäranden per sekund |
| Mål-genomströmning för enskild kön (1 KB meddelanden) |Upp till 2000-meddelanden per sekund |
| Mål-genomströmning för tabell partition (1 KB-enheter) |Upp till 2000 enheter per sekund |
| Mål-genomströmning för en filresurs |Upp till 60 MB per sekund |
| Max ingång<sup>3</sup> per lagringskonto (oss regioner) |10 Gbit/s om GRS/ZRS<sup>4</sup> aktiverad, 20 Gbit/s för LRS<sup>2</sup> |
| Maximalt antal utgående<sup>3</sup> per lagringskonto (oss regioner) |20 Gbit/s om RA-GRS/GRS/ZRS<sup>4</sup> aktiverad, 30 Gbit/s för LRS<sup>2</sup> |
| Max ingång<sup>3</sup> per lagringskonto (icke-amerikansk regioner) |5 Gbit/s om GRS/ZRS<sup>4</sup> aktiverad, 10 Gbit/s för LRS<sup>2</sup> |
| Maximalt antal utgående<sup>3</sup> per lagringskonto (icke-amerikansk regioner) |10 Gbit/s om RA-GRS/GRS/ZRS<sup>4</sup> aktiverad, 15 Gbit/s för LRS<sup>2</sup> |

<sup>1</sup>detta inkluderar både Standard- och Premium storage-konton. Om du behöver mer än 200 lagringskonton skickar du en begäran via [Azure-supporten](https://azure.microsoft.com/support/faq/). Azure Storage-teamet granskar ditt affärsfall och kan godkänna upp till 250 lagringskonton. 

<sup>2</sup> för att få dina lagringskonton som standard att växa annonserade gränser i kapacitet, ingång-/ utgång och begäran som gör en begäran via [Azure-supporten](https://azure.microsoft.com/support/faq/). Azure Storage-teamet kommer granska förfrågningen och godkänna högre gränser på en fall till fall.

<sup>3</sup>*ingång* refererar till alla data (antal begäranden) som skickas till ett lagringskonto. *Utgående* syftar på alla data (svar) som tas emot från ett lagringskonto.  

<sup>4</sup>azure Storage-replikeringsalternativ inkluderar:
* **RA-GRS**: geo-redundant lagring med läsbehörighet. Utgående mål för den sekundära platsen är samma som för den primära platsen om RA-GRS är aktiverad.
* **GRS**: Geo-redundant lagring. 
* **ZRS**: zonredundant lagring. Endast tillgängligt för blockblobbar. 
* **LRS**: lokalt redundant lagring. 


