---
title: "Använda rotprivilegier på Linux-datorer | Microsoft Docs"
description: "Lär dig hur du använder rotprivilegier på en Linux-dator i Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: a2c106a2-dceb-43a3-9dd1-50ed77685952
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: dc39db1f5fecffb60499a5420bfe72850e2fffd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Använd rotprivilegier på virtuella Linux-datorer i Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Som standard den `root` användaren är inaktiverad på Linux-datorer i Azure. Användare kan köra kommandon med utökade privilegier med hjälp av den `sudo` kommando. Upplevelsen kan dock variera beroende på hur systemet har etablerats.

1. **SSH-nyckel och lösenord eller lösenord endast** -den virtuella datorn har etablerats med antingen ett certifikat (`.CER` fil) eller SSH-nyckeln som ett lösenord, eller bara ett användarnamn och lösenord. I det här fallet `sudo` ombeds du att lösenordet innan kommandot körs.
2. **SSH-nyckeln** -den virtuella datorn har etablerats med ett certifikat (`.cer`, `.pem`, eller `.pub` filen) eller SSH-nyckel, men inget lösenord.  I det här fallet `sudo` **inte** fråga efter användarens lösenord innan kommandot körs.

## <a name="ssh-key-and-password-or-password-only"></a>SSH-nyckel och lösenord eller lösenord endast
Logga in på Linux virtuella datorer som använder SSH-nyckeln eller lösenordet autentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:

    # sudo <command>
    [sudo] password for azureuser:

I det här fallet uppmanas användaren att ange ett lösenord. När du har angett lösenordet `sudo` körs kommandot med `root` privilegier.

Du kan också aktivera passwordless sudo genom att redigera den `/etc/sudoers.d/waagent` -fil, till exempel:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Detta innebär att för passwordless sudo av användaren ”azureuser”.

## <a name="ssh-key-only"></a>SSH nyckeln endast
Logga in på Linux virtuella datorer som använder SSH-nyckelautentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:

    # sudo <command>

I det här fallet kommer användaren **inte** uppmanas att ange ett lösenord. När du trycker på `<enter>`, `sudo` körs kommandot med `root` privilegier.

