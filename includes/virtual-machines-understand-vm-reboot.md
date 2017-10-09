Virtuella Azure-datorer (VM) kan ibland startas utan någon uppenbar anledning utan din har initierat hello omstart igen. Den här artikeln visar hello åtgärder och händelser som kan orsaka tooreboot för virtuella datorer och ger inblick i hur tooavoid oväntat startar om problem eller minska hello påverkan av sådana problem.

## <a name="configure-hello-vms-for-high-availability"></a>Konfigurera hello virtuella datorer för hög tillgänglighet
hello bästa sätt tooprotect ett program som körs på Azure mot VM startar om och avbrottstid är tooconfigure hello virtuella datorer för hög tillgänglighet.

den här nivån av redundans tooyour programmet tooprovide, rekommenderar vi att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning. Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator är tillgänglig och uppfyller hello 99,95% [Azure-serviceavtalet](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Mer information om tillgänglighetsuppsättningar finns hello följande artiklar:

- [Hantera hello tillgängligheten för virtuella datorer](../articles/virtual-machines/windows/manage-availability.md)
- [Konfigurera tillgängligheten för virtuella datorer](../articles/virtual-machines/windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Information om hälsa 
Azure Resource Health är en tjänst som visar hello hälsotillståndet för enskilda Azure-resurser och tillämplig vägledning för att felsöka problem. I en molnmiljö där det är inte möjligt toodirectly servrar eller infrastruktur, är hello målet med Resource Health tooreduce hello tid som du ägnar åt felsökning. Hello syfte är särskilt tooreduce hello tid det tar att fastställa om hello rot hello problemet ligger i programmet hello eller i en händelse i hello Azure-plattformen. Mer information finns i [förstå och använda Resource Health](../articles/resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-hello-vm-tooreboot"></a>Åtgärder och händelser som kan orsaka hello VM tooreboot

### <a name="planned-maintenance"></a>Planerat underhåll
Microsoft Azure utför uppdateringar regelbundet över hello globen tooimprove hello tillförlitlighet, prestanda och säkerhet för hello värden underliggande infrastrukturen virtuella datorer. Många av dessa uppdateringar, inklusive minne bevara uppdateringar utförs utan någon inverkan på din virtuella datorer eller molntjänster.

Vissa uppdateringar kräver en omstart. I sådana fall måste hello virtuella datorer stängas medan vi korrigering hello infrastruktur och sedan hello virtuella datorer startas om.

toounderstand vilka Azure planerat underhåll är och hur de kan påverka hello tillgängligheten för din virtuella Linux-datorer, finns i hello artiklarna i den här listan. hello artiklar innehåller information om processen för hello Azure planerat underhåll och hur tooschedule planerat underhåll toofurther påverkan hello.

- [Planerat underhåll för virtuella datorer i Azure](../articles/virtual-machines/windows/planned-maintenance.md)
- [Hur tooschedule planerat underhåll på virtuella Azure-datorer](../articles/virtual-machines/windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Minnesbevarande uppdateringar   
För den här klassen för uppdateringar i Microsoft Azure märker användarna utan inverkan på sina virtuella datorer som körs. Många av de här uppdateringarna är toocomponents eller tjänster som kan uppdateras utan att störa hello kör-instansen. Vissa är plattform infrastrukturuppdateringar på hello värdoperativsystemet som kan användas utan en omstart av hello virtuella datorer.

Uppdateringarna minne bevara utförs med teknik som möjliggör Direktmigrering på plats. När den uppdateras hello VM placeras i en *pausats* tillstånd. Det här tillståndet bevarar hello RAM-minne medan hello underliggande värdoperativsystemet får hello nödvändiga uppdateringar och korrigeringar. hello VM återupptas inom 30 sekunder från paus. Efter hello VM återupptas, synkroniseras automatiskt klockan.

På grund av hello kort paus period minskar distribuera uppdateringar via den här mekanismen avsevärt hello inverkan på hello virtuella datorer. Inte alla uppdateringar kan inte distribueras på detta sätt. 

Flera instanser uppdateringar (för virtuella datorer i en tillgänglighetsuppsättning) är tillämpade en uppdateringsdomän i taget.

> [!NOTE]
> Linux-datorer som har gamla kernel-versioner som påverkas av en kernel-panik under den här metoden update. tooavoid problemet genom att uppdatera tookernel version 3.10.0-327.10.1 eller senare. Mer information finns i [ett virtuella Azure Linux-datorn på en 3.10-baserade kernel panics efter en uppgradering av värden nod](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>Användarinitierad åtgärder för omstart eller avstängning
 
Om du utför en omstart från hello Azure-portalen, Azure PowerShell, kommandoradsgränssnitt eller återställa API, hittar du hello händelse i hello [Azure-aktivitetsloggen](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Om du utför åtgärden hello från hello VM operativsystem hittar hello händelse i hello systemloggar.

Andra scenarier som orsakar vanligen hello VM tooreboot inkluderar flera konfigurationsändring åtgärder. Normalt visas ett varningsmeddelande som visar att köra en viss åtgärd leder till att en omstart av hello VM. Exempel inkluderar några VM storleksändring åtgärder, ändra hello lösenordet för hello administrativt konto och ange en statisk IP-adress.

### <a name="azure-security-center-and-windows-update"></a>Azure Security Center och Windows Update
Azure Security Center övervakar dagliga Windows och Linux virtuella datorer efter uppdateringar av operativsystemet. Security Center hämtar en lista med tillgängliga säkerhetsuppdateringar och viktiga uppdateringar från Windows Update eller Windows Server Update Services (WSUS), beroende på vilken tjänsten har konfigurerats på en Windows-VM. Security Center kontrollerar också hello senaste uppdateringarna för Linux-system. Om den virtuella datorn saknas för en uppdatering av system, rekommenderar Security Center att du installerar uppdateringar. hello tillämpning av uppdateringarna system styrs via hello Security Center i hello Azure-portalen. När du installerar vissa uppdateringar kan det krävas VM omstarter. Mer information finns i [tillämpa uppdateringar i Azure Security Center](../articles/security-center/security-center-apply-system-updates.md).

Som lokala servrar tvingar Azure inte uppdateringar från Windows Update tooWindows virtuella Azure-datorer, eftersom dessa datorer är avsedd toobe som hanteras av användarna. Du är dock uppmuntras tooleave hello automatisk Windows Update-inställningen är aktiverad. Automatisk installation av uppdateringar från Windows Update kan också medföra omstarter toooccur när hello uppdateringar tillämpas. Mer information finns i [Windows Update FAQ](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-hello-availability-of-your-vm"></a>Andra situationer som påverkar hello tillgängligheten för den virtuella datorn
Det finns andra fall där Azure aktivt kan pausa hello användning av en virtuell dator. Du får e-postmeddelanden innan den här åtgärden tas, så har du en chans tooresolve hello underliggande problem. Exempel på problem som påverkar tillgängligheten till VM är säkerhetsöverträdelser och hello förfallodatum för betalningssätt.

### <a name="host-server-faults"></a>Värd-serverfel 
hello VM finns på en fysisk server som körs i ett Azure-datacenter. hello fysisk server körs en agent kallas hello Värdagenten i tillägget tooa några andra Azure-komponenter. När dessa Azure programkomponenter på hello fysisk server slutar svara utlöser en omstart av hello värden tooattempt serveråterställning hello övervakningen. hello VM är vanligtvis tillgänglig igen inom fem minuter och fortsätter toolive på hello samma värden som tidigare.

Serverfel orsakas vanligtvis av maskinvarufel, till exempel hello fel på en hårddisk eller en SSD-enhet. Azure kontinuerligt övervakar dessa händelser, identifierar hello underliggande buggar och samlar ut uppdateringar när hello minskning har implementerat och testas.

Eftersom vissa värden serverfel kan vara specifika toothat server, kan en upprepad VM omstart situation förbättras genom att manuellt omdistribuera hello VM tooanother värdservern. Den här åtgärden kan aktiveras med hjälp av hello **omdistribuera** alternativ på sidan för hello information i hello VM eller genom att stoppa och starta om hello VM i hello Azure-portalen.

### <a name="auto-recovery"></a>Automatisk återställning
Om hello värdservern inte kan startas om av någon anledning, initierar en automatisk återställning åtgärd tootake hello felaktiga värdserver utanför rotation för ytterligare undersökning hello Azure-plattformen. 

Alla virtuella datorer på värden är automatiskt flyttade tooa olika felfri värdservern. Det är vanligtvis klart inom 15 minuter. toolearn mer om hello återskapa processen, se [automatisk återställning av virtuella datorer](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Oplanerat Underhåll
Hej åtgärder i Azure-teamet i sällsynta fall kan behöver tooperform Underhåll aktiviteter tooensure hello övergripande hälsa för hello Azure-plattformen. Det här beteendet kan påverka tillgängligheten för Virtuella och den vanligtvis resulterar i hello samma åtgärd för automatisk återställning som tidigare beskrivits.  

Oplanerad maintenances inkludera hello följande:

- Brådskande nod defragmentering
- Brådskande nätverket växeln uppdateringar

### <a name="vm-crashes"></a>VM kraschar
Virtuella datorer kan starta om på grund av problem inom hello Virtuella datorn. hello arbetsbelastning eller roll som körs på hello VM kan utlösa en bugg kontroll hello gästoperativsystem. Visa hello system- och programloggarna för virtuella Windows-datorer för hjälp med att fastställa hello orsak hello krascher och hello seriella loggar för virtuella Linux-datorer.

### <a name="storage-related-forced-shutdowns"></a>Lagringsrelaterade framtvingad avstängningar
Virtuella datorer i Azure förlitar sig på virtuella diskar för operativsystemet och lagring av data som finns i hello Azure Storage-infrastruktur. När mer än 120 sekunder påverkas hello tillgänglighet eller anslutningen mellan hello VM och hello associerade virtuella diskar, utför en framtvingad avstängning av hello VMs tooavoid datafel hello Azure-plattformen. hello virtuella datorer är automatiskt igång igen efter lagringsanslutning har återställts. 

hello varaktighet hello avstängning kan vara så kort som fem minuter, men kan vara betydligt längre. hello följande är en av hello särskilda fall som är associerad med lagringsrelaterade framtvingad avstängningar: 

**Överstiger IO begränsar**

Virtuella datorer kan vara tillfälligt stänga när i/o-begäranden är konsekvent begränsas eftersom hello volymen av i/o-åtgärder per sekund (IOPS) överskrider hello i/o-gränser för hello disk. (Standard disklagring begränsas too500 IOPS.) toomitigate det här problemet använder disk striping eller konfigurera hello lagringsutrymme i hello gäst VM, beroende på hello arbetsbelastning. Mer information finns i [Konfigurera virtuella Azure-datorer för optimala lagringsprestanda](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Högre IOPS-gränser är tillgängliga via Azure Premium-lagring med upp too80 000 IOPS. Mer information finns i [Premium-lagring med höga prestanda](../articles/storage/common/storage-premium-storage.md).

### <a name="other-incidents"></a>Andra incidenter
I sällsynta fall kan kan ett omfattande problem påverka flera servrar i ett Azure-datacenter. Om det här problemet inträffar skickar hello Azure-teamet e-post meddelanden toohello påverkas prenumerationer. Du kan kontrollera hello [hälsoinstrumentpanel för Azure Service](https://azure.microsoft.com/status/) och hello Azure portal för hello status för pågående avbrott och tidigare incidenter.
