---
title: aaaGet ett Azure Linux VM-ID | Microsoft Docs
description: "Beskriver hur tooget och använda en Azure Linux VM unika ID: t."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 136c5d28-ff6b-4466-b27f-7a29785b5d27
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/23/2017
ms.author: kmouss
ms.openlocfilehash: 4c8ddfc2e892824581e77649285ee8adbccd5def
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a>Visa och använda virtuella Azure-datorns unika ID: T
Azure VM unika ID: T är en 128-identifierare kodade och lagras i alla Azure IaaS Virtuella datorns SMBIOS och för närvarande kan läsas med hjälp av plattform BIOS-kommandon.

Azure VM unika ID: T är en skrivskyddad egenskap. Azure unikt ID för virtuell dator ändras inte vid omstart avstängning (antingen den planerade för oplanerad), starta eller stoppa Frigör, tjänsten återställning eller Återställ på plats. Om hello VM är en ögonblicksbild och kopierade toocreate en ny instans, konfigureras nya Azure VM-ID.

> [!NOTE]
> Om du har äldre virtuella datorer skapas och kör sedan denna nya funktion har distribuerat (18 September 2014), starta om VM-tooautomatically hämta ett Azure unikt ID.
> 
> 

tooaccess Azure unika VM-ID från inom hello VM:

## <a name="create-a-vm"></a>Skapa en virtuell dator
Mer information finns i [skapa en virtuell dator](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="connect-toohello-vm"></a>Ansluta toohello VM
Mer information finns i [SSH från Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="query-vm-unique-id"></a>Unikt ID för frågan VM
Kommandot (exempel används **Ubuntu**):

```bash
sudo dmidecode | grep UUID
```

Exempel förväntat resultat:

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

På grund av tooBig Endian bitar ordning, kommer hello faktiska unikt ID för virtuell dator i det här fallet att:

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

Azure VM unika ID: T kan användas i olika scenarier om hello VM körs på Azure eller lokalt och kan hjälpa dina licenser, reporting eller allmänna spårning krav i Azure IaaS-distributioner kan vara. Många oberoende programvaruleverantörer bygga program och certifierar dem på Azure kan kräva tooidentify en Azure VM under hela dess livscykel och tootell om hello VM körs på Azure, lokala eller andra molntjänstleverantörer av. Den här plattformen identifieraren kan till exempel att identifiera om hello programvaran licensieras korrekt eller att toocorrelate alla VM-tooits datakällor, till exempel tooassist på inställningen hello rätt mått för hello rätt plattform och tootrack och korrelera de här måtten bland andra använder.

