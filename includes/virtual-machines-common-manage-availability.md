## <a name="understand-vm-reboots---maintenance-vs-downtime"></a>Förstå omstarter av virtuella datorer – underhåll och driftavbrott
Det finns tre scenarier som kan leda toovirtual datorn i Azure som påverkas: oplanerad maskinvara underhåll, oplanerade driftstopp och planerat underhåll.

* **Oplanerad Underhållshändelse för maskinvara** inträffar när hello Azure-plattformen beräknar hello maskinvaran eller någon plattform komponenten associerade tooa fysisk dator, är om toofail. När hello plattform beräknar ett fel, utfärdar den en oplanerad maskinvara Underhåll händelse tooreduce hello påverkan toohello virtuella datorer som finns på den maskinvaran. Azure använder Direktmigrering teknik toomigrate hello virtuella datorer från hello misslyckas maskinvara tooa felfri fysisk dator. Direktmigrering är en virtuell dator som bevaras åtgärden att endast pausar hello virtuell dator för en kort tid. Minne, öppna filer och nätverksanslutningar bevaras, men prestanda kan minskas före eller efter hello händelsen. I fall där det inte går att använda Direktmigrering får hello VM oväntat driftstopp som beskrivs nedan.


* **Ett oväntat avbrott** sällan inträffar när hello maskinvara eller hello fysisk infrastruktur underliggande din virtuella dator fel har uppstått på något sätt. Det kan vara lokala nätverksfel, lokala diskfel eller andra fel på racknivå. När sådant fel identifieras hello Azure-plattformen migrerar automatiskt (heals) din virtuella tooa felfri fysisk dator. Under hello återställning procedur, virtuella datorer eventuellt driftstopp (omstart) och i vissa fall förlust av hello tillfälliga enheten. hello ansluten OS och datadiskar behålls alltid. 

* **Planerade underhållshändelser** är periodiska uppdateringar som görs av Microsoft toohello underliggande Azure-plattformen tooimprove övergripande tillförlitlighet, prestanda och säkerhet för hello plattformsinfrastruktur som de virtuella datorerna körs på. De flesta av de här uppdateringarna utförs utan att påverka dina virtuella datorer eller molntjänster (mer information finns i [VM Preserving Maintenance](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/preserving-maintenance) (Underhåll utan påverkan på virtuella datorer)). Hello Azure-plattformen försöker toouse VM bevara Underhåll i alla möjliga tillfällen, men det finns sällsynta fall när uppdateringarna kräver en omstart av din virtuella tooapply hello krävs toohello underliggande infrastruktur för programuppdateringar. I det här fallet kan du utföra Azure planerat underhåll med Underhåll Omdistributionen åtgärden genom att initiera hello Underhåll för sina virtuella datorer under hello lämplig tidsperioden. Mer information finns i [Planned Maintenance for Virtual Machines](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/planned-maintenance/) (Planerat underhåll för virtuella datorer).


tooreduce hello effekten av avbrott på grund av tooone eller flera av dessa händelser, rekommenderar vi hello följande metodtips för hög tillgänglighet för dina virtuella datorer:

* [Konfigurera flera virtuella datorer i en tillgänglighetsuppsättning för redundans]
* [Använda hanterade diskar för virtuella datorer i en tillgänglighetsuppsättning]
* [Använd schemalagda händelser tooproactively svar tooVM påverkar händelser] (https://docs.microsoft.com/en-us/azure/virtual-machines/virtual-machines-scheduled-events)
* [Konfigurera varje programnivå i separata tillgänglighetsuppsättningar]
* [Kombinera en belastningsutjämnare med tillgänglighetsuppsättningar]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Konfigurera flera virtuella datorer i en tillgänglighetsuppsättning för redundans
tooprovide redundans tooyour program, rekommenderar vi att du grupperar två eller flera virtuella datorer i en tillgänglighetsuppsättning. Den här konfigurationen garanterar att under antingen en planerad eller oplanerad underhållshändelse, minst en virtuell dator är tillgänglig och uppfyller hello 99,95% SLA för Azure. Mer information finns i hello [SLA för virtuella datorer](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Undvik att endast ha en enda virtuell dator i en tillgänglighetsuppsättning. Virtuella datorer med en sådan konfiguration är inte kvalificerade för serviceavtalets drifttidsgaranti vid planerat Azure-underhåll, med undantag för om den virtuella datorn använder [Azure Premium Storage](../articles/storage/common/storage-premium-storage.md). Enskild virtuella datorer med premium-lagring, hello Azure-serviceavtalet gäller.

Varje virtuell dator i din tillgänglighetsuppsättning har tilldelats en **uppdateringsdomän** och en **feldomän** av hello underliggande Azure-plattformen. För en given tillgänglighetsuppsättning fem konfigureras för användaren update domäner tilldelas som standard (Resource Manager distributioner kan sedan vara ökad tooprovide upp too20 uppdatering domäner) tooindicate grupper med virtuella datorer och underliggande fysisk maskinvara som kan startas på hello samtidigt. När fler än fem virtuella datorer är konfigurerade i en enda tillgänglighet är hello sjätte virtuella datorn placerad i samma uppdatera domänen som hello första virtuella datorn hello sjunde i hello samma domän som andra virtuella hello och programuppdateringar så hello på. hello ordningen för update domäner startas inte kan fortsätta sekventiellt under planerat underhåll, men bara en uppdateringsdomän startas i taget. Omstartad uppdateringsdomän ges toorecover 30 minuter innan Underhåll initieras på en annan uppdateringsdomän.

Feldomäner definiera hello grupp av virtuella datorer som delar en gemensam käll- och strömbrytare. Som standard separeras hello virtuella datorer som konfigurerats inom din tillgänglighetsuppsättning över in toothree feldomäner för Resource Manager distributioner (två fault domäner för klassisk). När du placerar dina virtuella datorer i en tillgänglighetsuppsättning inte skyddar programmet från operativsystemet eller programspecifika fel, begränsar den hello effekten av potentiella fysiska maskinvarufel, nätverksavbrott eller power avbrott.

<!--Image reference-->
   ![Konceptuell ritning av hello domän och fel domän uppdateringskonfiguration](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="use-managed-disks-for-vms-in-an-availability-set"></a>Använda hanterade diskar för virtuella datorer i en tillgänglighetsuppsättning
Om du använder virtuella datorer med ohanterad diskar, rekommenderar vi du [konvertera virtuella datorer i Tillgänglighetsuppsättningen toouse hanterade diskar](../articles/virtual-machines/windows/convert-unmanaged-to-managed-disks.md).

[Hanterade diskar](../articles/virtual-machines/windows/managed-disks-overview.md) ger bättre tillförlitlighet för Tillgänglighetsuppsättningar genom att säkerställa som hello diskar för virtuella datorer i en Tillgänglighetsuppsättning är tillräckligt isoleras från varandra tooavoid enskilda felpunkter. Detta sker automatiskt placerar hello diskar i olika lagringskluster. Om ett lagringskluster misslyckas på grund av toohardware-eller programvarufel misslyckas bara hello VM-instanser med diskar på dessa stämplar.

![Feldomäner med hanterade diskar](./media/virtual-machines-common-manage-availability/md-fd.png)

> [!IMPORTANT]
> hello antalet feldomäner för hanterade tillgänglighetsuppsättningar varierar beroende på region - två eller tre per region. hello visar följande tabell hello många per region

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

Om du planerar toouse virtuella datorer med [ohanterad diskar](../articles/virtual-machines/windows/about-disks-and-vhds.md#types-of-disks), följ nedan bästa praxis för Storage-konton där virtuella hårddiskar (VHD) för virtuella datorer lagras som [sidblobbar](https://docs.microsoft.com/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs).

1. **Hålla alla diskar (OS och data) som är kopplad till en virtuell dator i hello samma lagringskonto**
2. **Granska hello [gränser](../articles/storage/common/storage-scalability-targets.md) på hello antalet ohanterade diskar i ett lagringskonto** innan du lägger till flera virtuella hårddiskar tooa storage-konto
3. **Använd ett separat lagringskonto för varje virtuell dator i en tillgänglighetsuppsättning.** Dela inte Storage-konton med flera virtuella datorer i hello samma Tillgänglighetsuppsättning. Det är acceptabelt för virtuella datorer över olika Tillgänglighetsuppsättningar tooshare storage-konton om ovan rekommenderade metoder följs

## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Konfigurera varje programnivå i separata tillgänglighetsuppsättningar
Om dina virtuella datorer är alla nästan identisk och hantera hello syfte samma för programmet, rekommenderar vi att du konfigurerar en tillgänglighetsuppsättning för varje nivå i ditt program.  Om du placerar två olika nivåer i hello samma tillgänglighetsuppsättning, alla virtuella datorer i hello samma nivå av programmet kan startas på samma gång. Genom att konfigurera minst två virtuella datorer i en tillgänglighetsuppsättning för varje nivå garanterar du att minst en virtuell dator är tillgänglig på varje nivå.

Du kan till exempel placera alla hello virtuella datorer i hello klientdelen för ditt program som kör IIS, Apache, Nginx i en enda tillgänglighetsuppsättning. Se till att endast frontend virtuella datorer är placerade i hello samma tillgänglighetsuppsättning. Se också till att endast virtuella datanivådatorer placeras i en egen tillgänglighetsuppsättning, till exempel replikerade virtuella datorer som kör SQL Server eller MySQL.

<!--Image reference-->
   ![Programnivåer](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Kombinera en belastningsutjämnare med tillgänglighetsuppsättningar
Kombinera hello [Azure belastningsutjämnare](../articles/load-balancer/load-balancer-overview.md) med en tillgänglighet ange tooget hello återhämtning för de flesta program. hello Azure belastningsutjämnare distribuerar trafik mellan flera virtuella datorer. För vår standardnivån virtuella datorer ingår hello Azure belastningsutjämnare. Inte alla virtuella nivåer omfattar hello Azure belastningsutjämnare. Mer information om belastningsutjämning för virtuella datorer finns i [Belastningsutjämna virtuella datorer](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Om hello belastningsutjämning inte är konfigurerad toobalance trafik över flera virtuella datorer och sedan en planerad underhållshändelse påverkar hello endast trafik betjänar virtuell dator kan orsaka ett avbrott tooyour programmet nivå. Placerar flera virtuella datorer av hello datanivå samma under hello samma läsa in belastningsutjämning och tillgänglighet set aktiverar trafik toobe kontinuerligt hanteras av minst en instans.


<!-- Link references -->
[Konfigurera flera virtuella datorer i en tillgänglighetsuppsättning för redundans]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Konfigurera varje programnivå i separata tillgänglighetsuppsättningar]: #configure-each-application-tier-into-separate-availability-sets
[Kombinera en belastningsutjämnare med tillgänglighetsuppsättningar]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Använda hanterade diskar för virtuella datorer i en tillgänglighetsuppsättning]: #use-managed-disks-for-vms-in-an-availability-set
