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
# <a name="grant-permission-toomany-applications-tooaccess-a-key-vault"></a>Bevilja behörighet toomany program tooaccess ett nyckelvalv

## <a name="q-i-have-several-over-16-applications-that-need-tooaccess-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>F: Jag har flera (över 16) program som behöver tooaccess en nyckelvalvet. Eftersom Key Vault kan bara 16 åtkomstkontrollposter, hur kan jag få som?

Key Vault principer för åtkomstkontroll har endast stöd för 16-poster. Du kan dock skapa en Azure Active Directory-säkerhetsgrupp. Lägg till alla hello associerade tjänsten säkerhetsobjekt toothis säkerhetsgrupp och sedan ge åtkomst toothis säkerhet grupp tooKey valvet.

Här följer hello förutsättningar:
* [Installera Azure Active Directory PowerShell V2-modulen](https://www.powershellgallery.com/packages/AzureAD/2.0.0.30).
* [Installera Azure PowerShell](/powershell/azure/overview).
* toorun hello följande kommandon du behöver behörigheter toocreate och redigera grupper i hello Azure Active Directory-klient. Om du inte har behörighet, kanske du måste toocontact Azure Active Directory-administratör.

Kör nu hello följande kommandon i PowerShell.

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

Om du behöver toogrant en annan uppsättning behörigheter tooa grupp av program kan du skapa en separat Azure Active Directory-säkerhetsgrupp för dessa program.

## <a name="next-steps"></a>Nästa steg

Läs mer om hur för[Secure nyckelvalvet](key-vault-secure-your-key-vault.md).
