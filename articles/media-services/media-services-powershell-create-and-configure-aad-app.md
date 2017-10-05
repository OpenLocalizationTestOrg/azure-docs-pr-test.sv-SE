---
title: "Skapa en Azure AD-app för att komma åt Azure Media Services-API med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur du använder PowerShell för att skapa en app i Azure Active Directory (Azure AD) och konfigurera det åtkomst till Azure Media Services API."
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
ms.openlocfilehash: eea0f3a03dd77ce56484f32b192299bd97c05300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a><span data-ttu-id="7a4f5-103">Använda PowerShell för att skapa en Azure AD app ska använda med Azure Media Services-API</span><span class="sxs-lookup"><span data-stu-id="7a4f5-103">Use PowerShell to create an Azure AD app to use with the Azure Media Services API</span></span>

<span data-ttu-id="7a4f5-104">Lär dig hur du använder ett PowerShell-skript för att skapa ett Azure Active Directory (Azure AD)-program och tjänstens huvudnamn för att komma åt resurser i Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="7a4f5-104">Learn how to use a PowerShell script to create an Azure Active Directory (Azure AD) application and service principal to access Azure Media Services resources.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="7a4f5-105">Krav</span><span class="sxs-lookup"><span data-stu-id="7a4f5-105">Prerequisites</span></span>

- <span data-ttu-id="7a4f5-106">Ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7a4f5-106">An Azure account.</span></span> <span data-ttu-id="7a4f5-107">Om du inte har ett konto, börja med en [kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7a4f5-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="7a4f5-108">Ett Media Services-konto.</span><span class="sxs-lookup"><span data-stu-id="7a4f5-108">A Media Services account.</span></span> <span data-ttu-id="7a4f5-109">Mer information finns i [skapa ett Azure Media Services-konto i Azure portal](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="7a4f5-109">For more information, see [Create an Azure Media Services account in the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="7a4f5-110">Azure PowerShell version 0.8.8 eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="7a4f5-110">Azure PowerShell version 0.8.8 or a later version.</span></span> <span data-ttu-id="7a4f5-111">Mer information finns i [så använder du Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7a4f5-111">For more information, see [How to use Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
- <span data-ttu-id="7a4f5-112">Azure Resource Manager-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="7a4f5-112">Azure Resource Manager cmdlets.</span></span>  

## <a name="create-an-azure-ad-app-by-using-powershell"></a><span data-ttu-id="7a4f5-113">Skapa en Azure AD-app med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a4f5-113">Create an Azure AD app by using PowerShell</span></span>  

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
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

<span data-ttu-id="7a4f5-114">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="7a4f5-114">For more information, see the following articles:</span></span>

- [<span data-ttu-id="7a4f5-115">Använd Azure PowerShell för att skapa ett huvudnamn för tjänsten för resursåtkomst</span><span class="sxs-lookup"><span data-stu-id="7a4f5-115">Use Azure PowerShell to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [<span data-ttu-id="7a4f5-116">Hantera rollbaserad åtkomstkontroll med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a4f5-116">Manage Role-Based Access Control by using Azure PowerShell</span></span>](../active-directory/role-based-access-control-manage-access-powershell.md)
- [<span data-ttu-id="7a4f5-117">Hur du manuellt konfigurerar daemon appar med hjälp av certifikat</span><span class="sxs-lookup"><span data-stu-id="7a4f5-117">How to manually configure daemon apps by using certificates</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a><span data-ttu-id="7a4f5-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a4f5-118">Next steps</span></span>

<span data-ttu-id="7a4f5-119">Kom igång med [överföringen av filer till ditt konto](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="7a4f5-119">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
