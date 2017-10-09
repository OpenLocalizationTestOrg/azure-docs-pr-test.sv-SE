---
title: aaaAzure Key Vault-loggning | Microsoft Docs
description: "Använd den här självstudiekursen toohelp dig att komma igång med Azure Key Vault-loggning."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: cabailey
ms.openlocfilehash: 38a173297948748bef45e3d857c06b50b3e21e74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-logging"></a><span data-ttu-id="16bec-103">Azure Key Vault-loggning</span><span class="sxs-lookup"><span data-stu-id="16bec-103">Azure Key Vault Logging</span></span>
<span data-ttu-id="16bec-104">Azure Key Vault är tillgängligt i de flesta regioner.</span><span class="sxs-lookup"><span data-stu-id="16bec-104">Azure Key Vault is available in most regions.</span></span> <span data-ttu-id="16bec-105">Mer information finns i hello [Key Vault-priser](https://azure.microsoft.com/pricing/details/key-vault/).</span><span class="sxs-lookup"><span data-stu-id="16bec-105">For more information, see hello [Key Vault pricing page](https://azure.microsoft.com/pricing/details/key-vault/).</span></span>

## <a name="introduction"></a><span data-ttu-id="16bec-106">Introduktion</span><span class="sxs-lookup"><span data-stu-id="16bec-106">Introduction</span></span>
<span data-ttu-id="16bec-107">När du har skapat en eller flera nyckelvalv, vill du förmodligen toomonitor hur och när din nyckel valv är ofta, och av vem.</span><span class="sxs-lookup"><span data-stu-id="16bec-107">After you have created one or more key vaults, you will likely want toomonitor how and when your key vaults are accessed, and by whom.</span></span> <span data-ttu-id="16bec-108">Du kan göra det genom att aktivera loggning för nyckelvalvet, vilket sparar information i ett Azure-lagringskonto som du anger.</span><span class="sxs-lookup"><span data-stu-id="16bec-108">You can do this by enabling logging for Key Vault, which saves information in an Azure storage account that you provide.</span></span> <span data-ttu-id="16bec-109">En ny behållare med namnet **insights-logs-auditevent** skapas automatiskt för det angivna lagringskontot och du kan använda samma lagringskonto för att samla in loggar för flera nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="16bec-109">A new container named **insights-logs-auditevent** is automatically created for your specified storage account, and you can use this same storage account for collecting logs for multiple key vaults.</span></span>

<span data-ttu-id="16bec-110">Du kan komma åt din loggningsinformation högst, 10 minuter efter hello nyckeln valvet igen.</span><span class="sxs-lookup"><span data-stu-id="16bec-110">You can access your logging information at most, 10 minutes after hello key vault operation.</span></span> <span data-ttu-id="16bec-111">Oftast är informationen dock tillgänglig snabbare än så.</span><span class="sxs-lookup"><span data-stu-id="16bec-111">In most cases, it will be quicker than this.</span></span>  <span data-ttu-id="16bec-112">Är det upp tooyou toomanage loggarna på ditt lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="16bec-112">It's up tooyou toomanage your logs in your storage account:</span></span>

* <span data-ttu-id="16bec-113">Använd standard Azure access control metoder toosecure loggarna genom att begränsa vem som kan komma åt dem.</span><span class="sxs-lookup"><span data-stu-id="16bec-113">Use standard Azure access control methods toosecure your logs by restricting who can access them.</span></span>
* <span data-ttu-id="16bec-114">Ta bort loggar som du inte längre vill tookeep i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="16bec-114">Delete logs that you no longer want tookeep in your storage account.</span></span>

<span data-ttu-id="16bec-115">Använd den här självstudiekursen toohelp dig att komma igång med Azure Key Vault-loggning, toocreate ditt lagringskonto, aktivera loggning och tolka hello logga information som samlas in.</span><span class="sxs-lookup"><span data-stu-id="16bec-115">Use this tutorial toohelp you get started with Azure Key Vault logging, toocreate your storage account, enable logging, and interpret hello logging information collected.</span></span>  

> [!NOTE]
> <span data-ttu-id="16bec-116">Den här självstudiekursen innehåller inte instruktioner för hur toocreate nyckeln valv, nycklar och hemligheter.</span><span class="sxs-lookup"><span data-stu-id="16bec-116">This tutorial does not include instructions for how toocreate key vaults, keys, or secrets.</span></span> <span data-ttu-id="16bec-117">Den här informationen finns i [Komma igång med Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="16bec-117">For this information, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span> <span data-ttu-id="16bec-118">Anvisningar för plattformsoberoende kommandoradsgränssnitt finns i [den här självstudiekursen](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="16bec-118">Or, for Cross-Platform Command-Line Interface instructions, see [this equivalent tutorial](key-vault-manage-with-cli2.md).</span></span>
>
> <span data-ttu-id="16bec-119">För närvarande kan du konfigurera Azure Key Vault i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="16bec-119">Currently, you cannot configure Azure Key Vault in hello Azure portal.</span></span> <span data-ttu-id="16bec-120">I stället använder du dessa Azure PowerShell-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="16bec-120">Instead, use these Azure PowerShell instructions.</span></span>
>
>

<span data-ttu-id="16bec-121">Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="16bec-121">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16bec-122">Krav</span><span class="sxs-lookup"><span data-stu-id="16bec-122">Prerequisites</span></span>
<span data-ttu-id="16bec-123">toocomplete den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="16bec-123">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="16bec-124">Ett befintligt nyckelvalv som du har använt.</span><span class="sxs-lookup"><span data-stu-id="16bec-124">An existing key vault that you have been using.</span></span>  
* <span data-ttu-id="16bec-125">Azure PowerShell, **minst version 1.0.1**.</span><span class="sxs-lookup"><span data-stu-id="16bec-125">Azure PowerShell, **minimum version of 1.0.1**.</span></span> <span data-ttu-id="16bec-126">tooinstall Azure PowerShell och koppla den till din Azure-prenumeration, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="16bec-126">tooinstall Azure PowerShell and associate it with your Azure subscription, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="16bec-127">Om du redan har installerat Azure PowerShell och inte vet hello version från hello Azure PowerShell-konsolen, Skriv `(Get-Module azure -ListAvailable).Version`.</span><span class="sxs-lookup"><span data-stu-id="16bec-127">If you have already installed Azure PowerShell and do not know hello version, from hello Azure PowerShell console, type `(Get-Module azure -ListAvailable).Version`.</span></span>  
* <span data-ttu-id="16bec-128">Tillräckligt med utrymme i Azure för Key Vault-loggarna.</span><span class="sxs-lookup"><span data-stu-id="16bec-128">Sufficient storage on Azure for your Key Vault logs.</span></span>

## <span data-ttu-id="16bec-129"><a id="connect"></a>Ansluta tooyour prenumerationer</span><span class="sxs-lookup"><span data-stu-id="16bec-129"><a id="connect"></a>Connect tooyour subscriptions</span></span>
<span data-ttu-id="16bec-130">Starta en Azure PowerShell-session och logga in tooyour Azure-konto med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="16bec-130">Start an Azure PowerShell session and sign in tooyour Azure account with hello following command:</span></span>  

    Login-AzureRmAccount

<span data-ttu-id="16bec-131">Ange ditt användarnamn för Azure-konto och lösenord i hello popup-webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="16bec-131">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="16bec-132">Azure PowerShell får alla hello-prenumerationer som är associerade med det här kontot och som standard, använder hello första.</span><span class="sxs-lookup"><span data-stu-id="16bec-132">Azure PowerShell will get all hello subscriptions that are associated with this account and by default, uses hello first one.</span></span>

<span data-ttu-id="16bec-133">Om du har flera prenumerationer du kanske toospecify en som har använt toocreate Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="16bec-133">If you have multiple subscriptions, you might have toospecify a specific one that was used toocreate your Azure Key Vault.</span></span> <span data-ttu-id="16bec-134">Skriv hello följande toosee hello prenumerationer för ditt konto:</span><span class="sxs-lookup"><span data-stu-id="16bec-134">Type hello following toosee hello subscriptions for your account:</span></span>

    Get-AzureRmSubscription

<span data-ttu-id="16bec-135">Sedan toospecify hello prenumeration som är kopplad till nyckelvalvet du loggar, typ:</span><span class="sxs-lookup"><span data-stu-id="16bec-135">Then, toospecify hello subscription that's associated with your key vault you will be logging, type:</span></span>

    Set-AzureRmContext -SubscriptionId <subscription ID>

> [!NOTE]
> <span data-ttu-id="16bec-136">Detta är ett viktigt steg och särskilt användbart om du har flera prenumerationer som är kopplade till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="16bec-136">This is an important step and especially helpful if you have multiple subscriptions associated with your account.</span></span> <span data-ttu-id="16bec-137">Du kan få ett fel tooregister Microsoft.Insights om det här steget hoppas över.</span><span class="sxs-lookup"><span data-stu-id="16bec-137">You may receive an error tooregister Microsoft.Insights if this step is skipped.</span></span>
>   
>

<span data-ttu-id="16bec-138">Mer information om hur du konfigurerar Azure PowerShell finns [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="16bec-138">For more information about configuring Azure PowerShell, see  [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <span data-ttu-id="16bec-139"><a id="storage"></a>Skapa ett nytt lagringskonto för dina loggar</span><span class="sxs-lookup"><span data-stu-id="16bec-139"><a id="storage"></a>Create a new storage account for your logs</span></span>
<span data-ttu-id="16bec-140">Men du kan använda ett befintligt lagringskonto för dina loggar, ska vi skapa ett nytt lagringskonto som är dedikerad tooKey valvet loggar.</span><span class="sxs-lookup"><span data-stu-id="16bec-140">Although you can use an existing storage account for your logs, we'll create a new storage account that will be dedicated tooKey Vault logs.</span></span> <span data-ttu-id="16bec-141">Av praktiska skäl för när vi har toospecify detta senare hello information ska lagras i en variabel med namnet **sa**.</span><span class="sxs-lookup"><span data-stu-id="16bec-141">For convenience for when we have toospecify this later, we'll store hello details into a variable named **sa**.</span></span>

<span data-ttu-id="16bec-142">För ytterligare enkel hantering, vi också använder hello samma resursgrupp som hello en som innehåller våra nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="16bec-142">For additional ease of management, we'll also use hello same resource group as hello one that contains our key vault.</span></span> <span data-ttu-id="16bec-143">Från hello [komma igång-självstudiekurs](key-vault-get-started.md), den här resursgruppen heter **ContosoResourceGroup** och vi kommer att fortsätta toouse hello Östasien plats.</span><span class="sxs-lookup"><span data-stu-id="16bec-143">From hello [getting started tutorial](key-vault-get-started.md), this resource group is named **ContosoResourceGroup** and we'll continue toouse hello East Asia location.</span></span> <span data-ttu-id="16bec-144">Ersätt värdena med dina egna efter behov:</span><span class="sxs-lookup"><span data-stu-id="16bec-144">Substitute these values for your own, as applicable:</span></span>

    $sa = New-AzureRmStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'


> [!NOTE]
> <span data-ttu-id="16bec-145">Om du väljer toouse ett befintligt lagringskonto, måste den använda hello samma prenumeration som ditt nyckelvalv och den måste använda hello Resource Manager-modellen i stället för hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="16bec-145">If you decide toouse an existing storage account, it must use hello same subscription as your key vault and it must use hello Resource Manager deployment model, rather than hello Classic deployment model.</span></span>
>
>

## <span data-ttu-id="16bec-146"><a id="identify"></a>Identifiera hello nyckelvalv för loggarna</span><span class="sxs-lookup"><span data-stu-id="16bec-146"><a id="identify"></a>Identify hello key vault for your logs</span></span>
<span data-ttu-id="16bec-147">I vår komma igång-kursen var vår nyckelvalv namnet **ContosoKeyVault**, så vi kommer att fortsätta toouse som namn och lagra hello information till en variabel med namnet **kv**:</span><span class="sxs-lookup"><span data-stu-id="16bec-147">In our getting started tutorial, our key vault name was **ContosoKeyVault**, so we'll continue toouse that name and store hello details into a variable named **kv**:</span></span>

    $kv = Get-AzureRmKeyVault -VaultName 'ContosoKeyVault'


## <span data-ttu-id="16bec-148"><a id="enable"></a>Aktivera loggning</span><span class="sxs-lookup"><span data-stu-id="16bec-148"><a id="enable"></a>Enable logging</span></span>
<span data-ttu-id="16bec-149">tooenable loggning för Key Vault vi använder hello Set-AzureRmDiagnosticSetting cmdlet, tillsammans med hello variabler som vi skapade för vår nya storage-konto och våra nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="16bec-149">tooenable logging for Key Vault, we'll use hello Set-AzureRmDiagnosticSetting cmdlet, together with hello variables we created for our new storage account and our key vault.</span></span> <span data-ttu-id="16bec-150">Vi ska också ange hello **-aktiverad** flaggan för**$true** och ange hello kategori tooAuditEvent (hello endast kategori för Key Vault-loggning):</span><span class="sxs-lookup"><span data-stu-id="16bec-150">We'll also set hello **-Enabled** flag too**$true** and set hello category tooAuditEvent (hello only category for Key Vault logging), :</span></span>

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

<span data-ttu-id="16bec-151">hello utdata för detta omfattar:</span><span class="sxs-lookup"><span data-stu-id="16bec-151">hello output for this includes:</span></span>

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0


<span data-ttu-id="16bec-152">Det här bekräftar att loggning har nu aktiverats för nyckelvalvet, spara information tooyour storage-konto.</span><span class="sxs-lookup"><span data-stu-id="16bec-152">This confirms that logging is now enabled for your key vault, saving information tooyour storage account.</span></span>

<span data-ttu-id="16bec-153">Du kan också ange bevarandeprincip för loggar så att äldre loggar tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="16bec-153">Optionally you can also set retention policy for your logs such that older logs will be automatically deleted.</span></span> <span data-ttu-id="16bec-154">Till exempel kvarhållning princip genom att använda **- RetentionEnabled** flaggan för**$true** och ange **- RetentionInDays** parameter för**90** så att loggar som är äldre än 90 dagar tas automatiskt bort.</span><span class="sxs-lookup"><span data-stu-id="16bec-154">For example, set retention policy using **-RetentionEnabled** flag too**$true** and set **-RetentionInDays** parameter too**90** so that logs older than 90 days will be automatically deleted.</span></span>

    Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent -RetentionEnabled $true -RetentionInDays 90

<span data-ttu-id="16bec-155">Vad loggas:</span><span class="sxs-lookup"><span data-stu-id="16bec-155">What's logged:</span></span>

* <span data-ttu-id="16bec-156">Alla autentiserade REST-API-förfrågningar loggas, vilket omfattar förfrågningar som misslyckats på grund av åtkomstbehörigheter, systemfel eller ogiltiga förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="16bec-156">All authenticated REST API requests are logged, which includes failed requests as a result of access permissions, system errors or bad requests.</span></span>
* <span data-ttu-id="16bec-157">Åtgärder på hello nyckeln valvet sig själv, vilket inkluderar skapande, borttagning, inställningen nyckelvalv åtkomstprinciper, och uppdaterar nyckelvalv attribut som etiketter.</span><span class="sxs-lookup"><span data-stu-id="16bec-157">Operations on hello key vault itself, which includes creation, deletion, setting key vault access policies, and updating key vault attributes such as tags.</span></span>
* <span data-ttu-id="16bec-158">Åtgärder för nycklar och hemligheter i hello nyckelvalvet, vilket innefattar att skapa, ändra eller ta bort dessa nycklar och hemligheter; åtgärder som till exempel signera, verifiera, kryptera, dekryptera, omsluter och unwrap nycklar, hämta hemligheter, lista över nycklar och hemligheter och deras versioner.</span><span class="sxs-lookup"><span data-stu-id="16bec-158">Operations on keys and secrets in hello key vault, which includes creating, modifying, or deleting these keys or secrets; operations such as sign, verify, encrypt, decrypt, wrap and unwrap keys, get secrets, list keys and secrets and their versions.</span></span>
* <span data-ttu-id="16bec-159">Oautentiserade förfrågningar som resulterar i ett 401-svar.</span><span class="sxs-lookup"><span data-stu-id="16bec-159">Unauthenticated requests that result in a 401 response.</span></span> <span data-ttu-id="16bec-160">Till exempel förfrågningar som inte har någon ägartoken, som är felaktiga, som har upphört att gälla eller som har en ogiltig token.</span><span class="sxs-lookup"><span data-stu-id="16bec-160">For example, requests that do not have a bearer token, or are malformed or expired, or have an invalid token.</span></span>  

## <span data-ttu-id="16bec-161"><a id="access"></a>Komma åt loggarna</span><span class="sxs-lookup"><span data-stu-id="16bec-161"><a id="access"></a>Access your logs</span></span>
<span data-ttu-id="16bec-162">Nyckelvalv loggfilerna lagras i hello **insights-loggar-auditevent** behållare i hello storage-konto som du angav.</span><span class="sxs-lookup"><span data-stu-id="16bec-162">Key vault logs are stored in hello **insights-logs-auditevent** container in hello storage account you provided.</span></span> <span data-ttu-id="16bec-163">toolist skriver du alla hello blobbar i den här behållaren:</span><span class="sxs-lookup"><span data-stu-id="16bec-163">toolist all hello blobs in this container, type:</span></span>

<span data-ttu-id="16bec-164">Skapa först en variabel för hello behållarnamn.</span><span class="sxs-lookup"><span data-stu-id="16bec-164">First, create a variable for hello container name.</span></span> <span data-ttu-id="16bec-165">Detta kommer att användas i hela hello resten av hello genomgång.</span><span class="sxs-lookup"><span data-stu-id="16bec-165">This will be used throughout hello rest of hello walk through.</span></span>

    $container = 'insights-logs-auditevent'

<span data-ttu-id="16bec-166">toolist skriver du alla hello blobbar i den här behållaren:</span><span class="sxs-lookup"><span data-stu-id="16bec-166">toolist all hello blobs in this container, type:</span></span>

    Get-AzureStorageBlob -Container $container -Context $sa.Context
<span data-ttu-id="16bec-167">hello-utdata kommer att se något liknande toothis:</span><span class="sxs-lookup"><span data-stu-id="16bec-167">hello output will look something similar toothis:</span></span>

<span data-ttu-id="16bec-168">**Behållarens URI: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**</span><span class="sxs-lookup"><span data-stu-id="16bec-168">**Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent**</span></span>

<span data-ttu-id="16bec-169">**Namn**</span><span class="sxs-lookup"><span data-stu-id="16bec-169">**Name**</span></span>

- - -
<span data-ttu-id="16bec-170">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**</span><span class="sxs-lookup"><span data-stu-id="16bec-170">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json**</span></span>

<span data-ttu-id="16bec-171">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**</span><span class="sxs-lookup"><span data-stu-id="16bec-171">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json**</span></span>

<span data-ttu-id="16bec-172">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****</span><span class="sxs-lookup"><span data-stu-id="16bec-172">**resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json****</span></span>

<span data-ttu-id="16bec-173">Som du ser i den här utdatan hello blobbar följa en namngivningskonvention: **resourceId =<ARM resource ID>/y =<year>/m =<month>/d =<day of month>/tim =<hour>/m =<minute>/filename.json**</span><span class="sxs-lookup"><span data-stu-id="16bec-173">As you can see from this output, hello blobs follow a naming convention: **resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json**</span></span>

<span data-ttu-id="16bec-174">hello datum- och tidsvärden Använd UTC.</span><span class="sxs-lookup"><span data-stu-id="16bec-174">hello date and time values use UTC.</span></span>

<span data-ttu-id="16bec-175">Eftersom hello samma lagringskonto kan vara används toocollect loggar för olika resurser, är hello fullständiga resurs-ID i hello blob-namnet mycket användbar tooaccess eller hämta bara hello blob som du behöver.</span><span class="sxs-lookup"><span data-stu-id="16bec-175">Because hello same storage account can be used toocollect logs for multiple resources, hello full resource ID in hello blob name is very useful tooaccess or download just hello blobs that you need.</span></span> <span data-ttu-id="16bec-176">Men innan vi gör det kan vi först tar upp hur toodownload alla hello blobbar.</span><span class="sxs-lookup"><span data-stu-id="16bec-176">But before we do that, we'll first cover how toodownload all hello blobs.</span></span>

<span data-ttu-id="16bec-177">Först skapa en mapp toodownload hello blobbar.</span><span class="sxs-lookup"><span data-stu-id="16bec-177">First, create a folder toodownload hello blobs.</span></span> <span data-ttu-id="16bec-178">Exempel:</span><span class="sxs-lookup"><span data-stu-id="16bec-178">For example:</span></span>

    New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force

<span data-ttu-id="16bec-179">Hämta sedan en lista över alla blobbar:</span><span class="sxs-lookup"><span data-stu-id="16bec-179">Then get a list of all blobs:</span></span>  

    $blobs = Get-AzureStorageBlob -Container $container -Context $sa.Context

<span data-ttu-id="16bec-180">Skicka den här listan via 'Get-AzureStorageBlobContent' toodownload hello blobbar i vår målmappen:</span><span class="sxs-lookup"><span data-stu-id="16bec-180">Pipe this list through 'Get-AzureStorageBlobContent' toodownload hello blobs into our destination folder:</span></span>

    $blobs | Get-AzureStorageBlobContent -Destination 'C:\Users\username\ContosoKeyVaultLogs'

<span data-ttu-id="16bec-181">När du kör andra kommandot hello  **/**  avgränsare i hello blobbnamnen skapa en fullständig mappstruktur under hello målmappen och den här strukturen kommer att använda toodownload och lagra hello blob som filer.</span><span class="sxs-lookup"><span data-stu-id="16bec-181">When you run this second command, hello **/** delimiter in hello blob names create a full folder structure under hello destination folder, and this structure will be used toodownload and store hello blobs as files.</span></span>

<span data-ttu-id="16bec-182">tooselectively ladda ned blobbar kan använda jokertecken.</span><span class="sxs-lookup"><span data-stu-id="16bec-182">tooselectively download blobs, use wildcards.</span></span> <span data-ttu-id="16bec-183">Exempel:</span><span class="sxs-lookup"><span data-stu-id="16bec-183">For example:</span></span>

* <span data-ttu-id="16bec-184">Om du har flera viktiga valv och vill toodownload loggar för ett enda nyckelvalv med namnet CONTOSOKEYVAULT3:</span><span class="sxs-lookup"><span data-stu-id="16bec-184">If you have multiple key vaults and want toodownload logs for just one key vault, named CONTOSOKEYVAULT3:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
* <span data-ttu-id="16bec-185">Om du har flera resursgrupper och vill toodownload loggar för en resursgrupp, använda `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:</span><span class="sxs-lookup"><span data-stu-id="16bec-185">If you have multiple resource groups and want toodownload logs for just one resource group, use `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
* <span data-ttu-id="16bec-186">Om du vill toodownload alla hello loggar för hello månad januari 2016 använder `-Blob '*/year=2016/m=01/*'`:</span><span class="sxs-lookup"><span data-stu-id="16bec-186">If you want toodownload all hello logs for hello month of January 2016, use `-Blob '*/year=2016/m=01/*'`:</span></span>

        Get-AzureStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'

<span data-ttu-id="16bec-187">Du är nu redo toostart tittar på vad som finns i hello loggar.</span><span class="sxs-lookup"><span data-stu-id="16bec-187">You're now ready toostart looking at what's in hello logs.</span></span> <span data-ttu-id="16bec-188">Men innan du flyttar till två fler parametrar för att du kan behöva tooknow Get-AzureRmDiagnosticSetting som:</span><span class="sxs-lookup"><span data-stu-id="16bec-188">But before moving onto that, two more parameters for Get-AzureRmDiagnosticSetting that you might need tooknow:</span></span>

* <span data-ttu-id="16bec-189">tooquery hello status för diagnostikinställningar för nyckelvalvet-resurs:`Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`</span><span class="sxs-lookup"><span data-stu-id="16bec-189">tooquery hello  status of diagnostic settings for your key vault resource: `Get-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId`</span></span>
* <span data-ttu-id="16bec-190">toodisable loggning för nyckelvalvet-resurs:`Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`</span><span class="sxs-lookup"><span data-stu-id="16bec-190">toodisable logging for your key vault resource: `Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Categories AuditEvent`</span></span>

## <span data-ttu-id="16bec-191"><a id="interpret"></a>Tolka Key Vault-loggarna</span><span class="sxs-lookup"><span data-stu-id="16bec-191"><a id="interpret"></a>Interpret your Key Vault logs</span></span>
<span data-ttu-id="16bec-192">Enskilda blobbar lagras som text, formaterad som en JSON-blobb.</span><span class="sxs-lookup"><span data-stu-id="16bec-192">Individual blobs are stored as text, formatted as a JSON blob.</span></span> <span data-ttu-id="16bec-193">Det här är ett exempel på en loggpost från `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:</span><span class="sxs-lookup"><span data-stu-id="16bec-193">This is an example log entry from running `Get-AzureRmKeyVault -VaultName 'contosokeyvault'`:</span></span>

    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }


<span data-ttu-id="16bec-194">hello följande tabell visas hello fältnamn och beskrivningar.</span><span class="sxs-lookup"><span data-stu-id="16bec-194">hello following table lists hello field names and descriptions.</span></span>

| <span data-ttu-id="16bec-195">Fältnamn</span><span class="sxs-lookup"><span data-stu-id="16bec-195">Field name</span></span> | <span data-ttu-id="16bec-196">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="16bec-196">Description</span></span> |
| --- | --- |
| <span data-ttu-id="16bec-197">time</span><span class="sxs-lookup"><span data-stu-id="16bec-197">time</span></span> |<span data-ttu-id="16bec-198">Datum och tid (UTC).</span><span class="sxs-lookup"><span data-stu-id="16bec-198">Date and time (UTC).</span></span> |
| <span data-ttu-id="16bec-199">resourceId</span><span class="sxs-lookup"><span data-stu-id="16bec-199">resourceId</span></span> |<span data-ttu-id="16bec-200">Azure Resource Manager Resource-ID.</span><span class="sxs-lookup"><span data-stu-id="16bec-200">Azure Resource Manager Resource ID.</span></span> <span data-ttu-id="16bec-201">För Key Vault loggar är alltid hello Key Vault resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="16bec-201">For Key Vault logs, this is always hello  Key Vault resource ID.</span></span> |
| <span data-ttu-id="16bec-202">operationName</span><span class="sxs-lookup"><span data-stu-id="16bec-202">operationName</span></span> |<span data-ttu-id="16bec-203">Namnet på hello åtgärd utförs, enligt beskrivningen i hello nästa tabell.</span><span class="sxs-lookup"><span data-stu-id="16bec-203">Name of hello operation, as documented in hello next table.</span></span> |
| <span data-ttu-id="16bec-204">operationVersion</span><span class="sxs-lookup"><span data-stu-id="16bec-204">operationVersion</span></span> |<span data-ttu-id="16bec-205">Detta är hello REST API-version som begärs av hello-klient.</span><span class="sxs-lookup"><span data-stu-id="16bec-205">This is hello REST API version requested by hello client.</span></span> |
| <span data-ttu-id="16bec-206">category</span><span class="sxs-lookup"><span data-stu-id="16bec-206">category</span></span> |<span data-ttu-id="16bec-207">För Key Vault loggar är AuditEvent hello enda, tillgänglig värde.</span><span class="sxs-lookup"><span data-stu-id="16bec-207">For Key Vault logs, AuditEvent is hello single, available value.</span></span> |
| <span data-ttu-id="16bec-208">resultType</span><span class="sxs-lookup"><span data-stu-id="16bec-208">resultType</span></span> |<span data-ttu-id="16bec-209">Resultatet av REST-API-begäran.</span><span class="sxs-lookup"><span data-stu-id="16bec-209">Result of REST API request.</span></span> |
| <span data-ttu-id="16bec-210">resultSignature</span><span class="sxs-lookup"><span data-stu-id="16bec-210">resultSignature</span></span> |<span data-ttu-id="16bec-211">HTTP-status.</span><span class="sxs-lookup"><span data-stu-id="16bec-211">HTTP status.</span></span> |
| <span data-ttu-id="16bec-212">resultDescription</span><span class="sxs-lookup"><span data-stu-id="16bec-212">resultDescription</span></span> |<span data-ttu-id="16bec-213">Ytterligare beskrivning om hello resultat när det är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="16bec-213">Additional description about hello result, when available.</span></span> |
| <span data-ttu-id="16bec-214">durationMs</span><span class="sxs-lookup"><span data-stu-id="16bec-214">durationMs</span></span> |<span data-ttu-id="16bec-215">Tiden det tog tooservice hello REST-API-begäran, i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="16bec-215">Time it took tooservice hello REST API request, in milliseconds.</span></span> <span data-ttu-id="16bec-216">Detta inkluderar inte hello Nätverksfördröjningen, så hello tid du mäta på klientsidan för hello inte kanske stämmer med den här gången.</span><span class="sxs-lookup"><span data-stu-id="16bec-216">This does not include hello network latency, so hello time you measure on hello client side might not match this time.</span></span> |
| <span data-ttu-id="16bec-217">callerIpAddress</span><span class="sxs-lookup"><span data-stu-id="16bec-217">callerIpAddress</span></span> |<span data-ttu-id="16bec-218">Hello-klienten som gjorde begäran hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="16bec-218">IP address of hello client who made hello request.</span></span> |
| <span data-ttu-id="16bec-219">correlationId</span><span class="sxs-lookup"><span data-stu-id="16bec-219">correlationId</span></span> |<span data-ttu-id="16bec-220">Ett valfritt GUID som hello klienten kan skicka toocorrelate klientsidan loggar med loggar av tjänsten på klientsidan (Key Vault).</span><span class="sxs-lookup"><span data-stu-id="16bec-220">An optional GUID that hello client can pass toocorrelate client-side logs with service-side (Key Vault) logs.</span></span> |
| <span data-ttu-id="16bec-221">identity</span><span class="sxs-lookup"><span data-stu-id="16bec-221">identity</span></span> |<span data-ttu-id="16bec-222">Identitet från hello-token som angavs när du gör hello REST API-begäran.</span><span class="sxs-lookup"><span data-stu-id="16bec-222">Identity from hello token that was presented when making hello REST API request.</span></span> <span data-ttu-id="16bec-223">Detta är vanligtvis en ”användare”, ”tjänstens huvudnamn” eller en kombination av ”användare+app-ID”, till exempel då en begäran är resultatet av en Azure PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="16bec-223">This is usually a "user", a "service principal" or a combination "user+appId" as in case of a request resulting from a Azure PowerShell cmdlet.</span></span> |
| <span data-ttu-id="16bec-224">properties</span><span class="sxs-lookup"><span data-stu-id="16bec-224">properties</span></span> |<span data-ttu-id="16bec-225">Det här fältet innehåller olika typer av information baserat på hello åtgärden (operationName).</span><span class="sxs-lookup"><span data-stu-id="16bec-225">This field will contain different information based on hello operation (operationName).</span></span> <span data-ttu-id="16bec-226">I de flesta fall innehåller information om klienter (hello useragent sträng skickades av klientens hello), hello exakt URI för REST API-begäran och HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="16bec-226">In most cases, contains client information (hello useragent string passed by hello client), hello exact REST API request URI, and HTTP status code.</span></span> <span data-ttu-id="16bec-227">Dessutom, när ett objekt returneras ett resultat av en begäran (till exempel KeyCreate eller VaultGet) innehåller den också hello nyckeln URI (som ”id”), valvet URI eller hemlighet URI.</span><span class="sxs-lookup"><span data-stu-id="16bec-227">In addition, when an object is returned as a result of a request (for example, KeyCreate or VaultGet) it will also contain hello Key URI (as "id"), Vault URI, or Secret URI.</span></span> |

<span data-ttu-id="16bec-228">Hej **operationName** fältvärden har ObjectVerb format.</span><span class="sxs-lookup"><span data-stu-id="16bec-228">hello **operationName** field values are in ObjectVerb format.</span></span> <span data-ttu-id="16bec-229">Exempel:</span><span class="sxs-lookup"><span data-stu-id="16bec-229">For example:</span></span>

* <span data-ttu-id="16bec-230">Alla åtgärder i nyckelvalvet har hello ' valvet`<action>`-formatet som `VaultGet` och `VaultCreate`.</span><span class="sxs-lookup"><span data-stu-id="16bec-230">All key vault operations have hello 'Vault`<action>`' format, such as `VaultGet` and `VaultCreate`.</span></span>
* <span data-ttu-id="16bec-231">Alla åtgärder som nyckel har hello ' nyckel`<action>`-formatet som `KeySign` och `KeyList`.</span><span class="sxs-lookup"><span data-stu-id="16bec-231">All  key operations have hello 'Key`<action>`' format, such as `KeySign` and `KeyList`.</span></span>
* <span data-ttu-id="16bec-232">Alla hemliga åtgärder har hello ' hemlighet`<action>`-formatet som `SecretGet` och `SecretListVersions`.</span><span class="sxs-lookup"><span data-stu-id="16bec-232">All secret operations have hello 'Secret`<action>`' format, such as `SecretGet` and `SecretListVersions`.</span></span>

<span data-ttu-id="16bec-233">hello i den följande tabellen listas hello operationName och motsvarande REST API-kommandot.</span><span class="sxs-lookup"><span data-stu-id="16bec-233">hello following table lists hello operationName and corresponding REST API command.</span></span>

| <span data-ttu-id="16bec-234">operationName</span><span class="sxs-lookup"><span data-stu-id="16bec-234">operationName</span></span> | <span data-ttu-id="16bec-235">REST-API-kommando</span><span class="sxs-lookup"><span data-stu-id="16bec-235">REST API Command</span></span> |
| --- | --- |
| <span data-ttu-id="16bec-236">Autentisering</span><span class="sxs-lookup"><span data-stu-id="16bec-236">Authentication</span></span> |<span data-ttu-id="16bec-237">Via Azure Active Directory-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="16bec-237">Via Azure Active Directory endpoint</span></span> |
| <span data-ttu-id="16bec-238">VaultGet</span><span class="sxs-lookup"><span data-stu-id="16bec-238">VaultGet</span></span> |[<span data-ttu-id="16bec-239">Hämta information om ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="16bec-239">Get information about a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620026.aspx) |
| <span data-ttu-id="16bec-240">VaultPut</span><span class="sxs-lookup"><span data-stu-id="16bec-240">VaultPut</span></span> |[<span data-ttu-id="16bec-241">Skapa eller uppdatera ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="16bec-241">Create or update a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620025.aspx) |
| <span data-ttu-id="16bec-242">VaultDelete</span><span class="sxs-lookup"><span data-stu-id="16bec-242">VaultDelete</span></span> |[<span data-ttu-id="16bec-243">Ta bort ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="16bec-243">Delete a key vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620022.aspx) |
| <span data-ttu-id="16bec-244">VaultPatch</span><span class="sxs-lookup"><span data-stu-id="16bec-244">VaultPatch</span></span> |[<span data-ttu-id="16bec-245">Uppdatera ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="16bec-245">Update a key vault</span></span>](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| <span data-ttu-id="16bec-246">VaultList</span><span class="sxs-lookup"><span data-stu-id="16bec-246">VaultList</span></span> |[<span data-ttu-id="16bec-247">Visa en lista med alla nyckelvalv i en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="16bec-247">List all key vaults in a resource group</span></span>](https://msdn.microsoft.com/en-us/library/azure/mt620027.aspx) |
| <span data-ttu-id="16bec-248">KeyCreate</span><span class="sxs-lookup"><span data-stu-id="16bec-248">KeyCreate</span></span> |[<span data-ttu-id="16bec-249">Skapa en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-249">Create a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903634.aspx) |
| <span data-ttu-id="16bec-250">KeyGet</span><span class="sxs-lookup"><span data-stu-id="16bec-250">KeyGet</span></span> |[<span data-ttu-id="16bec-251">Hämta information om en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-251">Get information about a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878080.aspx) |
| <span data-ttu-id="16bec-252">KeyImport</span><span class="sxs-lookup"><span data-stu-id="16bec-252">KeyImport</span></span> |[<span data-ttu-id="16bec-253">Importera en nyckel till ett valv</span><span class="sxs-lookup"><span data-stu-id="16bec-253">Import a key into a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903626.aspx) |
| <span data-ttu-id="16bec-254">KeyBackup</span><span class="sxs-lookup"><span data-stu-id="16bec-254">KeyBackup</span></span> |<span data-ttu-id="16bec-255">[Säkerhetskopiera en nyckel](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).</span><span class="sxs-lookup"><span data-stu-id="16bec-255">[Backup a key](https://msdn.microsoft.com/en-us/library/azure/dn878058.aspx).</span></span> |
| <span data-ttu-id="16bec-256">KeyDelete</span><span class="sxs-lookup"><span data-stu-id="16bec-256">KeyDelete</span></span> |[<span data-ttu-id="16bec-257">Ta bort en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-257">Delete a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903611.aspx) |
| <span data-ttu-id="16bec-258">KeyRestore</span><span class="sxs-lookup"><span data-stu-id="16bec-258">KeyRestore</span></span> |[<span data-ttu-id="16bec-259">Återställa en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-259">Restore a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878106.aspx) |
| <span data-ttu-id="16bec-260">KeySign</span><span class="sxs-lookup"><span data-stu-id="16bec-260">KeySign</span></span> |[<span data-ttu-id="16bec-261">Signera med en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-261">Sign with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878096.aspx) |
| <span data-ttu-id="16bec-262">KeyVerify</span><span class="sxs-lookup"><span data-stu-id="16bec-262">KeyVerify</span></span> |[<span data-ttu-id="16bec-263">Verifiera med en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-263">Verify with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878082.aspx) |
| <span data-ttu-id="16bec-264">KeyWrap</span><span class="sxs-lookup"><span data-stu-id="16bec-264">KeyWrap</span></span> |[<span data-ttu-id="16bec-265">Omsluta en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-265">Wrap a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878066.aspx) |
| <span data-ttu-id="16bec-266">KeyUnwrap</span><span class="sxs-lookup"><span data-stu-id="16bec-266">KeyUnwrap</span></span> |[<span data-ttu-id="16bec-267">Ta bort en nyckelomslutning</span><span class="sxs-lookup"><span data-stu-id="16bec-267">Unwrap a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878079.aspx) |
| <span data-ttu-id="16bec-268">KeyEncrypt</span><span class="sxs-lookup"><span data-stu-id="16bec-268">KeyEncrypt</span></span> |[<span data-ttu-id="16bec-269">Kryptera med en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-269">Encrypt with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878060.aspx) |
| <span data-ttu-id="16bec-270">KeyDecrypt</span><span class="sxs-lookup"><span data-stu-id="16bec-270">KeyDecrypt</span></span> |[<span data-ttu-id="16bec-271">Dekryptera med en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-271">Decrypt with a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn878097.aspx) |
| <span data-ttu-id="16bec-272">KeyUpdate</span><span class="sxs-lookup"><span data-stu-id="16bec-272">KeyUpdate</span></span> |[<span data-ttu-id="16bec-273">Uppdatera en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-273">Update a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903616.aspx) |
| <span data-ttu-id="16bec-274">KeyList</span><span class="sxs-lookup"><span data-stu-id="16bec-274">KeyList</span></span> |[<span data-ttu-id="16bec-275">Lista hello nycklar i ett valv</span><span class="sxs-lookup"><span data-stu-id="16bec-275">List hello keys in a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903629.aspx) |
| <span data-ttu-id="16bec-276">KeyListVersions</span><span class="sxs-lookup"><span data-stu-id="16bec-276">KeyListVersions</span></span> |[<span data-ttu-id="16bec-277">Lista hello versioner av en nyckel</span><span class="sxs-lookup"><span data-stu-id="16bec-277">List hello versions of a key</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986822.aspx) |
| <span data-ttu-id="16bec-278">SecretSet</span><span class="sxs-lookup"><span data-stu-id="16bec-278">SecretSet</span></span> |[<span data-ttu-id="16bec-279">Skapa en hemlighet</span><span class="sxs-lookup"><span data-stu-id="16bec-279">Create a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903618.aspx) |
| <span data-ttu-id="16bec-280">SecretGet</span><span class="sxs-lookup"><span data-stu-id="16bec-280">SecretGet</span></span> |[<span data-ttu-id="16bec-281">Hämta en hemlighet</span><span class="sxs-lookup"><span data-stu-id="16bec-281">Get secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903633.aspx) |
| <span data-ttu-id="16bec-282">SecretUpdate</span><span class="sxs-lookup"><span data-stu-id="16bec-282">SecretUpdate</span></span> |[<span data-ttu-id="16bec-283">Uppdatera en hemlighet</span><span class="sxs-lookup"><span data-stu-id="16bec-283">Update a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986818.aspx) |
| <span data-ttu-id="16bec-284">SecretDelete</span><span class="sxs-lookup"><span data-stu-id="16bec-284">SecretDelete</span></span> |[<span data-ttu-id="16bec-285">Ta bort en hemlighet</span><span class="sxs-lookup"><span data-stu-id="16bec-285">Delete a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903613.aspx) |
| <span data-ttu-id="16bec-286">SecretList</span><span class="sxs-lookup"><span data-stu-id="16bec-286">SecretList</span></span> |[<span data-ttu-id="16bec-287">Visa en lista över hemligheterna i ett valv</span><span class="sxs-lookup"><span data-stu-id="16bec-287">List secrets in a vault</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn903614.aspx) |
| <span data-ttu-id="16bec-288">SecretListVersions</span><span class="sxs-lookup"><span data-stu-id="16bec-288">SecretListVersions</span></span> |[<span data-ttu-id="16bec-289">Visa en lista över versionerna av en hemlighet</span><span class="sxs-lookup"><span data-stu-id="16bec-289">List versions of a secret</span></span>](https://msdn.microsoft.com/en-us/library/azure/dn986824.aspx) |

## <span data-ttu-id="16bec-290"><a id="loganalytics"></a>Använda Log Analytics</span><span class="sxs-lookup"><span data-stu-id="16bec-290"><a id="loganalytics"></a>Use Log Analytics</span></span>

<span data-ttu-id="16bec-291">Du kan använda hello Azure Key Vault-lösning i logganalys tooreview Azure Key Vault AuditEvent loggar.</span><span class="sxs-lookup"><span data-stu-id="16bec-291">You can use hello Azure Key Vault solution in Log Analytics tooreview Azure Key Vault AuditEvent logs.</span></span> <span data-ttu-id="16bec-292">Mer information, inklusive hur tooset detta, se [Azure Key Vault-lösning i logganalys](../log-analytics/log-analytics-azure-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="16bec-292">For more information, including how tooset this up, see [Azure Key Vault solution in Log Analytics](../log-analytics/log-analytics-azure-key-vault.md).</span></span> <span data-ttu-id="16bec-293">Den här artikeln innehåller också instruktioner om du behöver toomigrate från hello gamla Key Vault-lösning som erbjöds hello logganalys förhandsversionen, där du först dirigeras din loggar tooan Azure Storage-konto och konfigurerat Log Analytics tooread därifrån.</span><span class="sxs-lookup"><span data-stu-id="16bec-293">This article also contains instructions if you need toomigrate from hello old Key Vault solution that was offered during hello Log Analytics preview, where you first routed your logs tooan Azure Storage account and configured Log Analytics tooread from there.</span></span>

## <span data-ttu-id="16bec-294"><a id="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16bec-294"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="16bec-295">En självstudiekurs där Azure Key Vault används i en webbapp finns i [Använda Azure Key Vault från en webbapp](key-vault-use-from-web-application.md).</span><span class="sxs-lookup"><span data-stu-id="16bec-295">For a tutorial that uses Azure Key Vault in a web application, see [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md).</span></span>

<span data-ttu-id="16bec-296">Programmering referenser finns [hello Azure Key Vault Utvecklarhandbok](key-vault-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="16bec-296">For programming references, see [hello Azure Key Vault developer's guide](key-vault-developers-guide.md).</span></span>

<span data-ttu-id="16bec-297">En lista med Azure PowerShell 1.0-cmdlets för Azure Key Vault finns i [Cmdlets för Azure Key Vault](/powershell/module/azurerm.keyvault/#key_vault).</span><span class="sxs-lookup"><span data-stu-id="16bec-297">For a list of Azure PowerShell 1.0 cmdlets for Azure Key Vault, see [Azure Key Vault Cmdlets](/powershell/module/azurerm.keyvault/#key_vault).</span></span>

<span data-ttu-id="16bec-298">En självstudiekurs om viktiga rotation och logg granskning med Azure Key Vault finns [hur toosetup Key Vault med end tooend nyckeln rotation och granskning](key-vault-key-rotation-log-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="16bec-298">For a tutorial on key rotation and log auditing with Azure Key Vault, see [How toosetup Key Vault with end tooend key rotation and auditing](key-vault-key-rotation-log-monitoring.md).</span></span>
