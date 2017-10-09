En virtuell dator i Azure stöder tillkoppling av ett antal datadiskar. För optimala prestanda vill du toolimit hello antalet hög utnyttjade diskar kopplade toohello virtuella tooavoid möjliga begränsning. Om alla diskar som hög inte används vid hello samtidigt hello lagringskonto stöder ett större antal diskar.

* **För Azure Managed diskar:** gränsvärde för hanterade diskar är regionala och beror också på hello lagringstyp. Hej standard och även hello maxgränsen är 10 000 per prenumeration, per region och lagringstyp. Exempelvis kan du skapa too10, 000 standard hanteras diskarna och 10 000 premium hanterade diskar i en prenumeration och en region. 

    Hanterad ögonblicksbilder och bilder räknas mot hello begränsa för hanterade diskar.

* **För standardlagringskonton:** Ett standardlagringskonto har en högsta totala begärandefrekvens på 20 000 IOPS. hello får totala IOPS för alla virtuella diskar i ett standardlagringskonto inte överskrida den här gränsen.
  
    Ungefär kan du beräkna hello antalet hög utnyttjade diskar som stöds av ett enda standard storage-konto baserat på hello hastighetsbegränsning för begäran. Till exempel för grundnivån VM, hello maximalt antal hög utnyttjade diskar är ungefär 66 (20 000/300 IOPS per disk) och en Standard-nivån VM, är det ungefär 40 (20 000/500 IOPS per disk), som visas i hello nedan. 
* **För premiumlagringskonton:** Ett premiumlagringskonto har en högsta totala dataflödeshastighet på 50 Gbit/s. hello totala genomströmningen för alla Virtuella diskar får inte överskrida den här gränsen.

