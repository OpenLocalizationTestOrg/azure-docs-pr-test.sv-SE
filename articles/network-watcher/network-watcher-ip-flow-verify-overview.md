---
title: "aaaIntroduction tooIP flöde verifiera i Azure-Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan innehåller en översikt av hello Network Watcher IP flödet verifieringsfunktioner"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: d352fb2d-4b4f-4ac4-9c2e-1cfccf0e7e03
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b648a4816a7ffdc6ca54462944b574e2395e8298
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooip-flow-verify-in-azure-network-watcher"></a>Introduktion tooIP flöde verifiera i Azure Nätverksbevakaren

IP-flöde kontrollera kontrollerar om ett paket tillåts eller nekas tooor från en virtuell dator baserat på 5-tuppel information. Den här informationen består av riktning, protokoll, lokal IP-, fjärr-IP, lokal port och Fjärrport. Om hello paket nekas av en säkerhetsgrupp, returneras hello namn på hello-regel som nekats hello-paket. När alla käll- eller IP-adresser kan väljas funktionen hjälper administratörer att snabbt diagnostisera problem med nätverksanslutningen från eller toohello internet och från eller toohello lokala miljö.

IP-flöde Kontrollera riktar sig till ett nätverksgränssnitt för en virtuell dator. Trafikflöde verifieras sedan baserat på konfigurerad hello inställningar tooor från nätverksgränssnittet. Den här funktionen är användbar för att bekräfta om en regel i en Nätverkssäkerhetsgrupp blockerar tooor för meddelanden om ingångs- eller utgående trafik från en virtuell dator.

Kontrollera en instans av Nätverksbevakaren behov toobe som skapats i alla regioner som du planerar toorun IP-flödet. Nätverksbevakaren är en tjänst som regional och kan endast kördes mot resurser i hello samma region. Detta påverkar inte hello resultaten av IP-flöde Kontrollera som hello väg som är associerade med hello NIC kommer fortfarande att returneras.

![1][1]

## <a name="next-steps"></a>Nästa steg

Besök hello efter artikel toolearn om ett paket tillåts eller nekas för en specifik virtuell dator via hello-portalen. [Kontrollera om trafik tillåts på en virtuell dator med IP-flöda Kontrollera med hjälp av hello portal](network-watcher-check-ip-flow-verify-portal.md)

[1]: ./media/network-watcher-ip-flow-verify-overview/figure1.png












