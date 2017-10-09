## <a name="meaning-of-migration-of-iaas-resources-from-hello-classic-deployment-model-tooresource-manager"></a>Innebörden av migreringen av IaaS-resurser från hello klassisk distribution modellen tooResource Manager
Innan vi detaljnivån hello information, ska vi titta på hello skillnaden mellan data-plan och management-plan åtgärder på hello IaaS-resurser.

* *Kontroll och Management plan* beskriver hello-anrop som kommer till hello kontroll och management-plan eller hello API för att ändra resurser. Till exempel hantera åtgärder som att skapa en virtuell dator, startar om en virtuell dator och uppdatera ett virtuellt nätverk med ett nytt undernät hello kör resurser. De påverkar inte direkt anslutning toohello instanser.
* *Dataplan* (program) beskriver hello körtiden för själva hello-programmet och inbegriper interaktion med instanser som inte gå igenom hello Azure API. När du öppnar din webbplats eller hämtar data från en aktiv SQL Server-instans eller en MongoDB-server, betraktas det som en dataplan- eller programinteraktion. Kopiera en blobb från ett lagringskonto och åtkomst till en offentlig IP-adress tooRDP eller SSH till hello virtuell dator är också dataplan. Dessa åtgärder Behåll hello-program som körs över beräkning, nätverk och lagring.

Hello bakgrunden är hello dataplan hello samma mellan både hello klassiska distributionsmodellen och Resource Manager-stacken. Under migreringen översätta vi hello representation av hello resurser från hello klassisk distribution modellen toothat i hello Resource Manager-stacken. Därför måste toouse nya verktyg, API: er, SDK: er toomanage dina resurser i hello Resource Manager-stacken.

![Skärmbild som visar skillnaden mellan hanterings-/kontrollplanet och dataplanet](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> Vissa Migreringsscenarier hello Azure-plattformen stoppas, tar bort och startar om de virtuella datorerna. Det medför ett kort driftstopp för dataplanet.
>

## <a name="hello-migration-experience"></a>hello migreringsupplevelse
Innan du börjar hello migreringsupplevelse bör hello följande:

* Kontrollera att hello resurser som du vill toomigrate inte använder några funktioner som inte stöds eller konfigurationer. Vanligtvis hello plattform identifierar dessa problem och genererar ett fel.
* Om du har virtuella datorer som inte ingår i ett virtuellt nätverk de stoppas och frigöra som en del av hello förbereda åtgärden. Om du inte vill toolose hello offentlig IP-adress, Förbered utseende till att reservera hello IP-adress innan hello igen. Om hello virtuella datorer i ett virtuellt nätverk, är de dock inte stoppats och frigöra.
* Planera migreringen under icke kontorstid tooaccommodate för oväntade fel som kan inträffa under migreringen.
* Hämta hello aktuella konfigureringen av dina virtuella datorer med hjälp av PowerShell, kommandoradsgränssnittet (CLI)-kommandon eller REST API: er toomake underlättar för verifiering när hello förbereda steg har slutförts.
* Uppdatera ditt automation/operationalization skript toohandle hello Resource Manager-modellen innan du påbörjar migreringen hello. Alternativt kan du göra GET-åtgärder när hello resurser i hello förberedd tillstånd.
* Utvärdera hello RBAC principer som är konfigurerade på hello klassiska IaaS-resurser och planera för när hello migreringen är klar.

arbetsflödet för migrering av hello är som följer

![Skärmbild som visar hello arbetsflödet för migrering](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Alla hello-åtgärder som beskrivs i följande avsnitt hello är idempotent. Om du har problem med än en funktion som inte stöds eller ett konfigurationsfel bör du försöka hello förbereda, avbrytas eller genomföras igen. hello Azure-plattformen försöker hello åtgärden igen.
>
>

### <a name="validate"></a>Verifiera
hello Validera åtgärden är hello första steget i hello migreringsprocessen. hello syftet med det här steget är tooanalyze hello tillstånd hello resurser som du vill toomigrate i hello klassiska distributionsmodellen och returnera lyckade/misslyckade om hello resurser kan migreringen.

Markerar du hello virtuellt nätverk eller en tjänst i molnet (om den inte är i ett virtuellt nätverk) som du vill toovalidate för migrering.

* Om hello resursen inte kan utföra migrering, visar hello Azure-plattformen alla hello orsaker till varför den inte stöds för migrering.

#### <a name="checks-not-done-in-validate"></a>Kontrollerar inte har gjort i Bekräfta

Validera åtgärden endast analyserar hello tillståndet för hello resurser i hello klassiska distributionsmodellen. Det kan söka efter alla fel och scenarier som inte stöds på grund av toovarious konfigurationer i hello klassiska distributionsmodellen. Det är inte möjligt toocheck för alla problem som hello Azure Resource Manager stack kan ge hello resurser under migreringen. De här problemen kontrolleras endast om hello resurser genomgår omvandling i hello nästa steg i migreringen, det vill säga Förbered. hello tabellen nedan listar alla hello problem inte incheckad verifiera.


|Nätverk kontrollerar inte i Bekräfta|
|-|
|Ett virtuellt nätverk med både ER och VPN-gateway|
|Virtuella nätverk gateway-anslutningen i Koppla från tillstånd|
|Alla ER kretsar är före migrerade tooAzure Resource Manager-stacken|
|Kvot för Azure Resource Manager söker efter nätverksresurser, som är statiska offentliga IP, dynamiska offentliga IP-adresser, belastningsutjämnare, Nätverkssäkerhetsgrupper, vägtabeller, nätverksgränssnitt |
| Kontrollera att alla belastningsutjämningsreglerna är giltig för distribution/VNET |
| Sök efter motstridiga privata IP-adresser mellan frigjorts stoppa virtuella datorer i hello samma virtuella nätverk |

### <a name="prepare"></a>Förbereda
hello förbereda åtgärden är hello andra steget hello migreringsprocessen. hello målet med det här steget är toosimulate hello transformering av hello IaaS-resurser från klassiska modellen tooResource Manager distributionsresurser och presentera detta sida vid sida du toovisualize.

> [!NOTE] 
> Klassiska resurser ändras inte under det här steget. Det är därför en säker steg toorun om du försöker ut migrering. 

Markerar du hello virtuella nätverk eller hello tjänst i molnet (om det inte är ett virtuellt nätverk) som du vill tooprepare för migrering.

* Om hello resursen inte kan utföra migrering, stoppar hello migreringsprocessen hello Azure-plattformen och visar hello orsak varför hello förbereda åtgärden misslyckades.
* Om hello resurs kan migreringen, låser hello Azure-plattformen först driften hello management-plan för hello resurser under migreringen. Du är till exempel inte kan tooadd en data disk tooa VM under migreringen.

hello startas Azure-plattformen hello migrering av metadata från klassisk distribution modellen tooResource Manager för hello migrera resurser.

När hello förbereda åtgärden är slutförd, har hello möjlighet att visualisera hello resurser för både klassiska distributionsmodellen och Resource Manager. För varje Molntjänsten i hello klassiska distributionsmodellen hello Azure-plattformen skapar ett Resursgruppsnamn som har hello mönster `cloud-service-name>-Migrated`.

> [!NOTE]
> Det är inte möjligt tooselect hello namnet på resursgruppen som skapats för migrerade resurser (t.ex ”.-migreras”) men när migreringen är klar, kan du använda Azure Resource Manager flytta funktionen toomove resurser tooany resursgrupp som du vill. Mer information läser tooread [flytta resurser toonew resursgrupp eller prenumeration](../articles/resource-group-move-resources.md)

Här följer två skärmar som visar hello resultatet efter utförts Förberedelseåtgärden. Första skärmen visar en resursgrupp som innehåller ursprungliga hello-Molntjänsten. Andra skärmen visar hello nya ”-migreras” resursgruppen som innehåller hello likvärdiga resurser i Azure Resource Manager.

![Skärmbild som visar Portal klassisk molntjänst](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Skärmbild som visar Portal Azure Resource Manager-resurser i förberedelsesteget](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Här är en dolda titt på dina resurser efter fasen Förbered hello har slutförts. Observera att hello resursen är hello dataplan är hello samma. Den representeras både hello management plan (klassiska distributionsmodellen) och hello kontrollplan (Resource Manager).

![Hello bakgrunden i fasen Förbered](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Virtuella datorer som inte ingår i ett klassiskt virtuellt nätverk är stoppad-frigjorts i den här fasen av migreringen.
>

### <a name="check-manual-or-scripted"></a>Kontrollera (manuellt eller via skript)
I hello Kontrollera steg kan du alternativt använda hello-konfiguration som du hämtade tidigare toovalidate att hello migrering verkar vara korrekta. Du kan också logga in toohello portal och plats hello egenskaper och resurser för toovalidate snygg metadata migrering.

Om du migrerar ett virtuellt nätverk startas inte de flesta konfigurationer av virtuella datorer om. För program på dessa virtuella datorer kan du verifiera att hello program är fortfarande aktiv och körs.

Du kan testa din övervakning och-automatisering och operativa skript toosee om hello virtuella datorer som fungerar som förväntat och uppdaterade skripten fungerar korrekt. Endast GET-åtgärder stöds när hello resurser i hello förberedd tillstånd.

Det finns inget set tidsfönster som du behöver toocommit hello migrering. Du kan ta den tid du vill i det här läget. Dock låses hello management plan för dessa resurser förrän du avbrytas eller genomföras.

Om du ser några problem du alltid avbryta hello migrering och gå tillbaka toohello klassiska distributionsmodellen. När du går tillbaka så hello Azure-plattformen öppnas hello management-plan åtgärder på hello resurser så att du kan återuppta normal drift på dessa virtuella datorer i hello klassiska distributionsmodellen.

### <a name="abort"></a>Avbryta
Avbryt är ett valfritt steg som du kan använda toorevert dina ändringar toohello klassiska distributionsmodellen och stoppa hello migrering. Den här åtgärden tar bort hello Resource Manager-metadata för dina resurser som har skapats i hello tidigare förberedelsesteget. 

![Hello bakgrunden i avbrytningsfasen](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Den här åtgärden kan inte utföras efter att du har utlösts hello commit-åtgärden.     
>

### <a name="commit"></a>Checka in
När du har slutfört hello verifiering genomföra du hello migrering. Resurser visas inte längre i den klassiska distributionsmodellen och är bara tillgängliga i hello Resource Manager-modellen. hello kan migrerade resurserna hanteras endast i nya hello-portalen.

> [!NOTE]
> Det här är en idempotent åtgärd. Om det misslyckas, bör du försöka hello igen. Om det fortfarande toofail skapa ett supportärende eller skapa ett forum post med en ClassicIaaSMigration tagg på vår [VM forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![Hello bakgrunden i allokeringsfasen](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="where-toobegin-migration"></a>Där toobegin migrering?

Här är ett flödesschema som visar hur tooproceed med migrering

![Skärmbild som visar hello migreringssteg](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-deployment-model-tooazure-resource-manager-resources"></a>Översättning av klassisk distribution modellen tooAzure Resource Manager-resurser
Du hittar hello klassiska distributionsmodellen och Resource Manager-representation av hello resurser i hello i den följande tabellen. Andra funktioner och resurser stöds för närvarande inte.

| Klassiskt läge | Resource Manager-läge | Detaljerad information |
| --- | --- | --- |
| Molntjänstens namn |DNS-namn |Under migreringen, skapas en ny resursgrupp för varje tjänst i molnet med mönstret för hello `<cloudservicename>-migrated`. Den här resursgruppen innehåller alla dina resurser. Hej molntjänstnamnet blir ett DNS-namn som är associerad med hello offentlig IP-adress. |
| Virtuell dator |Virtuell dator |Egenskaper för virtuella datorer migreras oförändrade. Vissa osProfile information, t.ex. datornamn, lagras inte i hello klassiska distributionsmodellen och förblir tom efter migreringen. |
| Diskresurser kopplade tooVM |Implicit diskar anslutna tooVM |Diskar modelleras inte som översta resurser i hello Resource Manager-distributionsmodellen. De migreras som implicit diskar under hello VM. Diskar som är kopplade tooa VM stöds för närvarande. Hanteraren för virtuella datorer kan nu använda klassiska lagringskonton, vilket gör att hello diskar toobe enkelt migreras utan några uppdateringar. |
| VM-tillägg |VM-tillägg |Alla hello resurstillägg, utom XML-tillägg, migreras från hello klassiska distributionsmodellen. |
| Certifikat för virtuell dator |Certifikat i Azure Key Vault |Om en tjänst i molnet innehåller tjänstcertifikat, en ny Azure nyckelvalv per tjänst i molnet och flyttar hello certifikat till hello nyckelvalvet. hello virtuella datorer är uppdaterade tooreference hello certifikat från hello nyckelvalvet. <br><br> **Obs:** ta inte bort hello keyvault eftersom det kan medföra hello VM toogo i ett felaktigt tillstånd. Vi arbetar på att förbättra saker i hello backend så att nyckeln valv kan tas bort på ett säkert sätt eller flyttas tillsammans med hello VM tooa ny prenumeration. |
| WinRM-konfiguration |WinRM-konfiguration under osProfile |Windows Remote Management-konfiguration flyttas oförändrat som en del av hello migrering. |
| Egenskap för tillgänglighetsuppsättning |Resurs för tillgänglighetsuppsättning | Tillgänglighetsuppsättningen specifikationen var en egenskap på hello VM i hello klassiska distributionsmodellen. Tillgänglighetsuppsättningar bli en översta resurs som en del av hello migrering. hello följande konfigurationer stöds inte: flera tillgänglighet per tjänst i molnet, eller anger en eller flera tillgänglighetsuppsättningar tillsammans med virtuella datorer som inte ingår i någon tillgänglighetsuppsättning i en tjänst i molnet. |
| Nätverkskonfiguration på en virtuell dator |Primärt nätverksgränssnitt |Nätverkskonfigurationen på en virtuell dator representeras som hello primära gränssnittet nätverksresurs efter migreringen. För virtuella datorer som inte ingår i ett virtuellt nätverk, hello interna IP-adressen ändras under migreringen. |
| Flera nätverksgränssnitt på en virtuell dator |Nätverksgränssnitt |Om en virtuell dator har flera nätverksgränssnitt som är kopplade till den, blir en översta resurs som en del av hello migrering i hello Resource Manager-distributionsmodellen, tillsammans med alla hello egenskaper i varje nätverksgränssnitt. |
| Belastningsutjämnad slutpunktsuppsättning |Belastningsutjämnare |Hello klassiska distributionsmodellen tilldelas hello plattform en implicit belastningsutjämnare för varje tjänst i molnet. Under migreringen, skapas en ny resurs belastningsutjämnare och hello belastningsutjämning slutpunktsuppsättning blir belastningsutjämnare regler. |
| Ingående NAT-regler |Ingående NAT-regler |Inkommande slutpunkter som definierats på hello VM är konverterade tooinbound network adress translation regler under hello belastningsutjämnare under hello migreringen. |
| Virtuell ip-adress |Offentlig ip-adress med DNS-namn |hello virtuella IP-adressen blir en offentlig IP-adress och är associerad med hello belastningsutjämnaren. En virtuell IP-adress kan endast migreras om det finns en slutpunkt för indata som tilldelats tooit. |
| Virtuellt nätverk |Virtuellt nätverk |hello virtuella nätverk migreras med alla egenskaper, toohello Resource Manager-modellen. Skapas en ny resursgrupp med namnet hello `-migrated`. |
| Reserverade ip-adresser |Offentlig ip-adress med fast fördelningsmetod |Reserverade IP-adresser som är associerade med belastningsutjämnaren hello migreras tillsammans med hello migrering av hello molntjänst eller hello virtuell dator. För närvarande finns inte stöd för migrering av reserverade ip-adresser utan koppling. |
| Offentlig ip-adress per virtuell dator |Offentlig ip-adress med dynamisk fördelningsmetod |hello offentlig IP-adress som är associerade med hello VM konverteras till en offentlig IP-adressresurs, med hello allokering metoden set toostatic. |
| NSG:er |NSG:er |Nätverkssäkerhetsgrupper som är associerad med ett undernät klonas som en del av hello migrering toohello Resource Manager-modellen. hello NSG i hello klassiska distributionsmodellen tas inte bort under hello migreringen. Hello management-plan åtgärder för hello NSG blockeras när hello migrering pågår. |
| DNS-servrar |DNS-servrar |DNS-servrar som är associerade med ett virtuellt nätverk eller hello VM migreras som en del av hello motsvarande resursmigrering tillsammans med alla hello egenskaper. |
| Användardefinierade vägar |Användardefinierade vägar |Användardefinierade vägar som är associerade med ett undernät klonas som en del av hello migrering toohello Resource Manager-modellen. Hej UDR i hello klassiska distributionsmodellen tas inte bort under hello migreringen. hello management-plan åtgärder för hello UDR blockeras när hello migrering pågår. |
| Egenskapen ip-vidarebefordran i en virtuell dators nätverkskonfiguration |IP-vidarebefordring egenskapen hello NIC |hello IP-vidarebefordring egenskapen på en virtuell dator är konverterade tooa egenskap i hello nätverksgränssnitt under hello migreringen. |
| Belastningsutjämnare med flera ip-adresser |Belastningsutjämnare med flera offentliga ip-resurser |Varje offentlig IP-adress som är kopplade till belastningsutjämnare hello är konverterade tooa offentliga IP-resurs och som är kopplade till belastningsutjämnare hello efter migreringen. |
| Den interna DNS-namn på hello VM |Den interna DNS-namn på hello NIC |Under migreringen är hello interna DNS-suffix för hello virtuella datorer migrerade tooa skrivskyddad egenskap med namnet ”InternalDomainNameSuffix” på hello NIC. hello suffix förblir oförändrad efter migreringen och VM-lösning bör fortsätta toowork som tidigare. |
| Virtuell nätverksgateway |Virtuell nätverksgateway |Egenskaperna för virtuell nätverksgateway migreras oförändrade. hello VIP som är associerade med hello gateway ändras inte heller. |
| Lokal nätverksplats |Lokal nätverksgateway |Egenskaper för lokalt nätverk är migrerade oförändrat tooa ny resurs som kallas lokala nätverkets Gateway. Det här motsvarar lokala adressprefix och fjärrgateway-ip. |
| Anslutningsreferenser |Anslutning |Anslutningsreferenser mellan gateway och lokal nätverksplats i nätverkskonfigurationen visas som en nyskapad resurs med namnet Connection i Resource Manager efter migreringen. Alla egenskaper för anslutning referens i nätverket konfigurationsfiler är kopierade oförändrat toohello nyskapad anslutning resurs. VNet tooVNet anslutning i klassiskt uppnås genom att skapa två IPsec-tunnlar toolocal nätverksplatser som representerar hello Vnet. Det här är transformerad tooVnet2Vnet anslutning typ i resource manager-modellen utan lokala nätverksgatewayer. |

## <a name="changes-tooyour-automation-and-tooling-after-migration"></a>Ändrar tooyour automation och verktygsuppsättning efter migreringen
Som en del av migreringen dina resurser från hello klassiska modellen toohello Resource Manager distribution distributionsmodellen kan ha tooupdate din befintliga automation eller verktygsuppsättning tooensure den fortsätter toowork efter hello migreringen.
