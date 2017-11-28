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
# <a name="use-powershell-toocreate-an-azure-ad-app-toouse-with-hello-azure-media-services-api"></a><span data-ttu-id="60613-103">Använda PowerShell toocreate toouse en Azure AD-app med hello Azure Media Services API</span><span class="sxs-lookup"><span data-stu-id="60613-103">Use PowerShell toocreate an Azure AD app toouse with hello Azure Media Services API</span></span>

<span data-ttu-id="60613-104">Lär dig hur toouse ett PowerShell-skript toocreate ett Azure Active Directory (Azure AD)-program och tjänstens huvudnamn tooaccess Azure Media Services-resurser.</span><span class="sxs-lookup"><span data-stu-id="60613-104">Learn how toouse a PowerShell script toocreate an Azure Active Directory (Azure AD) application and service principal tooaccess Azure Media Services resources.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="60613-105">Krav</span><span class="sxs-lookup"><span data-stu-id="60613-105">Prerequisites</span></span>

- <span data-ttu-id="60613-106">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="60613-106">An Azure account.</span></span> <span data-ttu-id="60613-107">Om du inte har ett konto, börja med en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="60613-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="60613-108">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="60613-108">A Media Services account.</span></span> <span data-ttu-id="60613-109">Mer information finns i [skapa ett Azure Media Services-konto i hello Azure-portalen](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="60613-109">For more information, see [Create an Azure Media Services account in hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="60613-110">Azure PowerShell version 0.8.8 eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="60613-110">Azure PowerShell version 0.8.8 or a later version.</span></span> <span data-ttu-id="60613-111">Mer information finns i [hur toouse Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="60613-111">For more information, see [How toouse Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
- <span data-ttu-id="60613-112">Azure Resource Manager-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="60613-112">Azure Resource Manager cmdlets.</span></span>  

## <a name="create-an-azure-ad-app-by-using-powershell"></a><span data-ttu-id="60613-113">Skapa en Azure AD-app med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="60613-113">Create an Azure AD app by using PowerShell</span></span>  

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

<span data-ttu-id="60613-114">Mer information finns i följande artiklar hello:</span><span class="sxs-lookup"><span data-stu-id="60613-114">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="60613-115">Använda Azure PowerShell toocreate en service principal tooaccess resurser</span><span class="sxs-lookup"><span data-stu-id="60613-115">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [<span data-ttu-id="60613-116">Hantera rollbaserad åtkomstkontroll med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="60613-116">Manage Role-Based Access Control by using Azure PowerShell</span></span>](../active-directory/role-based-access-control-manage-access-powershell.md)
- [<span data-ttu-id="60613-117">Hur toomanually konfigurera daemon appar med hjälp av certifikat</span><span class="sxs-lookup"><span data-stu-id="60613-117">How toomanually configure daemon apps by using certificates</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a><span data-ttu-id="60613-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60613-118">Next steps</span></span>

<span data-ttu-id="60613-119">Kom igång med [Överför filer tooyour konto](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="60613-119">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
