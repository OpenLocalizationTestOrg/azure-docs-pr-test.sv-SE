---
title: "aaaUse rotprivilegier på Linux-datorer | Microsoft Docs"
description: "Lär dig hur toouse rot behörighet på en virtuell Linux-dator i Azure."
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
ms.openlocfilehash: 9411588c5fd0c86c4c73b3e44fbb56ab150013d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-root-privileges-on-linux-virtual-machines-in-azure"></a>Använd rotprivilegier på virtuella Linux-datorer i Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Som standard hello `root` användaren är inaktiverad på Linux-datorer i Azure. Användare kan köra kommandon med utökade privilegier med hjälp av hello `sudo` kommando. Hello upplevelse kan dock variera beroende på hur hello systemet har etablerats.

1. **SSH-nyckel och lösenord eller lösenord endast** -hello virtuella datorn har etablerats med antingen ett certifikat (`.CER` fil) eller SSH-nyckeln som ett lösenord, eller bara ett användarnamn och lösenord. I det här fallet `sudo` ombeds du att hello användarens lösenord innan hello kommando körs.
2. **SSH-nyckeln** -hello virtuella datorn har etablerats med ett certifikat (`.cer`, `.pem`, eller `.pub` filen) eller SSH-nyckel, men inget lösenord.  I det här fallet `sudo` **inte** fråga efter hello användarens lösenord innan hello kommando körs.

## <a name="ssh-key-and-password-or-password-only"></a>SSH-nyckel och lösenord eller lösenord endast
Logga in på hello virtuell Linux-dator med hjälp av SSH-nyckeln eller lösenordet autentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:

    # sudo <command>
    [sudo] password for azureuser:

I det här fallet uppmanas hello användaren att ange ett lösenord. När du har angett hello lösenord `sudo` körs hello-kommando med `root` privilegier.

Du kan också aktivera passwordless sudo genom att redigera hello `/etc/sudoers.d/waagent` fil, till exempel:

    #/etc/sudoers.d/waagent
    azureuser ALL = (ALL) NOPASSWD: ALL

Detta innebär att för passwordless sudo av hello användare ”azureuser”.

## <a name="ssh-key-only"></a>SSH nyckeln endast
Logga in på hello virtuell Linux-dator med hjälp av SSH-nyckelautentisering och sedan köra kommandon med hjälp av `sudo`, till exempel:

    # sudo <command>

I det här fallet hello användaren kommer **inte** uppmanas att ange ett lösenord. När du trycker på `<enter>`, `sudo` körs hello-kommando med `root` privilegier.

