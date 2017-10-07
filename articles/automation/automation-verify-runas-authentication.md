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
# <a name="test-azure-automation-run-as-account-authentication"></a>Testa Kör som-kontoautentisering för Azure Automation
När ett Automation-konto har skapats, du kan utföra en enkel test tooconfirm kan toosuccessfully autentiseras i Azure Resource Manager eller Azure klassisk distribution med ditt nyligen skapats eller uppdaterats Automation kör som-konto.    

## <a name="automation-run-as-authentication"></a>Automation Kör som-autentisering
Använd hello exempelkoden nedan för[skapa en PowerShell-runbook](automation-creating-importing-runbook.md) tooverify autentisering med hjälp av hello kör som-konto och i din anpassade runbooks tooauthenticate och hantera Resource Manager-resurser med ditt Automation-konto.   

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

Lägg märke till hello cmdlet som används för att autentisera i hello runbook - **Add-AzureRmAccount**, använder hello *ServicePrincipalCertificate* parameteruppsättning.  Den autentiserar med hjälp av tjänstobjektets certifikat, inte autentiseringsuppgifter.  

När du [köra hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate din kör som-konto en [runbook-jobbet](automation-runbook-execution.md) har skapats hello jobbet bladet visas och hello jobbstatus visas i hello **jobbsammanfattning**panelen. hello jobbstatus startar som *i kö* som anger att den väntar på en runbook worker i hello molnet toobecome tillgängliga. Sedan flyttas för*Start* när en arbetsprocess anspråk hello jobb, och sedan *kör* när hello runbook faktiskt börjar köras.  När hello runbook-jobbet är slutfört, vi bör du se statusen **slutförd**.

toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.  På hello **utdata** bladet bör du se den har autentiserats och returnerar en lista över alla resurser i alla resursgrupper i din prenumeration.  

Men kom ihåg tooremove hello kodblock börjar med hello kommentar `#Get all ARM resources from all resource groups` när du återanvända hello kod för dina runbooks.

## <a name="classic-run-as-authentication"></a>Klassisk Kör som-autentisering
Använd hello exempelkoden nedan för[skapa en PowerShell-runbook](automation-creating-importing-runbook.md) tooverify autentisering med hello klassisk kör som-konto och i din anpassade runbooks tooauthenticate och hantera resurser i hello klassiska distributionsmodellen.  

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

När du [köra hello runbook](automation-starting-a-runbook.md#starting-a-runbook-with-the-azure-portal) toovalidate din kör som-konto en [runbook-jobbet](automation-runbook-execution.md) har skapats hello jobbet bladet visas och hello jobbstatus visas i hello **jobbsammanfattning**panelen. hello jobbstatus startar som *i kö* som anger att den väntar på en runbook worker i hello molnet toobecome tillgängliga. Sedan flyttas för*Start* när en arbetsprocess anspråk hello jobb, och sedan *kör* när hello runbook faktiskt börjar köras.  När hello runbook-jobbet är slutfört, vi bör du se statusen **slutförd**.

toosee Hej detaljerade resultat för hello runbook, klicka på hello **utdata** panelen.  På hello **utdata** bladet bör du se den har autentiserats och returnerar en lista över alla virtuella Azure-datorer med VMName som har distribuerats i din prenumeration.  

Men kom ihåg tooremove hello cmdlet **Get-AzureVM** när du återanvända hello kod för dina runbooks.

## <a name="next-steps"></a>Nästa steg
* tooget igång med PowerShell-runbooks, se [min första PowerShell-runbook](automation-first-runbook-textual-powershell.md).
* toolearn mer information om hur du grafiskt redigering finns [grafiska redigering i Azure Automation](automation-graphical-authoring-intro.md).
