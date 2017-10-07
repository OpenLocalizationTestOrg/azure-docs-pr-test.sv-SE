---
title: "aaaChange hello nyckelvalv klient-ID när en prenumeration flyttar | Microsoft Docs"
description: "Lär dig hur tooswitch hello klient-ID för nyckelvalvet när en prenumeration har flyttats tooa olika klient"
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 46d7bc21-fa79-49e4-8c84-032eef1d813e
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: 4d0607208c61c57959439d2d0bd8feade4141fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a><span data-ttu-id="5c866-103">Ändra nyckelvalvsklient-ID efter en prenumerationsflytt</span><span class="sxs-lookup"><span data-stu-id="5c866-103">Change a key vault tenant ID after a subscription move</span></span>
### <a name="q-my-subscription-was-moved-from-tenant-a-tootenant-b-how-do-i-change-hello-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a><span data-ttu-id="5c866-104">F: min prenumeration har flyttats från klient A tootenant B. Hur gör ändra hello klient-ID för Mina befintliga nyckelvalvet och ange rätt åtkomstkontrollistor för säkerhetsobjekt i B-klient?</span><span class="sxs-lookup"><span data-stu-id="5c866-104">Q: My subscription was moved from tenant A tootenant B. How do I change hello tenant ID for my existing key vault and set correct ACLs for principals in tenant B?</span></span>
<span data-ttu-id="5c866-105">När du skapar ett nytt nyckelvalv i en prenumeration är automatiskt bundet toohello standard Azure Active Directory klient-ID för den prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="5c866-105">When you create a new key vault in a subscription, it is automatically tied toohello default Azure Active Directory tenant ID for that subscription.</span></span> <span data-ttu-id="5c866-106">Alla åtkomst Principposter är också bundet toothis klient-ID.</span><span class="sxs-lookup"><span data-stu-id="5c866-106">All access policy entries are also tied toothis tenant ID.</span></span> <span data-ttu-id="5c866-107">När du flyttar din Azure-prenumeration från klient tootenant B, din befintliga nyckel valv inte tillgängliga som hello säkerhetsobjekt (användare och program) i klient B. toofix problemet måste du:</span><span class="sxs-lookup"><span data-stu-id="5c866-107">When you move your Azure subscription from tenant A tootenant B, your existing key vaults are inaccessible by hello principals (users and applications) in tenant B. toofix this issue, you need to:</span></span>

* <span data-ttu-id="5c866-108">Ändra hello klient-ID som är associerade med alla befintliga nyckelvalv i den här prenumerationen tootenant B.</span><span class="sxs-lookup"><span data-stu-id="5c866-108">Change hello tenant ID associated with all existing key vaults in this subscription tootenant B.</span></span>
* <span data-ttu-id="5c866-109">Ta bort alla åtkomstprincipposter.</span><span class="sxs-lookup"><span data-stu-id="5c866-109">Remove all existing access policy entries.</span></span>
* <span data-ttu-id="5c866-110">Lägga till nya åtkomstprincipposter som är associerade med klient B.</span><span class="sxs-lookup"><span data-stu-id="5c866-110">Add new access policy entries that are associated with tenant B.</span></span>

<span data-ttu-id="5c866-111">Till exempel om du har nyckelvalv 'myvault' i en prenumeration som har flyttats från klient A tootenant B, härs hur toochange hello klient-ID för den här nyckelvalvet och ta bort gamla åtkomstprinciper.</span><span class="sxs-lookup"><span data-stu-id="5c866-111">For example, if you have key vault 'myvault' in a subscription that has been moved from tenant A tootenant B, here's how toochange hello tenant ID for this key vault and remove old access policies.</span></span>

<pre>
$Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

<span data-ttu-id="5c866-112">Eftersom det här valvet var i klient A innan hello flytta, hello ursprungliga värde **$vault. Properties.TenantId** är klienten en stund **(Get-AzureRmContext). Tenant.TenantId** är klient B.</span><span class="sxs-lookup"><span data-stu-id="5c866-112">Because this vault was in tenant A before hello move, hello original value of **$vault.Properties.TenantId** is tenant A, while **(Get-AzureRmContext).Tenant.TenantId** is tenant B.</span></span>

<span data-ttu-id="5c866-113">Nu när ditt valv är associerad med hello rätt klient-ID och gamla åtkomst Principposter tas bort, ställa in nya Principposter med [Set AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).</span><span class="sxs-lookup"><span data-stu-id="5c866-113">Now that your vault is associated with hello correct tenant ID and old access policy entries are removed, set new access policy entries with [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c866-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5c866-114">Next steps</span></span>
<span data-ttu-id="5c866-115">Om du har frågor om Azure Key Vault finns hello [Azure Key Vault-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).</span><span class="sxs-lookup"><span data-stu-id="5c866-115">If you have questions about Azure Key Vault, visit hello [Azure Key Vault Forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).</span></span>

