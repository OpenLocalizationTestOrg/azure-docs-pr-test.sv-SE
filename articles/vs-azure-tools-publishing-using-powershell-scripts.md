---
title: "aaaUsing Windows PowerShell-skript tooPublish tooDev och testmiljöer | Microsoft Docs"
description: "Lär dig hur toouse Windows PowerShell-skript från Visual Studio toopublish toodevelopment- och testmiljöer."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5fff1301-5469-4d97-be88-c85c30f837c1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 491a058f96255576afa74f6156f20ae9559bb9f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-windows-powershell-scripts-toopublish-toodev-and-test-environments"></a>Med Windows PowerShell-skript toopublish toodev- och testmiljöer
När du skapar ett webbprogram i Visual Studio kan generera du en Windows PowerShell-skript som du kan använda senare tooautomate hello publicering av din webbplats tooAzure som en Webbapp i Azure App Service eller en virtuell dator. Du kan redigera och utöka hello Windows PowerShell-skript i hello Visual Studio-redigeraren toosuit dina krav eller integrera hello skript med befintliga bygga, testa och publishing skript.

Med dessa skript kan etablera du anpassade versioner (även kallat utvecklings- och testmiljöer) för din plats för temporär användning. Du kan till exempel konfigurera en viss version av din webbplats på en virtuell Azure-dator eller på hello mellanlagringsplatsen på en webbplats toorun ett test-paket, återskapa ett programfel, och testa en felkorrigering, utvärderingsversion föreslagna ändringen eller konfigurera en anpassad miljö för demonstration eller presentation. När du har skapat ett skript som publicerar ditt projekt kan du återskapa identiska miljöer genom att köra hello skript efter behov eller kör hello skript med din egen version av din web application toocreate en anpassad miljö för att testa.

## <a name="what-you-need"></a>Vad du behöver
* Azure SDK 2.3 eller senare. Se [Visual Studio-hämtningar](http://go.microsoft.com/fwlink/?LinkID=624384) för mer information.

Du behöver inte hello Azure SDK toogenerate hello skript för webbprojekt. Den här funktionen är för webbprojekt, inte webbroller i molntjänster.

* Azure PowerShell 0.7.4 eller senare. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information.
* [Windows PowerShell 3.0](http://go.microsoft.com/?linkid=9811175) eller senare.

## <a name="additional-tools"></a>Ytterligare verktyg
Det finns ytterligare verktyg och resurser för att arbeta med PowerShell i Visual Studio för Azure-utveckling. Se [PowerShell-verktyg för Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-hello-publish-scripts"></a>Generera hello publicera skript
Du kan generera hello publicera skript för en virtuell dator som är värd för din webbplats när du skapar ett nytt projekt genom att följa [instruktionerna](virtual-machines/windows/classic/web-app-visual-studio.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Du kan också [generera publicera skript för web apps i Azure App Service](app-service-web/app-service-web-get-started-dotnet.md).

## <a name="scripts-that-visual-studio-generates"></a>Skript som skapas av Visual Studio
Visual Studio skapar en lösning på objektnivå mapp med namnet **PublishScripts** som innehåller två filer i Windows PowerShell, ett publicera-skript för den virtuella datorn eller webbplats och en modul som innehåller funktioner som du kan använda i hello skript. Visual Studio genererar också en fil i hello JSON-format som anger hello detaljer i hello-projekt som du distribuerar.

### <a name="windows-powershell-publish-script"></a>Windows PowerShell publicera skript
hello publicera skript innehåller specifika publicera steg för att distribuera tooa webbplats eller virtuell dator. Visual Studio tillhandahåller syntax färgläggning för Windows PowerShell-utveckling. Hjälp för hello funktioner är tillgänglig och kan du fritt redigera hello funktioner i hello skriptet toosuit föränderliga behov.

### <a name="windows-powershell-module"></a>Windows PowerShell-modulen
hello Windows PowerShell-modulen som Visual Studio genererar innehåller funktioner som hello publicera skriptet använder. Dessa är Azure PowerShell-funktioner och är inte avsedda toobe ändras. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för mer information.

### <a name="json-configuration-file"></a>JSON-konfigurationsfil
hello JSON-filen skapas i hello **konfigurationer** mapp och innehåller konfigurationsdata som anger exakt vilka resurser toodeploy tooAzure. hello heter hello-filen som Visual Studio genererar projekt-namn-WAWS-dev.json om du har skapat en webbplats eller projekt namnet-VM-dev.json om du har skapat en virtuell dator. Här är ett exempel på en JSON-konfigurationsfil som genereras när du skapar en webbplats. De flesta hello värden är självförklarande. hello webbplatsnamn genereras av Azure, så den inte kan matcha ditt projektnamn.

```json
{
    "environmentSettings": {
        "webSite": {
            "name": "WebApplication26632",
            "location": "West US"
        },
        "databases": [{
            "connectionStringName": "DefaultConnection",
            "databaseName": "WebApplication26632_db",
            "serverName": "YourDatabaseServerName",
            "user": "sqluser2",
            "password": "",
            "edition": "",
            "size": "",
            "collation": "",
            "location": "West US"
        }]
    }
}
```
När du skapar en virtuell dator verkar liknande toohello följande hello JSON-konfigurationsfil. Observera att en tjänst i molnet har skapats som en behållare för hello virtuell dator. hello virtuella dator innehåller hello vanliga slutpunkter för webbåtkomst via HTTP och HTTPS, samt slutpunkter för webbdistribution där du kan publicera toohello webbplatsen från din lokala dator, fjärrskrivbord och Windows PowerShell.

```json
{
    "environmentSettings": {
        "cloudService": {
            "name": "myusernamevm1",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myusernamevm1",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [{
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [{
            "connectionStringName": "",
            "databaseName": "",
            "serverName": "",
            "user": "",
            "password": ""
        }],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Du kan redigera hello JSON configuration toochange vad som händer när du kör hello publicera skript. Hej `cloudService` och `virtualMachine` avsnitt krävs, men du kan ta bort hello `databases` avsnittet om du inte behöver den. hello-egenskaper som är tomma i konfigurationsfilen för hello standard som genereras av Visual Studio är valfritt. de som har värden i hello standardkonfigurationsfilen krävs.

Om du har en webbplats som har flera distributionsmiljöer (kallas även fack) i stället för en enda produktionsplatsen i Azure, kan du inkludera hello platsnamnet i hello namnet på hello-webbplats i hello JSON-konfigurationsfil. Om du har en webbplats som heter exempelvis **webbplats** och en plats för den namngivna **testa** hello URI är test.cloudapp.net webbplats, men hello rätt namn toouse i konfigurationsfilen för hello är mysite(test) . Du kan göra detta endast om hello webbplats och distributionsplatser som redan finns i din prenumeration. Om de inte redan finns, skapa hello webbplats genom att köra hello skript utan att ange hello plats och skapa sedan hello plats i hello [klassiska Azure-portalen](http://go.microsoft.com/fwlink/?LinkID=213885), och därefter köra hello skript med hello ändrade webbplatsnamn. Mer information om distributionsplatser för webbprogram finns [skapa mellanlagringsmiljöer för web apps i Azure App Service](app-service-web/web-sites-staged-publishing.md).

## <a name="how-toorun-hello-publish-scripts"></a>Hur toorun hello publicera skript
Om du aldrig har kört Windows PowerShell-skript före, måste du först ställa in hello körning princip tooenable skript toorun. Detta är säkerhet funktionen tooprevent användare från att köra Windows PowerShell-skript om de är sårbar toomalware eller virus som rör skript körs.

### <a name="run-hello-script"></a>Kör hello-skript
1. Skapa hello Web Deploy-paket för projektet. Ett Web Deploy-paket är en komprimerad fil (ZIP-fil) som innehåller filer som du vill toocopy tooyour webbplats eller virtuell dator. Du kan skapa Web Deploy-paket i Visual Studio för alla webbprogram.

![Skapa webb distribuera paket](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

Mer information finns i [så här: skapa ett Webbdistributionspaket i Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Du kan också automatisera hello skapandet av Web Deploy-paket, enligt beskrivningen i avsnittet hello **anpassa och utöka hello publicera skript** senare i det här avsnittet.

1. I **Solution Explorer**, öppna hello snabbmenyn för hello skript och välj sedan **öppna med PowerShell ISE**.
2. Om detta är hello första gången som du har kört Windows PowerShell-skript på den här datorn, öppna Kommandotolken med administratörsbehörighet och typen hello följande kommando:

    ```powershell
    Set-ExecutionPolicy RemoteSigned
    ```

3. Logga in tooAzure med hjälp av följande kommando hello.

    ```powershell
    Add-AzureAccount
    ```

    När du uppmanas, ange ditt användarnamn och lösenord.

    Observera att den här metoden för att tillhandahålla autentiseringsuppgifter för Azure inte fungerar när du automatiserar hello skript. I stället bör du använda hello .publishsettings-fil tooprovide autentiseringsuppgifter. En gång, kommandot hello **Get-AzurePublishSettingsFile** toodownload hello från Azure och därefter använda **importera AzurePublishSettingsFile** tooimport hello-filen. Detaljerade instruktioner finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

4. (Valfritt) Om du vill toocreate Azure resurser, till exempel hello virtuell dator, databas och webbplats utan att publicera ditt webbprogram använder hello **publicera WebApplication.ps1** med hello **-konfiguration** argumentet inställt toohello JSON-konfigurationsfil. Den här kommandoraden använder hello JSON configuration file toodetermine toocreate vilka resurser. Eftersom den använder hello standardinställningarna för andra kommandoradsargument skapar hello resurser, men publicera inte ditt webbprogram. hello – får utförlig alternativet du mer information om vad som händer.

    ```powershell
    Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json
    ```

5. Använd hello **publicera WebApplication.ps1** kommandot som visas i något av följande exempel tooinvoke hello skript hello och publicera ditt webbprogram. Om du behöver toooverride hello standardinställningarna för alla hello andra argument, till exempel hello prenumerationsnamn publicera paketnamn, autentiseringsuppgifter för virtuell dator eller server Databasautentiseringsuppgifter kan du ange parametrarna. Använd hello **– utförlig** alternativet toosee mer information om hello förloppet för hello publiceringsprocessen.

    ```powershell
    Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
    –SubscriptionName Contoso `
    -WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
    -DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
    -Verbose
    ```

    Om du skapar en virtuell dator, hello kommando som ser ut som följande hello. Det här exemplet visar även hur toospecify hello autentiseringsuppgifter för flera databaser. Hello virtuella datorer som dessa skript skapar är hello SSL-certifikatet inte från en betrodd rotcertifikatutfärdare. Du måste därför toouse hello **– AllowUntrusted** alternativet.

    ```powershell
    Publish-WebApplication.ps1 `
    -Configuration C:\Path\ADVM-VM-test.json `
    -SubscriptionName Contoso `
    -WebDeployPackage C:\Path\ADVM.zip `
    -AllowUntrusted `
    -VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
    -DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
    -Verbose
    ```

    hello skript kan skapa databaser, men det skapar inte databasservrar. Om du vill toocreate en databasserver, kan du använda hello **ny AzureSqlDatabaseServer** funktion i hello Azure-modulen.

## <a name="customizing-and-extending-hello-publish-scripts"></a>Anpassa och utöka hello publicera skript
Du kan anpassa hello publicera skript och JSON-konfigurationsfil. Hej funktioner i Windows PowerShell-modulen för hello **AzureWebAppPublishModule.psm1** är inte avsedda toobe ändras. Om du bara vill toospecify en annan databas eller ändra några av hello egenskaper för hello virtuell dator kan du redigera hello JSON-konfigurationsfil. Om du vill använda tooextend hello av hello skriptet tooautomate bygger och testar ditt projekt, kan du implementera funktionen stub-rutiner i **publicera WebApplication.ps1**.

tooautomate bygga projektet, lägga till kod som anropar MSBuild för`New-WebDeployPackage` som visas i det här kodexemplet. hello sökvägen toohello MSBuild-kommandot är olika beroende på hello version av Visual Studio som du har installerat. Hej tooget sökväg, som du kan använda funktionen för hello **Get-MSBuildCmd**som visas i det här exemplet.

### <a name="tooautomate-building-your-project"></a>tooautomate skapa projektet
1. Lägg till hello `$ProjectFile` parameter i hello globala param-avsnittet.

    ```powershell
    [Parameter(Mandatory = $false)]
    [ValidateScript({Test-Path $_ -PathType Leaf})]
    [String]
    $ProjectFile,
    ```

2. Kopiera hello funktionen `Get-MSBuildCmd` till skriptfilen.

    ```powershell
    function Get-MSBuildCmd
    {
            process
    {

                $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                    Sort-Object {[double]$_.PSChildName} -Descending |
                                    Select-Object -First 1 |
                                    Get-ItemProperty -Name MSBuildToolsPath |
                                    Select -ExpandProperty MSBuildToolsPath

                $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

            return Get-Item $path
        }
    }
    ```

3. Ersätt `New-WebDeployPackage` med hello följande kod och Ersätt hello platshållare i hello rad konstruera `$msbuildCmd`. Den här koden är för Visual Studio 2015. Om du använder Visual Studio 2013, ändra hello **VisualStudioVersion** egenskapen nedan för`12.0`.

    ```powershell
    function New-WebDeployPackage
    {
        #Write a function toobuild and package your web application
    ```

    toobuild ditt webbprogram som använder MsBuild.exe. Mer information finns i MSBuild Kommandoradsreferens på: [http://go.microsoft.com/fwlink/?LinkId=391339](http://go.microsoft.com/fwlink/?LinkId=391339)

    ```powershell
    Write-VerboseWithTime 'Build-WebDeployPackage: Start'

    $msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory

    Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
    ```

### <a name="start-execution-of-hello-build-command"></a>Starta körning av hello build-kommando

```powershell
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru

if ($job.ExitCode -ne 0) {
    throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain hello project name
$projectName = (Get-Item $ProjectFile).BaseName

#Construct hello path tooweb deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName

#Get hello full path for hello web deploy zip package. This is required for MSDeploy toowork
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir

Write-VerboseWithTime 'Build-WebDeployPackage: End'

return $WebDeployPackage
}
```

1. Anropa hello `New-WebDeployPackage` funktionen innan den här raden: `$Config = Read-ConfigFile $Configuration` för webbappar eller `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` för virtuella datorer.

    ```powershell
    if($ProjectFile)
    {
    $WebDeployPackage = New-WebDeployPackage
    }
    ```

2. Anropa hello anpassade skript från kommandoraden med hjälp av passera hello `$Project` argument, som i följande exempel kommandoraden hello.

    ```powershell
    .\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
    -ProjectFile ..\WebApplication5\WebApplication5.csproj `
    -VMPassword @{Name="VMUser";Password="Test.123"} `
    -AllowUntrusted `
    -Verbose
    ```

    tooautomate testning av programmet, lägger du till kod för`Test-WebApplication`. Vara säker på att toouncomment hello rader i **publicera WebApplication.ps1** där dessa funktioner kallas. Om du inte anger en implementering kan du skapa manuellt ditt projekt i Visual Studio och kör hello publicera skriptet toopublish tooAzure.

## <a name="publishing-function-summary"></a>Sammanfattning av Publishing funktioner
tooget hjälp för funktioner som du kan använda Kommandotolken hello Windows PowerShell, Använd hello kommando `Get-Help function-name`. hello-hjälpen innehåller parametern hjälp och exempel. hello samma hjälptexten finns också i hello skript källfiler **AzureWebAppPublishModule.psm1** och **publicera WebApplication.ps1**. hello skript och hjälper dig lokaliserade i Visual Studio-språk.

**AzureWebAppPublishModule**

| Funktionsnamn | Beskrivning |
| --- | --- |
| Lägg till AzureSQLDatabase |Skapar en ny Azure SQL-databas. |
| Lägg till AzureSQLDatabases |Skapar Azure SQL-databaser från hello-värdena i hello JSON-konfigurationsfil som genereras av Visual Studio. |
| Lägg till AzureVM |Skapar en virtuell dator i Azure och returnerar hello URL för hello distribueras VM. hello funktionen ställer in hello förutsättningar och sedan anropar hello **ny AzureVM** fungera (Azure-modulen) toocreate en ny virtuell dator. |
| Lägg till AzureVMEndpoints |Lägger till nya virtuella datorn för inkommande slutpunkter tooa och returnerar hello virtuell dator med hello ny slutpunkt. |
| Lägg till AzureVMStorage |Skapar ett nytt Azure storage-konto i hello nuvarande prenumeration. hello namnet på hello konto börjar med ”devtest” följt av en unik alfanumerisk sträng. hello returneras hello namnet på hello nytt lagringskonto. Du måste ange en plats eller en tillhörighetsgrupp för hello nytt lagringskonto. |
| Lägg till AzureWebsite |Skapar en webbplats med angivna hello-namn och plats. Den här funktionen anropar hello **ny AzureWebsite** funktion i hello Azure-modulen. Om hello prenumerationen inte redan innehåller en webbplats med angivna hello-namnet, den här funktionen skapar hello webbplats och returnerar ett objekt för webbplatsen. Annars returneras `$null`. |
| Backup-prenumeration |Sparar hello aktuella Azure-prenumeration i hello `$Script:originalSubscription` variabeln i skriptet omfång. Den här funktionen sparar hello aktuella Azure-prenumeration (som erhålls av `Get-AzureSubscription -Current`) och storage-konto, och hello-prenumeration som har ändrats med det här skriptet (lagrad i hello variabel `$UserSpecifiedSubscription`) och dess storage-kontot i skriptet omfång. Genom att spara hello värden kan du använda en funktion som `Restore-Subscription`, toorestore hello ursprungliga aktuell prenumeration och lagring toocurrent kontostatus om hello aktuell status har ändrats. |
| Hitta AzureVM |Hämtar hello angivna virtuella Azure-datorn. |
| Formatet DevTestMessageWithTime |Annat datum och tid tooa hälsningsmeddelande. Den här funktionen är avsedd för meddelanden som skrivs toohello fel och utförlig dataströmmar. |
| Get-AzureSQLDatabaseConnectionString |Monterar en anslutning sträng tooconnect tooan Azure SQL-databas. |
| Get-AzureVMStorage |Returnerar hello namnet på hello första lagringskonto med hello namnmönster ”devtest*” (skiftlägeskänsligt) i hello angivna plats eller tillhörighet gruppen. Om hello ”devtest*” lagringskontot matchar inte hello platsen eller tillhörighetsgruppen, hello funktionen ignoreras. Du måste ange en plats eller en tillhörighetsgrupp. |
| Get-MSDeployCmd |Returnerar ett kommando toorun hello MsDeploy.exe verktyg. |
| Ny AzureVMEnvironment |Söker efter eller skapar en virtuell dator i hello-prenumeration som matchar hello värden i hello JSON-konfigurationsfil. |
| Publicera WebPackage |Publicera paketet använder MsDeploy.exe och en webbplats. ZIP-filen toodeploy resurser tooa webbplats. Den här funktionen genererar inte inga utdata. Om hello anropet tooMSDeploy.exe misslyckas genereras hello ett undantag. tooget mer detaljerade utdata, Använd hello **-Verbose** alternativet. |
| Publicera WebPackageToVM |Verifierar hello parametervärden och anropar sedan hello **publicera WebPackage** funktion. |
| Läs ConfigFile |Validerar hello JSON-konfigurationsfil och returnerar en hash-tabell för valda värden. |
| Restore-prenumeration |Återställer hello aktuell prenumeration toohello ursprungliga prenumeration. |
| Testa AzureModule |Returnerar `$true` om hello installerade Azure Modulversion är 0.7.4 eller senare. Returnerar `$false` om hello modulen är inte installerat eller är en tidigare version. Den här funktionen har inga parametrar. |
| Testa AzureModuleVersion |Returnerar `$true` hello versionen om hello Azure-modulen är 0.7.4 eller senare. Returnerar `$false` om hello modulen är inte installerat eller är en tidigare version. Den här funktionen har inga parametrar. |
| Testa HttpsUrl |Konverterar hello inkommande URL tooa System.Uri-objekt. Returnerar `$True` om hello URL är absolut och dess schemat är https. Returnerar `$false` om hello URL: en är relativ, dess schema inte är HTTPS eller hello Indatasträngen går inte att konvertera tooa URL. |
| Test-medlem |Returnerar `$true` om en egenskap eller metod är medlem i hello-objektet. Annars returnerar `$false`. |
| Skriv ErrorWithTime |Skriver ett felmeddelande med hello prefixet aktuell tid. Den här funktionen anropar hello **Format DevTestMessageWithTime** funktionen tooprepend hello tid innan skrivning hello meddelandeströmmen toohello fel. |
| Skriv HostWithTime |Skriver ett meddelande toohello värdprogram (**Write-Host**) med hello prefixet aktuell tid. hello effekten av skriva toohello värdprogrammet varierar. De flesta program som är värdar för Windows PowerShell skriver dessa meddelanden toostandard utdata. |
| Skriv VerboseWithTime |Skriver ett utförligt meddelande med hello prefixet aktuell tid. Eftersom det anropar **Write-Verbose**, hello-meddelande visas endast när hello körs skriptet med hello **utförlig** parametern eller när hello **VerbosePreference** inställningar är ställa in också**Fortsätt**. |

**Publicera WebApplication**

| Funktionsnamn | Beskrivning |
| --- | --- |
| Ny AzureWebApplicationEnvironment |Skapar Azure-resurser, till exempel en webbplats eller virtuell dator. |
| Ny WebDeployPackage |Den här funktionen är inte implementerad. Du kan lägga till kommandon i den här funktionen toobuild projektet. |
| Publicera AzureWebApplication |Publicerar en web application tooAzure. |
| Publicera WebApplication |Skapar och distribuerar Web Apps, virtuella datorer, SQL-databaser och storage-konton för ett webbprojekt i Visual Studio. |
| Test-WebApplication |Den här funktionen är inte implementerad. Du kan lägga till kommandon i den här funktionen tootest ditt program. |

## <a name="next-steps"></a>Nästa steg
Mer information om PowerShell-skript genom att läsa [med Windows PowerShell-skript](https://technet.microsoft.com/library/bb978526.aspx) och se andra Azure PowerShell-skript på hello [Script Center](https://azure.microsoft.com/documentation/scripts/).
