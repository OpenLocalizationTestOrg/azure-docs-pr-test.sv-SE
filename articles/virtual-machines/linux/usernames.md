---
title: "aaaSelecting användarnamn för Linux | Microsoft Docs"
description: "Lär dig hur tooselect användaren namn för en virtuell Linux-dator i Azure."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: c65e2ac46f40bb8c9d74cccbaf248a070c0fa6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a>Välj användarnamn för Linux på Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

När du etablerar en Linux-dator på Azure måste du ange hello namnet på en icke-rotanvändare som du senare kan använda toolog i hello VM. Du kan välja hello namn för den nya användaren i hello eller om etablering hello klassiska Azure-portalen kan accepteras hello standard namnet ”azureuser”.

I de flesta fall den här användaren finns inte på hello basavbildning och kommer att skapas under hello etableringsprocessen. Om hello användaren finns på hello VM basavbildning, konfigurerar hello Azure Linux-agenten bara hello lösenord och/eller SSH-nyckeln för den användare som är baserade på hello information som du angav när du skapar hello VM.

**Men**, Linux definierar en uppsättning användarnamn som inte ska användas. hello etablering processen kommer **misslyckas** om du försöker tooprovision en Linux VM som använder en befintlig systemanvändare definieras som en användare med UID 0-99. Ett typiskt exempel är hello `root` användare som har UID 0.

* Se även: [Linux Standard Base - användar-ID-intervall](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)

hello följer en lista över vanliga inbyggda systemanvändare för CentOS och Ubuntu att bör du undvika att använda vid etablering av en virtuell Linux-dator på Azure. Den här listan är bara ett exempel, se toohello dokumentationen för din distribution tooensure som hello användarnamn som du väljer inte står i konflikt med en befintlig systemanvändare.

## <a name="centos"></a>CentOS
* abrt
* ADM
* ljud
* bin
* CDROM
* cgred
* Daemon
* dbus
* antingen
* DIP
* disk
* diskettenheter
* FTP
* FTP
* spel
* Gopher
* haldaemon
* Stoppa
* kmem
* Lås
* LP
* E-post
* man
* med
* nfsnobody
* Ingen
* NTP
* Operatorn
* oprofile
* postdrop
* UserName@Domain
* qpidd
* rot
* RPC
* rpcuser
* saslauth
* avstängning
* slocate
* sshd
* stapdev
* stapusr
* Synkronisering
* sys
* band
* Test
* tcpdump
* TTY
* användare
* utempter
* utmp
* UUCP
* vcsa
* video
* hjul

## <a name="ubuntu"></a>Ubuntu
* ADM
* Admin
* ljud
* säkerhetskopiering
* bin
* CDROM
* crontab
* Daemon
* antingen
* DIP
* disk
* Fax
* diskettenheter
* säkrad
* spel
* gnats
* IRC
* kmem
* liggande
* libuuid
* lista
* LP
* E-post
* man
* MessageBus
* mlocate
* netdev
* Nyheter
* Ingen
* nogroup
* Operatorn
* plugdev
* Proxy
* rot
* SASL
* skugga
* src
* SSH
* sshd
* Personal
* sudo
* Synkronisering
* sys
* syslog
* band
* TTY
* användare
* utmp
* UUCP
* video
* röst
* whoopsie
* www-data

