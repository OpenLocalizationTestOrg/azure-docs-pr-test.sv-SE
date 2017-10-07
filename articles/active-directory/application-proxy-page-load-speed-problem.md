---
title: "aaaAn Application Proxy-program tar för lång tooload | Microsoft Docs"
description: "Felsöka sidan belastningen prestandaproblem med hello Azure AD Application Proxy"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c7a51f96840966a1d88933fa4e30f39479d8a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="an-application-proxy-application-takes-too-long-tooload"></a>Ett program med Application Proxy tar för lång tooload

Den här artikeln hjälper dig toounderstand varför ett program för Azure AD Application Proxy kan ta en lång tid tooload och vad du kan göra tooresolve problemet.

## <a name="overview"></a>Översikt
Om dina program fungerar, men du ser en lång fördröjning, kan det finnas några mindre justeringar i nätverkets topologi som du anser tooimprove hello hastighet. En utvärdering av olika topologier finns hello [nätverk överväganden dokumentet](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Om dessa överväganden inte hjälper kan har vi tyvärr inte för närvarande ytterligare rekommendationer för prestandajustering. Eftersom hello Application Proxy-tjänsten utökar toomore datacenter som kan vara närmare tooyou, kanske du toosee bättre svarstid direkt. toosee hello fullständig lista över Azure data datacenter, du kan se hello [svarstid testsida](http://www.azurespeed.com/Azure/Latency). 

hello datacenter med hello Application Proxy-tjänsten kan hittas med hello [anslutningsverktyget portar Test](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Feedback om Application Proxy data center platser 
Det kan finnas Azure-datacenter som ännu inte med Application Proxy men leder tooa bra svarstid förbättring av du. hello Datacenter plats < aadapfeedback@microsoft.com > så att vi kan använda din feedback tooplan som vi expandera.

Vi arbetar med vissa ytterligare funktioner som förbättrar hello svarstid för klienter som för närvarande finns långa fördröjningar och vara säker på att tooshare dokumentation när det är tillgängligt.

## <a name="next-steps"></a>Nästa steg
[Arbeta med befintliga lokala proxyservrar](application-proxy-working-with-proxy-servers.md)
