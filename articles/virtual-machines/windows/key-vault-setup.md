---
title: "aaaSet in Key Vault för virtuella Windows-datorer i Azure Resource Manager | Microsoft Docs"
description: "Hur tooset in Key Vault för användning med en virtuell dator i Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: 53bbe90708202ecfdcf754822d04bf2469631f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>Ställ in Key Vault för virtuella datorer i Azure Resource Manager

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

I Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av hello resursprovidern för Nyckelvalvet. toolearn mer om Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. För Key Vault toobe användas med Azure Resource Manager virtuella datorer, hello **EnabledForDeployment** egenskapen i Nyckelvalvet måste anges tootrue. Du kan göra detta i olika klienter.
> 2. hello Key Vault behov toobe skapas i hello samma prenumeration och plats som hello virtuell dator.
>
>

## <a name="use-powershell-tooset-up-key-vault"></a>Använd PowerShell tooset in Key Vault
toocreate nyckelvalv med hjälp av PowerShell, se [Kom igång med Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).

Du kan använda följande PowerShell-cmdlet för nytt nyckelvalv:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Du kan använda följande PowerShell-cmdlet för befintliga nyckelvalv:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-tooset-up-key-vault"></a>Oss CLI tooset in Key Vault
toocreate nyckelvalv med hjälp av hello-kommandoradsgränssnittet (CLI), se [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

CLI har du toocreate hello nyckelvalv innan du tilldelar hello princip för programdistribution. Du kan göra detta med hjälp av hello följande kommando:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a>Använda mallar tooset in Key Vault
När du använder en mall måste tooset hello `enabledForDeployment` egenskapen för`true` för hello Key Vault-resurs.

    {
      "type": "Microsoft.KeyVault/vaults",
      "name": "ContosoKeyVault",
      "apiVersion": "2015-06-01",
      "location": "<location-of-key-vault>",
      "properties": {
        "enabledForDeployment": "true",
        ....
        ....
      }
    }

Andra alternativ som du kan konfigurera när du skapar ett nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
