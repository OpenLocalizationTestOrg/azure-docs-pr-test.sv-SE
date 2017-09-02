Den här artikeln beskrivs en uppsättning beprövade metoder för att köra en Windows-dator (VM) på Azure, med hänsyn till skalbarhet, tillgänglighet, hanterbarhet och säkerhet.

> [!NOTE]
> Azure har två olika distributionsmodeller: [Azure Resource Manager] [ resource-manager-overview] och klassisk. Den här artikeln använder Resurshanteraren, som Microsoft rekommenderar för nya distributioner.
>
>

Du bör inte använda en enskild virtuell dator för verksamhetskritiska arbetsbelastningar, eftersom den skapar en enskild felkälla. För högre tillgänglighet kan du distribuera flera virtuella datorer i en [tillgänglighetsuppsättning][availability-set]. Mer information finns i [Köra flera virtuella datorer på Azure][multi-vm].

## <a name="architecture-diagram"></a>Arkitekturdiagram

Att etablera en virtuell dator i Azure innebär att flytta fler delar än bara själva det virtuella nätverket. Det finns beräkning, nätverk och lagring-element.

> Ett Visio-dokument som innehåller det här arkitekturdiagrammet finns tillgängligt för hämtning från [Microsofts hämtningscenter][visio-download]. Det här diagrammet finns på sidan ”Beräkna – enskild virtuell dator”.
>
>

![[0]][0]

* **Resursgrupp.** En [*resursgrupp*][resource-manager-overview] är en behållare som innehåller närliggande resurser. Skapa en resursgrupp för att lagra resurser för den här virtuella datorn.
* **Virtuell dator**. Du kan etablera en virtuell dator från en lista över publicerade avbildningar eller från en virtuell hårddiskfil (VHD) som du överför till Azure Blob Storage.
* **OS-disk.** OS-disken är en virtuell hårddisk som lagras i [Azure Storage][azure-storage]. Det innebär att den finns kvar även om värddatorn stängs av.
* **Tillfällig disk.** Den virtuella datorn skapas med en tillfälliga disk (den `D:` Windows-enheten). Den här disken lagras på en fysisk enhet på värddatorn. Det sparas *inte* i Azure Storage och kan tas bort under omstarter och andra livscykelhändelser för virtuella datorer. Använd enbart den här disken för tillfälliga data, till exempel växlingsfiler.
* **Datadiskar.** En [datadisk][data-disk] är en permanent virtuell hårddisk för programdata. Datadiskar lagras i Azure Storage, som OS-disken.
* **Virtuellt nätverk och undernät.** Varje virtuell dator i Azure distribueras till ett virtuellt nätverk som delas upp ytterligare i undernät.
* **Offentlig IP-adress.** En offentlig IP-adress behövs för att kommunicera med den virtuella datorn&mdash;till exempel via fjärrskrivbord (RDP).
* **Nätverksgränssnitt**. Nätverkskortet som gör det möjligt för den virtuella datorn att kommunicera med det virtuella nätverket.
* **Nätverkssäkerhetsgrupp (NSG)**. [NSG][nsg] används för att tillåta/neka nätverkstrafik till undernätet. Du kan associera en NSG med ett enskilt nätverkskort eller ett undernät. Om du kopplar den till ett undernät gäller NSG-reglerna för alla virtuella datorer i det här undernätet.
* **Diagnostik.** Diagnostisk loggning är avgörande för att hantera och felsöka den virtuella datorn.

## <a name="recommendations"></a>Rekommendationer

Följande rekommendationer gäller för de flesta scenarier. Följ dessa rekommendationer om du inte har ett visst krav som åsidosätter dem.

### <a name="vm-recommendations"></a>Rekommendationer för virtuella datorer

Azure erbjuder många olika virtuella datorstorlekar, men vi rekommenderar DS - och GS-serien eftersom dessa datorstorlekar har stöd för [Premium Storage][premium-storage]. Välj en av dessa datorer såvida du inte har en särskild arbetsbelastning som databehandling med höga prestanda. Mer information finns i [Storlekar för virtuella datorer][virtual-machine-sizes].

Om du flyttar en befintlig arbetsbelastning till Azure börjar du med den virtuella datorstorlek som närmast motsvarar dina lokala servrar. Mät sedan prestanda hos din faktiska arbetsbelastning med avseende på processor, minne, och i/o-åtgärder för disken per sekund (IOPS) och justera storleken om det behövs. Om du behöver flera nätverkskort för den virtuella datorn måste du vara medveten om att det högsta antalet nätverkskort är en funktion av den [virtuella datorstorleken][vm-size-tables].   

När du etablerar den virtuella datorn och andra resurser måste du ange en region. Vanligtvis väljer du en region som är närmast dina interna användare eller kunder. Inte alla VM-storlekar kan dock finnas tillgänglig i alla regioner. Mer information finns i [tjänster efter region][services-by-region]. Om du vill se en lista över storlek på Virtuella datorer finns i en viss region, kör du följande kommando för Azure-kommandoradsgränssnittet (CLI):

```
azure vm sizes --location <location>
```

Information om hur du väljer en publicerad virtuell datoravbildning finns [navigera och välj virtuella Windows-avbildningar i Azure med Powershell eller CLI][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Disk- och lagringsrekommendationer

För bästa diskprestanda-i/o, rekommenderar vi [Premiumlagring][premium-storage], som lagrar data på solid state-hårddiskar (SSD). Kostnaden baseras på storleken på den allokerade disken. IOPS och genomströmning beror också på diskutrymme, så när du etablerar en disk, Tänk alla tre (kapacitet, IOPS och genomströmning).

Skapa separata Azure-lagringskonton för varje virtuell dator för att lagra de virtuella hårddiskarna (VHD) för att undvika att IOPS-gränserna för lagringskonton överskrids.

Lägg till en eller flera datadiskar. När du skapar en ny virtuell Hårddisk är den oformaterade. Logga in på den virtuella datorn att formatera disken. Om du har ett stort antal datadiskar bör du tänka på de totala i/o-gränserna för lagringskontot. Mer information finns i [diskgränser för virtuella datorer][vm-disk-limits].

När det är möjligt ska du installera program på en datadisk, inte OS-disken. Vissa äldre program kan behöva installera komponenterna på enhet C:. I så fall kan du [ändra storleken på operativsystemdisken] [ resize-os-disk] med hjälp av PowerShell.

Skapa ett separat lagringskonto för diagnostikloggar för bästa prestanda. Ett standardkonto för lokalt redundant lagring (LRS) är tillräckligt för diagnostikloggar.

### <a name="network-recommendations"></a>Nätverksrekommendationer

Den offentliga IP-adressen kan vara dynamisk eller statisk. Standardvärdet är dynamiskt.

* Reservera en [statisk IP-adress][static-ip] om du behöver en fast IP-adress som inte ändras &mdash; till exempel, om du behöver skapa en A-post i DNS eller behöver lägga till IP-adressen i listan över säkra adresser.
* Du kan också skapa ett fullständigt domännamn (FQDN) för IP-adressen. Du kan sedan registrera en [CNAME-post][cname-record] i DNS som pekar på det fullständiga domännamnet. Mer information finns i [Skapa ett fullständigt domännamn i Azure Portal][fqdn].

Alla nätverkssäkerhetsgrupper innehåller en uppsättning [standardregler][nsg-default-rules], inklusive en regel som blockerar all inkommande Internettrafik. Standardreglerna kan inte tas bort, men andra regler kan åsidosätta dem. Om du vill aktivera Internettrafik skapar du regler som tillåter inkommande trafik till specifika portar &mdash; till exempel port 80 för HTTP.  

Lägg till en NSG-regel som tillåter inkommande trafik till TCP-port 3389 för att aktivera RDP.

## <a name="scalability-considerations"></a>Skalbarhetsöverväganden

Du kan skala en VM uppåt eller nedåt genom [ändrar storlek på Virtuella](../articles/virtual-machines/windows/sizes.md). Om du vill skala ut vågrätt placerar du två eller flera virtuella datorer i en tillgänglighetsuppsättning bakom en belastningsutjämnare. Mer information finns i [flera virtuella datorer som körs på Azure för skalbarhet och tillgänglighet][multi-vm].

## <a name="availability-considerations"></a>Överväganden för tillgänglighet

Distribuera flera virtuella datorer i en tillgänglighetsuppsättning om du vill ha högre tillgänglighet. Det ger också en högre [servicenivåavtal] [ vm-sla] (SLA).

Din virtuella dator kan påverkas av [planerat underhåll] [ planned-maintenance] eller [oplanerat underhåll][manage-vm-availability]. Du kan använda [omstartsloggarna för virtuella datorer][reboot-logs] för att bedöma om en omstart av en virtuell dator orsakades av planerat underhåll.

Virtuella hårddiskar lagras i [Azure Storage][azure-storage], och Azure-lagringsutrymmet replikeras för hållbarhet och tillgänglighet.

För att skydda mot dataförluster under normal drift (t.ex, på grund av fel från användarens sida), bör du även implementera säkerhetskopieringar vid vissa tidpunkter med [blob-ögonblicksbilder][blob-snapshot] eller något annat verktyg.

## <a name="manageability-considerations"></a>Överväganden för hantering

**Resursgrupper.** Placera direkt kopplade resurser som delar samma livscykel i samma [resursgruppen][resource-manager-overview]. Resursgrupper kan du distribuera och övervaka resurser som en grupp och dyker upp fakturering kostnader med resursgrupp. Du kan också ta bort resurser som en uppsättning, vilket är mycket användbart vid testdistributioner. Ge resurserna meningsfulla namn. Detta gör det lättare att hitta en specifik resurs och att förstå dess roll. Se [Rekommenderade namnkonventioner för Azure-resurser][naming conventions].

**Diagnostik av virtuella datorer.** Aktivera övervakning och diagnostik, inklusive grundläggande hälsomätvärden, diagnostikinfrastrukturloggar och [startdiagnostik][boot-diagnostics]. Startdiagnostikinställningar hjälper dig att diagnostisera ett startfel om den virtuella datorn hämtar i ett tillstånd kan. Mer information finns i [Aktivera övervakning och diagnostik][enable-monitoring]. Använd den [Azure Logginsamling] [ log-collector] tillägg för att samla in loggar för Azure-plattformen och överföra dem till Azure-lagring.   

Följande CLI-kommando aktiverar diagnostik:

```
azure vm enable-diag <resource-group> <vm-name>
```

**Stoppa en virtuell dator.** Azure gör skillnad mellan tillståndet ”stoppad” och tillståndet ”frigjord”. Du debiteras när den virtuella datorns status stoppas, men inte när den virtuella datorn frigörs.

Du kan använda följande CLI-kommando för att frigöra en virtuell dator:

```
azure vm deallocate <resource-group> <vm-name>
```

I Azure-portalen tar knappen **Stoppa** bort den virtuella datorn. Om du stänger av via operativsystemet när du är inloggad stoppas den virtuella datorn, men frigörs dock *inte*, så du kommer fortfarande att debiteras.

**Ta bort en virtuell dator.** Om du tar bort en virtuell dator, tas inte de virtuella hårddiskarna bort. Det innebär att du kan ta bort den virtuella datorn på ett säkert sätt utan att förlora data. Men kommer du fortfarande att debiteras för lagring. Ta bort den virtuella hårddisken genom att ta bort filen från [Blob Storage][blob-storage].

Om du vill förhindra oavsiktlig borttagning använder du ett [resurslås] [resource-lock] och låser hela resursgruppen eller enskilda resurser, till exempel den virtuella datorn.

## <a name="security-considerations"></a>Säkerhetsöverväganden

Använd [Azure Security Center] [ security-center] att hämta en central vy av säkerhetstillståndet hos dina Azure-resurser. Security Center övervakar potentiella säkerhetsproblem och ger en heltäckande bild av säkerhetshälsa för din distribution. Security Center konfigureras per Azure-prenumeration. Aktivera insamling av säkerhet som beskrivs i [använda Security Center]. När datainsamling har aktiverats, söker Security Center automatiskt alla virtuella datorer som skapades under den prenumerationen.

**Uppdateringshantering.** Om aktiverad, kontrollerar Security Center om säkerhetsuppdateringar och viktiga uppdateringar saknas. Använd [grupprincipinställningar] [ group-policy] på den virtuella datorn att aktivera Automatiska uppdateringar.

**Program mot skadlig kod.** Om aktiverad, kontrollerar Security Center om program mot skadlig kod har installerats. Du kan också använda Security Center för att installera program mot skadlig kod från i Azure-portalen.

**Åtgärder.** Använd [rollbaserad åtkomstkontroll] [ rbac] (RBAC) för att kontrollera åtkomsten till de Azure-resurser som du distribuerar. Med RBAC kan du tilldela medlemmarna i DevOps-gruppen auktoriseringsroller. Till exempel kan den som har rollen Läsare visa Azure-resurser, men inte skapa, hantera eller ta bort dem. Vissa roller är specifika för vissa Azure-resurstyper. Till exempel kan virtuella deltagarrollen starta om eller frigöra en virtuell dator, återställa lösenord, skapa en ny virtuell dator och så vidare. Andra [inbyggda RBAC-roller][rbac-roles] som kan vara användbara för denna referensarkitektur är bland annat [DevTest Labs-användare] [rbac-devtest] och [Nätverksdeltagare][rbac-network]. En användare kan tilldelas till flera roller och du kan skapa egna roller för ännu mer detaljerade behörigheter.

> [!NOTE]
> RBAC begränsar inte de åtgärder som en användare som är inloggad på en virtuell dator kan utföra. Dessa behörigheter avgörs av kontotypen i gästoperativsystemet.   
>
>

Om du vill återställa det lokala administratörslösenordet, kör den `vm reset-access` Azure CLI-kommando.

```
azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Använd [granskningsloggar][audit-logs] om du vill visa etableringsåtgärder och andra virtuella datorhändelser.

**Kryptering av data.** Överväg att använda [Azure Disk Encryption][disk-encryption] om du behöver kryptera operativsystemet och datadiskarna.

## <a name="solution-deployment"></a>Lösningsdistribution

En distribution för denna Referensarkitektur är tillgängligt på [GitHub][github-folder]. Den innehåller ett virtuellt nätverk, NSG och en enda virtuell dator. Följ anvisningarna nedan om du vill distribuera arkitekturen:

1. Högerklicka på knappen nedan och välj antingen ”Öppna länk i ny flik” eller ”Öppna länk i nytt fönster”.  
   [![Distribuera till Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)
2. När länken har öppnats i Azure-portalen måste du ange värden för vissa inställningar:

   * Namnet **Resursgrupp** har redan definierats i parameterfilen så välj **Skapa nytt** och ange `ra-single-vm-rg` i textrutan.
   * Välj region från den listrutan **Plats**.
   * Redigera inte textrutorna för **mallrots-URI** eller **parameterrots-URI**.
   * Välj **windows** i den **Os-typen** listrutan.
   * Granska villkoren och klicka sedan i kryssrutan **Jag godkänner villkoren ovan**.
   * Klicka på **Köp**.
3. Vänta tills distributionen har slutförts.
4. Parameterfilerna i omfattar en hårdkodad administratörsanvändarnamn och lösenord och vi rekommenderar starkt att du direkt ändra båda. Klicka på den virtuella datorn med namnet `ra-single-vm0 ` i Azure Portal. Klicka på **Återställ lösenord** i den **stöd + felsökning** bladet. Välj **Återställ lösenord** i listrutan **Läge** och välj sedan ett nytt **Användarnamn** och **Lösenord**. Klicka på **Uppdatera** för att spara det nya användarnamnet och lösenordet.

Information om ytterligare sätt att distribuera den här referensen för arkitekturen finns i filen Viktigt i den [vägledning enskild vm][github-folder]] GitHub-mappen.

## <a name="customize-the-deployment"></a>Anpassa distributionen
Om du behöver ändra distributionen för att matcha dina behov, följ instruktionerna i den [viktigt][github-folder].

## <a name="next-steps"></a>Nästa steg
Distribuera två eller flera virtuella datorer bakom en belastningsutjämnare för högre tillgänglighet. Mer information finns i [Köra flera virtuella datorer på Azure][multi-vm].

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]:../articles/virtual-machines/windows/tutorial-availability-sets.md
[azure-cli]: /cli/azure/get-started-with-az-cli2
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/storage/storage-about-disks-and-vhds-windows.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]:../articles/virtual-machines/windows/portal-create-fqdn.md
[github-folder]: http://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm
[group-policy]: https://technet.microsoft.com/en-us/library/dn595129.aspx
[log-collector]: https://azure.microsoft.com/en-us/blog/simplifying-virtual-machine-troubleshooting-using-azure-log-collector/
[manage-vm-availability]:../articles/virtual-machines/windows/manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[planned-maintenance]:../articles/virtual-machines/windows/planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-labs-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[resize-os-disk]:../articles/virtual-machines/windows/expand-os-disk.md
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[security-center]: https://azure.microsoft.com/en-us/services/security-center/
[select-vm-image]:../articles/virtual-machines/windows/cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-account-limits]: ../articles/azure-subscription-service-limits.md#storage-limits
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[använda Security Center]: ../articles/security-center/security-center-get-started.md#use-security-center
[virtual-machine-sizes]: ../articles/virtual-machines/windows/sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]:../articles/virtual-machines/linux/change-vm-size.md
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[vm-size-tables]: ../articles/virtual-machines/windows/sizes.md
[0]: ./media/guidance-blueprints/compute-single-vm.png "Arkitekturen för enkel Windows VM i Azure"
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[blocks]: https://github.com/mspnp/template-building-blocks
