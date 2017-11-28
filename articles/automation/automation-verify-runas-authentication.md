---
title: aaaValidate Azure Automation-kontokonfigurationen | Microsoft Docs
description: "Den här artikeln beskriver hur tooconfirm hello av ditt Automation-konto har konfigurerats korrekt."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/07/2017
ms.author: magoedte
ms.openlocfilehash: 3a990dcc6661cf67c4b62592ce03d55a3791053a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-automation-run-as-account-authentication"></a><span data-ttu-id="69bae-103">Testa Kör som-kontoautentisering för Azure Automation</span><span class="sxs-lookup"><span data-stu-id="69bae-103">Test Azure Automation Run As account authentication</span></span>
<span data-ttu-id="69bae-104">När ett Automation-konto har skapats, du kan utföra en enkel test tooconfirm kan toosuccessfully autentiseras i Azure Resource Manager eller Azure klassisk distribution med ditt nyligen skapats eller uppdaterats Automation kör som-konto.</span><span class="sxs-lookup"><span data-stu-id="69bae-104">After an Automation account is successfully created, you can perform a simple test tooconfirm you are able toosuccessfully authenticate in Azure Resource Manager or Azure classic deployment using your newly created or updated Automation Run As account.</span></span>    

## <a name="automation-run-as-authentication"></a><span data-ttu-id="69bae-105">Automation Kör som-autentisering</span><span class="sxs-lookup"><span data-stu-id="69bae-105">Automation Run As authentication</span></span>
<span data-ttu-id="69bae-106">Använd hello exempelkoden nedan för[skapa en PowerShell-runbook](automation-creating-importing-runbook.md) tooverify autentisering med hjälp av hello kör som-konto och i din anpassade runbooks tooauthenticate och hantera Resource Manager-resurser med ditt Automation-konto.</span><span class="sxs-lookup"><span data-stu-id="69bae-106">Use hello sample code below too[create a PowerShell runbook](automation-creating-importing-runbook.md) tooverify authentication using hello Run As account and also in your custom runbooks tooauthenticate and manage Resource Manager resources with your Automation account.</span></span>   

    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get hello connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

        "Logging in tooAzure..."
        Add-AzureRmAccount `
           -ServicePrincipal `
           -TenantId $servicePrincipalConnection.TenantId `
           -ApplicationId $servicePrincipalConnection.ApplicationId `
           -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
       if (!$servicePrincipalConnection)
       {
          $ErrorMessage = "Connection $connectionName not found."
          throw $ErrorMessage
      } else{
          Write-Error -Message $_.Exception
          throw $_.Exception
      }
    }

    #Get all ARM resources from all resource groups
    $ResourceGroups = Get-AzureRmResourceGroup 

    foreach ($ResourceGroup in $ResourceGroups)
    {    
       Write-Output ("Showing resources in resource group " + $ResourceGroup.ResourceGroupName)
       $Resources = Find-AzureRmResource -ResourceGroupNameContains $ResourceGroup.ResourceGroupName | Select ResourceName, ResourceType
       ForEach ($Resource in $Resources)
       {
          Write-Output ($Resource.ResourceName + " of type " +  $Resource.ResourceType)
       }
       Write-Output ("")
    } 

<span data-ttu-id="69bae-107">Lägg märke till hello cmdlet som används för att autentisera i hello runbook - **Add-AzureRmAccount**, använder hello *ServicePrincipalCertificate* parameteruppsättning.</span><span class="sxs-lookup"><span data-stu-id="69bae-107">Notice hello cmdlet used for authenticating in hello runbook - **Add-AzureRmAccount**, uses hello *ServicePrincipalCertificate* parameter set.</span></span>  <span data-ttu-id="69bae-108">Den autentiserar med hjälp av tjänstobjektets certifikat, inte autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="69bae-108">It authenticates by using service principal certificate, not credentials.</span></span>  

<span data-ttu-id="69bae-109">När du [köra hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate din kör som-konto en [runbook-jobbet](automation-runbook-execution.md) har skapats hello jobbet bladet visas och hello jobbstatus visas i hello **jobbsammanfattning**panelen.</span><span class="sxs-lookup"><span data-stu-id="69bae-109">When you [run hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate your Run As account, a [runbook job](automation-runbook-execution.md) is created, hello Job blade is displayed, and hello job status displayed in hello **Job Summary** tile.</span></span> <span data-ttu-id="69bae-110">hello jobbstatus startar som *i kö* som anger att den väntar på en runbook worker i hello molnet toobecome tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="69bae-110">hello job status will start as *Queued* indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span> <span data-ttu-id="69bae-111">Sedan flyttas för*Start* när en arbetsprocess anspråk hello jobb, och sedan *kör* när hello runbook faktiskt börjar köras.</span><span class="sxs-lookup"><span data-stu-id="69bae-111">It will then move too*Starting* when a worker claims hello job, and then *Running* when hello runbook actually starts running.</span></span>  <span data-ttu-id="69bae-112">När hello runbook-jobbet är slutfört, vi bör du se statusen **slutförd**.</span><span class="sxs-lookup"><span data-stu-id="69bae-112">When hello runbook job completes, we should see a status of **Completed**.</span></span>

<span data-ttu-id="69bae-113">toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.</span><span class="sxs-lookup"><span data-stu-id="69bae-113">toosee hello detailed results of hello runbook, click on hello **Output** tile.</span></span>  <span data-ttu-id="69bae-114">På hello **utdata** bladet bör du se den har autentiserats och returnerar en lista över alla resurser i alla resursgrupper i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69bae-114">On hello **Output** blade, you should see it has successfully authenticated and returns a list of all resources in all resource groups in your subscription.</span></span>  

<span data-ttu-id="69bae-115">Men kom ihåg tooremove hello kodblock börjar med hello kommentar `#Get all ARM resources from all resource groups` när du återanvända hello kod för dina runbooks.</span><span class="sxs-lookup"><span data-stu-id="69bae-115">Just remember tooremove hello block of code starting with hello comment `#Get all ARM resources from all resource groups` when you reuse hello code for your runbooks.</span></span>

## <a name="classic-run-as-authentication"></a><span data-ttu-id="69bae-116">Klassisk Kör som-autentisering</span><span class="sxs-lookup"><span data-stu-id="69bae-116">Classic Run As authentication</span></span>
<span data-ttu-id="69bae-117">Använd hello exempelkoden nedan för[skapa en PowerShell-runbook](automation-creating-importing-runbook.md) tooverify autentisering med hello klassisk kör som-konto och i din anpassade runbooks tooauthenticate och hantera resurser i hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="69bae-117">Use hello sample code below too[create a PowerShell runbook](automation-creating-importing-runbook.md) tooverify authentication using hello Classic Run As account and also in your custom runbooks tooauthenticate and manage resources in hello classic deployment model.</span></span>  

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID
    
    #Get all VMs in hello subscription and return list with name of each
    Get-AzureVM | ft Name

<span data-ttu-id="69bae-118">När du [köra hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate din kör som-konto en [runbook-jobbet](automation-runbook-execution.md) har skapats hello jobbet bladet visas och hello jobbstatus visas i hello **jobbsammanfattning**panelen.</span><span class="sxs-lookup"><span data-stu-id="69bae-118">When you [run hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate your Run As account, a [runbook job](automation-runbook-execution.md) is created, hello Job blade is displayed, and hello job status displayed in hello **Job Summary** tile.</span></span> <span data-ttu-id="69bae-119">hello jobbstatus startar som *i kö* som anger att den väntar på en runbook worker i hello molnet toobecome tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="69bae-119">hello job status will start as *Queued* indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span> <span data-ttu-id="69bae-120">Sedan flyttas för*Start* när en arbetsprocess anspråk hello jobb, och sedan *kör* när hello runbook faktiskt börjar köras.</span><span class="sxs-lookup"><span data-stu-id="69bae-120">It will then move too*Starting* when a worker claims hello job, and then *Running* when hello runbook actually starts running.</span></span>  <span data-ttu-id="69bae-121">När hello runbook-jobbet är slutfört, vi bör du se statusen **slutförd**.</span><span class="sxs-lookup"><span data-stu-id="69bae-121">When hello runbook job completes, we should see a status of **Completed**.</span></span>

<span data-ttu-id="69bae-122">toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.</span><span class="sxs-lookup"><span data-stu-id="69bae-122">toosee hello detailed results of hello runbook, click on hello **Output** tile.</span></span>  <span data-ttu-id="69bae-123">På hello **utdata** bladet bör du se den har autentiserats och returnerar en lista över alla virtuella Azure-datorer med VMName som har distribuerats i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69bae-123">On hello **Output** blade, you should see it has successfully authenticated and returns a list of all Azure VMs by VMName that are deployed in your subscription.</span></span>  

<span data-ttu-id="69bae-124">Men kom ihåg tooremove hello cmdlet **Get-AzureVM** när du återanvända hello kod för dina runbooks.</span><span class="sxs-lookup"><span data-stu-id="69bae-124">Just remember tooremove hello cmdlet **Get-AzureVM** when you reuse hello code for your runbooks.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69bae-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69bae-125">Next steps</span></span>
* <span data-ttu-id="69bae-126">tooget igång med PowerShell-runbooks, se [min första PowerShell-runbook](automation-first-runbook-textual-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="69bae-126">tooget started with PowerShell runbooks, see [My first PowerShell runbook](automation-first-runbook-textual-powershell.md).</span></span>
* <span data-ttu-id="69bae-127">toolearn mer information om hur du grafiskt redigering finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).</span><span class="sxs-lookup"><span data-stu-id="69bae-127">toolearn more about Graphical Authoring, see [Graphical authoring in Azure Automation](automation-graphical-authoring-intro.md).</span></span>
