# <a name="platform-supported-migration-of-iaas-resources-from-classic-tooazure-resource-manager"></a>Plattform som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager
I den här artikeln beskriver vi hur vi är att aktivera migrering av infrastruktur som en tjänst (IaaS)-resurser från hello klassisk tooResource Manager distributionsmodellerna. Du kan läsa mer om [Azure Resource Manager-funktioner och fördelar](../articles/azure-resource-manager/resource-group-overview.md). Vi i detalj hur tooconnect resurser från hello två distributionsmodeller som finnas i din prenumeration med hjälp av virtuella nätverk plats-till-plats-gatewayer.

## <a name="goal-for-migration"></a>Målet för migrering
Hanteraren för filserverresurser kan distribuera komplexa program via mallar, konfigurerar virtuella datorer med hjälp av VM-tillägg och inkluderar åtkomsthantering och Taggning. Azure Resource Manager innehåller skalbar, parallell distribution för virtuella datorer till tillgänglighetsuppsättningar. hello nya distributionsmodell dessutom livscykelhantering beräkning, nätverk och lagring oberoende av varandra. Slutligen finns fokuserar på hur du aktiverar säkerhet som standard med hello verkställandet av virtuella datorer i ett virtuellt nätverk.

Nästan alla hello funktioner från hello klassiska distributionsmodellen stöds för beräkning, nätverk och lagring under Azure Resource Manager. toobenefit från hello nya funktioner i Azure Resource Manager kan du migrera befintliga distributioner från hello klassiska distributionsmodellen.

## <a name="supported-resources-for-migration"></a>Resurser som stöds för migrering
Dessa klassiska IaaS-resurser som stöds under migreringen

* Virtuella datorer
* Tillgänglighetsuppsättningar
* Molntjänster
* Lagringskonton
* Virtuella nätverk
* VPN-gatewayer
* Express Route-gatewayer _(i hello samma prenumeration som virtuellt nätverk endast)_
* Nätverkssäkerhetsgrupper 
* Routningstabeller 
* Reserverade ip-adresser 

## <a name="supported-scopes-of-migration"></a>Stöds scope för migrering
Det finns 4 olika sätt toocomplete migrering av beräkning, nätverk och lagringsresurser. Dessa är 

* Migrering av virtuella datorer (inte i ett virtuellt nätverk)
* Migrering av virtuella datorer (inom ett virtuellt nätverk)
* Lagringsmigrering för konton
* Fria resurser (Nätverkssäkerhetsgrupper, vägtabeller och reserverade IP-adresser)

### <a name="migration-of-virtual-machines-not-in-a-virtual-network"></a>Migrering av virtuella datorer (inte i ett virtuellt nätverk)
I hello Resource Manager-distributionsmodellen upprätthålls säkerheten för dina program som standard. Alla virtuella datorer måste toobe i ett virtuellt nätverk i hello Resource Manager-modellen. Hej omstarter av Azure-plattformen (`Stop`, `Deallocate`, och `Start`) hello virtuella datorer som en del av hello migrering. Du har två alternativ för hello virtuella nätverk som hello virtuella datorer kommer att migreras till:

* Du kan begära hello plattform toocreate ett nytt virtuellt nätverk och migrera hello virtuell dator till hello nytt virtuellt nätverk.
* Du kan migrera hello virtuell dator i ett befintligt virtuellt nätverk i Resource Manager.

> [!NOTE]
> I det här omfånget för migrering, både hello operations management-plan och hello data-plan åtgärder tillåts inte för en viss tidsperiod under hello migreringen.
>
>

### <a name="migration-of-virtual-machines-in-a-virtual-network"></a>Migrering av virtuella datorer (inom ett virtuellt nätverk)
För de flesta konfigurationer av virtuell dator migrerar endast hello metadata mellan hello klassisk och Resource Manager distributionsmodellerna. hello underliggande virtuella datorer som körs på hello samma maskinvara i hello samma nätverk och hello med samma lagringsutrymme. hello management-plan åtgärder kan inte tillåten för en viss tidsperiod under hello migreringen. Hello dataplan fortsätter dock toowork. Dina program som körs på virtuella datorer (klassiskt) det vill säga inte till driftstopp under hello migreringen.

hello följande konfigurationer stöds inte för närvarande. Om stöd har lagts till i hello framtida, vissa virtuella datorer i den här konfigurationen kan innebära avbrottstid (gå via stopp, frigöra och starta om VM operations).

* Du har mer än en tillgänglighetsuppsättning i en enda molntjänst.
* Du har en eller flera tillgänglighetsuppsättningar och virtuella datorer som inte ingår i en tillgänglighetsuppsättning i en enda molntjänst.

> [!NOTE]
> I den här migreringen omfång tillåtas hello management plan inte för en viss tidsperiod under hello migreringen. För vissa konfigurationer enligt beskrivningen tidigare, data-plan avbrottstiden inträffar.
>
>

### <a name="storage-accounts-migration"></a>Lagringsmigrering för konton
tooallow sömlös migrering, kan du distribuera Resource Manager virtuella datorer i ett klassiskt storage-konto. Med den här funktionen kan beräknings- och nätverksresurser och de ska migreras oberoende av storage-konton. När du migrerar över dina virtuella datorer och virtuella nätverk måste toomigrate över migreringsprocessen storage-konton toocomplete hello.

> [!NOTE]
> hello Resource Manager-modellen har inte hello begreppet klassisk avbildningar och diskar. När hello storage-konto är migrerade, klassiska avbildningar och diskar visas inte i hello Resource Manager-stacken men hello säkerhetskopiera virtuella hårddiskar finns kvar i hello storage-konto.
>
>

### <a name="unattached-resources-network-security-groups-route-tables--reserved-ips"></a>Fria resurser (Nätverkssäkerhetsgrupper, vägtabeller och reserverade IP-adresser)
Nätverkssäkerhetsgrupper, vägtabeller och reserverade IP-adresser som inte är ansluten tooany virtuella datorer och virtuella nätverk som kan migreras oberoende av varandra.

<br>

## <a name="unsupported-features-and-configurations"></a>Funktioner som inte stöds och konfigurationer
Vi stöder för närvarande inte vissa funktioner och konfigurationer. hello följande avsnitt beskrivs våra rekommendationer runtom.

### <a name="unsupported-features"></a>Funktioner som inte stöds
hello följande funktioner stöds inte för närvarande. Du kan om du vill ta bort de här inställningarna, migrera hello virtuella datorer och sedan återaktivera hello inställningar i hello Resource Manager-distributionsmodellen.

| Resursprovider | Funktion | Rekommendation |
| --- | --- | --- |
| Compute |Olänkade virtuella diskar. | hello VHD-blobbar bakom diskarna som ska migreras när migreras hello Storage-konto |
| Compute |Avbildningar av virtuella datorer. | hello VHD-blobbar bakom diskarna som ska migreras när migreras hello Storage-konto |
| Nätverk |Slutpunkts-ACL:. | Ta bort slutpunkts-ACL: och gör om migreringen. |
| Nätverk |Virtuellt nätverk med både ExpressRoute-Gateway och VPN-Gateway  | Ta bort hello VPN-Gateway innan du påbörjar migreringen och sedan återskapa hello VPN-Gateway när migreringen är klar. Lär dig mer om [ExpressRoute migrering](../articles/expressroute/expressroute-migration-classic-resource-manager.md).|
| Nätverk |ExpressRoute med auktorisering länkar  | Ta bort nätverksanslutning för hello ExpressRoute-kretsen toovirtaul innan du påbörjar migreringen och sedan återskapa hello-anslutning när migreringen är klar. Lär dig mer om [ExpressRoute migrering](../articles/expressroute/expressroute-migration-classic-resource-manager.md). |
| Nätverk |Application Gateway | Ta bort hello Programgateway innan du påbörjar migreringen och sedan återskapa hello Programgateway när migreringen är klar. |
| Nätverk |Virtuella nätverk med hjälp av VNet-Peering. | Migrera virtuella nätverk tooResource Manager och sedan peer. Lär dig mer om [VNet-Peering](../articles/virtual-network/virtual-network-peering-overview.md). | 

### <a name="unsupported-configurations"></a>Konfigurationer som inte stöds
hello följande konfigurationer stöds inte för närvarande.

| Tjänst | Konfiguration | Rekommendation |
| --- | --- | --- |
| Resource Manager |Rollen åtkomstkontroll (RBAC) för klassiska resurser |Eftersom hello URI för hello resurser har ändrats efter migreringen bör du planera hello RBAC principuppdateringar som kräver toohappen efter migreringen. |
| Compute |Flera undernät som är kopplad till en virtuell dator |Uppdatera hello undernät configuration tooreference endast undernät. |
| Compute |Virtuella datorer som tillhör tooa virtuellt nätverk men har inte ett explicit undernät som tilldelats |Du kan eventuellt ta bort hello VM. |
| Compute |Virtuella datorer som har aviseringar, Autoskala principer |hello migreringen går igenom och inställningarna tas bort. Vi rekommenderar starkt att du utvärderar din miljö innan du hello migrering. Du kan också konfigurera hello aviseringsinställningar när migreringen är klar. |
| Compute |XML-VM-tillägg (BGInfo-1.*, Visual Studio-felsökaren, Web Deploy och fjärrfelsökning) |Detta stöds inte. Vi rekommenderar att du tar bort dessa tillägg från hello virtuella toocontinue migrering eller de kommer att tas bort automatiskt under hello migreringsprocessen. |
| Compute |Startdiagnostikinställningar med Premium-lagring |Inaktivera Startdiagnostikinställningar funktionen för hello virtuella datorer innan du fortsätter med migreringen. Du kan återaktivera startdiagnostikinställningar i hello Resource Manager-stacken när hello migreringen är klar. Blobbar som används för skärmbild och seriella loggar bör dessutom tas bort så att du inte längre debiteras för de blobbar. |
| Compute |Molntjänster som innehåller web/worker-roller |Detta stöds för närvarande inte. |
| Nätverk |Virtuella nätverk som innehåller virtuella datorer och web/worker-roller |Detta stöds för närvarande inte. |
| Azure App Service |Virtuella nätverk som innehåller apptjänstmiljöer |Detta stöds för närvarande inte. |
| Azure HDInsight |Virtuella nätverk som innehåller HDInsight-tjänster |Detta stöds för närvarande inte. |
| Microsoft Dynamics livscykel Services |Virtuella nätverk som innehåller virtuella datorer som hanteras av Dynamics livscykel Services |Detta stöds för närvarande inte. |
| Azure AD Domain Services |Virtuella nätverk som innehåller Azure AD Domain services |Detta stöds för närvarande inte. |
| Azure RemoteApp |Virtuella nätverk som innehåller Azure RemoteApp-distributioner |Detta stöds för närvarande inte. |
| Azure API Management |Virtuella nätverk som innehåller Azure API Management-distributioner |Detta stöds för närvarande inte. toomigrate hello IaaS VNET, ändra hello VNET av hello API Management-distribution som en ingen åtgärd har driftstopp. |
| Compute |Tillägg för Azure Security Center med ett virtuellt nätverk som har en VPN-gateway i överföringen anslutningen eller ExpressRoute-gateway med lokala DNS-server |Azure Security Center installerar tillägg på din virtuella datorer toomonitor säkerhet och generera aviseringar automatiskt. Dessa tillägg installeras vanligtvis automatiskt om hello Azure Security Center-princip aktiveras på hello prenumeration. ExpressRoute-gateway migrering stöds inte för närvarande och VPN-gatewayer med överföring anslutningen förlorar lokal åtkomst. Ta bort ExpressRoute gör gateway eller migrera VPN-gateway med överföring anslutningen att internet access tooVM lagring konto toobe går förlorade vid fortsätter med genomför hello migrering. hello migreringen kommer inte att fortsätta när detta inträffar eftersom statusblobben för hello gäst-agenten inte kan fyllas i. Det rekommenderas toodisable Azure Security Center-princip på hello prenumeration 3 timmar innan du fortsätter med migreringen. |

