---
title: aaaUse PowerShell toocreate en Azure AD app tooaccess hello Azure Media Services API | Microsoft Docs
description: "Lär dig hur toouse PowerShell toocreate en app i Azure Active Directory (Azure AD) och konfigurera tooaccess hello Azure Media Services API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 1a8b4a53ad10b559f6ee4242b95c5bd15ee8e903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-ad-app-toouse-with-hello-azure-media-services-api"></a>Använda PowerShell toocreate toouse en Azure AD-app med hello Azure Media Services API

Lär dig hur toouse ett PowerShell-skript toocreate ett Azure Active Directory (Azure AD)-program och tjänstens huvudnamn tooaccess Azure Media Services-resurser.  

## <a name="prerequisites"></a>Krav

- Ett Azure-konto. Om du inte har ett konto, börja med en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Ett Media Services-konto. Mer information finns i [skapa ett Azure Media Services-konto i hello Azure-portalen](media-services-portal-create-account.md).
- Azure PowerShell version 0.8.8 eller en senare version. Mer information finns i [hur toouse Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
- Azure Resource Manager-cmdlets.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Skapa en Azure AD-app med hjälp av PowerShell  

```powershell
Login-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Mer information finns i följande artiklar hello:

- [Använda Azure PowerShell toocreate en service principal tooaccess resurser](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [Hantera rollbaserad åtkomstkontroll med hjälp av Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
- [Hur toomanually konfigurera daemon appar med hjälp av certifikat](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Nästa steg

Kom igång med [Överför filer tooyour konto](media-services-portal-upload-files.md).
