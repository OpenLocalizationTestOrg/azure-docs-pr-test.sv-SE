---
title: "aaaAzure Site Recovery-distribution planner för VMware till Azure | Microsoft Docs"
description: "Detta är hello Azure Site Recovery-distribution planner användarhandboken."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/28/2017
ms.author: nisoneji
ms.openlocfilehash: a8c13cd47850575769e0186528807bc525bdeec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-site-recovery-deployment-planner"></a>Kapacitetsplaneraren i Azure Site Recovery
Den här artikeln är hello Azure Site Recovery-distribution Planner användarhandboken för VMware till Azure Produktionsdistribution.

## <a name="overview"></a>Översikt

Innan du börjar skydda alla virtuella VMware-datorer (VM) med hjälp av Site Recovery allokera tillräckligt med bandbredd, baserat på din dagliga data ändras, toomeet din önskade återställningspunktmål (RPO). Vara säker på att toodeploy hello rätt antal konfigurationsservrar och processen servrar lokalt.

Du måste också toocreate hello rätt typ och antal mål Azure storage-konton. Du skapar antingen Standard Storage- eller Premium Storage-konton, och väger in tillväxt på källproduktionsservrarna på grund av ökad användning under en viss tid. Du väljer hello lagringstyp per VM, baserat på arbetsbelastning egenskaper (till exempel läsning och skrivning i/o-åtgärder per sekund [IOPS] eller dataomsättningen) och begränsar Site Recovery.

förhandsversion av hello Site Recovery-distribution planner är ett kommandoradsverktyg som är för närvarande endast tillgängligt för hello VMware-Azure-scenario. Du kan via fjärranslutning profil din virtuella VMware-datorer med hjälp av det här verktyget (med inga produktion inverkan som helst) toounderstand hello bandbredd och krav för Azure Storage för replikering som lyckades och testa redundans. Du kan köra verktyget hello utan att installera alla Site Recovery-komponenter på lokala. Dock tooget korrekt uppnås genomströmning resultat, rekommenderar vi att du kör hello planner på en Windows-Server som uppfyller hello krav som finns i hello Site Recovery konfigurationsservern att så småningom måste toodeploy som ett första steg i hello i Produktionsdistribution.

hello verktyget ger hello följande information:

**Utvärdering av kompatibilitet**

* En utvärdering av om den virtuella datorn stöds baserat på antal diskar, diskstorlek, IOPS, dataomsättninn och starttyp (EFI/BIOS)
* Hej uppskattade bandbredd som krävs för deltareplikering

**Nätverkets bandbreddsbehov kontra utvärdering av återställningspunktmål**

* Hej uppskattade bandbredd som krävs för deltareplikering
* hello dataflödet som Site Recovery kan hämta från lokala tooAzure
* hello antalet virtuella datorer toobatch, baserat på hello Uppskattad bandbredd toocomplete inledande replikering i en viss tidsperiod

**Krav på infrastruktur för Azure**

* hello lagringsutrymmet typ (standard eller premium storage-konto) som krävs för varje virtuell dator
* hello Totalt antal standard och premium storage-konton toobe ställts in för replikering
* Namnförslag för lagringskonton baserade på riktlinjer för Azure Storage
* Hej lagringskonto placering för alla virtuella datorer
* hello antal Azure kärnor toobe ställa in innan du testa redundans eller växling vid fel på hello prenumeration
* hello Azure VM-rekommenderade storleken för varje lokala VM

**Krav på lokal infrastruktur**
* hello krävs antal konfigurationsservrar och processen servrar toobe distribueras lokalt

>[!IMPORTANT]
>
>Eftersom användning är sannolikt tooincrease över tid, alla hello föregående verktyget beräkningar utförs under förutsättning att ett 30 procent tillväxtfaktor arbetsbelastning egenskaper, och använder en 95 percentilvärdet för alla hello profilering mått (läsning och skrivning IOPS, omsättningsuppdateringar o.s.v. tillbaka). Båda parametrarna (tillväxtfaktorn och percentilberäkningen) kan konfigureras. toolearn mer om tillväxtfaktor, i avsnittet hello ”tillväxt faktorer”. toolearn mer om percentilvärdet, i avsnittet hello ”percentilvärdet används för beräkning av hello”.
>

## <a name="requirements"></a>Krav
hello-verktyget har två huvudsakliga faser: profilering och rapportgenerering. Det finns också en tredje alternativet toocalculate genomströmning endast. hello kraven för hello server från vilken hello profilering och genomströmning mätning startas visas i hello följande tabell:

| Serverkrav | Beskrivning|
|---|---|
|Profilering och mätning av dataflöde| <ul><li>Operativsystem: Microsoft Windows Server 2012 R2<br>(helst matchar minst hello [storlek rekommendationer för hello konfigurationsservern](https://aka.ms/asr-v2a-on-prem-components))</li><li>Datorkonfiguration: 8 virtuella processorer, 16 GB RAM-minne, 300 GB hårddisk</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Microsoft Visual C++ Redistributable for Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>TooAzure för Internet-åtkomst från den här servern</li><li>Azure Storage-konto</li><li>Administratörsbehörighet på hello-server</li><li>Minst 100 GB ledigt diskutrymme (förutsatt 1 000 virtuella datorer med ett medeltal av de tre diskar vardera, profilerade under 30 dagar)</li><li>VMware vCenter statistik inställningar ska ställas in too2 eller hög nivå</li><li>Tillåt port 443: ASR distribution Planner använder den här porten tooconnect toovCenter server/ESXi-värd</ul></ul>|
| Rapportgenerering | En Windows-dator eller Windows Server med Microsoft Excel 2013 eller senare |
| Användarbehörigheter | Läsbehörighet för hello-användarkonto som har använt tooaccess hello VMware vCenter server/VMware vSphere ESXi-värd under profilering |

> [!NOTE]
>
>hello-verktyget kan profile endast virtuella datorer med VMDK och RDM diskar. Den kan inte profilera virtuella datorer med iSCSI- eller NFS-diskar. Site Recovery har stöd för iSCSI och NFS-diskar för VMware-servrar, men eftersom hello distribution planner finns inte i hello Gäst och den profiler med vCenter prestandaräknare, hello verktyget har inte insyn i dessa disktyper.
>

## <a name="download-and-extract-hello-public-preview"></a>Ladda ned och extrahera hello förhandsversion
1. Hämta hello senaste versionen av hello [Site Recovery-distribution planner förhandsversion](https://aka.ms/asr-deployment-planner).  
hello verktyget paketeras i en .zip-mapp. hello aktuella versionen av hello verktyget stöder bara VMware till Azure för hello-scenariot.

2. Kopiera hello .zip mappen toohello Windows server som du vill toorun hello-verktyget.  
Du kan köra verktyget hello från Windows Server 2012 R2 om hello-servern har network access tooconnect toohello vCenter server/vSphere ESXi-värd som innehåller hello VMs toobe profileras startas. Vi rekommenderar dock att du kör hello verktyget på en server vars maskinvarukonfiguration uppfyller hello [configuration server sizing riktlinje](https://aka.ms/asr-v2a-on-prem-components). Om du redan har distribuerat Site Recovery-komponenter på lokala kör du verktyget hello från hello konfigurationsservern.

 Vi rekommenderar att du har hello samma maskinvarukonfiguration som hello konfigurationsservern (som har en inbyggda processervern) på hello server där du kör hello-verktyget. En sådan konfiguration säkerställer att hello uppnås genomströmning som hello verktyget rapporter matchar hello faktiska dataflödet som Site Recovery kan uppnå vid replikering. hello genomströmning beräkningen beror på tillgänglig nätverksbandbredd på hello-servern och maskinvarukonfiguration (CPU, lagring och så vidare) av hello-server. Om du kör verktyget hello från någon annan server beräknas hello genomströmning från denna server tooMicrosoft Azure. Dessutom eftersom hello maskinvarukonfiguration hello-Server kan skilja sig från hello konfigurationsservern, kan hello uppnås dataflödet som hello verktyget rapporter vara felaktig.

3. Extrahera hello ZIP-mappen.  
hello mappen innehåller flera filer och undermappar. hello körbara filen är ASRDeploymentPlanner.exe i hello överordnade mapp.

    Exempel:  
    Kopiera hello ZIP-filen tooE: \-enheten och extraherar du det.
   E:\ASR Deployment Planner-Preview_v1.2.zip

    E:\ASR Deployment Planner-Preview_v1.2\ ASR Deployment Planner-Preview_v1.2\ ASRDeploymentPlanner.exe

## <a name="capabilities"></a>Funktioner
Du kan köra hello-kommandoradsverktyget (ASRDeploymentPlanner.exe) i något av följande tre lägen hello:

1. Profilering  
2. Rapportgenerering
3. Beräkna dataflöde

Kör först hello-verktyget i profilering läge toogather VM dataomsättningen och IOPS. Kör hello verktyget toogenerate hello rapporten toofind hello bandbredd och lagring nätverkskrav.

## <a name="profiling"></a>Profilering
I profilering läge ansluter hello distribution kapacitetsplaneringsverktyget för toohello vCenter server/vSphere ESXi-värd toocollect prestandadata om hello VM.

* Profilering påverkar inte hello prestanda av hello virtuella datorer, eftersom ingen direkt anslutning upprättas toothem. Alla prestandadata samlas in från hello vCenter server/vSphere ESXi-värd.
* tooensure att det finns en minimera effekten på hello-servern på grund av profilering hello verktyget frågor hello vCenter server/vSphere ESXi-värd var 15: e minut. Den här frågeintervall äventyrar inte profilering Precision, eftersom hello verktyget lagras prestandaräknardata för varje minut.

### <a name="create-a-list-of-vms-tooprofile"></a>Skapa en lista över virtuella datorer tooprofile
Du måste först en lista över hello VMs toobe profileras startas. Du kan hämta alla hello namnen på virtuella datorer på en vCenter server/vSphere ESXi-värd med hjälp av hello VMware vSphere PowerCLI kommandon i hello nedan. Alternativt kan du ange ett eget namn för filen hello eller IP-adresser för hello virtuella datorer som du vill tooprofile manuellt.

1. Logga in toohello VM som VMware vSphere PowerCLI är installerad på.
2. Öppna hello VMware vSphere PowerCLI-konsolen.
3. Kontrollera att hello körningsprincipen är aktiverat för hello skript. Om den är inaktiverad, starta hello VMware vSphere PowerCLI konsolen i administratörsläge och aktivera det genom att köra följande kommando hello:

            Set-ExecutionPolicy –ExecutionPolicy AllSigned

4. Du kan optionly måste toorun hello följande kommando om Anslut VIServer inte är giltig hello namnet på cmdlet.
 
            Add-PSSnapin VMware.VimAutomation.Core 

5. tooget alla hello namnen på virtuella datorer på en vCenter server/vSphere ESXi värd och lagra hello listan i en txt-fil, kör hello två kommandon som visas här.
Ersätt &lsaquo;servernamn&rsaquo;, &lsaquo;användarnamn&rsaquo;, &lsaquo;lösenord&rsaquo; och &lsaquo;utdatafil.txt&rsaquo; med egna värden.

            Connect-VIServer -Server <server name> -User <user name> -Password <password>

            Get-VM |  Select Name | Sort-Object -Property Name >  <outputfile.txt>

6. Öppna utdatafilen hello i anteckningar och kopiera hello namnen på alla virtuella datorer som du vill tooprofile tooanother fil (till exempel ProfileVMList.txt) namn på en virtuell dator per rad. Den här filen används som indata toohello *- VMListFile* parametern för hello-kommandoradsverktyget.

    ![Lista med VM i hello distribution planner](./media/site-recovery-deployment-planner/profile-vm-list.png)

### <a name="start-profiling"></a>Starta profilering
När du har hello lista över virtuella datorer toobe profileras startas, kan du köra verktyget hello i profilering läge. Här är hello listan över obligatoriska och valfria parametrar för hello verktyget toorun i profilering läge.

ASRDeploymentPlanner.exe -Operation StartProfiling /?

| Parameternamn | Beskrivning |
|---|---|
| -Operation | StartProfiling |
| -Server | hello fullständigt domännamn eller IP-adressen för hello vCenter server/vSphere ESXi-värd vars är toobe profileras startas.|
| -User | hello användaren namnet tooconnect toohello vCenter server/vSphere ESXi-värd. hello användare behöver toohave läsbehörighet, åtminstone.|
| -VMListFile | hello-fil som innehåller hello lista över virtuella datorer toobe profileras startas. hello-sökvägen kan vara absolut eller relativ. hello-filen ska innehålla en VM namn eller IP-adress per rad. Virtuellt datornamn som angetts i hello-filen ska hello samma som hello VM-namnet på hello vCenter server/vSphere ESXi-värd.<br>Hello filen VMList.txt innehåller till exempel hello följande virtuella datorer:<ul><li>virtuell_dator_A</li><li>10.150.29.110</li><li>virtuell_dator_B</li><ul> |
| -NoOfDaysToProfile | hello antal dagar som profilering är toobe körning. Vi rekommenderar att du kör profilering mer än 15 dagar tooensure som hello arbetsbelastning mönster i din miljö över hello angivna period observeras och används tooprovide en korrekt rekommendation. |
| -Directory | (Valfritt) hello universal naming convention (UNC) eller lokal katalog sökvägen toostore profilering data som genereras under profilering. Om ett katalognamn inte anges används hello katalog med namnet 'ProfiledData' under hello aktuella sökvägen som hello standardkatalogen. |
| -Password | (Valfritt) hello lösenord toouse tooconnect toohello vCenter server/vSphere ESXi-värd. Om du inte anger något nu, att du uppmanas den när hello kommandot körs.|
| -StorageAccountName | (Valfritt) hello lagringskonto namn som används toofind hello genomströmning hastigheterna för replikering av data från lokalt tooAzure. hello verktyget överföringar test toothis storage-konto toocalculate datagenomströmning.|
| -StorageAccountKey | (Valfritt) hello lagringskonto nyckel som har använt tooaccess hello storage-konto. Gå toohello Azure-portalen > lagringskonton ><*lagringskontonamnet*>> Inställningar > åtkomstnycklar > Key1 (eller primärnyckeln för klassiska storage-konto). |
| -Environment | (Valfritt) Det här är din målmiljö för Azure Storage-kontot. Detta kan vara ett av tre värden – AzureCloud, AzureUSGovernment eller AzureChinaCloud. Standardvärdet är AzureCloud. Använd hello parametern när mål-Azure-regionen är Azure som tillhör amerikanska myndigheter eller Azure Kina moln. |


Vi rekommenderar att du profil dina virtuella datorer för minst 15 too30 dagar. Under hello profilering period, körs ASRDeploymentPlanner.exe. hello verktyget tar profilering tid indata i dagar. Om du vill tooprofile några timmar eller minuter om ett kort test hello-verktyget i hello förhandsversion behöver tooconvert hello tid till hello motsvarande mått på dagar. Till exempel tooprofile i 30 minuter hello inmatningen måste vara 30/(60*24) = 0.021 dagar. hello minsta tillåtna profilering tid är 30 minuter.

Under profilering, kan du kan också skickas ett lagringskonto namn och nyckel toofind hello dataflödet som Site Recovery kan uppnå för närvarande hello av replikeringen från hello konfigurationsservern eller processen server tooAzure. Om inte hello lagring-kontonamnet och nyckeln skickas under profilering beräknas hello verktyget inte möjligt genomflöde.

Du kan köra flera instanser av hello-verktyget för olika uppsättningar av virtuella datorer. Se till att hello VM namn inte upprepas i någon av hello profilering uppsättningar. Om du har profileras startas tio VMs exempelvis (VM1 via VM10) och efter några dagar du vill tooprofile en annan fem virtuella datorer (VM11 via VM15), kan du köra hello verktyget från en annan kommandoradskonsol för hello andra virtuella datorer (VM11 via VM15). Se till att hello andra uppsättning virtuella datorer har inte någon VM-namn från hello första profilering instansen du använder en annan målkatalogen för hello andra kör. Om två instanser av hello verktyget används för profilering hello samma virtuella datorer och Använd Hej samma målkatalogen, hello genereras rapporten kan vara felaktig.

Konfigurationer för virtuell dator skapas en gång hello början av hello profilering åtgärd och lagras i en fil med namnet VMDetailList.xml. Den här informationen används när hello rapporten genereras. Ändringar i VM-konfiguration (till exempel ett ökat antal kärnor, diskar och nätverkskort) från hello början toohello slutet av profilering samlas inte in. Om en profilerad VM-konfiguration har ändrats under hello profilering i hello förhandsversion, är här hello lösning tooget senaste VM information vid generering av hello rapporten:

* Säkerhetskopiera VMdetailList.xml och ta bort hello-filen från dess aktuella plats.
* Skicka - argumenten för användare och -lösenord när hello rapportgenerering.

hello profilering kommandot genererar flera filer i hello profilering directory. Ta inte bort några av hello filer eftersom detta påverkar så rapportgenerering.

#### <a name="example-1-profile-vms-for-30-days-and-find-hello-throughput-from-on-premises-tooazure"></a>Exempel 1: Profil VMs för 30 dagar och hitta hello genomströmning från lokala tooAzure
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  30  -User vCenterUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Exempel 2: Profilera virtuella datorer i 15 dagar

```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User vCenterUser1
```

#### <a name="example-3-profile-vms-for-1-hour-for-a-quick-test-of-hello-tool"></a>Exempel 3: Profil VMs för timmen för en snabb hello-verktyget
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Directory “E:\vCenter1_ProfiledData” -Server vCenter1.contoso.com -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  0.04  -User vCenterUser1
```

>[!NOTE]
>
>* Om hello servern hello verktyget körs på startas om eller har kraschat, eller om du stänger hello verktyget genom att använda Ctrl + C, hello profileras startas data bevaras. Det finns dock en risken för saknade hello senaste 15 minuterna profilerad data. I en instans, kör du hello-verktyget i profilering läge när hello servern startas om.
>* När hello lagring-kontonamnet och nyckeln skickas, hello verktyget åtgärder hello genomströmning vid hello sista steget i profilering. Om hello-verktyget är stängd innan profilering slutförs beräknas inte hello genomflöde. toofind hello genomströmning innan du genererar Hej rapport, kan du köra hello GetThroughput åtgärden från hello kommandoradskonsol. Annars innehåller hello genereras rapporten inte hello genomströmning information.


## <a name="generate-a-report"></a>Generera en rapport
hello verktyget genererar en makron Microsoft Excel-fil (XLSM-fil) som hello rapportutdata som sammanfattar alla rekommendationer för distribution av hello. hello rapporten har namnet DeploymentPlannerReport_ <*Unik numerisk identifierare*> .xlsm och placeras i hello anges directory.

När profilering är klar kan köra du hello-verktyget i rapportgenerering läge. hello följande tabell innehåller en lista över verktyg för obligatoriska och valfria parametrar toorun i rapportgenerering läge.

`ASRDeploymentPlanner.exe -Operation GenerateReport /?`

|Parameternamn | Beskrivning |
|-|-|
| -Operation | GenerateReport |
| -Server |  Hej vCenter/vSphere-serverns fullständigt kvalificerade domännamn eller IP-adress (Använd hello samma namn eller IP-adress som du använde när hello profilering) där hello profileras startas virtuella datorer vars rapporten är toobe genereras finns. Observera att om du använde en vCenter-server när hello profilering av du inte använda en vSphere-server för rapportgenerering och vice versa.|
| -VMListFile | hello-filen som innehåller hello lista över profilerad virtuella datorer som hello rapporten är toobe genereras för. hello-sökvägen kan vara absolut eller relativ. hello-filen ska innehålla ett VM-namn eller IP-adress per rad. hello VM-namn som anges i hello-filen ska hello samma som hello VM namn på hello vCenter server/vSphere ESXi-värd och matchar det du använde under profilering.|
| -Directory | (Valfritt) hello UNC eller lokal sökväg där hello profileras startas data (filer som genereras under profilering) lagras. Den här informationen krävs för att generera rapporten hello. Om du inte anger något namn används katalogen ProfiledData. |
| -GoalToCompleteIR | (Valfritt) hello antalet timmar i vilka hello inledande replikering av hello profileras startas virtuella datorer måste toobe slutförts. hello genereras rapporten innehåller hello antal virtuella datorer som den första replikeringen kan utföras på hello angetts tid. hello standardvärdet är 72 timmar. |
| -User | (Valfritt) hello användaren namnet toouse tooconnect toohello vCenter/vSphere-server. hello heter används toofetch hello senaste konfigurationsinformation för hello virtuella datorer, till exempel hello antalet diskar, antalet kärnor och antal nätverkskort, toouse i hello rapporten. Om hello namn inte tillhandahålls används hello konfigurationsinformation som samlas in hello början av hello profilering kickoff. |
| -Password | (Valfritt) hello lösenord toouse tooconnect toohello vCenter server/vSphere ESXi-värd. Om hello lösenord har inte angetts som en parameter, att du uppmanas det senare när hello kommandot körs. |
| -DesiredRPO | (Valfritt) hello önskat mål för återställningspunkt, i minuter. hello standardvärdet är 15 minuter.|
| -Bandwidth | Bandbredd i Mbit/s. hello parametern toouse toocalculate hello Återställningspunktmål som kan uppnås för hello angetts bandbredd. |
| -StartDate | (Valfritt) hello starta datum och tid i MM-DD-YYYY:HH:MM (24-timmarsformat). *StartDate* måste anges tillsammans med *EndDate*. Om du har angett StartDate genereras hello rapporten för hello profileras startas data som samlas in mellan StartDate och EndDate. |
| -EndDate | (Valfritt) hello slutdatum och sluttid i MM-DD-YYYY:HH:MM (24-timmarsformat). *EndDate* måste anges tillsammans med *StartDate*. Om du har angett EndDate genereras hello rapporten för hello profileras startas data som samlas in mellan StartDate och EndDate. |
| -GrowthFactor | (Valfritt) hello tillväxtfaktor, uttryckt i procent. hello standardvärdet är 30 procent. |
| -UseManagedDisks | (Valfritt) UseManagedDisks - Ja/Nej. Standardvärdet är Ja. hello antalet virtuella datorer som kan placeras i ett enda lagringskonto beräknas med tanke på om växling vid fel och testning av redundansväxling görs på hanterade diskar i stället för ohanterade disk. |

#### <a name="example-1-generate-a-report-with-default-values-when-hello-profiled-data-is-on-hello-local-drive"></a>Exempel 1: Skapa en rapport med standardvärden när hello profileras startas data på hello lokal enhet
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-hello-profiled-data-is-on-a-remote-server"></a>Exempel 2: Skapa en rapport när hello profileras startas data på en fjärrserver
Du bör ha läs-/ skrivbehörighet på hello fjärrkatalog.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “\\PS1-W2K12R2\vCenter1_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-and-goal-toocomplete-ir-within-specified-time"></a>Exempel 3: Skapa en rapport med en viss bandbredd och målet toocomplete IR inom angiven tid
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -Bandwidth 100 -GoalToCompleteIR 24
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-hello-default-30-percent"></a>Exempel 4: Skapa en rapport med en 5 procent tillväxtfaktor i stället för hello standard 30 procent
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Exempel 5: Generera en rapport med en delmängd av profileringsdata
Exempelvis har 30 dagar profilerad data och vill toogenerate en rapport för endast 20 dagar.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minute-rpo"></a>Exempel 6: Generera en rapport för ett återställningspunktmål på 5 minuter
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Server vCenter1.contoso.com -Directory “E:\vCenter1_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

## <a name="percentile-value-used-for-hello-calculation"></a>Percentilvärdet som används för beräkning av hello
**Vilka percentil standardvärdet hello prestandamått samlas in under profilering har hello verktyget används när den skapar en rapport?**

hello verktyget standardvärden toohello 95 percentil värdena för läsning och skrivning IOPS, skriva IOPS och dataomsättningen som samlas in under profilering av alla hello virtuella datorer. Det här måttet garanterar att hello 100: e percentilen topp dina virtuella datorer kan se på grund av tillfälliga händelser är används inte toodetermine dina mål-lagringskontot och källa bandbredd krav. Till exempel kan en tillfällig händelse vara ett säkerhetskopieringsjobb som körs en gång om dagen, periodisk databasindexering eller en analysrapportgenereringsaktivitet eller andra liknande kortvariga händelser vid vissa tidpunkter.

Med hjälp av 95 percentilvärden hello ger en riktig bild av verkliga arbetsbelastning egenskaper och det ger dig bästa prestanda när hello arbetsbelastningar körs på Azure. Vi räknar med att du behöver toochange numret. Om du ändrar hello-värde (toohello 90: e percentilen, till exempel), kan du uppdatera konfigurationsfilen för hello *ASRDeploymentPlanner.exe.config* i hello standardmapp och spara den toogenerate en ny rapport i hello befintliga profileras startas data.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Överväganden för tillväxtfaktorer
**Varför bör jag överväga för tillväxtfaktor när jag planerar distributioner?**

Det är kritiska tooaccount tillväxt i din arbetsbelastning egenskaper, förutsatt att en potentiell ökning av användning över tid. När skydd är på plats, om din arbetsbelastning egenskaper ändrar kan inte du växla tooa annat lagringskonto för skydd utan att inaktivera och återaktivera hello skydd.

Anta exempelvis att din virtuella dator i dag passar på ett standardlagringskonto för replikering. Över hello kommande tre månaderna flera ändringar är sannolikt toooccur:

* hello antalet användare av hello-program som körs på hello VM ökar.
* hello resulterande ökad omsättning på hello VM kräver hello VM toogo toopremium lagring så att Site Recovery replikering kan hålla jämna steg.
* Därför måste du ha toodisable och återaktivera skydd tooa premium storage-konto.

Vi rekommenderar starkt att du planerar för tillväxt under distributionsplanering och medan hello standardvärdet är 30 procent. Du är hello expert på ditt program användning mönster och tillväxt projektioner och du kan ändra antalet därefter vid generering av en rapport. Dessutom kan du generera flera rapporter med olika tillväxt faktorer med hello samma data i listan och ta reda på vilka mål lagrings- och bandbredd rekommendationer fungerar bäst för dig.

hello genereras Microsoft Excel-rapport innehåller hello följande information:

* [Indata](site-recovery-deployment-planner.md#input)
* [Rekommendationer](site-recovery-deployment-planner.md#recommendations-with-desired-rpo-as-input)
* [Rekommendationer för bandbreddsindata](site-recovery-deployment-planner.md#recommendations-with-available-bandwidth-as-input)
* [VM<->Storage Placement](site-recovery-deployment-planner.md#vm-storage-placement) (VM<->lagringsplacering)
* [Compatible VMs](site-recovery-deployment-planner.md#compatible-vms) (Kompatibla virtuella datorer)
* [Incompatible VMs](site-recovery-deployment-planner.md#incompatible-vms) (Inkompatibla virtuella datorer)

![Kapacitetsplaneraren](./media/site-recovery-deployment-planner/dp-report.png)

## <a name="get-throughput"></a>Beräkna dataflöde

tooestimate hello dataflödet som Site Recovery kan få ut från en lokal tooAzure under körning hello-verktyget i GetThroughput läge. hello verktyget beräknar hello genomströmning från hello-server som hello verktyget körs på. Vi rekommenderar är den här servern baserad på hello server sizing konfigurationsguiden. Om du redan har distribuerat Site Recovery-infrastruktur komponenter på lokala kör du verktyget hello på hello konfigurationsservern.

Öppna en kommandoradskonsol och gå toohello Site Recovery distributionsplanering Verktygsmapp. Kör ASRDeploymentPlanner.exe med följande parametrar.

`ASRDeploymentPlanner.exe -Operation GetThroughput /?`

|Parameternamn | Beskrivning |
|-|-|
| -Operation | GetThroughput |
| -Directory | (Valfritt) hello UNC eller lokal sökväg där hello profileras startas data (filer som genereras under profilering) lagras. Den här informationen krävs för att generera rapporten hello. Om namnet på en katalog inte anges används katalogen ProfiledData. |
| -StorageAccountName | namn på hello-lagringskonto som har använt toofind hello bandbredd för replikering av data från lokalt tooAzure. hello verktyget överföringar test data toothis storage-konto toofind hello bandbredd. |
| -StorageAccountKey | Hej lagringskonto nyckel som har använt tooaccess hello storage-konto. Gå toohello Azure-portalen > lagringskonton ><*lagringskontonamnet*>> Inställningar > åtkomstnycklar > Key1 (eller en primärnyckeln för klassiska lagringskonto). |
| -VMListFile | hello-fil som innehåller hello lista över virtuella datorer toobe profileras startas för beräkning av hello bandbredd. hello-sökvägen kan vara absolut eller relativ. hello-filen ska innehålla en VM namn eller IP-adress per rad. hello VM-namn som angetts i hello-filen ska hello samma som hello VM namn på hello vCenter server/vSphere ESXi-värd.<br>Hello filen VMList.txt innehåller till exempel hello följande virtuella datorer:<ul><li>VM_A</li><li>10.150.29.110</li><li>VM_B</li></ul>|
| -Environment | (Valfritt) Det här är din målmiljö för Azure Storage-kontot. Detta kan vara ett av tre värden – AzureCloud, AzureUSGovernment eller AzureChinaCloud. Standardvärdet är AzureCloud. Använd hello parametern när mål-Azure-regionen är Azure som tillhör amerikanska myndigheter eller Azure Kina moln. |

hello verktyget skapar flera 64 MB asrvhdfile <> # VHD filer (där är ”#” hello antal filer) på hello angiven katalog. hello verktyget filöverföringar hello filer toohello konto toofind hello genomflödet. Hello genomströmning mäts bort hello verktyget när alla hello-filer från hello storage-konto och hello lokal server. Om hello verktyget avslutas av någon anledning medan den beräknar dataflöde, bort inte hello filer från hello lagring eller hello lokal server. Du måste toodelete dem manuellt.

hello genomströmning mäts vid en angiven tidpunkt, och det är hello maximalt dataflöde Site Recovery kan uppnå vid replikering, förutsatt att alla andra faktorer förblir hello samma. Till exempel om alla program börjar förbruka mer bandbredd på hello samma nätverk, hello faktiska genomströmning varierar vid replikering. Om du kör hello GetThroughput kommando från en server configuration är hello-verktyget medveten om alla skyddade virtuella datorerna och pågående replikering. hello skiljer resultatet av hello uppmätta genomströmning sig om hello GetThroughput operationen körs när hello skyddade virtuella datorer har hög data omsättningsuppdateringar. Vi rekommenderar att du kör verktyget hello vid olika tillfällen under profilering toounderstand vilken genomflödet nivåer kan ske vid olika tidpunkter. Hello-verktyget visar hello senaste uppmätta genomflöde i hello rapport.

### <a name="example"></a>Exempel
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Directory  E:\vCenter1_ProfiledData -VMListFile E:\vCenter1_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

>[!NOTE]
>
> Kör hello verktyget på en server som har hello samma lagrings- och CPU-egenskaper som hello konfigurationsservern.
>
> Ange hello rekommenderade bandbredd toomeet hello Återställningspunktmål 100 procent hello tid för replikering. När du ställer in hello rätt bandbredd, om du inte ser öka hello uppnås genomflödet som rapporterats av hello-verktyget i hello följande:
>
>  1. Kontrollera toodetermine om det inte finns något nätverk tjänstkvalitet (QoS) som är att begränsa Site Recovery genomflöde.
>
>  2. Kontrollera toodetermine om Site Recovery-valvet hello närmsta fysiskt stöds Microsoft Azure-region toominimize Nätverksfördröjningen.
>
>  3. Kontrollera din lokal lagring egenskaper toodetermine om du kan förbättra hello maskinvara (till exempel HDD tooSSD).
>
>  4. Ändra hello Site Recovery-inställningarna i hello processervern också[öka hello mängden nätverksbandbredd som används för replikering](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

## <a name="recommendations-with-desired-rpo-as-input"></a>Rekommendationer med önskat RPO-mål som indata

### <a name="profiled-data"></a>Profileringsdata

![hello profileras startas data vyn i hello distribution planner](./media/site-recovery-deployment-planner/profiled-data-period.png)

**Profilerad Dataperiod**: hello period under vilken hello profilering kördes. Som standard innehåller hello verktyget alla profilerad data hello beräkning, såvida inte hello rapporten genereras för en viss period med hjälp av StartDate och EndDate alternativ under rapportgenerering av.

**Servernamnet**: hello namn eller IP-adressen för hello VMware vCenter eller ESXi-värd vars VMs rapporten genereras.

**Önskad Återställningspunktmål**: hello återställningspunktmål för din distribution. Som standard krävs hello nätverksbandbredd beräknas för värden på 15, 30 och 60 minuter. Baserat på val av hello uppdateras hello påverkas värdena för hello-blad. Om du har använt hello *DesiredRPOinMin* parameter när genererades hello rapporten, det värde som visas i hello önskad Återställningspunktmål resultat.

### <a name="profiling-overview"></a>Profileringsöversikt

![Profilering resulterar i hello distribution planner](./media/site-recovery-deployment-planner/profiling-overview.png)

**Totalt antal profileras startas virtuella datorer**: hello antalet virtuella datorer vars profilerad data är tillgängliga. Om hello VMListFile har namnen på virtuella datorer som inte har profileras startas, dessa virtuella datorer räknas inte med i hello rapportgenerering och undantas hello totala profilerad VMs antalet.

**Kompatibla virtuella datorer**: hello antal virtuella datorer som kan vara skyddade tooAzure genom att använda Site Recovery. Det är hello antalet kompatibla virtuella datorer för vilka hello krävs nätverksbandbredd, antal lagringskonton, antal Azure kärnor och antalet konfigurationsservrar och ytterligare servrar beräknas. hello information om varje kompatibel VM är tillgängliga i hello ”kompatibla virtuella datorer” avsnittet.

**Inkompatibel virtuella datorer**: hello antal profilerad virtuella datorer som är inkompatibla för skydd med Site Recovery. hello orsakerna till inkompatibilitet anges i hello ”inkompatibla VMs” avsnittet. Om hello VMListFile har namnen på virtuella datorer som inte har profileras startas, är dessa virtuella datorer undantagna från hello inkompatibla VMs count. Dessa virtuella datorer listas som ”Data kunde inte hittas” hello slutet av hello ”inkompatibla VMs” avsnittet.

**Desired RPO** (Önskat återställningspunktmål): Önskat mål för återställningspunkten (RPO) i minuter. hello rapporten genereras för tre värden: 15 (standard), 30 och 60 minuter. hello bandbredd rekommendation i hello rapporten ändras baserat på ditt val i hello önskad Återställningspunktmål nedrullningsbara listan hello upp till höger i hello-bladet. Om du har genererat hello rapport med hjälp av hello *- DesiredRPO* parameter med ett anpassat värde anpassade värdet visas som standard hello i hello önskad Återställningspunktmål nedrullningsbara listan.

### <a name="required-network-bandwidth-mbps"></a>Required Network Bandwidth (Mbps) (Nödvändig nätverksbandbredd (Mbit/s))

![Nödvändiga nätverkets bandbredd i hello distribution planner](./media/site-recovery-deployment-planner/required-network-bandwidth.png)

**toomeet Återställningspunktmål 100 procent hello tid:** hello rekommenderas bandbredd i Mbit/s toobe allokerade toomeet din önskade Återställningspunktmål 100 procent hello tid. Den här mängden bandbredd måste vara dedikerade stabilitet deltareplikering för alla dina kompatibel VMs-tooavoid eventuella överträdelser för Återställningspunktmål.

**toomeet Återställningspunktmål 90 procent av hello tid**: på grund av bredband priser eller av någon annan anledning, om du inte kan ange hello bandbredden som behövs för toomeet din önskade Återställningspunktmål 100 procent hello tid, kan du välja toogo med en mindre bandbredd som kan uppfylla dina önskade Återställningspunktmål 90 procent av hello tid. toounderstand hello följderna av att ställa in den här mindre bandbredd, hello rapporten innehåller en konsekvensanalys på hello tal och varaktighet för Återställningspunktmål överträdelser tooexpect.

**Uppnådd genomströmning:** hello genomströmning från hello-server som du har kört hello GetThroughput kommandot toohello Microsoft Azure-region där hello storage-konto finns. Det här genomströmning talet anger hello beräknad nivå som du kan få när du skyddar hello hello kompatibla virtuella datorer med hjälp av Site Recovery, förutsatt att din konfigurationsservern eller processen server lagring och nätverk Egenskaper förblir densamma som för hello-server som du har kört hello-verktyget.

Du bör ange hello rekommenderade bandbredd toomeet hello Återställningspunktmål 100 procent hello tid för replikering. När du ställer in hello bandbredd, om du inte ser en ökning i hello uppnås dataflöde som rapporteras av hello verktyget gör du följande hello:

1. Kontrollera toosee om det inte finns något nätverk tjänstkvalitet (QoS) som är att begränsa Site Recovery genomflöde.

2. Kontrollera toosee om Site Recovery-valvet hello närmsta fysiskt stöds Microsoft Azure-region toominimize Nätverksfördröjningen.

3. Kontrollera din lokal lagring egenskaper toodetermine om du kan förbättra hello maskinvara (till exempel HDD tooSSD).

4. Ändra hello Site Recovery-inställningarna i hello processervern också[öka hello mängden nätverksbandbredd som används för replikering](./site-recovery-plan-capacity-vmware.md#control-network-bandwidth).

Om du kör hello verktyget på en konfiguration eller processerver som redan har skyddat virtuella datorer, kör du verktyget för hello några gånger. hello uppnås genomströmning ändras beroende på hello mängden omsättning bearbetas samtidigt i tid.

För alla företagsdistributioner av Site Recovery bör du använda [ExpressRoute](https://aka.ms/expressroute).

### <a name="required-storage-accounts"></a>Nödvändiga lagringskonton
följande diagram visar hello Totalt antal storage-konton (standard och premium) som är nödvändiga tooprotect alla hello hello kompatibla virtuella datorer. toolearn vilket konto toouse för varje virtuell dator i avsnittet hello ”VM-storage placering”.

![Nödvändiga storage-konton i hello distribution planner](./media/site-recovery-deployment-planner/required-azure-storage-accounts.png)

### <a name="required-number-of-azure-cores"></a>Nödvändigt antal Azure-kärnor
Resultatet är hello Totalt antal kärnor toobe ställa in innan redundans eller testa redundans för alla hello kompatibla virtuella datorer. Om det finns för få kärnor i prenumerationen hello Site Recovery misslyckas toocreate VMs när hello testa redundans eller växling vid fel.

![Antal Azure kärnor i hello distribution planner](./media/site-recovery-deployment-planner/required-number-of-azure-cores.png)

### <a name="required-on-premises-infrastructure"></a>Krav på lokal infrastruktur
Den här bilden är hello Totalt antal konfigurationsservrar och ytterligare processer servrar toobe konfigurerats som skulle vara tillräckligt tooprotect hello alla kompatibla virtuella datorer. Beroende på hello stöds [storlek rekommendationer för hello konfigurationsservern](https://aka.ms/asr-v2a-on-prem-components), hello verktyget rekommendera ytterligare servrar. hello rekommendation baseras på hello större hello per dag omsättning eller hello maximalt antal skyddade virtuella datorerna (under förutsättning att ett genomsnitt av tre diskar per VM), som är träffar första på hello konfigurationsservern eller hello ytterligare processervern. Du hittar hello information om totala omsättningen per dag och antalet skyddade diskar i hello ”Input”-avsnittet.

![Lokala infrastrukturen i hello distribution planner](./media/site-recovery-deployment-planner/required-on-premises-infrastructure.png)

### <a name="what-if-analysis"></a>Konsekvensanalys
Den här analysen beskrivs hur många överträdelser kan inträffa under hello profilering period när du anger en mindre bandbredd för hello önskad Återställningspunktmål toobe uppfyllda endast 90 procent av hello tid. En eller flera överträdelser av återställningspunktmålen kan inträffa på en viss dag. hello diagrammet visar hello belastning Återställningspunktmålet för hello dag.
Baserat på denna analys, kan du bestämma om hello antalet överträdelser av Återställningspunktsmål över alla dagar och belastning Återställningspunktmål träffar per dag är acceptabel med hello angetts låg bandbredd. Om du accepterar du kan allokera hello låg bandbredd för replikering, annars allokera hello högre bandbredd som föreslagna toomeet hello önskad Återställningspunktmål 100 procent hello tid.

![Konsekvensanalys i hello distribution planner](./media/site-recovery-deployment-planner/what-if-analysis.png)

### <a name="recommended-vm-batch-size-for-initial-replication"></a>Recommended VM batch size for initial replication (Rekommenderad VM-batchstorlek för den initiala replikeringen)
I det här avsnittet rekommenderar vi hello antal virtuella datorer som kan skyddas i parallella toocomplete hello inledande replikering inom 72 timmar med hello förslag bandbredd toomeet önskad Återställningspunktmål 100 procent hello tid har angetts. Det här värdet är ett konfigurerbart värde. toochange den för närvarande skapade rapporter, Använd hello *GoalToCompleteIR* parameter.

hello här diagrammet visar ett intervall med värden för bandbredd och en beräknade VM batch storlek antal toocomplete inledande replikering 72 timmar baserat på hello medelvärde upptäckte VM storlek för alla hello kompatibla virtuella datorer.

Hello tillgänglig som förhandsversion ange hello rapporten inte vilka virtuella datorer som ska tas med i en batch. Du kan använda hello diskstorleken visas i hello ”kompatibla virtuella datorer” avsnittet toofind varje VM-storlek och markera dem för en grupp eller välja hello virtuella datorer baserat på arbetsbelastning kända egenskaper. tid för slutförande av hello hello inledande replikering ändringar proportionellt, baserat på hello faktiska VM diskens storlek, används diskutrymme och tillgängliga genomflödet.

![Rekommenderad batchstorlek för virtuella datorer](./media/site-recovery-deployment-planner/recommended-vm-batch-size.png)

### <a name="growth-factor-and-percentile-values-used"></a>Tillväxtfaktor och percentilvärde som används
Det här avsnittet längst ned hello hello sheet visar hello percentilvärdet används för alla hello prestandaräknare hello profileras startas virtuella datorer (standard är 95: e percentilen) och hello tillväxtfaktor (standardvärdet är 30 procent) som används i alla hello beräkningar.

![Tillväxtfaktor och percentilvärde som används](./media/site-recovery-deployment-planner/max-iops-and-data-churn-setting.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Rekommendationer med tillgänglig bandbredd som indata

![Rekommendationer med tillgänglig bandbredd som indata](./media/site-recovery-deployment-planner/profiling-overview-bandwidth-input.png)

Du kan ha en situation där du vet att du inte kan ange en bandbredd på mer än x Mbit/s för Site Recovery-replikering. hello-verktyget kan du tooinput tillgänglig bandbredd (med hjälp av hello - bandbredd parametern under rapportgenerering) och få hello hastigheterna Återställningspunktmål i minuter. Med detta är möjligt värde för Återställningspunktsmål, kan du bestämma om du behöver tooset upp ytterligare bandbredd eller om det är OK att ha en lösning för katastrofåterställning med den här Återställningspunktmål.

![Möjligt återställningspunktmål för 500 Mbit/s bandbredd](./media/site-recovery-deployment-planner/achievable-rpos.png)

## <a name="input"></a>Indata
hello indata kalkylbladet innehåller en översikt över hello profileras startas VMware-miljön.

![Översikt över hello profileras startas VMware-miljön](./media/site-recovery-deployment-planner/Input.png)

**Startdatum** och **slutdatum**: hello start- och slutdatum för hello profildata för skapa rapporter. Som standard är hello startdatum hello datum då profilering påbörjas, och hello slutdatum är hello datum när profilering slutar. Detta kan vara hello 'StartDate' och 'EndDate' värden om hello rapporten genereras med följande parametrar.

**Totalt antal profilering dagar**: hello Totalt antal dagar som profilering mellan hello start- och slutdatum för vilka hello rapporten genereras.

**Antal kompatibla virtuella datorer**: hello antalet kompatibla virtuella datorer för vilka hello krävs nätverkets bandbredd, antalet lagring krävs konton, Microsoft Azure kärnor, konfigurationsservrar och ytterligare servrar är Beräkna.

**Totalt antal diskar för alla kompatibla virtuella datorer**: hello-nummer som används som en hello inmatningar toodecide hello antalet konfigurationsservrar och ytterligare processer servrar toobe används i hello distributionen.

**Genomsnittligt antal diskar per kompatibla virtuella**: hello Genomsnittligt antal diskar har beräknats över alla kompatibla virtuella datorer.

**Genomsnittlig diskens storlek (GB)**: hello genomsnittlig diskstorleken beräknats över alla kompatibla virtuella datorer.

**Önskad Återställningspunktmål (minuter)**: antingen hello recovery punkt mål eller hello standardvärdet skickades för hello 'DesiredRPO' parametern när rapporten generation tooestimate krävs hello bandbredd.

**Önskad bandbredd (Mbps)**: hello-värde som du har överfört för hello 'bandbredd-parametern när rapporten generation tooestimate hello hastigheterna Återställningspunktmål.

**Observerade vanliga dataomsättningen per dag (GB)**: hello genomsnittlig omsättning observeras i alla profilering dagar. Numret används som indata hello toodecide hello flera konfigurationsservrar och ytterligare processer servrar toobe används i hello distributionen.


## <a name="vm-storage-placement"></a>Placering av VM-lagring

![Placering av VM-lagring](./media/site-recovery-deployment-planner/vm-storage-placement.png)

**Disken lagringstyp**: antingen en standard- eller premium storage konto, vilket är används tooreplicate alla hello motsvarande virtuella datorer som nämns i hello **VMs tooPlace** kolumn.

**Föreslagen prefixet**: hello föreslagna tre tecken prefix som kan användas för att namnge hello storage-konto. Du kan använda din egen prefix men hello verktyget förslag följer hello [partitions namngivningskonvention för lagringskonton](https://aka.ms/storage-performance-checklist).

**Föreslagen kontonamn**: hello lagringskonto namnet när du använder hello föreslagna prefix. Ersätt hello namn inom hakparenteser hello (< och >) med dina egna indata.

**Logga Lagringskonto**: alla hello replikeringsloggar lagras i ett standardlagringskonto. För virtuella datorer som replikerar tooa premium storage-konto kan du konfigurera en ytterligare ett standardlagringskonto för att lagra loggen. Flera lagringskonton för premiumreplikering kan använda samma standardkonto för logglagring. Virtuella datorer som är replikerade toostandard lagring konton använder hello samma lagringskonto för loggar.

**Förslag på loggen kontonamn**: Logga namnet på ditt lagringskonto när du använder hello föreslagna prefix. Ersätt hello namn inom hakparenteser hello (< och >) med dina egna indata.

**Placering sammanfattning**: en sammanfattning av hello Totalt antal virtuella datorer belastningen på hello storage-konto när hello replikering och testa redundans eller växling vid fel. Den omfattar hello Totalt antal virtuella datorer mappade toohello storage-konto, totalt antal läsning och skrivning IOPS över alla virtuella datorer placeras i det här lagringskontot totala skriva (replikering) IOPS, installationsprogrammet för total storlek för alla diskar och Totalt antal diskar.

**Virtuella datorer tooPlace**: en lista över alla hello virtuella datorer som ska placeras på hello angivna storage-konto för optimala prestanda och användning.

## <a name="compatible-vms"></a>Compatible VMs (Kompatibla virtuella datorer)
![Excel-kalkylblad med kompatibla virtuella datorer](./media/site-recovery-deployment-planner/compatible-vms.png)

**Namn på virtuell**: hello VM-namn eller IP-adress som används i hello VMListFile när en rapport skapas. Den här kolumnen visar också hello-diskar (VMDKs) som är bifogade toohello virtuella datorer. toodistinguish vCenter virtuella datorer med samma namn eller IP-adresser, hello namn inkluderar hello ESXi värdnamn. hello är listade ESXi-värd hello en där hello VM placerades när hello verktyget identifieras under hello profilering period.

**VM-kompatibilitet**: Värdena är **Ja** och **Ja**\*. **Ja** \* avser instanser i vilka hello VM är en anpassning för [Azure Premium Storage](https://aka.ms/premium-storage-workload). Här hello profileras startas hög omsättning eller IOPS disk passar i hello P20 eller P30 kategori, men hello hello diskens storlek gör att den mappade ned tooa P10 eller P20 toobe. Hej lagringskonto beslutar premium storage disk skriver toomap en disk till, baserat på dess storlek. Exempel:
* < 128 GB är en P10.
* 128 GB too512 GB är en P20.
* 512 GB too1024 GB är en P30.
* 1025 GB too2048 GB är en P40.
* 2049 GB too4095 GB är en p 50.

Om hello arbetsbelastning egenskaperna för en disk placera den i hello P20 eller P30 kategori, men hello storlek mappar den ned tooa lägre premium storage disktyp, hello verktyget markerar den virtuella datorn som **Ja**\*. hello verktyget rekommenderar också att du antingen ändra hello källa disk storlek toofit till hello rekommenderas disktyp för premium-lagring eller ändra hello mål disk typen postredundans.

**Lagringstyp**: Standard eller premium.

**Föreslagen prefixet**: hello tre tecken lagringskonto prefix.

**Lagringskontot**: hello-namnet som använder hello föreslagna lagringskonto prefix.

**Läs-/ skrivåtkomst IOPS (med tillväxtfaktor)**: hello belastning arbetsbelastning läsning och skrivning IOPS på hello disk (standard är 95: e percentilen), inklusive hello framtida tillväxtfaktor (standardvärdet är 30 procent). Observera att hello totala läsning och skrivning IOPS för en virtuell dator är inte alltid hello summan av hello VM individuella diskar läsning och skrivning IOPS, eftersom hello belastning läsning och skrivning IOPS för hello VM är hello belastning av hello summan av dess enskilda diskar läsning och skrivning IOPS under minuten hello profilering period.

**Data Omsättningsuppdateringar i Mbit/s (med tillväxtfaktor)**: hello belastning omsättningen på hello disk (standard är 95: e percentilen), inklusive hello framtida tillväxtfaktor (standardvärdet är 30 procent). Observera att hello totala dataomsättningen av hello VM är inte alltid hello summan av hello VM individuella diskar dataomsättningen eftersom hello belastning dataomsättningen av hello VM är hello belastning av hello summan av dess enskilda diskar omsättning under minuten hello profilering period.

**Azure VM-storlek**: hello perfekt mappade Azure Cloud Services virtuella datorer storleken för det lokala VM. hello mappning baseras på hello lokala VM-minne, antalet diskar-kärnor-nätverkskort och läsning/skrivning IOPS. hello rekommendation är alltid hello lägsta Azure VM-storlek som matchar alla hello lokala VM egenskaper.

**Antal diskar**: hello Totalt antal virtuella diskar (VMDKs) på hello VM.

**Diskstorlek (GB)**: hello totala installationsprogrammet storleken på alla diskar på hello VM. hello-verktyget visar också hello diskstorleken för hello individuella diskar i hello VM.

**Kärnor**: hello antalet CPU-kärnor på hello VM.

**Minne (MB)**: hello RAM-minne på hello VM.

**Nätverkskort**: hello antalet nätverkskort på hello VM.

**Starta typen**: det är Start typ av hello VM. Den kan vara BIOS eller EFI. Azure Site Recovery stöder för närvarande endast starttypen BIOS. Alla hello virtuella datorer i EFI-Start typ visas i kalkylbladet för inkompatibla virtuella datorer.

**OS-typen**: hello är hello VM OS-typen. Typen kan vara Windows, Linux eller någon annan typ.

## <a name="incompatible-vms"></a>Incompatible VMs (Inkompatibla virtuella datorer)

![Excel-ark med inkompatibla virtuella datorer](./media/site-recovery-deployment-planner/incompatible-vms.png)

**Namn på virtuell**: hello VM-namn eller IP-adress som används i hello VMListFile när en rapport skapas. Den här kolumnen visar också hello VMDKs som är bifogade toohello virtuella datorer. toodistinguish vCenter virtuella datorer med samma namn eller IP-adresser, hello namn inkluderar hello ESXi värdnamn. hello är listade ESXi-värd hello en där hello VM placerades när hello verktyget identifieras under hello profilering period.

**VM-kompatibilitet**: anger varför hello angivna VM är inkompatibla för användning med Site Recovery. hello orsaker beskrivs för varje inkompatibla hello VM och, baserat på publicerade [Lagringsgränser](https://aka.ms/azure-storage-scalbility-performance), kan vara något av följande hello:

* Diskstorleken är > 4 095 GB. Azure Storage har för närvarande inte stöd för diskar som är större än 4 095 GB.
* Operativsystemets disk är > 2 048 GB. Azure Storage har för närvarande inte stöd för operativsystemdiskar som är större än 2 048 GB.
* Starttypen är EFI. Azure Site Recovery stöder för närvarande endast starttypen BIOS för virtuella datorer.

* Total VM-storlek (replikering + TFO) överskrider storleksgränsen för hello stöds storage-konto (35 TB). Den här inkompatibiliteten sker vanligtvis när en enskild disk i hello VM har en prestanda-egenskap som överskrider hello stöds Azure eller Site Recovery gränsvärden för standardlagring. Denna instans push-meddelanden hello VM till hello premium storage zon. Hello maximalt stöds av ett premiumlagringskonto är 35 TB och en enda skyddade virtuella datorn kan inte skyddas över flera lagringskonton. Observera även att när ett redundanstest körs på en skyddad virtuell dator körs i hello samma lagringskonto där replikering pågår. Ställ in 2 x hello diskens för replikering tooprogress hello storlek och testa redundans toosucceed parallellt i den här instansen.
* Käll-IOPS överskrider IOPS-gränsen för lagring på 5 000 per disk.
* Käll-IOPS överskrider IOPS-gränsen för lagring på 80 000 per virtuell dator.
* Genomsnittlig omsättning överskrider stöds Site Recovery data omsättning gränsen på 10 Mbit/s för genomsnittlig i/o-storlek för hello disken.
* Totalt antal dataomsättningen över alla diskar på hello VM längre än hello maximala stöds Site Recovery data omsättning 54 Mbit/s per VM.
* Genomsnittlig effektiv skrivåtgärder IOPS längre än hello stöds Site Recovery IOPS 840 för disken.
* Lagring beräknade ögonblicksbilder överskrider hello stöds ögonblicksbild lagringsgräns på 10 TB.

**Läs-/ skrivåtkomst IOPS (med tillväxtfaktor)**: hello belastning arbetsbelastningen IOPS för disk hello (standard är 95: e percentilen), inklusive hello framtida tillväxtfaktor (standardvärdet är 30 procent). Observera att hello totala läsning och skrivning IOPS för hello VM är inte alltid hello summan av hello VM individuella diskar läsning och skrivning IOPS, eftersom hello belastning läsning och skrivning IOPS för hello VM är hello belastning av hello summan av dess enskilda diskar läsning och skrivning IOPS under minuten hello profilering period.

**Data Omsättningsuppdateringar i Mbit/s (med tillväxtfaktor)**: hello belastning omsättningen på hello disk (standard 95: e percentilen), inklusive hello framtida tillväxtfaktor (standard 30 procent). Observera att hello totala dataomsättningen av hello VM är inte alltid hello summan av hello VM individuella diskar dataomsättningen eftersom hello belastning dataomsättningen av hello VM är hello belastning av hello summan av dess enskilda diskar omsättning under minuten hello profilering period.

**Antal diskar**: hello Totalt antal VMDKs på hello VM.

**Diskstorlek (GB)**: hello totala installationsprogrammet storleken på alla diskar på hello VM. hello-verktyget visar också hello diskstorleken för hello individuella diskar i hello VM.

**Kärnor**: hello antalet CPU-kärnor på hello VM.

**Minne (MB)**: hello mängden RAM-minne på hello VM.

**Nätverkskort**: hello antalet nätverkskort på hello VM.

**Starta typen**: det är Start typ av hello VM. Den kan vara BIOS eller EFI. Azure Site Recovery stöder för närvarande endast starttypen BIOS. Alla hello virtuella datorer i EFI-Start typ visas i kalkylbladet för inkompatibla virtuella datorer.

**OS-typen**: hello är hello VM OS-typen. Typen kan vara Windows, Linux eller någon annan typ.


## <a name="site-recovery-limits"></a>Gränser för Site Recovery

**Replication Storage Target** (Lagringsmål för replikering) | **Average Source Disk I/O Size** (Genomsnittlig I/O-storlek för källdisk) |**Average Source Disk Data Churn** (Genomsnittlig dataomsättning för källdisk) | **Total Source Disk Data Churn Per Day** (Total dataomsättning per dag för källdisk)
---|---|---|---
Standard Storage | 8 kB | 2 Mbit/s | 168 GB per disk
Premium P10-disk | 8 kB | 2 Mbit/s | 168 GB per disk
Premium P10-disk | 16 kB | 4 Mbit/s | 336 GB per disk
Premium P10-disk | 32 kB eller mer | 8 Mbit/s | 672 GB per disk
P20- eller P30-premiumdisk | 8 kB  | 5 Mbit/s | 421 GB per disk
P20- eller P30-premiumdisk | minst 16 kB |10 Mbit/s | 842 GB per disk

Det här är genomsnittliga värden baserade på en I/O-överlappning på 30 procent. Site Recovery kan hantera högre dataflöden med annan överlappning, större skrivningsstorlek och verkligt I/O-beteende under arbetsbelastningen. hello förutsätter föregående siffror en typisk eftersläpning fem minuter. Det vill säga, när data har överförts bearbetas de och en återställningspunkt skapas inom fem minuter.

Dessa gränser är baserade på våra tester, men de täcker inte alla möjliga kombinationer av program-I/O. De faktiska resultaten kan variera beroende på blandningen av I/O i ditt program. För bästa resultat bör rekommenderar även efter att planera distribution, alltid vi att du genomför omfattande program testa med hjälp av en växling vid fel tooget hello true prestanda testbild.

## <a name="updating-hello-deployment-planner"></a>Uppdatera hello distribution planner
tooupdate hello distribution planner hello följande:

1. Hämta hello senaste versionen av hello [Azure Site Recovery-distribution planner](https://aka.ms/asr-deployment-planner).

2. Kopiera hello .zip mappen tooa server som du vill toorun på.

3. Extrahera hello ZIP-mappen.

4. Gör något av följande hello:
 * Om hello senaste versionen innehåller inte en profilering korrigering och profilering är redan pågår på din nuvarande version av hello planner, Fortsätt hello profilering.
 * Om hello senaste versionen innehåller en profilering korrigering, rekommenderar vi att du stoppar profilering på din nuvarande version och starta om hello profilering med hello ny version.

  >[!NOTE]
  >
  >När du startar profilering med hello ny version, pass hello samma utdata katalogsökväg så att hello lägger verktyget till profildata på hello befintliga filer. En fullständig uppsättning profilerad data kommer att användas toogenerate hello rapporten. Om du skickar ett annat målkatalogen nya filer skapas och gamla profileras startas data används inte toogenerate hello rapporten.
  >
  >Varje ny distribution planner är en ackumulerad uppdatering av hello ZIP-filen. Du behöver inte toocopy hello senaste toohello tidigare-mappen. Du kan skapa och använda en ny mapp.


## <a name="version-history"></a>Versionshistorik

### <a name="131"></a>1.3.1
Senast uppdaterad: 19 juli 2017

En ny funktion har lagts till:

* Tillagt stöd för stora diskar (> 1 TB) under rapportgenerering. Du kan nu använda distribution planner tooplan replikering för virtuella datorer som har storlekar för diskar som är större än 1 TB (upp till 4095 GB).
Läs mer om [stöd för stora diskar i Azure Site Recovery](https://azure.microsoft.com/en-us/blog/azure-site-recovery-large-disks/)


### <a name="13"></a>1.3
Uppdaterad: 9 maj 2017

En ny funktion har lagts till:

* Stöd för hanterad disk i rapportgenerering. hello antalet virtuella datorer kan placeras tooa enda storage-kontot är beräknade baserat på om hanteras disk har valts för växling vid fel/testa redundans.        


### <a name="12"></a>1.2
Uppdaterat: 7 april 2017

Lade till följande korrigeringar:

* Tillagda Start Skriv (BIOS eller EFI) kontrollera att varje virtuell dator toodetermine om hello virtuella datorn är kompatibel eller inkompatibel för hello skydd.
* Tillagda OS ange information för varje virtuell dator i hello kompatibla virtuella datorer och virtuella datorer som inkompatibel kalkylblad.
* Hej GetThroughput åtgärden stöds nu i hello som tillhör amerikanska myndigheter och Kina Microsoft Azure-regioner.
* Lade till några fler nödvändiga kontroller för vCenter- och ESXi-Server.
* Hämtning av felaktig rapporten genererades när nationella inställningar har angetts toonon engelska.


### <a name="11"></a>1.1
Uppdaterad: 9 mars 2017

Fast hello följande problem:

* hello-verktyget kan inte profilen virtuella datorer om hello vCenter har två eller flera virtuella datorer med hello samma namn eller IP-adress på olika ESXi-värdar.
* Kopiera och söka efter är inaktiverad för hello kompatibla virtuella datorer och virtuella datorer som inkompatibel kalkylblad.

### <a name="10"></a>1.0
Uppdaterat: 23 februari 2017

Azure Site Recovery-distribution Planner förhandsversion 1.0 har hello följande kända problem (toobe åtgärdas i kommande uppdateringar):

* hello verktyget fungerar endast för scenarier med VMware till Azure, inte för Hyper-V-till-Azure-distributioner. Scenarier med Hyper-V-till-Azure använder hello [kapacitetsplaneringsverktyget för Hyper-V](./site-recovery-capacity-planning-for-hyper-v-replication.md).
* Hej GetThroughput åtgärden stöds inte i hello som tillhör amerikanska myndigheter och Kina Microsoft Azure-regioner.
* hello-verktyget kan inte profilen virtuella datorer om hello vCenter-servern har två eller flera virtuella datorer med hello samma namn eller IP-adress på olika ESXi-värdar. I den här versionen hoppar hello verktyget profilering för samma VM-namn eller IP-adresser i hello VMListFile. hello-lösningen är tooprofile hello virtuella datorer med hjälp av en ESXi-värd i stället för hello vCenter-servern. Du måste köra en instans för varje ESXi-värd.
