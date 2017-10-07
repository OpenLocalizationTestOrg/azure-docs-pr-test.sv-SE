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
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Ändra nyckelvalvsklient-ID efter en prenumerationsflytt
### <a name="q-my-subscription-was-moved-from-tenant-a-tootenant-b-how-do-i-change-hello-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>F: min prenumeration har flyttats från klient A tootenant B. Hur gör ändra hello klient-ID för Mina befintliga nyckelvalvet och ange rätt åtkomstkontrollistor för säkerhetsobjekt i B-klient?
När du skapar ett nytt nyckelvalv i en prenumeration är automatiskt bundet toohello standard Azure Active Directory klient-ID för den prenumerationen. Alla åtkomst Principposter är också bundet toothis klient-ID. När du flyttar din Azure-prenumeration från klient tootenant B, din befintliga nyckel valv inte tillgängliga som hello säkerhetsobjekt (användare och program) i klient B. toofix problemet måste du:

* Ändra hello klient-ID som är associerade med alla befintliga nyckelvalv i den här prenumerationen tootenant B.
* Ta bort alla åtkomstprincipposter.
* Lägga till nya åtkomstprincipposter som är associerade med klient B.

Till exempel om du har nyckelvalv 'myvault' i en prenumeration som har flyttats från klient A tootenant B, härs hur toochange hello klient-ID för den här nyckelvalvet och ta bort gamla åtkomstprinciper.

<pre>
$Select-AzureRmSubscription -SubscriptionId YourSubscriptionID
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Eftersom det här valvet var i klient A innan hello flytta, hello ursprungliga värde **$vault. Properties.TenantId** är klienten en stund **(Get-AzureRmContext). Tenant.TenantId** är klient B.

Nu när ditt valv är associerad med hello rätt klient-ID och gamla åtkomst Principposter tas bort, ställa in nya Principposter med [Set AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx).

## <a name="next-steps"></a>Nästa steg
Om du har frågor om Azure Key Vault finns hello [Azure Key Vault-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

