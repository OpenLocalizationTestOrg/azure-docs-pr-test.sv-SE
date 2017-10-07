---
title: "begränsningar för aaaAzure moln Shell (förhandsversion) | Microsoft Docs"
description: "Översikt över begränsningar i Azure Cloud Shell"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: juluk
ms.openlocfilehash: 8462b0b9850fcde790a386433009439bbab52c0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="limitations-of-azure-cloud-shell"></a>Begränsningar i Azure-molnet Shell
Azure Cloud Shell har hello följande kända begränsningar:

## <a name="system-state-and-persistence"></a>Systemtillstånd och beständiga
hello-dator som innehåller molnet Shell sessionen är temporär och den återanvänds när sessionen har varit inaktiv i 20 minuter. Molnet Shell kräver en filresursen toobe monterade. Din prenumeration måste därför vara kan tooset in lagring resurser tooaccess moln Shell. Andra överväganden omfattar:
* Med monterade storage kan endast ändringar i din `$Home` directory eller `clouddrive` directory sparas.
* Filresurser kan monteras endast från din [tilldelade region](persisting-shell-storage.md#mount-a-new-clouddrive).
* Azure Files stöder endast lokalt redundant lagring och konton för geo-redundant lagring.

## <a name="user-permissions"></a>Användarbehörigheter
Behörigheterna anges som en vanlig användare utan åtkomst till sudo. En installation utanför din `$Home` katalog kommer inte att spara.
Även om vissa kommandon i hello `clouddrive` katalogen som `git clone`, har inte rätt behörighet din `$Home` directory har behörighet.

## <a name="browser-support"></a>Stöd för webbläsare
Molnet stödjer hello senaste versionerna av Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox och Apple Safari. Safari i privat läge stöds inte.

## <a name="copy-and-paste"></a>Kopiera och klistra in
CTRL + C och Ctrl + V fungerar inte som att kopiera och klistra in genvägar i molnet Shell på Windows-datorer kan använda Ctrl + Insert och SKIFT + Insert toocopy och klistra in respektive.

Högerklicka på Kopiera och klistra in alternativ är också tillgängliga, men genom att högerklicka på funktion är ämne toobrowser-specifika Urklipp åtkomst.

## <a name="editing-bashrc"></a>Redigera .bashrc
Vara försiktig när du redigerar .bashrc, gör det kan orsaka oväntade fel i moln Shell.

## <a name="bashhistory"></a>.bash_history
Historik för bash kommandon kan vara inkonsekvent på grund av avbrott i molnet Shell session eller samtidiga sessioner.

## <a name="usage-limits"></a>Gränserna för Resursanvändning
Moln-gränssnittet är avsedd för interaktiva användningsfall. Därför kan avslutas alla icke-interaktiv tidskrävande sessioner utan varning.

## <a name="network-connectivity"></a>Nätverksanslutning
En fördröjning i molnet Shell ämne toolocal Internetanslutning, molnet Shell fortsätter tooattempt toocarry ut instruktionerna som skickas.

## <a name="next-steps"></a>Nästa steg
[Molnet Shell Snabbstart](quickstart.md)
