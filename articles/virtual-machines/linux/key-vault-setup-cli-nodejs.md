---
title: "aaaSet in Key Vault för Linux virtuella datorer med hello Azure CLI 1.0 | Microsoft Docs"
description: "Hur hello tooset in Key Vault för användning med en virtuell dator i Azure Resource Manager med Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 275022e4e7e26d7363784c289dd7512047c07bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-hello-azure-cli-10"></a>Ställ in Key Vault för virtuella datorer i Azure Resource Manager med hello Azure CLI 1.0
I hello Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av hello resursprovidern för Nyckelvalvet. toolearn mer om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md) För Key Vault toobe användas med Azure Resource Manager virtuella datorer, hello *EnabledForDeployment* egenskapen i Nyckelvalvet måste anges tootrue. Du kan göra detta i olika klienter. Den här artikeln beskrivs hur du tooset in Key Vault för användning med Azure Virtual Machines.

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="use-cli-10-tooset-up-key-vault"></a>Använda CLI 1.0 tooset in Key Vault
toocreate nyckelvalv med hjälp av hello-kommandoradsgränssnittet (CLI), se [hantera Key Vault med hjälp av CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

CLI version 1.0 har du toocreate hello nyckelvalv innan du tilldelar hello princip för programdistribution. Du kan sedan tilldela hello-princip med hello följande kommando:

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
