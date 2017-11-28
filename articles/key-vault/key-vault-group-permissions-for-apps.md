---
title: "aaaGrant behörighet toomany program tooaccess ett Azure key vault | Microsoft Docs"
description: "Lär dig hur toogrant behörighet toomany program tooaccess en nyckel valvet"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 5258149f939856f91b3848fc50399e58e5894f0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="grant-permission-toomany-applications-tooaccess-a-key-vault"></a><span data-ttu-id="623ba-103">Bevilja behörighet toomany program tooaccess ett nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="623ba-103">Grant permission toomany applications tooaccess a key vault</span></span>

## <a name="q-i-have-several-over-16-applications-that-need-tooaccess-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a><span data-ttu-id="623ba-104">F: Jag har flera (över 16) program som behöver tooaccess en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="623ba-104">Q: I have several (over 16) applications that need tooaccess a key vault.</span></span> <span data-ttu-id="623ba-105">Eftersom Key Vault kan bara 16 åtkomstkontrollposter, hur kan jag få som?</span><span class="sxs-lookup"><span data-stu-id="623ba-105">Since Key Vault only allows 16 access control entries, how can I achieve that?</span></span>

<span data-ttu-id="623ba-106">Key Vault principer för åtkomstkontroll har endast stöd för 16-poster.</span><span class="sxs-lookup"><span data-stu-id="623ba-106">Key Vault access control policy only supports 16 entries.</span></span> <span data-ttu-id="623ba-107">Du kan dock skapa en Azure Active Directory-säkerhetsgrupp.</span><span class="sxs-lookup"><span data-stu-id="623ba-107">However you can create an Azure Active Directory security group.</span></span> <span data-ttu-id="623ba-108">Lägg till alla hello associerade tjänsten säkerhetsobjekt toothis säkerhetsgrupp och sedan ge åtkomst toothis säkerhet grupp tooKey valvet.</span><span class="sxs-lookup"><span data-stu-id="623ba-108">Add all hello associated service principals toothis security group and then grant access toothis security group tooKey Vault.</span></span>

<span data-ttu-id="623ba-109">Här följer hello förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="623ba-109">Here are hello pre-requisites:</span></span>
* <span data-ttu-id="623ba-110">[Installera Azure Active Directory PowerShell V2-modulen](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span><span class="sxs-lookup"><span data-stu-id="623ba-110">[Install Azure Active Directory V2 PowerShell module](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).</span></span>
* <span data-ttu-id="623ba-111">[Installera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="623ba-111">[Install Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="623ba-112">toorun hello följande kommandon du behöver behörigheter toocreate och redigera grupper i hello Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="623ba-112">toorun hello following commands, you need permissions toocreate/edit groups in hello Azure Active Directory tenant.</span></span> <span data-ttu-id="623ba-113">Om du inte har behörighet, kanske du måste toocontact Azure Active Directory-administratör.</span><span class="sxs-lookup"><span data-stu-id="623ba-113">If you don't have permissions, you may need toocontact your Azure Active Directory administrator.</span></span>

<span data-ttu-id="623ba-114">Kör nu hello följande kommandon i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="623ba-114">Now run hello following commands in PowerShell.</span></span>

```powershell
# Connect tooAzure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members toothis group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members toothis group, in this fashion. 
 
# Set hello Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust hello permissions as required 
```

<span data-ttu-id="623ba-115">Om du behöver toogrant en annan uppsättning behörigheter tooa grupp av program kan du skapa en separat Azure Active Directory-säkerhetsgrupp för dessa program.</span><span class="sxs-lookup"><span data-stu-id="623ba-115">If you need toogrant a different set of permissions tooa group of applications, create a separate Azure Active Directory security group for such applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="623ba-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="623ba-116">Next steps</span></span>

<span data-ttu-id="623ba-117">Läs mer om hur för[Secure nyckelvalvet](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="623ba-117">Learn more about how too[Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>
