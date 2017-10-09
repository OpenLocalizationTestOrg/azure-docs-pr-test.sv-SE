
Om din Azure problemet inte åtgärdas i den här artikeln, gå hello [Azure-forum på MSDN och Stack Overflow](https://azure.microsoft.com/support/forums/). Du kan publicera problemet på dessa forum eller too@AzureSupport på Twitter. Du kan också filen en Azure-supportbegäran genom att välja **få support** på hello [Azure-supporten](https://azure.microsoft.com/support/options/) plats.

## <a name="general-troubleshooting-steps"></a>Allmänna instruktioner för felsökning
### <a name="troubleshoot-common-allocation-failures-in-hello-classic-deployment-model"></a>Felsöka vanliga Tilldelningsfel i hello klassiska distributionsmodellen
Dessa steg hjälper dig att lösa många buffertallokeringsfel på virtuella datorer:

* Ändra storlek på hello-VM tooa annan VM-storlek.<br>
    Klicka på **Bläddra igenom alla** > **virtuella datorer (klassisk)** > din virtuella dator > **inställningar** > **storlek**. Detaljerade anvisningar finns i [ändra storlek på virtuella hello](https://msdn.microsoft.com/library/dn168976.aspx).
* Ta bort alla virtuella datorer från hello-Molntjänsten och skapa virtuella datorer.<br>
    Klicka på **Bläddra igenom alla** > **virtuella datorer (klassisk)** > din virtuella dator > **ta bort**. Klicka på **ny** > **Compute** > [avbildning av virtuell dator].

### <a name="troubleshoot-common-allocation-failures-in-hello-azure-resource-manager-deployment-model"></a>Felsöka vanliga Tilldelningsfel i hello Azure Resource Manager-distributionsmodellen
Dessa steg hjälper dig att lösa många buffertallokeringsfel på virtuella datorer:

* Stoppa (frigöra) alla virtuella datorer i hello samma tillgänglighet ange och sedan starta om var och en.<br>
    toostop: Klicka på **resursgrupper** > resursgruppen > **resurser** > din tillgänglighetsuppsättning > **virtuella datorer** > din virtuella dator >  **Stoppa**.
  
    När alla virtuella datorer har slutat, Välj hello första virtuella datorn och klicka på **starta**.

## <a name="background-information"></a>Bakgrundsinformation
### <a name="how-allocation-works"></a>Så här fungerar allokering
hello-servrar i Azure-Datacenter partitioneras i kluster. Normalt en begäran om minnesallokering görs i flera kluster, men det är möjligt att vissa villkor från hello allokeringsbegäran gällande hello Azure-plattformen tooattempt hello begäran i ett kluster. I den här artikeln ska vi refererar toothis som ”Fäst tooa cluster”. Bild 1 nedan visar hello skiftläget för en normal allokering som görs i flera kluster. Figur 2 visar hello fallet för en allokering som har fäst tooCluster 2 eftersom det är där hello befintliga Cloud Service CS_1 eller tillgänglighet körs.
![Allokering av Diagram](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Varför Allokeringsfel inträffa
När en begäran om minnesallokering är fäst tooa klustret, finns det en högre risken för felaktiga toofind lediga resurser eftersom hello tillgängliga resurspool är mindre. Dessutom misslyckas din begäran även om hello klustret har lediga resurser om din begäran om minnesallokering är fäst tooa kluster men hello typ av resurs som du har begärt stöds inte av klustret. Bild 3 nedan visar hello fall där en fast allokering misslyckas eftersom hello endast candidate klustret inte har lediga resurser. Diagram 4 illustrerar hello fall där en fast allokering misslyckas eftersom hello endast kandidat kluster inte stöder hello begärda VM-storlek, även om hello klustret har frigöra resurser.

![Fäst Allokeringsfel](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-hello-classic-deployment-model"></a>Detaljerad felsökning av steg allokering av specifika scenarier i hello klassiska distributionsmodellen
Här följer vanliga scenarier för allokering som orsakar en allokering begäran toobe Fäst. Vi kommer fördjupa dig i varje scenario senare i den här artikeln.

* Ändra storlek på en virtuell dator eller Lägg till virtuella datorer eller rollen instanser tooan befintlig molntjänst
* Starta om delvis stoppats (frigjorts) virtuella datorer
* Starta om fullständigt stoppats (frigjorts) virtuella datorer
* Förproduktion/Produktionsdistribution (plattform som en tjänst endast)
* Tillhörighetsgruppen (VM-tjänsten närhet)
* Mappning mellan en gruppbaserad virtuellt nätverk

När du får ett Allokeringsfel se om någon av hello scenarier beskrivs gäller tooyour fel. Använd hello Allokeringsfel som returneras av hello Azure-plattformen tooidentify hello motsvarande scenario. Om din begäran är fäst, ta bort några av hello fästa begränsningar tooopen din begäran toomore kluster, vilket ökar hello sannolikheten att allokering lyckas.

I allmänhet så länge hello fel inte anger ”hello begärda VM-storlek inte stöds”, du kan alltid försök vid ett senare tillfälle som har tillräckligt med resurser för frigöras hello klustret tooaccommodate din begäran. Om hello problemet är att hello begärde inte finns stöd för VM-storlek, prova en annan VM-storlek. Annars är hello endast alternativet tooremove hello fästning begränsningen.

Två vanliga scenarier är relaterade tooaffinity grupper. En tillhörighetsgrupp har använt tooprovide nära tooVMs/tjänstinstanser i hello tidigare, eller så har använt tooenable hello skapandet av ett virtuellt nätverk. Med hello introduktionen av regionala virtuella nätverk är tillhörighetsgrupper inte längre krävs toocreate ett virtuellt nätverk. Med hello minskning av svarstid för nätverk i Azure-infrastrukturen, hello rekommendation toouse tillhörighetsgrupper för VM-tjänsten närhet har ändrats.

Diagram över 5 nedan visar hello taxonomi hello (fast) fördelning scenarier.
![Fäst allokering taxonomi](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [!NOTE]
> hello fel i varje allokering scenario är en kort form. Se toohello [sträng felsökning](#Error string lookup) för detaljerade felsträngar.
> 
> 

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-tooan-existing-cloud-service"></a>Allokering av scenario: ändra storlek på en virtuell dator eller Lägg till virtuella datorer eller roll instanser tooan befintlig molntjänst
**Fel**

Upgrade_VMSizeNotSupported eller GeneralError

**Orsaken till klustret fästning**

En begäran om tooresize en virtuell dator eller lägga till en virtuell dator eller en roll instans tooan befintlig molntjänst har toobe med hello ursprungliga klustret som är värd för hello befintlig molntjänst. Skapa en ny molntjänst kan hello Azure-plattformen toofind ett annat kluster som har lediga resurser eller stöder hello VM-storlek som du har begärt.

**Lösning**

Om hello fel är Upgrade_VMSizeNotSupported *, kan du prova en annan VM-storlek. Om med en annan VM-storlek inte är ett alternativ, men om den är godkänd toouse en annan virtuell IP-adress (VIP) kan du skapa en ny cloud service toohost hello ny virtuell dator och Lägg till hello nya cloud service toohello regionalt virtuellt nätverk där hello befintliga virtuella datorer som körs. Om en befintlig molntjänst inte använder ett regionalt virtuellt nätverk, du kan fortfarande skapa ett nytt virtuellt nätverk för hello ny molntjänst och ansluter din [befintliga virtuella nätverk toohello nytt virtuellt nätverk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Läs mer [regionala virtuella nätverk](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Om hello fel GeneralError *, är det troligt att hello typ av resurs (till exempel en viss VM-storlek) stöds av hello kluster, men hello klustret inte har lediga resurser hello tillfället. Liknande toohello ovan scenario lägger till hello önskad beräkningsresurser genom att skapa en ny molntjänst (Observera att hello ny molntjänst har toouse på en annan VIP) och använder ett regionalt virtuellt nätverk tooconnect dina molntjänster.

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Allokering av scenario: starta om delvis stoppats (frigjorts) virtuella datorer
**Fel**

GeneralError *

**Orsaken till klustret fästning**

Flyttningen är delvis innebär att du har stoppats (frigjorts) en eller mer, men inte alla virtuella datorer i en molntjänst. När du stoppar (frigöra) en VM, hello associerade resurser släpps. Starta om den stoppats (frigjorts) VM är därför en ny begäran om minnesallokering. Starta om virtuella datorer i en delvis frigjord molntjänst är likvärdiga tooadding VMs tooan befintlig molntjänst. hello allokeringsbegäran har toobe med hello ursprungliga klustret som är värd för hello befintlig molntjänst. Skapa en annan molntjänst kan hello Azure-plattformen toofind ett annat kluster som har fri resurs eller stöder hello VM-storlek som du har begärt.

**Lösning**

Om du accepterar toouse en annan VIP, ta bort hello stoppats (frigjorts) virtuella datorer (men behåll hello associerade diskar) och Lägg till hello VMs tillbaka via en annan molntjänst. Använd ett regionalt virtuellt nätverk tooconnect dina molntjänster:

* Om en befintlig molntjänst använder ett regionalt virtuellt nätverk, lägger du till hello nya cloud service toohello samma virtuella nätverk.
* Om en befintlig molntjänst inte använder ett regionalt virtuellt nätverk, skapa ett nytt virtuellt nätverk för hello ny molntjänst och sedan [ansluta dina befintliga virtuella nätverk toohello nytt virtuellt nätverk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Läs mer [regionala virtuella nätverk](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>Allokering av scenario: starta om fullständigt stoppats (frigjorts) virtuella datorer
**Fel**

GeneralError *

**Orsaken till klustret fästning**

Fullständig flyttningen innebär att du har stoppats frigjorts () alla virtuella datorer från en tjänst i molnet. hello allokering begär toorestart dessa virtuella datorer har toobe med hello ursprungliga klustret som är värd för hello-Molntjänsten. Skapa en ny molntjänst kan hello Azure-plattformen toofind ett annat kluster som har lediga resurser eller stöder hello VM-storlek som du har begärt.

**Lösning**

Om den är godkänd toouse en annan VIP, ta bort hello ursprungliga stoppats (frigjorts) virtuella datorer (men behåll hello associerade diskar) och ta bort hello motsvarande tjänst i molnet (hello associerade beräkning resurser har redan getts ut när du stoppats (frigjorts) hello virtuella datorer). Skapa en ny cloud service tooadd hello VMs tillbaka.

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>Allokering av scenario: mellanlagring/Produktionsdistribution (plattform som en tjänst endast)
**Fel**

New_General * eller * New_VMSizeNotSupported

**Orsaken till klustret fästning**

hello mellanlagring av distribution och hello Produktionsdistribution av en tjänst i molnet finns i hello samma kluster. När du lägger till hello andra distribution görs hello motsvarande allokeringsbegäran i samma kluster som är värd för hello första distributionen hello.

**Lösning**

Ta bort hello första distributionen och hello ursprungliga Molntjänsten och omdistribuera hello-Molntjänsten. Den här åtgärden kan hamna hello första distributionen i ett kluster som har tillräckligt med lediga resurser toofit båda distributionerna eller i ett kluster som har stöd för hello VM-storlekar som du har begärt.

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>Allokering av scenario: tillhörighetsgrupp (VM-tjänsten närhet)
**Fel**

New_General * eller * New_VMSizeNotSupported

**Orsaken till klustret fästning**

Alla beräkningsresurser tilldelade tooan tillhörighetsgruppen är kopplad tooone klustret. Nya beräkning resurs begäranden i den tillhörighetsgruppen görs i samma kluster där hello befintliga resurser finns hello. Detta är SANT om hello nya resurser skapas via en ny molntjänst eller en befintlig molntjänst.

**Lösning**

Om en tillhörighetsgrupp inte är nödvändigt, inte använda en tillhörighetsgrupp, eller gruppera dina beräkningsresurser i flera tillhörighetsgrupper.

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>Allokering av scenario: tillhörighet-grupp-baserade virtuella nätverk
**Fel**

New_General * eller * New_VMSizeNotSupported

**Orsaken till klustret fästning**

Innan regionala virtuella nätverk har introducerats har nödvändiga tooassociate ett virtuellt nätverk med en tillhörighetsgrupp. Därför beräkningsresurser som placeras i en tillhörighetsgrupp är bundna av hello samma begränsningar som beskrivs i hello ”allokering scenario: tillhörighetsgrupp (VM-tjänsten närhet)” ovan. hello beräkningsresurser är bundet tooone klustret.

**Lösning**

Om du inte behöver en tillhörighetsgrupp, skapa ett nytt regionalt virtuellt nätverk för hello nya resurser som du vill lägga till, och sedan [ansluta dina befintliga virtuella nätverk toohello nytt virtuellt nätverk](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Läs mer [regionala virtuella nätverk](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Du kan också [migrera din tillhörighet-grupp-baserade virtuella nätverk tooa regionalt virtuellt nätverk](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), och Lägg sedan till önskad hello resurser.

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-hello-azure-resource-manager-deployment-model"></a>Detaljerad felsökning steg allokering av specifika scenarier i hello Azure Resource Manager-distributionsmodellen
Här följer vanliga scenarier för allokering som orsakar en allokering begäran toobe Fäst. Vi kommer fördjupa dig i varje scenario senare i den här artikeln.

* Ändra storlek på en virtuell dator eller Lägg till virtuella datorer eller rollen instanser tooan befintlig molntjänst
* Starta om delvis stoppats (frigjorts) virtuella datorer
* Starta om fullständigt stoppats (frigjorts) virtuella datorer

När du får ett Allokeringsfel se om någon av hello scenarier beskrivs gäller tooyour fel. Använd hello Allokeringsfel som returneras av hello Azure-plattformen tooidentify hello motsvarande scenario. Om din begäran är fäst tooan befintligt kluster, ta bort några av hello fästa begränsningar tooopen din begäran toomore kluster, vilket ökar hello sannolikheten att allokering lyckas.

I allmänhet så länge hello fel inte anger ”hello begärda VM-storlek inte stöds”, du kan alltid försök vid ett senare tillfälle som har tillräckligt med resurser för frigöras hello klustret tooaccommodate din begäran. Om hello problemet är att hello begärda VM-storlek inte stöds, kan du se nedan för lösningar.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-tooan-existing-availability-set"></a>Allokering av scenario: ändra storlek på en virtuell dator eller Lägg till virtuella datorer tooan befintlig tillgänglighetsuppsättning
**Fel**

Upgrade_VMSizeNotSupported * eller * GeneralError

**Orsaken till klustret fästning**

En begäran tooresize en virtuell dator eller Lägg till en VM-tooan befintliga tillgänglighetsuppsättning har toobe med hello ursprungliga klustret som är värd för hello befintlig tillgänglighetsuppsättning. Skapa en ny tillgänglighetsuppsättning kan hello Azure-plattformen toofind ett annat kluster som har lediga resurser eller stöder hello VM-storlek som du har begärt.

**Lösning**

Om hello fel är Upgrade_VMSizeNotSupported *, kan du prova en annan VM-storlek. Stoppa alla virtuella datorer i tillgänglighetsuppsättningen hello om med en annan VM-storlek inte är ett alternativ. Därefter kan du ändra hello storlek för hello virtuell dator som kommer att allokera hello VM tooa klustret som stöder hello önskad VM-storlek.

Om hello fel GeneralError *, är det troligt att hello typ av resurs (till exempel en viss VM-storlek) stöds av hello kluster, men hello klustret inte har lediga resurser hello tillfället. Om hello VM kan vara en del av en annan tillgänglighetsuppsättning, skapa en ny virtuell dator i en annan tillgänglighetsuppsättning (i hello samma region). Den här nya virtuella datorn kan sedan läggas toohello samma virtuella nätverk.  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Allokering av scenario: starta om delvis stoppats (frigjorts) virtuella datorer
**Fel**

GeneralError *

**Orsaken till klustret fästning**

Flyttningen är delvis innebär att du har stoppats (frigjorts) en eller flera, men inte alla, virtuella datorer i en tillgänglighetsgrupp. När du stoppar (frigöra) en VM, hello associerade resurser släpps. Starta om den stoppats (frigjorts) VM är därför en ny begäran om minnesallokering. Starta om virtuella datorer i en delvis frigjord tillgänglighetsuppsättning anges motsvarande tooadding befintliga tillgänglighet för virtuella datorer tooan. hello allokeringsbegäran har toobe med hello ursprungliga klustret som är värd för hello befintlig tillgänglighetsuppsättning.

**Lösning**

Stoppa alla virtuella datorer i hello tillgänglighetsuppsättning innan du startar om hello första. Detta garanterar att ett nytt försök för allokering körs och att ett nytt kluster kan väljas som har tillgänglig kapacitet.

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>Allokering av scenario: starta om fullständigt stoppats (frigjorts)
**Fel**

GeneralError *

**Orsaken till klustret fästning**

Fullständig flyttningen innebär att du har stoppats frigjorts () alla virtuella datorer i en tillgänglighetsuppsättning. hello allokeringsbegäran toorestart dessa virtuella datorer gäller alla kluster som har stöd för hello önskad storlek.

**Lösning**

Välj en ny tooallocate för VM-storlek. Försök igen senare om det inte fungerar.

## <a name="error-string-lookup"></a>Felsökning av sträng
**New_VMSizeNotSupported***

”Hej VM storlek (eller kombinationer av VM-storlekar) krävs av den här distributionen kan inte etableras på grund av toodeployment begränsningar för begäran. Om möjligt försök släppa på begränsningarna, t.ex virtuella nätverksbindningar, distribution av tooa värdbaserad tjänst med ingen distribution i den ingående och tooa olika tillhörighet grupp- eller utan någon tillhörighetsgrupp, eller försök distribuera tooa annan region ”.

**New_General***

”Gick inte att allokera; Det går inte toosatisfy begränsningar i begäran. hello begärda nya tjänstdistributionen är bunden tooan tillhörighetsgrupp, eller den riktar sig till ett virtuellt nätverk eller det finns en befintlig distribution under den här värdbaserade tjänsten. Dessa omständigheter begränsar hello ny distribution toospecific Azure resurser. Försök igen senare eller försök att minska hello VM-storlek eller antalet rollinstanser. Alternativt, om möjligt ta bort hello ovan nämnda begränsningarna eller försök att distribuera tooa annan region ”.

**Upgrade_VMSizeNotSupported***

”Det gick inte tooupgrade hello distribution. hello begärda VM-storlek XXX inte kanske är tillgängliga i hello resurser som stödjer hello befintlig distribution. Försök igen senare, försök med en annan VM-storlek eller ett mindre antal rollinstanser eller skapa en distribution under en tom värdtjänst med en ny tillhörighetsgrupp eller utan tillhörighetsgruppsbindning ”.

**GeneralError***

”Hej servern påträffade ett internt fel. Försök hello begäran ”. Eller ”det gick inte tooproduce fördelningen av tjänsten hello”.

