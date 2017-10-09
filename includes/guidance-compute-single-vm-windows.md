Den här artikeln beskrivs en uppsättning beprövade metoder för att köra en Windows-dator (VM) på Azure, betalar uppmärksamhet tooscalability, tillgänglighet, hanterbarhet och säkerhet.

> [!NOTE]
> Azure har två olika distributionsmodeller: [Azure Resource Manager] [ resource-manager-overview] och klassisk. Den här artikeln använder Resurshanteraren, som Microsoft rekommenderar för nya distributioner.
>
>

Du bör inte använda en enskild virtuell dator för verksamhetskritiska arbetsbelastningar, eftersom den skapar en enskild felkälla. För högre tillgänglighet kan du distribuera flera virtuella datorer i en [tillgänglighetsuppsättning][availability-set]. Mer information finns i [Köra flera virtuella datorer på Azure][multi-vm].

## <a name="architecture-diagram"></a>Arkitekturdiagram

Etablera en virtuell dator i Azure omfattar fler rörliga delar än bara hello Virtuella datorn. Det finns beräkning, nätverk och lagring-element.

> Ett Visio-dokument som innehåller den här Arkitekturdiagram är tillgängliga för nedladdning från hello [Microsoft download center][visio-download]. Det här diagrammet är på hello ”beräknings - enskild VM” sidan.
>
>

![[0]][0]

* **Resursgrupp.** En [*resursgrupp*][resource-manager-overview] är en behållare som innehåller närliggande resurser. Skapa en resurs gruppera toohold hello resurser för den här virtuella datorn.
* **Virtuell dator**. Du kan etablera en virtuell dator från en lista över publicerade bilder eller från en virtuell hårddisk (VHD)-fil som du överför tooAzure Blob storage.
* **OS-disk.** hello OS-disken är en virtuell Hårddisk som lagrats i [Azure Storage][azure-storage]. Det innebär den kvarstår även om hello värddatorn kraschar.
* **Tillfällig disk.** hello VM har skapats med en tillfälliga disk (hello `D:` Windows-enheten). Den här disken är lagrade på en fysisk enhet på hello värddatorn. Det sparas *inte* i Azure Storage och kan tas bort under omstarter och andra livscykelhändelser för virtuella datorer. Använd enbart den här disken för tillfälliga data, till exempel växlingsfiler.
* **Datadiskar.** En [datadisk][data-disk] är en permanent virtuell hårddisk för programdata. Datadiskar som lagras i Azure Storage som hello OS-disk.
* **Virtuellt nätverk och undernät.** Varje virtuell dator i Azure distribueras till ett virtuellt nätverk som delas upp ytterligare i undernät.
* **Offentlig IP-adress.** En offentlig IP-adress är nödvändiga toocommunicate med hello VM&mdash;till exempel via fjärrskrivbord (RDP).
* **Nätverksgränssnitt**. hello NIC kan hello VM toocommunicate med hello virtuellt nätverk.
* **Nätverkssäkerhetsgrupp (NSG)**. Hej [NSG] [ nsg] är används tooallow/neka trafik toohello undernät. Du kan associera en NSG med ett enskilt nätverkskort eller ett undernät. Om du kopplar den till ett undernät tillämpas hello NSG-regler tooall virtuella datorer i undernätet.
* **Diagnostik.** Diagnostikloggning är avgörande för att hantera och felsöka hello VM.

## <a name="recommendations"></a>Rekommendationer

hello följande rekommendationer gäller för de flesta scenarier. Följ dessa rekommendationer om du inte har ett visst krav som åsidosätter dem.

### <a name="vm-recommendations"></a>Rekommendationer för virtuella datorer

Azure erbjuder många olika virtuella datorstorlekar, men vi rekommenderar hello DS - och GS-serien eftersom dessa datorstorlekar stöder [Premiumlagring][premium-storage]. Välj en av dessa datorer såvida du inte har en särskild arbetsbelastning som databehandling med höga prestanda. Mer information finns i [Storlekar för virtuella datorer][virtual-machine-sizes].

Om du flyttar en befintlig arbetsbelastning tooAzure starta med hello VM-storlek som är hello närmaste matchar tooyour lokala servrar. Måttet hello prestandan för din faktiska arbetsbelastning med respektera tooCPU, minne och disk-i/o-åtgärder per sekund (IOPS) och sedan justera hello storlek vid behov. Om du behöver flera nätverkskort för den virtuella datorn kan vara medveten om att hello maximalt antal nätverkskort som är en funktion av hello [VM-storlek][vm-size-tables].   

När du etablerar hello VM och andra resurser, måste du ange en region. I allmänhet väljer region närmaste tooyour interna användare eller kunder. Inte alla VM-storlekar kan dock finnas tillgänglig i alla regioner. Mer information finns i [tjänster efter region][services-by-region]. toosee en lista över hello VM storlekar som finns tillgängliga i en viss region, kör följande kommando för Azure-kommandoradsgränssnittet (CLI) hello:

```
azure vm sizes --location <location>
```

Information om hur du väljer en publicerad virtuell datoravbildning finns [navigera och välj virtuella Windows-avbildningar i Azure med Powershell eller CLI][select-vm-image].

### <a name="disk-and-storage-recommendations"></a>Disk- och lagringsrekommendationer

För bästa diskprestanda-i/o, rekommenderar vi [Premiumlagring][premium-storage], som lagrar data på solid state-hårddiskar (SSD). Kostnaden är baserad på hello storleken på hello allokerad disk. IOPS och genomströmning beror också på diskutrymme, så när du etablerar en disk, Tänk alla tre (kapacitet, IOPS och genomströmning).

Skapa separata Azure storage-konton för varje VM toohold hello virtuella hårddiskar (VHD) i ordning tooavoid träffa hello IOPS-gränser för lagringskonton.

Lägg till en eller flera datadiskar. När du skapar en ny virtuell Hårddisk är den oformaterade. Logga in på hello VM tooformat hello disk. Om du har ett stort antal datadiskar vara medveten om hello total i/o-gränserna för hello storage-konto. Mer information finns i [diskgränser för virtuella datorer][vm-disk-limits].

När det är möjligt ska du installera program på en datadisk, inte hello OS-disken. Vissa äldre program kan dock behöva tooinstall komponenter på hello C:-enheten. I så fall kan du [ändra storlek på hello OS-disk] [ resize-os-disk] med hjälp av PowerShell.

För bästa prestanda bör du skapa ett separat lagringskonto toohold diagnostikloggar. Ett standardkonto för lokalt redundant lagring (LRS) är tillräckligt för diagnostikloggar.

### <a name="network-recommendations"></a>Nätverksrekommendationer

hello offentliga IP-adressen kan vara dynamiska eller statiska. hello standardvärdet är dynamisk.

* Reservera en [statisk IP-adress] [ static-ip] om du behöver en fast IP-adress som inte ändras &mdash; till exempel om du behöver toocreate en A registreras i DNS, eller behöver hello IP-adress toobe tillagda tooa listan över säkra.
* Du kan också skapa ett fullständigt kvalificerat domännamn (FQDN) för hello IP-adress. Du kan sedan registrera en [CNAME-post] [ cname-record] i DNS som pekar toohello FQDN. Mer information finns i [skapa ett fullständigt kvalificerat domännamn i hello Azure-portalen][fqdn].

Alla nätverkssäkerhetsgrupper innehåller en uppsättning [standardregler][nsg-default-rules], inklusive en regel som blockerar all inkommande Internettrafik. hello standardreglerna kan inte tas bort, men andra regler kan åsidosätta dem. tooenable Internet-trafik skapar regler som tillåter inkommande trafik toospecific portar &mdash; till exempel port 80 för HTTP.  

tooenable RDP, Lägg till en NSG-regel som tillåter inkommande trafik tooTCP port 3389.

## <a name="scalability-considerations"></a>Skalbarhetsöverväganden

Du kan skala en VM uppåt eller nedåt genom [ändrar hello VM-storlek](../articles/virtual-machines/windows/sizes.md). tooscale ut vågrätt, placera två eller flera virtuella datorer i en tillgänglighetsuppsättning bakom en belastningsutjämnare. Mer information finns i [flera virtuella datorer som körs på Azure för skalbarhet och tillgänglighet][multi-vm].

## <a name="availability-considerations"></a>Överväganden för tillgänglighet

Distribuera flera virtuella datorer i en tillgänglighetsuppsättning om du vill ha högre tillgänglighet. Det ger också en högre [servicenivåavtal] [ vm-sla] (SLA).

Din virtuella dator kan påverkas av [planerat underhåll] [ planned-maintenance] eller [oplanerat underhåll][manage-vm-availability]. Du kan använda [VM omstart loggar] [ reboot-logs] toodetermine om en virtuell dator startas om orsakades av planerat underhåll.

Virtuella hårddiskar lagras i [Azure Storage][azure-storage], och Azure-lagringsutrymmet replikeras för hållbarhet och tillgänglighet.

tooprotect mot oavsiktlig dataförlust under normal drift (t.ex, på grund av fel), bör du också implementera point-in-time-säkerhetskopior, använda [blob ögonblicksbilder] [ blob-snapshot] eller något annat verktyg.

## <a name="manageability-considerations"></a>Överväganden för hantering

**Resursgrupper.** Placera direkt kopplade resurser som resursen hello samma livslängd växla till hello samma [resursgruppen][resource-manager-overview]. Resursgrupper kan du toodeploy och övervaka resurser som en grupp och dyker upp fakturering kostnader med resursgrupp. Du kan också ta bort resurser som en uppsättning, vilket är mycket användbart vid testdistributioner. Ge resurserna meningsfulla namn. Som gör det enklare toolocate en specifik resurs och förstå dess roll. Se [Rekommenderade namnkonventioner för Azure-resurser][naming conventions].

**Diagnostik av virtuella datorer.** Aktivera övervakning och diagnostik, inklusive grundläggande hälsomätvärden, diagnostikinfrastrukturloggar och [startdiagnostik][boot-diagnostics]. Startdiagnostikinställningar hjälper dig att diagnostisera ett startfel om den virtuella datorn hämtar i ett tillstånd kan. Mer information finns i [Aktivera övervakning och diagnostik][enable-monitoring]. Använd hello [Azure Logginsamling] [ log-collector] tillägget toocollect Azure-plattformen loggar och överför dem tooAzure lagring.   

hello följande CLI kommando aktiverar diagnostik:

```
azure vm enable-diag <resource-group> <vm-name>
```

**Stoppa en virtuell dator.** Azure gör skillnad mellan tillståndet ”stoppad” och tillståndet ”frigjord”. Du debiteras när hello VM-statusen har stoppats, men inte när hello VM har frigjorts.

Använd hello följande CLI kommandot toodeallocate en virtuell dator:

```
azure vm deallocate <resource-group> <vm-name>
```

I hello Azure-portalen, hello **stoppa** knappen tar bort hello VM. Men om du stänger av via hello OS medan du är inloggad hello VM stoppas men *inte* frigjorts, så att du kommer fortfarande att debiteras.

**Ta bort en virtuell dator.** Om du tar bort en virtuell dator, raderas inte hello virtuella hårddiskar. Det innebär att du kan ta bort hello VM utan att förlora data. Men kommer du fortfarande att debiteras för lagring. toodelete hello virtuell Hårddisk, ta bort hello-filen från [Blob storage][blob-storage].

tooprevent oavsiktlig borttagning, Använd en [resurslås] [ resource-lock] toolock hello hela resursen grupp eller Lås enskilda resurser, till exempel hello VM.

## <a name="security-considerations"></a>Säkerhetsöverväganden

Använd [Azure Security Center] [ security-center] tooget en central vy över hello säkerhetstillståndet hos dina Azure-resurser. Security Center övervakar potentiella säkerhetsproblem och ger en heltäckande bild av hello säkerhetshälsa för din distribution. Security Center konfigureras per Azure-prenumeration. Aktivera insamling av säkerhet som beskrivs i [använda Security Center]. När datainsamling har aktiverats, söker Security Center automatiskt alla virtuella datorer som skapades under den prenumerationen.

**Uppdateringshantering.** Om aktiverad, kontrollerar Security Center om säkerhetsuppdateringar och viktiga uppdateringar saknas. Använd [grupprincipinställningar] [ group-policy] hello VM tooenable automatiska uppdateringar.

**Program mot skadlig kod.** Om aktiverad, kontrollerar Security Center om program mot skadlig kod har installerats. Du kan också använda Security Center tooinstall program mot skadlig kod från inuti hello Azure-portalen.

**Åtgärder.** Använd [rollbaserad åtkomstkontroll] [ rbac] (RBAC) toocontrol åtkomst toohello Azure resurser som du distribuerar. RBAC kan du tilldela auktorisering roller toomembers för DevOps-team. Till exempel kan rollen för hello läsare visa Azure-resurser men inte skapa, hantera eller ta bort dem. Vissa roller är särskilda tooparticular Azure resurstyper. Till exempel kan hello virtuella deltagarrollen starta om eller frigöra en virtuell dator, återställa hello administratörslösenordet, skapa en ny virtuell dator och så vidare. Andra [inbyggda RBAC-roller][rbac-roles] som kan vara användbara för denna referensarkitektur är bland annat [DevTest Labs-användare] [rbac-devtest] och [Nätverksdeltagare][rbac-network]. En användare kan tilldelas toomultiple roller och du kan skapa anpassade roller för ännu mer detaljerade behörigheter.

> [!NOTE]
> RBAC begränsar inte hello-åtgärder som en användare som har loggat in på en virtuell dator kan utföra. Dessa behörigheter bestäms av hello kontotyp på hello gästoperativsystemet.   
>
>

tooreset hello lokala administratörslösenordet, kör hello `vm reset-access` Azure CLI-kommando.

```
azure vm reset-access -u <user> -p <new-password> <resource-group> <vm-name>
```

Använd [granskningsloggar] [ audit-logs] toosee etablering åtgärder och andra VM-händelser.

**Kryptering av data.** Överväg att [Azure Disk Encryption] [ disk-encryption] om du behöver tooencrypt hello OS- och datadiskar.

## <a name="solution-deployment"></a>Lösningsdistribution

En distribution för denna Referensarkitektur är tillgängligt på [GitHub][github-folder]. Den innehåller ett virtuellt nätverk, NSG och en enda virtuell dator. toodeploy Hej arkitektur, gör du följande:

1. Högerklicka på hello knappen nedan och välj antingen ”öppna länken i ny flik” eller ”öppna länk i nytt fönster”.  
   [![Distribuera tooAzure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)
2. När hello länk har öppnats i hello Azure-portalen, måste du ange värden för vissa hello inställningar:

   * Hej **resursgruppen** namn har redan definierats i hello parameterfil, så du väljer **Skapa nytt** och ange `ra-single-vm-rg` i hello textruta.
   * Välj hello region från hello **plats** listrutan.
   * Redigera inte hello **mall rot Uri** eller hello **parametern rot Uri** textrutor.
   * Välj **windows** i hello **Os-typen** listrutan.
   * Granska hello villkoren och klicka sedan på hello **acceptera toohello villkoren ovan** kryssrutan.
   * Klicka på hello **inköp** knappen.
3. Vänta tills hello distribution toocomplete.
4. hello parametern filer innehåller ett hårdkodat administratörsanvändarnamn och lösenord och vi rekommenderar starkt att du direkt ändra båda. Klicka på hello virtuella datorn med namnet `ra-single-vm0 `i hello Azure-portalen. Klicka på **Återställ lösenord** i hello **stöd + felsökning** bladet. Välj **Återställ lösenord** i hello **läge** nedrullningsbara rutan, välj sedan en ny **användarnamn** och **lösenord**. Klicka på hello **uppdatering** knappen toopersist hello nytt användarnamn och lösenord.

Information om ytterligare sätt toodeploy denna referera arkitekturen, finns hello filen readme hello [vägledning enskild vm][github-folder]] GitHub-mappen.

## <a name="customize-hello-deployment"></a>Anpassa hello-distribution
Om du behöver toochange hello distribution toomatch dina behov, följer du anvisningarna för hello i hello [viktigt][github-folder].

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
