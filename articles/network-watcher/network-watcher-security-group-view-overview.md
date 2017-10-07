---
title: "aaaIntroduction toosecurity gruppvyn i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Nätverksbevakaren säkerhet visa kapaciteten"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: ad27ab85-9d84-4759-b2b9-e861ef8ea8d8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: gwallace
ms.openlocfilehash: c2f6dbbffd0aedbb9db4b69d1758f2e66dd7abb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonetwork-security-group-view-in-azure-network-watcher"></a>Vyn av grupp om introduktion toonetwork i Azure Nätverksbevakaren

Nätverkssäkerhetsgrupper associeras på en undernätverksnivå eller på en NIC-nivå. När associerade på en undernätverksnivå, tillämpas tooall hello VM-instanser i hello undernät. Nätverkssäkerhetsgruppen vyn returnerar alla hello konfigurerats NSG: er och regler som är associerade på ett nätverkskort och undernät nivå för en virtuell dator som ger inblick i hello konfiguration. Dessutom returneras hello effektiva säkerhetsregler för varje hello nätverkskort i en virtuell dator. Med hjälp av Nätverkssäkerhetsgruppen vyn du utvärdera en virtuell dator för säkerhetsrisker i nätverket, till exempel öppna portar. Du kan också verifiera om säkerhetsgrupp för nätverk fungerar som förväntat baserat på en [jämförelse mellan hello konfigurerad och hello effektiva säkerhetsregler](network-watcher-nsg-auditing-powershell.md).

Det är ett mer utökade användningsfall i säkerhetsefterlevnaden och granskning. Du kan definiera en normativ säkerhetsregler som en modell för styrning av säkerhet i din organisation. En periodisk efterlevnad audit kan implementeras på programmässiga sätt genom att jämföra hello normativ regler med hello effektiva regler för varje hello virtuella datorer i nätverket.

I hello delas portal regler som gäller, undernät, nätverksgränssnittet och standard. Detta ger en enkel vy till hello reglerna tooa virtuell dator. En knapp för hämtning tillhandahålls tooeasily hämta alla hello säkerhetsregler oavsett hello fliken till en CSV-fil.

![gruppvy för säkerhet][1]

Regler kan väljas och ett nytt blad öppnas tooshow hello Nätverkssäkerhetsgruppen och käll- och prefix. Från det här bladet kan du gå direkt toohello Nätverkssäkerhetsgruppen resurs.

![nedbrytning][2]

### <a name="next-steps"></a>Nästa steg

Lär dig hur tooaudit nätverkssäkerheten grupp inställningar genom att besöka [Audit Nätverkssäkerhetsgruppen inställningar med PowerShell](network-watcher-nsg-auditing-powershell.md)

[1]: ./media/network-watcher-security-group-view-overview/securitygroupview.png
[2]: ./media/network-watcher-security-group-view-overview/figure1.png









