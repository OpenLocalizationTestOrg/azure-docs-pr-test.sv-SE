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
# <a name="azure-automation-integration-modules"></a><span data-ttu-id="012a7-103">Azure Automation-integreringsmoduler</span><span class="sxs-lookup"><span data-stu-id="012a7-103">Azure Automation Integration Modules</span></span>
<span data-ttu-id="012a7-104">PowerShell är hello grundläggande tekniken bakom Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-104">PowerShell is hello fundamental technology behind Azure Automation.</span></span> <span data-ttu-id="012a7-105">Eftersom Azure Automation bygger på PowerShell är PowerShell-moduler viktiga toohello utökning av Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-105">Since Azure Automation is built on PowerShell, PowerShell modules are key toohello extensibility of Azure Automation.</span></span> <span data-ttu-id="012a7-106">I den här artikeln vi vägleder dig genom hello detaljerna i Azure Automation-användning av PowerShell-moduler, tooas ”integreringsmoduler” och bästa praxis för att skapa toomake att de fungerar som integreringsmoduler inom din egen PowerShell-moduler Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-106">In this article, we will guide you through hello specifics of Azure Automation’s use of PowerShell modules, referred tooas “Integration Modules”, and best practices for creating your own PowerShell modules toomake sure they work as Integration Modules within Azure Automation.</span></span> 

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="012a7-107">Vad är en PowerShell-modul?</span><span class="sxs-lookup"><span data-stu-id="012a7-107">What is a PowerShell Module?</span></span>
<span data-ttu-id="012a7-108">En PowerShell-modul är en grupp med PowerShell-cmdlets som **Get-Date** eller **Copy-Item**, som kan användas från hello PowerShell-konsolen, skript, arbetsflöden, runbooks och PowerShell DSC-resurser som WindowsFeature eller en fil som kan användas från PowerShell DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="012a7-108">A PowerShell module is a group of PowerShell cmdlets like **Get-Date** or **Copy-Item**, that can be used from hello PowerShell console, scripts, workflows, runbooks, and PowerShell DSC resources like WindowsFeature or File, that can be used from PowerShell DSC configurations.</span></span> <span data-ttu-id="012a7-109">Hello funktioner av PowerShell exponeras via cmdlets och DSC-resurser och varje cmdlet/DSC-resource backas upp av en PowerShell-modul många av som levereras med PowerShell sig själv.</span><span class="sxs-lookup"><span data-stu-id="012a7-109">All of hello functionality of PowerShell is exposed through cmdlets and DSC resources, and every cmdlet/DSC resource is backed by a PowerShell module, many of which ship with PowerShell itself.</span></span> <span data-ttu-id="012a7-110">Till exempel hello **Get-Date** cmdleten är en del av hello Microsoft.PowerShell.Utility PowerShell-modulen och **Copy-Item** cmdleten är en del av hello Microsoft.PowerShell.Management PowerShell-modulen och hello paketet DSC-resurs är en del av hello PSDesiredStateConfiguration PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="012a7-110">For example, hello **Get-Date** cmdlet is part of hello Microsoft.PowerShell.Utility PowerShell module, and **Copy-Item** cmdlet is part of hello Microsoft.PowerShell.Management PowerShell module and hello Package DSC resource is part of hello PSDesiredStateConfiguration PowerShell module.</span></span> <span data-ttu-id="012a7-111">Båda dessa moduler medföljer PowerShell.</span><span class="sxs-lookup"><span data-stu-id="012a7-111">Both of these modules ship with PowerShell.</span></span> <span data-ttu-id="012a7-112">Men många PowerShell-moduler som inte levereras som en del av PowerShell och distribueras i stället med första eller tredje parts produkter som System Center 2012 Configuration Manager eller av hello stora PowerShell på platser som PowerShell-galleriet.</span><span class="sxs-lookup"><span data-stu-id="012a7-112">But many PowerShell modules do not ship as part of PowerShell, and are instead distributed with first or third-party products like System Center 2012 Configuration Manager or by hello vast PowerShell community on places like PowerShell Gallery.</span></span>  <span data-ttu-id="012a7-113">hello moduler är användbara eftersom de göra komplicerade uppgifter enklare via inkapslade funktioner.</span><span class="sxs-lookup"><span data-stu-id="012a7-113">hello modules are useful because they make complex tasks simpler through encapsulated functionality.</span></span>  <span data-ttu-id="012a7-114">Du kan lära dig mer om [PowerShell-moduler på MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="012a7-114">You can learn more about [PowerShell modules on MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span></span> 

## <a name="what-is-an-azure-automation-integration-module"></a><span data-ttu-id="012a7-115">Vad är en Azure Automation-integreringsmodul?</span><span class="sxs-lookup"><span data-stu-id="012a7-115">What is an Azure Automation Integration Module?</span></span>
<span data-ttu-id="012a7-116">En integreringsmodul skiljer sig inte så mycket från en PowerShell-modul.</span><span class="sxs-lookup"><span data-stu-id="012a7-116">An Integration Module isn't very different from a PowerShell module.</span></span> <span data-ttu-id="012a7-117">Dess helt enkelt en PowerShell-modul som alternativt innehåller en ytterligare fil - en metadatafil att ange en Azure Automation-anslutning typ toobe som används med hello modulen cmdlets i runbooks.</span><span class="sxs-lookup"><span data-stu-id="012a7-117">Its simply a PowerShell module that optionally contains one additional file - a metadata file specifying an Azure Automation connection type toobe used with hello module's cmdlets in runbooks.</span></span> <span data-ttu-id="012a7-118">Valfritt filen eller inte, dessa PowerShell moduler kan importeras till Azure Automation-toomake deras cmdlets som är tillgängliga för använder i runbooks och deras DSC-resurser som är tillgängliga för användning i DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="012a7-118">Optional file or not, these PowerShell modules can be imported into Azure Automation toomake their cmdlets available for use within runbooks and their DSC resources available for use within DSC configurations.</span></span> <span data-ttu-id="012a7-119">Azure Automation hello bakgrunden, lagrar dessa moduler och på runbook-jobb och körningstid för DSC-kompilering jobbet läser in dem i hello Azure Automation sandboxar där runbooks körs och DSC-konfigurationer har kompilerats.</span><span class="sxs-lookup"><span data-stu-id="012a7-119">Behind hello scenes, Azure Automation stores these modules, and at runbook job and DSC compilation job execution time, loads them into hello Azure Automation sandboxes where runbooks are executed and DSC configurations are compiled.</span></span>  <span data-ttu-id="012a7-120">DSC resurser i moduler placeras automatiskt på hämtningsservern för hello Automation DSC, så att de kan hämtas av datorer som försöker tooapply DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="012a7-120">Any DSC resources in modules are also automatically placed on hello Automation DSC pull server, so that they can be pulled by machines attempting tooapply DSC configurations.</span></span>  

<span data-ttu-id="012a7-121">Vi levererar ett antal Azure PowerShell-moduler från hello rutan i Azure Automation för du toouse så att du kan komma igång automatisera hanteringen av Azure direkt, men du kan importera PowerShell-moduler för det system, en tjänst eller ett verktyg som du vill toointegrate med.</span><span class="sxs-lookup"><span data-stu-id="012a7-121">We ship a number of Azure  PowerShell modules out of hello box in Azure Automation for you toouse so you can get started automating Azure management right away, but you can import PowerShell modules for whatever system, service, or tool you want toointegrate with.</span></span> 

> [!NOTE]
> <span data-ttu-id="012a7-122">Vissa moduler levereras som ”globala-moduler i hello Automation-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="012a7-122">Certain modules are shipped as “global modules” in hello Automation service.</span></span> <span data-ttu-id="012a7-123">De här globala moduler är tillgängliga tooyou när du skapar ett automation-konto och vi uppdatera dem ibland som automatiskt skickar dem tooyour automation-konto.</span><span class="sxs-lookup"><span data-stu-id="012a7-123">These global modules are available tooyou when you create an automation account, and we update them sometimes which automatically pushes them out tooyour automation account.</span></span> <span data-ttu-id="012a7-124">Om du inte vill att de toobe automatiskt uppdaterade alltid kan du importera hello samma modul själv och som har högre prioritet än hello global Modulversion på den modul som vi leverera hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="012a7-124">If you don’t want them toobe auto-updated, you can always import hello same module yourself, and that will take precedence over hello global module version of that module that we ship in hello service.</span></span> 

<span data-ttu-id="012a7-125">hello-formatet som du importerar en integrationsmodulspaketet är en komprimerad fil med samma namn som modulen hello och filtillägget .zip hello.</span><span class="sxs-lookup"><span data-stu-id="012a7-125">hello format in which you import an Integration Module package is a compressed file with hello same name as hello module and a .zip extension.</span></span> <span data-ttu-id="012a7-126">Den innehåller hello Windows PowerShell-modulen och alla stödfiler, inklusive en manifestfilen (.psd1) om hello modulen har en sådan.</span><span class="sxs-lookup"><span data-stu-id="012a7-126">It contains hello Windows PowerShell module and any supporting files, including a manifest file (.psd1) if hello module has one.</span></span>

<span data-ttu-id="012a7-127">Om hello modulen bör innehålla en Azure Automation-anslutningstypen, måste den också innehålla en fil med namnet hello `<ModuleName>-Automation.json` som anger hello egenskaperna för anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="012a7-127">If hello module should contain an Azure Automation connection type, it must also contain a file with hello name `<ModuleName>-Automation.json` that specifies hello connection type properties.</span></span> <span data-ttu-id="012a7-128">Detta är en json-fil placeras i hello modulen mapp i din komprimerad ZIP-fil som innehåller hello fält för en ”anslutning” som är nödvändiga tooconnect toohello system eller hello tjänstemodulen representerar.</span><span class="sxs-lookup"><span data-stu-id="012a7-128">This is a json file placed within hello module folder of your compressed .zip file, and contains hello fields of a “connection” that is required tooconnect toohello system or service hello module represents.</span></span> <span data-ttu-id="012a7-129">Detta resulterar i att en anslutningstyp skapas i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-129">This will end up creating a connection type in Azure Automation.</span></span> <span data-ttu-id="012a7-130">Med den här filen som du kan ange hello fältnamn typer, och om hello-fält ska vara krypterade och / eller valfria för hello anslutningstypen för hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="012a7-130">Using this file you can set hello field names, types, and whether hello fields should be encrypted and / or optional, for hello connection type of hello module.</span></span> <span data-ttu-id="012a7-131">hello följande är en mall i hello json-format:</span><span class="sxs-lookup"><span data-stu-id="012a7-131">hello following is a template in hello json file format:</span></span>

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

<span data-ttu-id="012a7-132">Om du har distribuerat Service Management Automation och skapat integreringsmoduler paket för automation-runbooks, bör detta vara insatt tooyou.</span><span class="sxs-lookup"><span data-stu-id="012a7-132">If you have deployed Service Management Automation and created Integration Modules packages for your automation runbooks, this should look very familiar tooyou.</span></span> 

## <a name="authoring-best-practices"></a><span data-ttu-id="012a7-133">Metodtips för redigering</span><span class="sxs-lookup"><span data-stu-id="012a7-133">Authoring Best Practices</span></span>
<span data-ttu-id="012a7-134">Även om integreringsmoduler i stort sett PowerShell-moduler kan det fortfarande är ett antal saker som vi rekommenderar att du överväger när du redigerar en PowerShell-modul, toomake den mest användbara i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-134">Even though Integration Modules are essentially PowerShell modules, there’s still a number of things we recommend you consider while authoring a PowerShell module, toomake it most usable in Azure Automation.</span></span> <span data-ttu-id="012a7-135">Vissa av dessa är Azure Automation-specifika och några av dem är användbar bara toomake modulerna fungerar bra i PowerShell-arbetsflöde, oavsett om du använder Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-135">Some of these are Azure Automation specific, and some of them are useful just toomake your modules work well in PowerShell Workflow, regardless of whether or not you’re using Automation.</span></span> 

1. <span data-ttu-id="012a7-136">Innehåller en sammanfattning, beskrivning, och hjälpa URI för varje cmdlet i hello-modulen.</span><span class="sxs-lookup"><span data-stu-id="012a7-136">Include a synopsis, description, and help URI for every cmdlet in hello module.</span></span> <span data-ttu-id="012a7-137">I PowerShell, kan du definiera vissa hjälpinformation för cmdlets tooallow hello tooreceive hjälp om hur du använder dem med hello **Get-Help** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="012a7-137">In PowerShell, you can define certain help information for cmdlets tooallow hello user tooreceive help on using them with hello **Get-Help** cmdlet.</span></span> <span data-ttu-id="012a7-138">Så här kan du till exempel definiera en sammanfattning och hjälp-URI för en PowerShell-modul som skrivits i en .psm1-fil.</span><span class="sxs-lookup"><span data-stu-id="012a7-138">For example, here’s how you can define a synopsis and help URI for a PowerShell module written in a .psm1 file.</span></span><br>  
   
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
   <br> <span data-ttu-id="012a7-139">Att tillhandahålla den här informationen endast visas inte det här hjälp med att använda hello **Get-Help** cmdlet i Hej PowerShell-konsolen, den visar även funktionen hjälp i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-139">Providing this info will not only show this help using hello **Get-Help** cmdlet in hello PowerShell console, it will also expose this help functionality within Azure Automation.</span></span>  <span data-ttu-id="012a7-140">Till exempel när aktiviteter infogas i samband med runbook-redigering.</span><span class="sxs-lookup"><span data-stu-id="012a7-140">For example, when inserting activities during runbook authoring.</span></span> <span data-ttu-id="012a7-141">Klicka på ”Visa detaljerad hjälp” kommer öppna hello hjälp URI: N på en annan flik hello webbläsare du använder tooaccess Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-141">Clicking “View detailed help” will open hello help URI in another tab of hello web browser you’re using tooaccess Azure Automation.</span></span><br><span data-ttu-id="012a7-142">![Hjälp med integreringsmoduler](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span><span class="sxs-lookup"><span data-stu-id="012a7-142">![Integration Module Help](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span></span>
2. <span data-ttu-id="012a7-143">Om hello körs modulen mot ett fjärrsystem</span><span class="sxs-lookup"><span data-stu-id="012a7-143">If hello module runs against a remote system,</span></span>

    <span data-ttu-id="012a7-144">a.</span><span class="sxs-lookup"><span data-stu-id="012a7-144">a.</span></span> <span data-ttu-id="012a7-145">Den ska innehålla en Integrationsmodul metadata-fil som definierar hello information som behövs för tooconnect toothat fjärrsystemet, vilket innebär att hello anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="012a7-145">It should contain an Integration Module metadata file that defines hello information needed tooconnect toothat remote system, meaning hello connection type.</span></span>  
    <span data-ttu-id="012a7-146">b.</span><span class="sxs-lookup"><span data-stu-id="012a7-146">b.</span></span> <span data-ttu-id="012a7-147">Varje cmdlet i hello modulen ska kunna tootake i ett anslutningsobjekt (en instans av den typ av anslutning) som en parameter.</span><span class="sxs-lookup"><span data-stu-id="012a7-147">Each cmdlet in hello module should be able tootake in a connection object (an instance of that connection type) as a parameter.</span></span>  

    <span data-ttu-id="012a7-148">Cmdletar i hello modulen blir enklare toouse i Azure Automation om du tillåter skickar ett objekt med hello fält i hello anslutningstyp som en parameter toohello cmdlet.</span><span class="sxs-lookup"><span data-stu-id="012a7-148">Cmdlets in hello module become easier toouse in Azure Automation if you allow passing an object with hello fields of hello connection type as a parameter toohello cmdlet.</span></span> <span data-ttu-id="012a7-149">Det här sättet användarna har inte toomap parametrarna för motsvarande hello anslutning tillgången toohello cmdlet-parametrar varje gång de anropa en cmdlet.</span><span class="sxs-lookup"><span data-stu-id="012a7-149">This way users don’t have toomap parameters of hello connection asset toohello cmdlet's corresponding parameters each time they call a cmdlet.</span></span> <span data-ttu-id="012a7-150">Baserat på hello runbook-exemplet ovan, den använder en Twilio anslutningstillgång kallas CorpTwilio tooaccess Twilio och returnera alla hello telefonnummer i hello-konto.</span><span class="sxs-lookup"><span data-stu-id="012a7-150">Based on hello runbook example above, it uses a Twilio connection asset called CorpTwilio tooaccess Twilio and return all hello phone numbers in hello account.</span></span>  <span data-ttu-id="012a7-151">Observera hur den mappning hello fält i hello toohello anslutningsparametrar för hello cmdlet?</span><span class="sxs-lookup"><span data-stu-id="012a7-151">Notice how it is mapping hello fields of hello connection toohello parameters of hello cmdlet?</span></span><br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    <span data-ttu-id="012a7-152">Ett enklare och bättre sätt tooapproach är detta direkt att hello anslutning objektet toohello cmdlet-</span><span class="sxs-lookup"><span data-stu-id="012a7-152">An easier and better way tooapproach this is directly passing hello connection object toohello cmdlet -</span></span>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    <span data-ttu-id="012a7-153">Du kan aktivera beteende så här för din cmdlets genom att låta dem tooaccept ett anslutningsobjekt direkt som en parameter i stället för bara Anslutningsfält för parametrar.</span><span class="sxs-lookup"><span data-stu-id="012a7-153">You can enable behavior like this for your cmdlets by allowing them tooaccept a connection object directly as a parameter, instead of just connection fields for parameters.</span></span> <span data-ttu-id="012a7-154">Vanligtvis vill du en parameter för var och en, så att en användare som inte använder Azure Automation kan anropa din cmdlets utan att konstruera en hash-tabell tooact som hello connection-objektet.</span><span class="sxs-lookup"><span data-stu-id="012a7-154">Usually you’ll want a parameter set for each, so that a user not using Azure Automation can call your cmdlets without constructing a hashtable tooact as hello connection object.</span></span> <span data-ttu-id="012a7-155">Parameteruppsättningen **SpecifyConnectionFields** nedan finns används toopass hello fältet anslutningsegenskaper i taget.</span><span class="sxs-lookup"><span data-stu-id="012a7-155">Parameter set **SpecifyConnectionFields** below is used toopass hello connection field properties one by one.</span></span> <span data-ttu-id="012a7-156">**UseConnectionObject** kan du skicka hello direkt via anslutningen.</span><span class="sxs-lookup"><span data-stu-id="012a7-156">**UseConnectionObject** lets you pass hello connection straight through.</span></span> <span data-ttu-id="012a7-157">Som du ser hello skicka TwilioSMS cmdlet i hello [Twilio PowerShell-modulen](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) kan skicka antingen sätt:</span><span class="sxs-lookup"><span data-stu-id="012a7-157">As you can see, hello Send-TwilioSMS cmdlet in hello [Twilio PowerShell module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) allows passing either way:</span></span> 
   
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
3. <span data-ttu-id="012a7-158">Definiera utdata för alla cmdletar i hello modul.</span><span class="sxs-lookup"><span data-stu-id="012a7-158">Define output type for all cmdlets in hello module.</span></span> <span data-ttu-id="012a7-159">Definiera Utdatatyp för en cmdlet: en kan designläge IntelliSense utdata toohelp du fastställa hello egenskaper för hello-cmdlet för användning under redigering.</span><span class="sxs-lookup"><span data-stu-id="012a7-159">Defining an output type for a cmdlet allows design-time IntelliSense toohelp you determine hello output properties of hello cmdlet, for use during authoring.</span></span> <span data-ttu-id="012a7-160">Det är särskilt användbart under Automation runbook grafiska redigering, där design tid är viktiga tooan enkelt användarupplevelse med din modul.</span><span class="sxs-lookup"><span data-stu-id="012a7-160">It is especially helpful during Automation runbook graphical authoring, where design time knowledge is key tooan easy user experience with your module.</span></span><br><br> <span data-ttu-id="012a7-161">![Utdatatyp för grafiska runbooks](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span><span class="sxs-lookup"><span data-stu-id="012a7-161">![Graphical Runbook Output Type](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span></span><br> <span data-ttu-id="012a7-162">Detta påminner om toohello ”vidare typ” funktionerna för en cmdlet utdata i PowerShell ISE utan toorun den.</span><span class="sxs-lookup"><span data-stu-id="012a7-162">This is similar toohello "type ahead" functionality of a cmdlet's output in PowerShell ISE without having toorun it.</span></span><br><br> <span data-ttu-id="012a7-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span><span class="sxs-lookup"><span data-stu-id="012a7-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span></span><br>
4. <span data-ttu-id="012a7-164">Cmdletar i hello modulen bör inte ta komplexa objekt av typen för parametrar.</span><span class="sxs-lookup"><span data-stu-id="012a7-164">Cmdlets in hello module should not take complex object types for parameters.</span></span> <span data-ttu-id="012a7-165">Till skillnad från PowerShell lagrar PowerShell Workflow komplexa typer i avserialiserat format.</span><span class="sxs-lookup"><span data-stu-id="012a7-165">PowerShell Workflow is different from PowerShell in that it stores complex types in deserialized form.</span></span> <span data-ttu-id="012a7-166">Primitiva typer finns kvar som primitiver, men komplexa typer som är konverterade tootheir avserialiseras versioner som är i stort sett egenskapsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="012a7-166">Primitive types will stay as primitives, but complex types are converted tootheir deserialized versions, which are essentially property bags.</span></span> <span data-ttu-id="012a7-167">Om du använde hello exempelvis **Get-Process** cmdlet i en runbook (eller ett PowerShell-arbetsflöde att) skulle den returnerar ett objekt av typen [Deserialized.System.Diagnostic.Process], inte hello förväntade [ Typ av System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="012a7-167">For example, if you used hello **Get-Process** cmdlet in a runbook (or a PowerShell Workflow for that matter), it would return an object of type [Deserialized.System.Diagnostic.Process], not hello expected [System.Diagnostic.Process] type.</span></span> <span data-ttu-id="012a7-168">Den här typen har alla hello samma egenskaper som hello inte deserialisera typen, men ingen av hello-metoderna.</span><span class="sxs-lookup"><span data-stu-id="012a7-168">This type has all hello same properties as hello non-deserialized type, but none of hello methods.</span></span> <span data-ttu-id="012a7-169">Om du försöker toopass värdet som en parameter tooa cmdlet, där hello cmdlet förväntar sig ett [System.Diagnostic.Process]-värde för den här parametern får du hello följande fel: *kan inte bearbeta omvandling av argumentet i parametern ”process” . Fel: ”Det går inte att konvertera hello” System.Diagnostics.Process (CcmExec) ”värde av typen” Deserialized.System.Diagnostics.Process ”tootype” System.Diagnostics.Process ”.*</span><span class="sxs-lookup"><span data-stu-id="012a7-169">And if you try toopass this value as a parameter tooa cmdlet, where hello cmdlet expects a [System.Diagnostic.Process] value for this parameter, you’ll receive hello following error: *Cannot process argument transformation on parameter 'process'. Error: "Cannot convert hello "System.Diagnostics.Process (CcmExec)" value of type  "Deserialized.System.Diagnostics.Process" tootype "System.Diagnostics.Process".*</span></span>   <span data-ttu-id="012a7-170">Det beror på att det finns ett typmatchningsfel mellan hello förväntat [System.Diagnostic.Process] typ och hello [Deserialized.System.Diagnostic.Process] typ som angetts.</span><span class="sxs-lookup"><span data-stu-id="012a7-170">This is because there is a type mismatch between hello expected [System.Diagnostic.Process] type and hello given [Deserialized.System.Diagnostic.Process] type.</span></span> <span data-ttu-id="012a7-171">hello sätt runt det här problemet är tooensure hello cmdlets modulens inte börjar komplexa typer för parametrar.</span><span class="sxs-lookup"><span data-stu-id="012a7-171">hello way around this issue is tooensure hello cmdlets of your module do not take complex types for parameters.</span></span> <span data-ttu-id="012a7-172">Här är hello fel sätt toodo den.</span><span class="sxs-lookup"><span data-stu-id="012a7-172">Here is hello wrong way toodo it.</span></span>
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    <span data-ttu-id="012a7-173">Här är hello rätt sätt, i en primitiv som kan användas internt av hello cmdlet toograb hello komplexa objekt och använda den.</span><span class="sxs-lookup"><span data-stu-id="012a7-173">And here is hello right way, taking in a primitive that can be used internally by hello cmdlet toograb hello complex object and use it.</span></span> <span data-ttu-id="012a7-174">Eftersom cmdlets kör hello gäller PowerShell, blir inte PowerShell-arbetsflöde, inuti hello cmdlet $process hello rätt [System.Diagnostic.Process]-typen.</span><span class="sxs-lookup"><span data-stu-id="012a7-174">Since cmdlets execute in hello context of PowerShell, not PowerShell Workflow, inside hello cmdlet $process becomes hello correct [System.Diagnostic.Process] type.</span></span>  
   
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
   <span data-ttu-id="012a7-175">Anslutningstillgångar i runbooks är hashtabeller som är en komplex typ, och ännu dessa hashtabeller verka toobe kan toobe skickades till cmdlets för sina – anslutningsparametern perfekt med inget cast-undantag.</span><span class="sxs-lookup"><span data-stu-id="012a7-175">Connection assets in runbooks are hashtables, which are a complex type, and yet these hashtables seem toobe able toobe passed into cmdlets for their –Connection parameter perfectly, with no cast exception.</span></span> <span data-ttu-id="012a7-176">Tekniskt sett vissa typer av PowerShell är kan toocast korrekt från sin serialiserade formuläret tootheir avserialiseras form och därför kan skickas till cmdlets för parametrar som accepterar hello inte deserialisera typen.</span><span class="sxs-lookup"><span data-stu-id="012a7-176">Technically, some PowerShell types are able toocast properly from their serialized form tootheir deserialized form, and hence can be passed into cmdlets for parameters accepting hello non-deserialized type.</span></span> <span data-ttu-id="012a7-177">Hashtable är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="012a7-177">Hashtable is one of these.</span></span> <span data-ttu-id="012a7-178">Det är möjligt för en modul Skapad definierade typer toobe införts på ett sätt som de kan korrekt deserialisera samt, men det finns vissa avvägningarna tooconsider.</span><span class="sxs-lookup"><span data-stu-id="012a7-178">It’s possible for a module author’s defined types toobe implemented in a way that they can correctly deserialize as well, but there are some trade-offs tooconsider.</span></span> <span data-ttu-id="012a7-179">hello typ måste toohave en standardkonstruktor, har alla dess egenskaper för offentliga och har en PSTypeConverter.</span><span class="sxs-lookup"><span data-stu-id="012a7-179">hello type needs toohave a default constructor, have all of its properties public, and have a PSTypeConverter.</span></span> <span data-ttu-id="012a7-180">Men för redan användardefinierade typer som hello modul Skapad inte äger det finns en alldeles för ”åtgärdas” därför hello rekommendation tooavoid komplexa typer för parametrar i sin helhet.</span><span class="sxs-lookup"><span data-stu-id="012a7-180">However, for already-defined types that hello module author does not own, there is no way too“fix” them, hence hello recommendation tooavoid complex types for parameters all together.</span></span> <span data-ttu-id="012a7-181">Runbook Authoring-tips: om av vissa skäl din cmdlets måste tootake en komplex typparametern eller du använder en annan modul som kräver en komplex typparametern hello lösning i PowerShell-arbetsflöde runbooks och PowerShell-arbetsflöden i lokala PowerShell, är toowrap hello cmdlet som genererar hello komplex typ och hello-cmdletar som förbrukar hello komplex typ i hello samma InlineScript-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="012a7-181">Runbook Authoring tip: If for some reason your cmdlets need tootake a complex type parameter, or you are using someone else’s module that requires a complex type parameter, hello workaround in PowerShell Workflow runbooks and PowerShell Workflows in local PowerShell, is toowrap hello cmdlet that generates hello complex type and hello cmdlet that consumes hello complex type in hello same InlineScript activity.</span></span> <span data-ttu-id="012a7-182">Eftersom InlineScript utförs innehållet som PowerShell i stället för att PowerShell-arbetsflöde, hello cmdlet genererar hello komplex typ resulterar i att rätt typ, avserialiseras inte hello komplexa typen.</span><span class="sxs-lookup"><span data-stu-id="012a7-182">Since InlineScript executes its contents as PowerShell rather than PowerShell Workflow, hello cmdlet generating hello complex type would produce that correct type, not hello deserialized complex type.</span></span>
5. <span data-ttu-id="012a7-183">Gör alla cmdletar i hello modulen tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="012a7-183">Make all cmdlets in hello module stateless.</span></span> <span data-ttu-id="012a7-184">PowerShell-arbetsflödet kör varje cmdlet anropas i hello arbetsflöde i en annan session.</span><span class="sxs-lookup"><span data-stu-id="012a7-184">PowerShell Workflow runs every cmdlet called in hello workflow in a different session.</span></span> <span data-ttu-id="012a7-185">Det innebär att alla cmdlets som är beroende av sessionstillstånd skapas / ändras av andra cmdletar i samma modul inte fungerar i PowerShell-arbetsflöde runbooks hello.</span><span class="sxs-lookup"><span data-stu-id="012a7-185">This means any cmdlets that depend on session state created / modified by other cmdlets in hello same module will not work in PowerShell Workflow runbooks.</span></span>  <span data-ttu-id="012a7-186">Här är ett exempel på vad du inte toodo.</span><span class="sxs-lookup"><span data-stu-id="012a7-186">Here is an example of what not toodo.</span></span>
   
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
6. <span data-ttu-id="012a7-187">hello modulen ska fullständigt finns i ett Xcopy kan paket.</span><span class="sxs-lookup"><span data-stu-id="012a7-187">hello module should be fully contained in an Xcopy-able package.</span></span> <span data-ttu-id="012a7-188">Eftersom Azure Automation-moduler är distribuerade toohello Automation sandboxar när runbooks måste tooexecute, måste de toowork oberoende av hello värden som de körs på.</span><span class="sxs-lookup"><span data-stu-id="012a7-188">Because Azure Automation modules are distributed toohello Automation sandboxes when runbooks need tooexecute, they need toowork independently of hello host they are running on.</span></span> <span data-ttu-id="012a7-189">Det innebär att du ska vara kan tooZip in hello modulen paketet, flytta den tooany andra värden med hello samma eller en senare version av PowerShell och det fungerar som vanligt när de importeras till den värd PowerShell-miljö.</span><span class="sxs-lookup"><span data-stu-id="012a7-189">What this means is that you should be able tooZip up hello module package, move it tooany other host with hello same or newer PowerShell version, and have it function as normal when imported into that host’s PowerShell environment.</span></span> <span data-ttu-id="012a7-190">För att toohappen hello modulen bör inte beroende av några filer utanför hello modulen mapp (hello mapp som hämtar zippade in när du importerar till Azure Automation) eller unika registerinställningar på en värd till exempel de som anges av hello installerar av en produkt.</span><span class="sxs-lookup"><span data-stu-id="012a7-190">In order for that toohappen, hello module should not depend on any files outside hello module folder (hello folder that gets zipped up when importing into Azure Automation), or on any unique registry settings on a host, such as those set by hello install of a product.</span></span> <span data-ttu-id="012a7-191">Om denna bästa praxis inte följs hello modulen inte kan användas i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="012a7-191">If this best practice is not followed, hello module will not be usable in Azure Automation.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="012a7-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="012a7-192">Next steps</span></span>

* <span data-ttu-id="012a7-193">tooget igång med PowerShell arbetsflöde runbooks finns [min första PowerShell-arbetsflödesrunbook](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="012a7-193">tooget started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="012a7-194">toolearn mer om hur du skapar PowerShell-moduler finns [skriva en Windows PowerShell-modul](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="012a7-194">toolearn more about creating PowerShell Modules, see [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span></span>

