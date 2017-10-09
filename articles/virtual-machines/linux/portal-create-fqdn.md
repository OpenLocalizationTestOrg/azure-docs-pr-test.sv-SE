---
title: "aaaCreate FQDN för en Linux-VM i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate ett fullständigt domännamn eller FQDN för Resource Manager baserad virtuell dator i hello Azure-portalen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2cd6c249-a737-4a0a-b5ba-e1c09e551b30
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1494a0cb1caa62069c72096a739aee111ac8b383
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-qualified-domain-name-in-hello-azure-portal-for-a-linux-vm"></a>Skapa ett fullständigt kvalificerat domännamn i hello Azure-portalen för en Linux-VM

När du skapar en virtuell dator (VM) i hello [Azure-portalen](https://portal.azure.com), en offentlig IP-resurs för hello virtuell dator skapas automatiskt. Du kan använda den här IP-adressen tooremotely åtkomst hello VM. Även om hello portal inte skapar en [fullständigt kvalificerade domännamnet](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), eller FQDN, du kan lägga till en när hello VM har skapats. Den här artikeln visar hello steg toocreate ett DNS-namn eller FQDN.

## <a name="create-a-fqdn"></a>Skapa ett fullständigt domännamn
Den här artikeln förutsätter att du redan har skapat en virtuell dator. Om det behövs kan du [skapa en virtuell dator i hello portal](quick-create-portal.md) eller [med hello Azure CLI](quick-create-cli.md). Följ dessa steg när den virtuella datorn är igång:

[!INCLUDE [virtual-machines-common-portal-create-fqdn](../../../includes/virtual-machines-common-portal-create-fqdn.md)]

Du kan nu fjärransluta toohello VM som använder den här DNS-namn som med `ssh azureuser@mydns.westus.cloudapp.azure.com`.

## <a name="next-steps"></a>Nästa steg
Nu när den virtuella datorn har ett offentligt IP-adress och DNS-namn, du kan distribuera gemensamt ramverk för programmet eller tjänster, till exempel nginx, MongoDB, Docker, osv.

Du kan också läsa mer om [med Resource Manager](../../azure-resource-manager/resource-group-overview.md) tips om hur du skapar dina Azure-distributioner.

