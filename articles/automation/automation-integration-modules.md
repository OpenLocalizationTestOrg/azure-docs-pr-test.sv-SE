---
title: "aaaCreate en Azure Automation-modul för integrering | Microsoft Docs"
description: "Självstudiekurs som vägleder dig genom hello skapa, testa och exempel användning av integreringsmoduler i Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 27798efb-08b9-45d9-9b41-5ad91a3df41e
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/13/2017
ms.author: magoedte
ms.openlocfilehash: d4064d8b106496f4dab442024131886c4affccab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-integration-modules"></a>Azure Automation-integreringsmoduler
PowerShell är hello grundläggande tekniken bakom Azure Automation. Eftersom Azure Automation bygger på PowerShell är PowerShell-moduler viktiga toohello utökning av Azure Automation. I den här artikeln vi vägleder dig genom hello detaljerna i Azure Automation-användning av PowerShell-moduler, tooas ”integreringsmoduler” och bästa praxis för att skapa toomake att de fungerar som integreringsmoduler inom din egen PowerShell-moduler Azure Automation. 

## <a name="what-is-a-powershell-module"></a>Vad är en PowerShell-modul?
En PowerShell-modul är en grupp med PowerShell-cmdlets som **Get-Date** eller **Copy-Item**, som kan användas från hello PowerShell-konsolen, skript, arbetsflöden, runbooks och PowerShell DSC-resurser som WindowsFeature eller en fil som kan användas från PowerShell DSC-konfigurationer. Hello funktioner av PowerShell exponeras via cmdlets och DSC-resurser och varje cmdlet/DSC-resource backas upp av en PowerShell-modul många av som levereras med PowerShell sig själv. Till exempel hello **Get-Date** cmdleten är en del av hello Microsoft.PowerShell.Utility PowerShell-modulen och **Copy-Item** cmdleten är en del av hello Microsoft.PowerShell.Management PowerShell-modulen och hello paketet DSC-resurs är en del av hello PSDesiredStateConfiguration PowerShell-modulen. Båda dessa moduler medföljer PowerShell. Men många PowerShell-moduler som inte levereras som en del av PowerShell och distribueras i stället med första eller tredje parts produkter som System Center 2012 Configuration Manager eller av hello stora PowerShell på platser som PowerShell-galleriet.  hello moduler är användbara eftersom de göra komplicerade uppgifter enklare via inkapslade funktioner.  Du kan lära dig mer om [PowerShell-moduler på MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Vad är en Azure Automation-integreringsmodul?
En integreringsmodul skiljer sig inte så mycket från en PowerShell-modul. Dess helt enkelt en PowerShell-modul som alternativt innehåller en ytterligare fil - en metadatafil att ange en Azure Automation-anslutning typ toobe som används med hello modulen cmdlets i runbooks. Valfritt filen eller inte, dessa PowerShell moduler kan importeras till Azure Automation-toomake deras cmdlets som är tillgängliga för använder i runbooks och deras DSC-resurser som är tillgängliga för användning i DSC-konfigurationer. Azure Automation hello bakgrunden, lagrar dessa moduler och på runbook-jobb och körningstid för DSC-kompilering jobbet läser in dem i hello Azure Automation sandboxar där runbooks körs och DSC-konfigurationer har kompilerats.  DSC resurser i moduler placeras automatiskt på hämtningsservern för hello Automation DSC, så att de kan hämtas av datorer som försöker tooapply DSC-konfigurationer.  

Vi levererar ett antal Azure PowerShell-moduler från hello rutan i Azure Automation för du toouse så att du kan komma igång automatisera hanteringen av Azure direkt, men du kan importera PowerShell-moduler för det system, en tjänst eller ett verktyg som du vill toointegrate med. 

> [!NOTE]
> Vissa moduler levereras som ”globala-moduler i hello Automation-tjänsten. De här globala moduler är tillgängliga tooyou när du skapar ett automation-konto och vi uppdatera dem ibland som automatiskt skickar dem tooyour automation-konto. Om du inte vill att de toobe automatiskt uppdaterade alltid kan du importera hello samma modul själv och som har högre prioritet än hello global Modulversion på den modul som vi leverera hello-tjänsten. 

hello-formatet som du importerar en integrationsmodulspaketet är en komprimerad fil med samma namn som modulen hello och filtillägget .zip hello. Den innehåller hello Windows PowerShell-modulen och alla stödfiler, inklusive en manifestfilen (.psd1) om hello modulen har en sådan.

Om hello modulen bör innehålla en Azure Automation-anslutningstypen, måste den också innehålla en fil med namnet hello `<ModuleName>-Automation.json` som anger hello egenskaperna för anslutningstypen. Detta är en json-fil placeras i hello modulen mapp i din komprimerad ZIP-fil som innehåller hello fält för en ”anslutning” som är nödvändiga tooconnect toohello system eller hello tjänstemodulen representerar. Detta resulterar i att en anslutningstyp skapas i Azure Automation. Med den här filen som du kan ange hello fältnamn typer, och om hello-fält ska vara krypterade och / eller valfria för hello anslutningstypen för hello-modulen. hello följande är en mall i hello json-format:

```
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Om du har distribuerat Service Management Automation och skapat integreringsmoduler paket för automation-runbooks, bör detta vara insatt tooyou. 

## <a name="authoring-best-practices"></a>Metodtips för redigering
Även om integreringsmoduler i stort sett PowerShell-moduler kan det fortfarande är ett antal saker som vi rekommenderar att du överväger när du redigerar en PowerShell-modul, toomake den mest användbara i Azure Automation. Vissa av dessa är Azure Automation-specifika och några av dem är användbar bara toomake modulerna fungerar bra i PowerShell-arbetsflöde, oavsett om du använder Automation. 

1. Innehåller en sammanfattning, beskrivning, och hjälpa URI för varje cmdlet i hello-modulen. I PowerShell, kan du definiera vissa hjälpinformation för cmdlets tooallow hello tooreceive hjälp om hur du använder dem med hello **Get-Help** cmdlet. Så här kan du till exempel definiera en sammanfattning och hjälp-URI för en PowerShell-modul som skrivits i en .psm1-fil.<br>  
   
    ```
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='http://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> Att tillhandahålla den här informationen endast visas inte det här hjälp med att använda hello **Get-Help** cmdlet i Hej PowerShell-konsolen, den visar även funktionen hjälp i Azure Automation.  Till exempel när aktiviteter infogas i samband med runbook-redigering. Klicka på ”Visa detaljerad hjälp” kommer öppna hello hjälp URI: N på en annan flik hello webbläsare du använder tooaccess Azure Automation.<br>![Hjälp med integreringsmoduler](media/automation-integration-modules/automation-integration-module-activitydesc.png)
2. Om hello körs modulen mot ett fjärrsystem

    a. Den ska innehålla en Integrationsmodul metadata-fil som definierar hello information som behövs för tooconnect toothat fjärrsystemet, vilket innebär att hello anslutningstypen.  
    b. Varje cmdlet i hello modulen ska kunna tootake i ett anslutningsobjekt (en instans av den typ av anslutning) som en parameter.  

    Cmdletar i hello modulen blir enklare toouse i Azure Automation om du tillåter skickar ett objekt med hello fält i hello anslutningstyp som en parameter toohello cmdlet. Det här sättet användarna har inte toomap parametrarna för motsvarande hello anslutning tillgången toohello cmdlet-parametrar varje gång de anropa en cmdlet. Baserat på hello runbook-exemplet ovan, den använder en Twilio anslutningstillgång kallas CorpTwilio tooaccess Twilio och returnera alla hello telefonnummer i hello-konto.  Observera hur den mappning hello fält i hello toohello anslutningsparametrar för hello cmdlet?<br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Ett enklare och bättre sätt tooapproach är detta direkt att hello anslutning objektet toohello cmdlet-
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    Du kan aktivera beteende så här för din cmdlets genom att låta dem tooaccept ett anslutningsobjekt direkt som en parameter i stället för bara Anslutningsfält för parametrar. Vanligtvis vill du en parameter för var och en, så att en användare som inte använder Azure Automation kan anropa din cmdlets utan att konstruera en hash-tabell tooact som hello connection-objektet. Parameteruppsättningen **SpecifyConnectionFields** nedan finns används toopass hello fältet anslutningsegenskaper i taget. **UseConnectionObject** kan du skicka hello direkt via anslutningen. Som du ser hello skicka TwilioSMS cmdlet i hello [Twilio PowerShell-modulen](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) kan skicka antingen sätt: 
   
    ```
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='http://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
3. Definiera utdata för alla cmdletar i hello modul. Definiera Utdatatyp för en cmdlet: en kan designläge IntelliSense utdata toohelp du fastställa hello egenskaper för hello-cmdlet för användning under redigering. Det är särskilt användbart under Automation runbook grafiska redigering, där design tid är viktiga tooan enkelt användarupplevelse med din modul.<br><br> ![Utdatatyp för grafiska runbooks](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> Detta påminner om toohello ”vidare typ” funktionerna för en cmdlet utdata i PowerShell ISE utan toorun den.<br><br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
4. Cmdletar i hello modulen bör inte ta komplexa objekt av typen för parametrar. Till skillnad från PowerShell lagrar PowerShell Workflow komplexa typer i avserialiserat format. Primitiva typer finns kvar som primitiver, men komplexa typer som är konverterade tootheir avserialiseras versioner som är i stort sett egenskapsuppsättningar. Om du använde hello exempelvis **Get-Process** cmdlet i en runbook (eller ett PowerShell-arbetsflöde att) skulle den returnerar ett objekt av typen [Deserialized.System.Diagnostic.Process], inte hello förväntade [ Typ av System.Diagnostic.Process]. Den här typen har alla hello samma egenskaper som hello inte deserialisera typen, men ingen av hello-metoderna. Om du försöker toopass värdet som en parameter tooa cmdlet, där hello cmdlet förväntar sig ett [System.Diagnostic.Process]-värde för den här parametern får du hello följande fel: *kan inte bearbeta omvandling av argumentet i parametern ”process” . Fel: ”Det går inte att konvertera hello” System.Diagnostics.Process (CcmExec) ”värde av typen” Deserialized.System.Diagnostics.Process ”tootype” System.Diagnostics.Process ”.*   Det beror på att det finns ett typmatchningsfel mellan hello förväntat [System.Diagnostic.Process] typ och hello [Deserialized.System.Diagnostic.Process] typ som angetts. hello sätt runt det här problemet är tooensure hello cmdlets modulens inte börjar komplexa typer för parametrar. Här är hello fel sätt toodo den.
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    Här är hello rätt sätt, i en primitiv som kan användas internt av hello cmdlet toograb hello komplexa objekt och använda den. Eftersom cmdlets kör hello gäller PowerShell, blir inte PowerShell-arbetsflöde, inuti hello cmdlet $process hello rätt [System.Diagnostic.Process]-typen.  
   
    ```
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   Anslutningstillgångar i runbooks är hashtabeller som är en komplex typ, och ännu dessa hashtabeller verka toobe kan toobe skickades till cmdlets för sina – anslutningsparametern perfekt med inget cast-undantag. Tekniskt sett vissa typer av PowerShell är kan toocast korrekt från sin serialiserade formuläret tootheir avserialiseras form och därför kan skickas till cmdlets för parametrar som accepterar hello inte deserialisera typen. Hashtable är ett exempel. Det är möjligt för en modul Skapad definierade typer toobe införts på ett sätt som de kan korrekt deserialisera samt, men det finns vissa avvägningarna tooconsider. hello typ måste toohave en standardkonstruktor, har alla dess egenskaper för offentliga och har en PSTypeConverter. Men för redan användardefinierade typer som hello modul Skapad inte äger det finns en alldeles för ”åtgärdas” därför hello rekommendation tooavoid komplexa typer för parametrar i sin helhet. Runbook Authoring-tips: om av vissa skäl din cmdlets måste tootake en komplex typparametern eller du använder en annan modul som kräver en komplex typparametern hello lösning i PowerShell-arbetsflöde runbooks och PowerShell-arbetsflöden i lokala PowerShell, är toowrap hello cmdlet som genererar hello komplex typ och hello-cmdletar som förbrukar hello komplex typ i hello samma InlineScript-aktivitet. Eftersom InlineScript utförs innehållet som PowerShell i stället för att PowerShell-arbetsflöde, hello cmdlet genererar hello komplex typ resulterar i att rätt typ, avserialiseras inte hello komplexa typen.
5. Gör alla cmdletar i hello modulen tillståndslösa. PowerShell-arbetsflödet kör varje cmdlet anropas i hello arbetsflöde i en annan session. Det innebär att alla cmdlets som är beroende av sessionstillstånd skapas / ändras av andra cmdletar i samma modul inte fungerar i PowerShell-arbetsflöde runbooks hello.  Här är ett exempel på vad du inte toodo.
   
    ```
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
6. hello modulen ska fullständigt finns i ett Xcopy kan paket. Eftersom Azure Automation-moduler är distribuerade toohello Automation sandboxar när runbooks måste tooexecute, måste de toowork oberoende av hello värden som de körs på. Det innebär att du ska vara kan tooZip in hello modulen paketet, flytta den tooany andra värden med hello samma eller en senare version av PowerShell och det fungerar som vanligt när de importeras till den värd PowerShell-miljö. För att toohappen hello modulen bör inte beroende av några filer utanför hello modulen mapp (hello mapp som hämtar zippade in när du importerar till Azure Automation) eller unika registerinställningar på en värd till exempel de som anges av hello installerar av en produkt. Om denna bästa praxis inte följs hello modulen inte kan användas i Azure Automation.  

## <a name="next-steps"></a>Nästa steg

* tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)
* toolearn mer om hur du skapar PowerShell-moduler finns [skriva en Windows PowerShell-modul](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)

