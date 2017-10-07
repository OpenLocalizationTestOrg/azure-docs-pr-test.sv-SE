---
title: "aaaCreate FQDN för en virtuell Windows-dator i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate ett fullständigt domännamn eller FQDN för Resource Manager baserad virtuell dator i hello Azure-portalen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a2ae5887-76df-485e-ae19-0efd96df8600
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 67c817ec97073803e513bc41ebde67b75ced565e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-windows-vm"></a>Skapa ett fullständigt kvalificerat domännamn i hello Azure-portalen för en virtuell Windows-dator

När du skapar en virtuell dator (VM) i hello [Azure-portalen](https://portal.azure.com), en offentlig IP-resurs för hello virtuell dator skapas automatiskt. Du kan använda den här IP-adressen tooremotely åtkomst hello VM. Även om hello portal inte skapar en [fullständigt kvalificerade domännamnet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), eller FQDN, kan du skapa en när hello VM har skapats. Den här artikeln visar hello steg toocreate ett DNS-namn eller FQDN.

## <a name="create-a-fqdn"></a>Skapa ett fullständigt domännamn
Den här artikeln förutsätter att du redan har skapat en virtuell dator. Om det behövs kan du [skapa en virtuell dator i hello portal](quick-create-portal.md) eller [med Azure PowerShell](quick-create-powershell.md). Följ dessa steg när den virtuella datorn är igång:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Du kan nu fjärransluta toohello VM som använder den här DNS-namn som för Remote Desktop Protocol (RDP).

## <a name="next-steps"></a>Nästa steg
Nu när den virtuella datorn har ett offentligt IP-adress och DNS-namn, kan du distribuera gemensamt ramverk för programmet eller tjänster som IIS, SQL och SharePoint.

Du kan också läsa mer om [med Resource Manager](../../azure-resource-manager/resource-group-overview.md) tips om hur du skapar dina Azure-distributioner.

