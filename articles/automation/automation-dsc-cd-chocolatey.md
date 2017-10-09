---
title: aaaAzure Automation DSC kontinuerlig distribution med Chocolatey | Microsoft Docs
description: "DevOps kontinuerlig distribution med Azure Automation DSC och Chocolatey Pakethanteraren.  Exempel med fullständig JSON ARM-mallen och PowerShell-källa."
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.assetid: c0baa411-eb76-4f91-8d14-68f68b4805b6
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 10/29/2016
ms.author: golive
ms.openlocfilehash: 60af52af5f834fd48e3a0dc4677919397b53f0f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="usage-example-continuous-deployment-toovirtual-machines-using-automation-dsc-and-chocolatey"></a>Exempel på användning: Kontinuerlig distribution tooVirtual datorer med hjälp av Automation DSC och Chocolatey
Det finns många verktyg tooassist med olika punkter i hello kontinuerlig Integration pipeline i en DevOps-värld.  Konfigurationen för Azure Automation önskade tillstånd (DSC) är en Välkommen nya tillägg toohello alternativ DevOps team kan använda.  Den här artikeln visar inställningen in kontinuerlig distribution (CD) för en Windows-dator.  Du kan enkelt utöka hello tekniken tooinclude så många Windows-datorer som behövs i hello roll (till exempel en webbplats) och från tooadditional roller samt.

![Kontinuerlig distribution för IaaS-VM](./media/automation-dsc-cd-chocolatey/cdforiaasvm.png)

## <a name="at-a-high-level"></a>På en hög nivå
Det finns lite som pågår, men som tur är det kan delas i två huvudsakliga processer: 

* Skriva kod och testa det, skapar och publicerar installationspaket för högre och lägre version av hello system. 
* Skapa och hantera virtuella datorer som ska installera och köra hello kod i hello-paket.  

När båda dessa kärnprocesser är på plats är det ett kort steg tooautomatically hello uppdateringspaket som körs på en viss VM nya versioner skapas och distribueras.

## <a name="component-overview"></a>Översikt över komponenten
Paketet chefer som [lgh get](https://en.wikipedia.org/wiki/Advanced_Packaging_Tool) är ganska välkända i Linux hälsningsmeddelande, men inte så mycket i Windows hälsningsmeddelande.  [Chocolatey](https://chocolatey.org/) sådana sak och Scott Hanselman [blogg](http://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx) på hello avsnittet är en bra introduktion.  I kort sagt kan kan Chocolatey du tooinstall paket från en central databas av paket i en Windows-dator med hjälp av kommandoraden hello.  Du kan skapa och hantera egna databasen och Chocolatey kan installera paket från valfritt antal databaser som du har angett.

Desired Configuration tillstånd (DSC) ([översikt](https://technet.microsoft.com/library/dn249912.aspx)) är ett PowerShell-verktyg som gör att du toodeclare hello konfiguration som du vill använda för en dator.  Anta exempelvis att du kan, ”jag vill Chocolatey installerad, jag vill att IIS har installerats, jag vill öppna port 80, jag vill 1.0.0 på webbplatsen installerade versionen”.  hello DSC Local Configuration Manager implementerar (MGM) denna konfiguration. En DSC Pull-Server innehåller en databas av konfigurationer för dina datorer. hello MGM på varje dator kontrollerar i regelbundet toosee om dess konfiguration matchar hello lagrade konfigurationen. Det kan rapportera status eller försök toobring hello datorn tillbaka till anpassningen hello lagrade konfigurationen. Du kan redigera hello lagrade konfigurationen på hello pull server toocause en dator eller en uppsättning datorer toocome till justering med hello ändrat konfigurationen.

Azure Automation är en hanterad tjänst i Microsoft Azure som gör att du tooautomate olika uppgifter med hjälp av runbooks, noder, autentiseringsuppgifter, resurser och resurser, t.ex scheman och globala variabler. Azure Automation DSC utökar denna automatisering kapaciteten tooinclude PowerShell DSC-verktyg.  Här är en bra [översikt](automation-dsc-overview.md).

En DSC-resurs är en modul av kod som har specifika funktioner, t.ex hantering av nätverk, Active Directory eller SQL Server.  Hej Chocolatey DSC-resurs vet hur tooaccess en NuGet-Server (bland annat) ladda ned paket, installera paket och så vidare.  Det finns många andra DSC-resurser i hello [PowerShell-galleriet](http://www.powershellgallery.com/packages?q=dsc+resources&prerelease=&sortOrder=package-title).  Dessa moduler är installerade i Azure Automation DSC Pull-servern (av du) så att de kan användas av dina konfigurationer.

Resource Manager-mallar tillhandahåller en deklarativ metod för att skapa din infrastruktur – till exempel nätverk, undernät, nätverkssäkerhet och routning, läsa in belastningsutjämning, nätverkskort, virtuella datorer och så vidare.  Här är en [artikel](../azure-resource-manager/resource-manager-deployment-model.md) att jämför hello Resource Manager-modellen (deklarativ) med hello Azure Service Management (ASM eller klassisk) distribution modellen (tvingande) och beskriver hello core resurs providers, beräkning, lagring och nätverk.

En nyckelfunktion i Resource Manager-mall är dess möjlighet tooinstall VM-tillägget i hello VM eftersom den har etablerats.  En VM-tillägget har specifika funktioner, till exempel använda ett anpassat skript, installera antivirusprogram eller ett DSC-konfigurationsskript.  Det finns många andra typer av VM-tillägg.

## <a name="quick-trip-around-hello-diagram"></a>Snabb kommunikation runt hello diagram
Startar hello överst du skriva koden, skapa, testa och sedan skapa ett installationspaket.  Chocolatey kan hantera olika typer av installationspaket, till exempel MSI MSU, ZIP.  Och du har hello alla fördelar med PowerShell toodo hello verklig installation om Chocolateys inbyggda funktioner som inte är helt upp tooit.  Placera hello paketet i okänd nås – en paket-databas.  Det här exemplet användning använder en offentlig mapp i ett Azure blob storage-konto, men det kan finnas var som helst.  Chocolatey fungerar internt med NuGet-servrar och några andra för hantering av paketmetadata.  [Den här artikeln](https://github.com/chocolatey/choco/wiki/How-To-Host-Feed) beskrivs hello alternativ.  Det här exemplet användning använder NuGet.  En Nuspec är metadata om dina paket.  Hej Nuspec ”kompileras” till Nupkgs och lagras i en NuGet-server.  När konfigurationen av begäranden i ett paket med namnet och refererar till en NuGet-server, hämtar hello paketet hello Chocolatey DSC-resurs (nu på hello VM) och installerar du.  Du kan också begära en viss version av ett paket.

Hello nedre vänstra hörnet i hello bild finns det en mall för Azure Resource Manager (ARM).  I det här exemplet användning registrerar hello VM-tillägget hello VM med hello Hämtningsservern för Azure Automation DSC (det vill säga en pull-server) som en nod.  hello konfigurationen lagras i hello pull-server.  Faktiskt, lagras två gånger: en gång i klartext och när kompilerats som en MOF-fil (för de vet om sådant.)  Hello-portalen är hello MOF ”nodkonfiguration” (som skillnad från toosimply ”configuration”).  Dess hello artefakt som är associerad med en nod så hello nod vet dess konfiguration.  Informationen nedan visar hur tooassign hello nod configuration toohello nod.

Förmodligen gör du redan hello bitars hello överst eller de flesta av den.  Skapa hello nuspec, kompilering och lagra det i en NuGet-server är en liten sak.  Och du hanterar redan virtuella datorer.  Tar hello nästa steg toocontinuous distribution kräver ställer in hello pull-server (en gång), registrerar noderna med den (en gång), och skapa och lagra det hello-konfiguration (först).  Uppdatera sedan som paket som uppgraderas och distribueras toohello databasen hello konfiguration och konfiguration av noden i hello pull-server (Upprepa efter behov).

Om du inte behöver börja med en ARM-mall, är det också OK.  Det finns PowerShell cmdlets som är utformade toohelp du registrera dina virtuella datorer med hello pull-server och alla hello rest. Mer information finns i den här artikeln: [Onboarding datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md)

## <a name="step-1-setting-up-hello-pull-server-and-automation-account"></a>Steg 1: Konfigurera hello pull-servern och automation-konto
Vid en autentiserad (Add-AzureRmAccount) PowerShell-kommandorad: (kan ta några minuter innan hello pull-server har konfigurerats)

    New-AzureRmResourceGroup –Name MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES
    New-AzureRmAutomationAccount –ResourceGroupName MY-AUTOMATION-RG –Location MY-RG-LOCATION-IN-QUOTES –Name MY-AUTOMATION-ACCOUNT 

Du kan placera ditt automation-konto till någon av hello följande regioner (aka plats): östra USA 2, södra centrala USA, oss Gov Virginia, Västeuropa, Sydostasien, östra, centrala Indien och Australien-sydost, Kanada Central, Norra Europa.

## <a name="step-2-vm-extension-tweaks-toohello-arm-template"></a>Steg 2: VM-tillägget justeringar toohello ARM-mall
Information för VM-registrering (med hello PowerShell DSC VM-tillägget) i det här [Azure Quickstart mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/dsc-extension-azure-automation-pullserver).  Det här steget registrerar den nya virtuella datorn med hello hämtningsservern i hello lista över DSC-noder.  En del av denna registrering är att ange hello nod configuration toobe tillämpas toohello nod.  Konfiguration av den här noden inte redan har tooexist i hello pull-servern så att det är OK steg 4 är där detta är klar för hello första gången.  Men här i steg 2 behöver toohave bestämt hello namn för hello nod och hello på hello konfiguration.  I det här exemplet användning hello nod är 'isvbox' och hello konfigurationen är 'ISVBoxConfig'.  Hej nodkonfigurationsnamnet (toobe anges i DeploymentTemplate.json) är därför 'ISVBoxConfig.isvbox'.  

## <a name="step-3-adding-required-dsc-resources-toohello-pull-server"></a>Steg 3: Lägga till nödvändiga resurser toohello hämtningsservern DSC
hello PowerShell-galleriet är instrumenterade tooinstall DSC-resurser i Azure Automation-konto.  Navigera toohello resurs du vill använda och klicka på hello ”distribuera tooAzure Automation”.

![PowerShell-galleriet exempel](./media/automation-dsc-cd-chocolatey/xNetworking.PNG)

En annan metod nyligen lagt till toohello Azure-portalen kan du toopull i nya moduler eller uppdatera befintliga moduler. Klicka dig igenom hello Automation-konto resurs hello tillgångar panelen och slutligen hello moduler panelen.  hello Bläddra galleriet ikonen kan du toosee hello lista med moduler i hello-galleriet, detaljnivån information och slutligen importera till ditt Automation-konto. Detta är ett bra sätt tookeep modulerna in toodate från tid tootime. Och hello importfunktionen kontrollerar beroenden med andra moduler tooensure ingenting hämtar synkroniserad.

Eller så har hello manuell metod.  hello mappstruktur för en PowerShell-modul för integrering för en Windows-dator skiljer sig något från hello mappstrukturen förväntades av hello Azure Automation.  Detta kräver lite modifiera från din sida.  Men det är inte svårt och det görs bara en gång per resurs (såvida du inte vill tooupgrade den i framtiden.)  Mer information om redigering av PowerShell integreringsmoduler finns i den här artikeln: [redigering integreringsmoduler för Azure Automation](https://azure.microsoft.com/blog/authoring-integration-modules-for-azure-automation/)

* Installera hello-modul som du behöver på din arbetsstation, enligt följande:
  * Installera [Windows Management Framework, v5](http://aka.ms/wmf5latest) (behövs inte för Windows 10)
  * `Install-Module –Name MODULE-NAME`< – grabs hello modul från hello PowerShell-galleriet 
* Kopiera hello modulen mapp från `c:\Program Files\WindowsPowerShell\Modules\MODULE-NAME` tooa tillfällig mapp 
* Ta bort exempel och dokumentation från hello huvudsakliga mappen 
* Komprimerad hello huvudsakliga mapp hello naming hello ZIP-filen exakt samma som hello mapp 
* Placera hello ZIP-filen i en HTTP-plats som kan nås, till exempel blob-lagring i Azure Storage-konto.
* Kör följande PowerShell:
  
      New-AzureRmAutomationModule `
          -ResourceGroupName MY-AUTOMATION-RG -AutomationAccountName MY-AUTOMATION-ACCOUNT `
          -Name MODULE-NAME –ContentLink "https://STORAGE-URI/CONTAINERNAME/MODULE-NAME.zip"

hello ingår exempel utför de här stegen för cChoco och xNetworking. Se hello [anteckningar](#notes) för särskild hantering för cChoco.

## <a name="step-4-adding-hello-node-configuration-toohello-pull-server"></a>Steg 4: Lägga till hello nod configuration toohello pull-server
Det finns inget särskilt om hello första gången du importera konfigurationen till hello hämtningsservern och kompilering.  Alla efterföljande import/kompilerar av hello samma konfiguration utseende exakt hello samma.  Varje gång du uppdaterar paketet och behöver toopush ut tooproduction du göra det här steget när du har säkerställt hello konfigurationsfilen är korrekt – inklusive hello ny version av paketet.  Här är hello konfigurationsfilen och PowerShell:

ISVBoxConfig.ps1:

    Configuration ISVBoxConfig 
    { 
        Import-DscResource -ModuleName cChoco 
        Import-DscResource -ModuleName xNetworking

        Node "isvbox" {   

            cChocoInstaller installChoco 
            { 
                InstallDir = "C:\choco" 
            }

            WindowsFeature installIIS 
            { 
                Ensure="Present" 
                Name="Web-Server" 
            }

            xFirewall WebFirewallRule 
            { 
                Direction = "Inbound" 
                Name = "Web-Server-TCP-In" 
                DisplayName = "Web Server (TCP-In)" 
                Description = "IIS allow incoming web site traffic." 
                DisplayGroup = "IIS Incoming Traffic" 
                State = "Enabled" 
                Access = "Allow" 
                Protocol = "TCP" 
                LocalPort = "80" 
                Ensure = "Present" 
            }

            cChocoPackageInstaller trivialWeb 
            {            
                Name = "trivialweb" 
                Version = "1.0.0" 
                Source = “MY-NUGET-V2-SERVER-ADDRESS” 
                DependsOn = "[cChocoInstaller]installChoco", 
                "[WindowsFeature]installIIS" 
            } 
        }    
    }

Nya-ConfigurationScript.ps1:

    Import-AzureRmAutomationDscConfiguration ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -SourcePath C:\temp\AzureAutomationDsc\ISVBoxConfig.ps1 ` 
        -Published –Force

    $jobData = Start-AzureRmAutomationDscCompilationJob ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -ConfigurationName ISVBoxConfig 

    $compilationJobId = $jobData.Id

    Get-AzureRmAutomationDscCompilationJob ` 
        -ResourceGroupName MY-AUTOMATION-RG –AutomationAccountName MY-AUTOMATION-ACCOUNT ` 
        -Id $compilationJobId

Dessa steg resultera i en ny konfiguration av noden med namnet ”ISVBoxConfig.isvbox” Hej hämtningsservern placeras.  namn på konfigurationsnod hello bygger som ”configurationName.nodeName”.

## <a name="step-5-creating-and-maintaining-package-metadata"></a>Steg 5: Skapa och underhålla paketmetadata
För varje paket du föra in hello paketdatabasen, behöver du en nuspec som beskriver den.  Den nuspec måste kompileras och lagras i NuGet-server. Den här processen beskrivs [här](http://docs.nuget.org/create/creating-and-publishing-a-package).  Du kan använda MyGet.org som en NuGet-server.  De sälja den här tjänsten, men har en starter SKU som är ledigt.  Vid NuGet.org hittar du anvisningar om hur du installerar en egen NuGet-server för dina privata paket.

## <a name="step-6-tying-it-all-together"></a>Steg 6: Binda alla ihop
Varje gång en version skickar QA och har godkänts för distribution, hello paketet skapas, nuspec och nupkg uppdateras och distribuerat toohello NuGet-servern.  Hello konfigurationen (steg 4 ovan) måste dessutom vara uppdaterade tooagree med hello nytt versionsnummer.  Det måste skickas toohello hämtningsservern och kompileras.  Från den punkten på är det toohello virtuella datorer som är beroende av att konfigurationen toopull hello update och installera den.  Var och en av de här uppdateringarna är enkla - bara en eller två rader i PowerShell.  Några av dem är inkapslade i build-uppgifter som kan sammankopplas i en version i Visual Studio Team Services hello fallet.  Detta [artikel](https://www.visualstudio.com/en-us/docs/alm-devops-feature-index#continuous-delivery) innehåller mer information.  Detta [GitHub-repo-](https://github.com/Microsoft/vso-agent-tasks) information hello olika tillgängliga build-uppgifter.

## <a name="notes"></a>Anteckningar
Det här exemplet för användning som börjar med en virtuell dator från en allmän Windows Server 2012 R2-avbildning från hello Azure-galleriet.  Du kan starta från en lagrad bild och sedan justera därifrån med hello DSC-konfigurationen.  Ändra konfiguration som är inbyggd i en bild är dock mycket svårare än hello-konfiguration som använder DSC uppdateras dynamiskt.

Du har inte toouse en ARM-mall och hello VM-tillägget toouse den här tekniken med dina virtuella datorer.  Och dina virtuella datorer har inte toobe på Azure toobe CD: N hanteras.  Allt som krävs är att Chocolatey installeras och hello MGM konfigurerats på hello VM så att den vet var hello hämtningsservern finns.  

Naturligtvis när du uppdaterar ett paket på en virtuell dator som är i produktion, måste tootake den virtuella datorn utanför rotation medan hello uppdateringen är installerad.  Hur du gör detta varierar mycket.  Till exempel med en virtuell dator bakom en belastningsutjämnare i Azure, du kan lägga till en anpassad avsökning.  När du uppdaterar hello VM har hello avsökningen endpoint returnera en 400.  hello justera nödvändiga toocause den här ändringen kan vara i din konfiguration som kan hello justera tooswitch tillbaka tooreturning en 200 när hello uppdateringen är klar.

Fullständig källan för det här exemplet för användning finns i [Visual Studio-projekt](https://github.com/sebastus/ARM/tree/master/CDIaaSVM) på GitHub.

## <a name="related-articles"></a>Relaterade artiklar
* [Azure Automation DSC-översikt](automation-dsc-overview.md)
* [Azure Automation DSC-cmdlets](https://msdn.microsoft.com/library/mt244122.aspx)
* [Onboarding-datorer för hantering av Azure Automation DSC](automation-dsc-onboarding.md)

