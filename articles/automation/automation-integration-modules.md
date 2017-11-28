---
title: <span data-ttu-id="ae44b-101">Skapa en Azure Automation-integreringsmodul | Microsoft Docs</span><span class="sxs-lookup"><span data-stu-id="ae44b-101">Create an Azure Automation Integration Module | Microsoft Docs</span></span>
description: "<span data-ttu-id=\"ae44b-102\">Självstudie som steg för steg beskriver hur du skapar, testar och använder integreringsmoduler i Azure Automation.</span><span class=\"sxs-lookup\"><span data-stu-id=\"ae44b-102\">Tutorial that walks you through the creation, testing, and example use of  integration modules in Azure Automation.</span></span>"
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
ms.openlocfilehash: aeb06276a52e5472667ae0a741fb3007a91910fe
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-integration-modules"></a><span data-ttu-id="ae44b-103">Azure Automation-integreringsmoduler</span><span class="sxs-lookup"><span data-stu-id="ae44b-103">Azure Automation Integration Modules</span></span>
<span data-ttu-id="ae44b-104">PowerShell är den grundläggande tekniken bakom Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-104">PowerShell is the fundamental technology behind Azure Automation.</span></span> <span data-ttu-id="ae44b-105">Eftersom Azure Automation bygger på PowerShell är PowerShell-moduler fundamentala för att kunna expandera Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-105">Since Azure Automation is built on PowerShell, PowerShell modules are key to the extensibility of Azure Automation.</span></span> <span data-ttu-id="ae44b-106">I den här artikeln beskriver vi steg för steg hur Azure Automation använder PowerShell-moduler, även kallade ”integreringsmoduler”. Artikeln innehåller också metodtips som hjälper dig att skapa egna PowerShell-moduler som fungerar som integreringsmoduler i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-106">In this article, we will guide you through the specifics of Azure Automation’s use of PowerShell modules, referred to as “Integration Modules”, and best practices for creating your own PowerShell modules to make sure they work as Integration Modules within Azure Automation.</span></span> 

## <a name="what-is-a-powershell-module"></a><span data-ttu-id="ae44b-107">Vad är en PowerShell-modul?</span><span class="sxs-lookup"><span data-stu-id="ae44b-107">What is a PowerShell Module?</span></span>
<span data-ttu-id="ae44b-108">En PowerShell-modul är en grupp med PowerShell-cmdlets som **Get-Date** eller **Copy-Item**, som kan användas från PowerShell-konsolen, skript, arbetsflöden, runbooks och PowerShell DSC-resurser som WindowsFeature eller File, som kan användas från PowerShell DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="ae44b-108">A PowerShell module is a group of PowerShell cmdlets like **Get-Date** or **Copy-Item**, that can be used from the PowerShell console, scripts, workflows, runbooks, and PowerShell DSC resources like WindowsFeature or File, that can be used from PowerShell DSC configurations.</span></span> <span data-ttu-id="ae44b-109">Alla funktioner i PowerShell är tillgängliga via cmdlets och DSC-resurser, och varje cmdlet/DSC-resurs backas upp av en PowerShell-modul. Många av dessa medföljer själva PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae44b-109">All of the functionality of PowerShell is exposed through cmdlets and DSC resources, and every cmdlet/DSC resource is backed by a PowerShell module, many of which ship with PowerShell itself.</span></span> <span data-ttu-id="ae44b-110">Exempelvis är cmdleten **Get-Date** en del av PowerShell-modulen Microsoft.PowerShell.Utility, cmdleten **Copy-Item** en del av PowerShell-modulen Microsoft.PowerShell.Management och Package DSC-resursen en del av PowerShell-modulen PSDesiredStateConfiguration.</span><span class="sxs-lookup"><span data-stu-id="ae44b-110">For example, the **Get-Date** cmdlet is part of the Microsoft.PowerShell.Utility PowerShell module, and **Copy-Item** cmdlet is part of the Microsoft.PowerShell.Management PowerShell module and the Package DSC resource is part of the PSDesiredStateConfiguration PowerShell module.</span></span> <span data-ttu-id="ae44b-111">Båda dessa moduler medföljer PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ae44b-111">Both of these modules ship with PowerShell.</span></span> <span data-ttu-id="ae44b-112">Men många PowerShell-moduler levereras inte som en del av PowerShell och distribueras i stället med tillverkarens produkter eller med tredjepartsprodukter som System Center 2012 Configuration Manager eller av den stora PowerShell-communityn, t.ex. via PowerShell-galleriet.</span><span class="sxs-lookup"><span data-stu-id="ae44b-112">But many PowerShell modules do not ship as part of PowerShell, and are instead distributed with first or third-party products like System Center 2012 Configuration Manager or by the vast PowerShell community on places like PowerShell Gallery.</span></span>  <span data-ttu-id="ae44b-113">Modulerna är användbara eftersom de gör avancerade uppgifter enklare genom kapslade funktioner.</span><span class="sxs-lookup"><span data-stu-id="ae44b-113">The modules are useful because they make complex tasks simpler through encapsulated functionality.</span></span>  <span data-ttu-id="ae44b-114">Du kan lära dig mer om [PowerShell-moduler på MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="ae44b-114">You can learn more about [PowerShell modules on MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx).</span></span> 

## <a name="what-is-an-azure-automation-integration-module"></a><span data-ttu-id="ae44b-115">Vad är en Azure Automation-integreringsmodul?</span><span class="sxs-lookup"><span data-stu-id="ae44b-115">What is an Azure Automation Integration Module?</span></span>
<span data-ttu-id="ae44b-116">En integreringsmodul skiljer sig inte så mycket från en PowerShell-modul.</span><span class="sxs-lookup"><span data-stu-id="ae44b-116">An Integration Module isn't very different from a PowerShell module.</span></span> <span data-ttu-id="ae44b-117">Det är bara en PowerShell-modul som kan innehålla ytterligare en fil – en metadatafil som anger en Azure Automation-anslutningstyp som ska användas med modulens cmdletar i runbooks.</span><span class="sxs-lookup"><span data-stu-id="ae44b-117">Its simply a PowerShell module that optionally contains one additional file - a metadata file specifying an Azure Automation connection type to be used with the module's cmdlets in runbooks.</span></span> <span data-ttu-id="ae44b-118">Oavsett om det gäller valfria filer eller inte kan dessa PowerShell-moduler importeras till Azure Automation så att deras cmdlets kan användas i runbooks och deras DSC-resurser i DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="ae44b-118">Optional file or not, these PowerShell modules can be imported into Azure Automation to make their cmdlets available for use within runbooks and their DSC resources available for use within DSC configurations.</span></span> <span data-ttu-id="ae44b-119">Azure Automation lagrar dessa moduler i bakgrunden och när runbook-jobben och DSC-kompileringsjobben körs läser tjänsten in dem i begränsade Azure Automation-lägen där runbooks körs och DSC-konfigurationer kompileras.</span><span class="sxs-lookup"><span data-stu-id="ae44b-119">Behind the scenes, Azure Automation stores these modules, and at runbook job and DSC compilation job execution time, loads them into the Azure Automation sandboxes where runbooks are executed and DSC configurations are compiled.</span></span>  <span data-ttu-id="ae44b-120">DSC-resurser i moduler placeras också automatiskt på Automation DSC-hämtningsservern så att de kan hämtas av datorer som försöker använda DSC-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="ae44b-120">Any DSC resources in modules are also automatically placed on the Automation DSC pull server, so that they can be pulled by machines attempting to apply DSC configurations.</span></span>  

<span data-ttu-id="ae44b-121">Vi levererar ett antal färdiga Azure PowerShell-moduler med Azure Automation som du kan använda för att snabbt komma igång med automatiseringen av Azure-hanteringen, men du kan importera PowerShell-moduler för det system, den tjänst eller det verktyg som du vill integrera med.</span><span class="sxs-lookup"><span data-stu-id="ae44b-121">We ship a number of Azure  PowerShell modules out of the box in Azure Automation for you to use so you can get started automating Azure management right away, but you can import PowerShell modules for whatever system, service, or tool you want to integrate with.</span></span> 

> [!NOTE]
> <span data-ttu-id="ae44b-122">Vissa moduler levereras som ”globala moduler” i Automation-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ae44b-122">Certain modules are shipped as “global modules” in the Automation service.</span></span> <span data-ttu-id="ae44b-123">De här globala modulerna är tillgängliga när du skapar ett Automation-konto och vi uppdaterar dem ibland, varpå de skickas ut automatiskt till ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="ae44b-123">These global modules are available to you when you create an automation account, and we update them sometimes which automatically pushes them out to your automation account.</span></span> <span data-ttu-id="ae44b-124">Om du inte vill att de ska uppdateras automatiskt kan du alltid importera samma modul själv och den modulen äger företräde framför den globala modulversionen av den modul som vi leverererar automatiskt i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ae44b-124">If you don’t want them to be auto-updated, you can always import the same module yourself, and that will take precedence over the global module version of that module that we ship in the service.</span></span> 

<span data-ttu-id="ae44b-125">Formatet som du importerar ett integreringsmodulpaket i är en komprimerad fil med samma namn som modulen och filnamnstillägget .zip.</span><span class="sxs-lookup"><span data-stu-id="ae44b-125">The format in which you import an Integration Module package is a compressed file with the same name as the module and a .zip extension.</span></span> <span data-ttu-id="ae44b-126">Det innehåller Windows PowerShell-modulen och eventuella stödfiler, inklusive en manifestfil (.psd1) om modulen har en.</span><span class="sxs-lookup"><span data-stu-id="ae44b-126">It contains the Windows PowerShell module and any supporting files, including a manifest file (.psd1) if the module has one.</span></span>

<span data-ttu-id="ae44b-127">Om modulen ska innehålla en Azure Automation-anslutningstyp måste den också innehålla en fil med namnet `<ModuleName>-Automation.json` som anger egenskaperna för anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="ae44b-127">If the module should contain an Azure Automation connection type, it must also contain a file with the name `<ModuleName>-Automation.json` that specifies the connection type properties.</span></span> <span data-ttu-id="ae44b-128">Det här är en JSON-fil som placeras i modulmappen för den komprimerade ZIP-filen och som innehåller fälten för en ”anslutning” som krävs för att ansluta till systemet eller tjänsten som modulen representerar.</span><span class="sxs-lookup"><span data-stu-id="ae44b-128">This is a json file placed within the module folder of your compressed .zip file, and contains the fields of a “connection” that is required to connect to the system or service the module represents.</span></span> <span data-ttu-id="ae44b-129">Detta resulterar i att en anslutningstyp skapas i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-129">This will end up creating a connection type in Azure Automation.</span></span> <span data-ttu-id="ae44b-130">Genom att använda den här filen kan du ange fältnamn, typer och huruvida fälten ska vara krypterade och/eller valfria för modulens anslutningstyp.</span><span class="sxs-lookup"><span data-stu-id="ae44b-130">Using this file you can set the field names, types, and whether the fields should be encrypted and / or optional, for the connection type of the module.</span></span> <span data-ttu-id="ae44b-131">Följande är en mall i JSON-format:</span><span class="sxs-lookup"><span data-stu-id="ae44b-131">The following is a template in the json file format:</span></span>

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

<span data-ttu-id="ae44b-132">Om du har distribuerat Service Management Automation och skapat integreringsmodulpaket för dina Automation-runbooks bör du känna igen detta.</span><span class="sxs-lookup"><span data-stu-id="ae44b-132">If you have deployed Service Management Automation and created Integration Modules packages for your automation runbooks, this should look very familiar to you.</span></span> 

## <a name="authoring-best-practices"></a><span data-ttu-id="ae44b-133">Metodtips för redigering</span><span class="sxs-lookup"><span data-stu-id="ae44b-133">Authoring Best Practices</span></span>
<span data-ttu-id="ae44b-134">Även om integreringsmodulerna i grunden är PowerShell-moduler finns det ett antal åtgärder som vi rekommenderar att du överväger när du redigerar en PowerShell-modul så att den blir så användbar som möjligt i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-134">Even though Integration Modules are essentially PowerShell modules, there’s still a number of things we recommend you consider while authoring a PowerShell module, to make it most usable in Azure Automation.</span></span> <span data-ttu-id="ae44b-135">Vissa av dessa är specifika för Azure Automation och några av dem är användbara för att dina moduler ska fungerar bra i PowerShell Workflow, oavsett om du använder Automation eller inte.</span><span class="sxs-lookup"><span data-stu-id="ae44b-135">Some of these are Azure Automation specific, and some of them are useful just to make your modules work well in PowerShell Workflow, regardless of whether or not you’re using Automation.</span></span> 

1. <span data-ttu-id="ae44b-136">Lägg till en sammanfattning, beskrivning och hjälp-URI för varje cmdlet i modulen.</span><span class="sxs-lookup"><span data-stu-id="ae44b-136">Include a synopsis, description, and help URI for every cmdlet in the module.</span></span> <span data-ttu-id="ae44b-137">I PowerShell kan du definiera viss hjälpinformation för cmdlets så att användaren kan få hjälp med att använda dem genom att köra cmdleten **Get-Help**.</span><span class="sxs-lookup"><span data-stu-id="ae44b-137">In PowerShell, you can define certain help information for cmdlets to allow the user to receive help on using them with the **Get-Help** cmdlet.</span></span> <span data-ttu-id="ae44b-138">Så här kan du till exempel definiera en sammanfattning och hjälp-URI för en PowerShell-modul som skrivits i en .psm1-fil.</span><span class="sxs-lookup"><span data-stu-id="ae44b-138">For example, here’s how you can define a synopsis and help URI for a PowerShell module written in a .psm1 file.</span></span><br>  
   
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
   <br> <span data-ttu-id="ae44b-139">Om du tillhandahåller den här informationen visas den här hjälpen med cmdleten **Get-Help** i PowerShell-konsolen, men hjälpfunktionen visas också i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-139">Providing this info will not only show this help using the **Get-Help** cmdlet in the PowerShell console, it will also expose this help functionality within Azure Automation.</span></span>  <span data-ttu-id="ae44b-140">Till exempel när aktiviteter infogas i samband med runbook-redigering.</span><span class="sxs-lookup"><span data-stu-id="ae44b-140">For example, when inserting activities during runbook authoring.</span></span> <span data-ttu-id="ae44b-141">Om du klickar på ”Visa detaljerad hjälp” öppnas hjälp-URI:n på en annan flik i webbläsaren som du använder för att få åtkomst till Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-141">Clicking “View detailed help” will open the help URI in another tab of the web browser you’re using to access Azure Automation.</span></span><br><span data-ttu-id="ae44b-142">![Hjälp med integreringsmoduler](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span><span class="sxs-lookup"><span data-stu-id="ae44b-142">![Integration Module Help](media/automation-integration-modules/automation-integration-module-activitydesc.png)</span></span>
2. <span data-ttu-id="ae44b-143">Om modulen körs mot ett fjärrsystem:</span><span class="sxs-lookup"><span data-stu-id="ae44b-143">If the module runs against a remote system,</span></span>

    <span data-ttu-id="ae44b-144">a.</span><span class="sxs-lookup"><span data-stu-id="ae44b-144">a.</span></span> <span data-ttu-id="ae44b-145">Den bör innehålla en metadatafil för integreringsmodulen som definierar informationen som behövs för att ansluta till fjärrsystemet, eller anslutningstypen.</span><span class="sxs-lookup"><span data-stu-id="ae44b-145">It should contain an Integration Module metadata file that defines the information needed to connect to that remote system, meaning the connection type.</span></span>  
    <span data-ttu-id="ae44b-146">b.</span><span class="sxs-lookup"><span data-stu-id="ae44b-146">b.</span></span> <span data-ttu-id="ae44b-147">Varje cmdlet i modulen ska kunna använda ett anslutningsobjekt (en instans av anslutningstypen) som en parameter.</span><span class="sxs-lookup"><span data-stu-id="ae44b-147">Each cmdlet in the module should be able to take in a connection object (an instance of that connection type) as a parameter.</span></span>  

    <span data-ttu-id="ae44b-148">Cmdlets i modulen blir enklare att använda i Azure Automation om ett objekt kan skickas med fälten för anslutningstypen som en parameter till cmdleten.</span><span class="sxs-lookup"><span data-stu-id="ae44b-148">Cmdlets in the module become easier to use in Azure Automation if you allow passing an object with the fields of the connection type as a parameter to the cmdlet.</span></span> <span data-ttu-id="ae44b-149">På så sätt behöver inte användarna mappa parametrar för anslutningstillgången till cmdletens motsvarande parametrar varje gång de anropar en cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ae44b-149">This way users don’t have to map parameters of the connection asset to the cmdlet's corresponding parameters each time they call a cmdlet.</span></span> <span data-ttu-id="ae44b-150">Baserat på runbook-exemplet ovan används en Twilio-anslutningstillgång kallad CorpTwilio för att komma åt Twilio och returnera alla telefonnummer i kontot.</span><span class="sxs-lookup"><span data-stu-id="ae44b-150">Based on the runbook example above, it uses a Twilio connection asset called CorpTwilio to access Twilio and return all the phone numbers in the account.</span></span>  <span data-ttu-id="ae44b-151">Observera hur den mappar fälten för anslutningen till parametrarna för cmdleten?</span><span class="sxs-lookup"><span data-stu-id="ae44b-151">Notice how it is mapping the fields of the connection to the parameters of the cmdlet?</span></span><br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    <span data-ttu-id="ae44b-152">Ett enklare och bättre sätt är att skicka anslutningsobjektet direkt till cmdleten:</span><span class="sxs-lookup"><span data-stu-id="ae44b-152">An easier and better way to approach this is directly passing the connection object to the cmdlet -</span></span>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   
    <span data-ttu-id="ae44b-153">Du kan aktivera beteenden som detta för dina cmdlets genom att tillåta att de accepterar ett anslutningsobjekt direkt som en parameter i stället för bara anslutningsfält för parametrar.</span><span class="sxs-lookup"><span data-stu-id="ae44b-153">You can enable behavior like this for your cmdlets by allowing them to accept a connection object directly as a parameter, instead of just connection fields for parameters.</span></span> <span data-ttu-id="ae44b-154">Normalt vill du ha en parameteruppsättning för vart och ett så att en användare som inte använder Azure Automation kan anropa dina cmdlets utan att skapa en hash-tabell som fungerar som anslutningsobjektet.</span><span class="sxs-lookup"><span data-stu-id="ae44b-154">Usually you’ll want a parameter set for each, so that a user not using Azure Automation can call your cmdlets without constructing a hashtable to act as the connection object.</span></span> <span data-ttu-id="ae44b-155">Parameteruppsättningen **SpecifyConnectionFields** nedan används för att skicka egenskaperna för anslutningsfältet en i taget.</span><span class="sxs-lookup"><span data-stu-id="ae44b-155">Parameter set **SpecifyConnectionFields** below is used to pass the connection field properties one by one.</span></span> <span data-ttu-id="ae44b-156">Med **UseConnectionObject** kan du skicka anslutningen rakt igenom.</span><span class="sxs-lookup"><span data-stu-id="ae44b-156">**UseConnectionObject** lets you pass the connection straight through.</span></span> <span data-ttu-id="ae44b-157">Som du ser kan du skicka den på endera sätt med cmdleten Send-TwilioSMS i [Twilio PowerShell-modulen](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8):</span><span class="sxs-lookup"><span data-stu-id="ae44b-157">As you can see, the Send-TwilioSMS cmdlet in the [Twilio PowerShell module](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) allows passing either way:</span></span> 
   
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
3. <span data-ttu-id="ae44b-158">Definiera utdatatypen för alla cmdlets i modulen.</span><span class="sxs-lookup"><span data-stu-id="ae44b-158">Define output type for all cmdlets in the module.</span></span> <span data-ttu-id="ae44b-159">Genom att definiera en utdatatyp för en cmdlet kan IntelliSense hjälpa dig under designfasen med att fastställa utdataegenskaperna för cmdleten, för användning under redigeringar.</span><span class="sxs-lookup"><span data-stu-id="ae44b-159">Defining an output type for a cmdlet allows design-time IntelliSense to help you determine the output properties of the cmdlet, for use during authoring.</span></span> <span data-ttu-id="ae44b-160">Det är särskilt användbart under grafisk redigering av Automation-runbooks där goda kunskaper om designfasen är en förutsättning för en enkel användarupplevelse med modulen.</span><span class="sxs-lookup"><span data-stu-id="ae44b-160">It is especially helpful during Automation runbook graphical authoring, where design time knowledge is key to an easy user experience with your module.</span></span><br><br> <span data-ttu-id="ae44b-161">![Utdatatyp för grafiska runbooks](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span><span class="sxs-lookup"><span data-stu-id="ae44b-161">![Graphical Runbook Output Type](media/automation-integration-modules/runbook-graphical-module-output-type.png)</span></span><br> <span data-ttu-id="ae44b-162">Detta påminner om funktionen ”type-ahead” (ifyllningsförslag) för en cmdlets utdata i PowerShell ISE utan att den behöver köras.</span><span class="sxs-lookup"><span data-stu-id="ae44b-162">This is similar to the "type ahead" functionality of a cmdlet's output in PowerShell ISE without having to run it.</span></span><br><br> <span data-ttu-id="ae44b-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span><span class="sxs-lookup"><span data-stu-id="ae44b-163">![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)</span></span><br>
4. <span data-ttu-id="ae44b-164">Cmdlets i modulen ska inte acceptera komplexa objekttyper för parametrar.</span><span class="sxs-lookup"><span data-stu-id="ae44b-164">Cmdlets in the module should not take complex object types for parameters.</span></span> <span data-ttu-id="ae44b-165">Till skillnad från PowerShell lagrar PowerShell Workflow komplexa typer i avserialiserat format.</span><span class="sxs-lookup"><span data-stu-id="ae44b-165">PowerShell Workflow is different from PowerShell in that it stores complex types in deserialized form.</span></span> <span data-ttu-id="ae44b-166">Primitiva typer förblir primitiver, men komplexa typer konverteras till deras avserialiserade versioner, som i grunden är egenskapsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="ae44b-166">Primitive types will stay as primitives, but complex types are converted to their deserialized versions, which are essentially property bags.</span></span> <span data-ttu-id="ae44b-167">Om du till exempel använde cmdleten **Get-process** i en runbook (eller ett PowerShell-arbetsflöde för den delen) returneras ett objekt av typen [Deserialized.System.Diagnostic.Process], inte den förväntade typen [System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="ae44b-167">For example, if you used the **Get-Process** cmdlet in a runbook (or a PowerShell Workflow for that matter), it would return an object of type [Deserialized.System.Diagnostic.Process], not the expected [System.Diagnostic.Process] type.</span></span> <span data-ttu-id="ae44b-168">Den här typen har samma egenskaper som den icke-avserialiserade typen, men inga av metoderna.</span><span class="sxs-lookup"><span data-stu-id="ae44b-168">This type has all the same properties as the non-deserialized type, but none of the methods.</span></span> <span data-ttu-id="ae44b-169">Om du försöker överföra det här värdet som en parameter till en cmdlet, där cmdleten förväntar sig ett [System.Diagnostic.Process]-värde för den här parametern får du följande fel: *Det går inte att bearbeta argumenttransformation på parametern 'process'. Fel: Det går inte att konvertera värdet "System.Diagnostics.Process (CcmExec)" av typen "Deserialized.System.Diagnostics.Process" till typen "System.Diagnostics.Process".*</span><span class="sxs-lookup"><span data-stu-id="ae44b-169">And if you try to pass this value as a parameter to a cmdlet, where the cmdlet expects a [System.Diagnostic.Process] value for this parameter, you’ll receive the following error: *Cannot process argument transformation on parameter 'process'. Error: "Cannot convert the "System.Diagnostics.Process (CcmExec)" value of type  "Deserialized.System.Diagnostics.Process" to type "System.Diagnostics.Process".*</span></span>   <span data-ttu-id="ae44b-170">Detta beror på ett typmatchningsfel mellan den förväntade typen [System.Diagnostic.Process] och den angivna typen [Deserialized.System.Diagnostic.Process].</span><span class="sxs-lookup"><span data-stu-id="ae44b-170">This is because there is a type mismatch between the expected [System.Diagnostic.Process] type and the given [Deserialized.System.Diagnostic.Process] type.</span></span> <span data-ttu-id="ae44b-171">Du kan kringgå det här problemet genom att se till att modulens cmdlets inte accepterar komplexa typer för parametrar.</span><span class="sxs-lookup"><span data-stu-id="ae44b-171">The way around this issue is to ensure the cmdlets of your module do not take complex types for parameters.</span></span> <span data-ttu-id="ae44b-172">Det här är fel sätt att göra det på.</span><span class="sxs-lookup"><span data-stu-id="ae44b-172">Here is the wrong way to do it.</span></span>
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
    <br>
    <span data-ttu-id="ae44b-173">Och det här är rätt sätt, där en primitiv hämtas som kan användas internt av cmdleten för att hämta det komplexa objektet och använda det.</span><span class="sxs-lookup"><span data-stu-id="ae44b-173">And here is the right way, taking in a primitive that can be used internally by the cmdlet to grab the complex object and use it.</span></span> <span data-ttu-id="ae44b-174">Eftersom cmdlets körs i PowerShell-kontexten, inte i PowerShell Workflow, blir $process rätt [System.Diagnostic.Process]-typ i cmdleten.</span><span class="sxs-lookup"><span data-stu-id="ae44b-174">Since cmdlets execute in the context of PowerShell, not PowerShell Workflow, inside the cmdlet $process becomes the correct [System.Diagnostic.Process] type.</span></span>  
   
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
   <span data-ttu-id="ae44b-175">Anslutningstillgångar i runbooks är hash-tabeller, som är en komplex typ, och ändå verkar dessa hash-tabeller kunna användas utan problem i cmdlets för deras -Connection-parameter, utan typkonverteringsundantag.</span><span class="sxs-lookup"><span data-stu-id="ae44b-175">Connection assets in runbooks are hashtables, which are a complex type, and yet these hashtables seem to be able to be passed into cmdlets for their –Connection parameter perfectly, with no cast exception.</span></span> <span data-ttu-id="ae44b-176">Rent tekniskt kan vissa PowerShell-typer konverteras korrekt från deras serialiserade form till deras avserialiserade form och kan därför skickas till cmdlets för parametrar som accepterar den icke-avserialiserade typen.</span><span class="sxs-lookup"><span data-stu-id="ae44b-176">Technically, some PowerShell types are able to cast properly from their serialized form to their deserialized form, and hence can be passed into cmdlets for parameters accepting the non-deserialized type.</span></span> <span data-ttu-id="ae44b-177">Hashtable är ett exempel.</span><span class="sxs-lookup"><span data-stu-id="ae44b-177">Hashtable is one of these.</span></span> <span data-ttu-id="ae44b-178">Modulskaparens definierade typer kan implementeras på ett sätt så att de även kan deserialiseras korrekt, men det finns några nackdelar som du bör vara medveten om.</span><span class="sxs-lookup"><span data-stu-id="ae44b-178">It’s possible for a module author’s defined types to be implemented in a way that they can correctly deserialize as well, but there are some trade-offs to consider.</span></span> <span data-ttu-id="ae44b-179">Typen måste ha en standardkonstruktor och en PSTypeConverter, och alla dess egenskaper måste vara offentliga.</span><span class="sxs-lookup"><span data-stu-id="ae44b-179">The type needs to have a default constructor, have all of its properties public, and have a PSTypeConverter.</span></span> <span data-ttu-id="ae44b-180">Men för redan definierade typer som modulägaren inte äger finns det inget sätt att ”åtgärda” dem, därav rekommendationen att undvika komplexa typer för parametrar helt och hållet.</span><span class="sxs-lookup"><span data-stu-id="ae44b-180">However, for already-defined types that the module author does not own, there is no way to “fix” them, hence the recommendation to avoid complex types for parameters all together.</span></span> <span data-ttu-id="ae44b-181">Redigeringstips för runbook! Om dina cmdlets av någon anledning behöver använda en komplex parametertyp, eller om du använder någon annans modul som kräver det, kan du kringgå problemet i PowerShell Workflow-runbooks och PowerShell-arbetsflöden i lokala PowerShell genom att innesluta cmdleten som genererar den komplexa typen och cmdleten som använder den komplexa typen i samma InlineScript-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ae44b-181">Runbook Authoring tip: If for some reason your cmdlets need to take a complex type parameter, or you are using someone else’s module that requires a complex type parameter, the workaround in PowerShell Workflow runbooks and PowerShell Workflows in local PowerShell, is to wrap the cmdlet that generates the complex type and the cmdlet that consumes the complex type in the same InlineScript activity.</span></span> <span data-ttu-id="ae44b-182">Eftersom InlineScript kör sitt innehåll som PowerShell i stället för som ett PowerShell-arbetsflöde kommer cmdleten som genererar den komplexa typen att generera rätt typ, inte den avserialiserade komplexa typen.</span><span class="sxs-lookup"><span data-stu-id="ae44b-182">Since InlineScript executes its contents as PowerShell rather than PowerShell Workflow, the cmdlet generating the complex type would produce that correct type, not the deserialized complex type.</span></span>
5. <span data-ttu-id="ae44b-183">Gör alla cmdlets i modulen tillståndslösa.</span><span class="sxs-lookup"><span data-stu-id="ae44b-183">Make all cmdlets in the module stateless.</span></span> <span data-ttu-id="ae44b-184">PowerShell Workflow kör alla cmdlets som anropas i arbetsflödet i en annan session.</span><span class="sxs-lookup"><span data-stu-id="ae44b-184">PowerShell Workflow runs every cmdlet called in the workflow in a different session.</span></span> <span data-ttu-id="ae44b-185">Det innebär att cmdlets som är beroende av sessionstillstånd som skapas/ändras av andra cmdlets i samma modul inte fungerar i PowerShell Workflow-runbooks.</span><span class="sxs-lookup"><span data-stu-id="ae44b-185">This means any cmdlets that depend on session state created / modified by other cmdlets in the same module will not work in PowerShell Workflow runbooks.</span></span>  <span data-ttu-id="ae44b-186">Här är ett exempel på vad du inte ska göra.</span><span class="sxs-lookup"><span data-stu-id="ae44b-186">Here is an example of what not to do.</span></span>
   
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
6. <span data-ttu-id="ae44b-187">Modulen bör finnas i ett Xcopy-aktiverat paket.</span><span class="sxs-lookup"><span data-stu-id="ae44b-187">The module should be fully contained in an Xcopy-able package.</span></span> <span data-ttu-id="ae44b-188">Eftersom Azure Automation-moduler distribueras till begränsat Automation-läge när runbooks behöver köra, måste de fungera oberoende av värden som de körs på.</span><span class="sxs-lookup"><span data-stu-id="ae44b-188">Because Azure Automation modules are distributed to the Automation sandboxes when runbooks need to execute, they need to work independently of the host they are running on.</span></span> <span data-ttu-id="ae44b-189">Det innebär att du måste kunna dekomprimera modulpaketet, flytta det till en annan värd med samma eller en senare version av PowerShell och se till att det fungerar som vanligt när det importeras till den värdens PowerShell-miljö.</span><span class="sxs-lookup"><span data-stu-id="ae44b-189">What this means is that you should be able to Zip up the module package, move it to any other host with the same or newer PowerShell version, and have it function as normal when imported into that host’s PowerShell environment.</span></span> <span data-ttu-id="ae44b-190">För att det ska ske får modulen inte vara beroende av några filer utanför modulmappen (den mapp som dekomprimeras vid importen till Azure Automation) eller av unika registerinställningar på en värd, till exempel de som anges av installationsprogrammet för en produkt.</span><span class="sxs-lookup"><span data-stu-id="ae44b-190">In order for that to happen, the module should not depend on any files outside the module folder (the folder that gets zipped up when importing into Azure Automation), or on any unique registry settings on a host, such as those set by the install of a product.</span></span> <span data-ttu-id="ae44b-191">Om du inte följer denna riktlinje kan modulen inte användas i Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="ae44b-191">If this best practice is not followed, the module will not be usable in Azure Automation.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="ae44b-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae44b-192">Next steps</span></span>

* <span data-ttu-id="ae44b-193">Se hur du kommer igång med runbooks baserade på PowerShell-arbetsflöden i [Min första PowerShell-arbetsflödesbaserade runbook](automation-first-runbook-textual.md)</span><span class="sxs-lookup"><span data-stu-id="ae44b-193">To get started with PowerShell workflow runbooks, see [My first PowerShell workflow runbook](automation-first-runbook-textual.md)</span></span>
* <span data-ttu-id="ae44b-194">Mer information om hur du skapar PowerShell-moduler finns i [Skriva en Windows PowerShell-modul](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="ae44b-194">To learn more about creating PowerShell Modules, see [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)</span></span>

