Azure utför regelbundet uppdateringar tooimprove hello tillförlitlighet, prestanda och säkerhet för hello värden infrastruktur för virtuella datorer. Dessa uppdateringar mellan korrigering programvarukomponenter i hello värdmiljön (till exempel operativsystem, hypervisor och olika agenter distribueras på hello värden) uppgraderar nätverkskomponenter, inaktivering av toohardware. hello merparten av de här uppdateringarna utförs utan någon inverkan toohello som värd för virtuella datorer. Det finns dock fall där uppdateringar har konsekvenser:

- Om hello underhåll som inte kräver en omstart, använder Azure migrering på plats toopause hello VM medan hello värden uppdateras.

- Om en omstart krävs för underhåll, får du ett meddelande om när hello Underhåll planeras. I dessa fall kan också får du ett tidsfönster där du kan starta hello underhåll själv, samtidigt som passar dig.

Den här sidan beskrivs hur Microsoft Azure utför båda typerna av underhåll. Mer information om oplanerade händelser (avbrott) finns i Hantera hello tillgängligheten för virtuella datorer för [Windows] (... / articles/virtual-machines/windows/manage-availability.md) eller [Linux](../articles/virtual-machines/linux/manage-availability.md).

Program som körs på en virtuell dator kan samla information om kommande uppdateringar med hjälp av hello Azure Metadata Service för [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) eller [Linux] (... / articles/virtual-machines/linux/instance-metadata-service.md).

## <a name="in-place-vm-migration"></a>VM-migrering på plats

När uppdateringar inte kräver en omstart för fullständig, används en aktiv migrering på plats. Under hello uppdatering är hello virtuella datorn pausad i 30 sekunder, bevarar hello minne i RAM, medan hello värdmiljön gäller hello nödvändiga uppdateringar och korrigeringar. hello virtuella sedan återupptas och hello klocka hello virtuella synkroniseras automatiskt.

För virtuella datorer i tillgänglighetsuppsättningar kan är uppdatera domäner uppdaterade ett i taget. Alla virtuella datorer i en uppdateringsdomän (UD) pausas, uppdateras och återupptas innan planerat underhåll flyttas på toohello nästa UD.

Vissa program kan påverkas av dessa typer av uppdateringar. Program som utför realtid händelsebearbetning, som direktuppspelning eller omkodning eller hög genomströmning nätverk scenarier, kanske inte utformad tootolerate en paus på 30 sekund. <!-- sooooo, what should they do? --> 


## <a name="maintenance-requiring-a-reboot"></a>Underhåll som kräver en omstart

När virtuella datorer måste startas om för planerat underhåll toobe, meddelas du i förväg. Planerat underhåll har två faser: hello självbetjäning fönster och schemalagda underhållsperiod.

Hej **självbetjäning fönstret** kan du initiera hello Underhåll på dina virtuella datorer. Under denna tid kan du fråga efter varje VM-toosee deras status och kontrollera hello resultatet av din senaste begäran.

När du startar självbetjäning Underhåll är den virtuella datorn har flyttats tooa nod som redan har uppdaterats och sedan aktiveras den tillbaka. Eftersom hello VM startas hello diskutrymme går förlorad och dynamiska IP-adresser som är kopplade till virtuella nätverksgränssnittet har uppdaterats.

Om du startar självbetjäning underhåll och det finns ett fel under processen hello hello åtgärden är stoppad, hello VM inte uppdateras och den också tas bort från hello planerat underhåll iteration. Du kontaktad på senare med ett nytt schema och en ny affärsmöjlighet toodo självbetjäning underhåll som erbjuds. 

När hello självbetjäning fönstret har passerat hello **schemalagda underhållsperiod** börjar. Under den här tidsfönstret du fortfarande fråga efter hello underhållsperiod, men inte längre kan toostart hello underhåll själv.

## <a name="availability-considerations-during-planned-maintenance"></a>Tillgänglighet under planerat underhåll 

Om du väljer toowait tills hello planerad underhållsperiod, finns det några saker tooconsider för att underhålla hello högsta availabilty för dina virtuella datorer. 

### <a name="paired-regions"></a>Parad regioner

Varje Azure-region paras ihop med en annan region inom hello samma geografi, tillsammans med de gör ett regionalt par. Under planerat underhåll uppdatera Azure bara hello virtuella datorer i en region i en region-par. Till exempel när du uppdaterar hello virtuella datorer i norra centrala USA, Azure kommer inte att uppdatera alla virtuella datorer i södra centrala USA på hello samtidigt. Men andra regioner som Nordeuropa kan vara under Underhåll på hello samma tid som östra USA. Så här fungerar region par kan hjälpa dig att förstå bättre distribuera din virtuella dator över regioner. Mer information finns i [Azure-region par](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="availability-sets-and-scale-sets"></a>Tillgänglighetsuppsättningar och skaluppsättningar

När du distribuerar en arbetsbelastningen på virtuella Azure-datorer kan skapa du hello virtuella datorer i ett tillgänglighet set tooprovide hög tillgänglighet tooyour program. Detta garanterar att minst en virtuell dator vid ett strömavbrott eller underhåll händelser är tillgänglig.

I en tillgänglighetsuppsättning sprids enskilda virtuella datorer installerar too20 uppdatering domäner (UDs). Endast en enskild uppdateringsdomän påverkas vid en given tidpunkt under planerat underhåll. Tänk på att hello ordningen för update-domäner som påverkas inte nödvändigtvis sker sekventiellt. 

Skaluppsättningar för den virtuella datorn är en Azure compute-resurs som du kan använda toodeploy och hantera en uppsättning identiska virtuella datorer som en enskild resurs. Hej skaluppsättning distribueras automatiskt uppdatera domäner som virtuella datorer i en tillgänglighetsuppsättning. Precis som med tillgänglighetsuppsättningar påverkas med skalningsuppsättningar i en enda uppdateringsdomän vid en given tidpunkt.

Mer information om hur du konfigurerar dina virtuella datorer för hög tillgänglighet finns i Hantera hello tillgängligheten för virtuella datorer för Windows (... / articles/virtual-machines/windows/manage-availability.md) eller [Linux](../articles/virtual-machines/linux/manage-availability.md).
