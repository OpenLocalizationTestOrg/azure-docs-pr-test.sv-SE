---
title: aaaUpgrade tooAzure AD Application Proxy | Microsoft Docs
description: "Välj vilka proxy-lösning som är bäst om du uppgraderar från Microsoft Forefront eller enhetlig åtkomst Gateway."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 7dc2633140b384e25792470dadbb7f3fa7992a2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="compare-remote-access-solutions"></a>Jämför fjärråtkomstlösningar

Azure Active Directory Application Proxy är en av två fjärråtkomstlösningarna som Microsoft erbjuder. hello andra är Web Application Proxy hello lokala versionen. Dessa två lösningar ersätta tidigare produkter som erbjuds av Microsoft: Microsoft Forefront Threat Management Gateway (TMG) och enhetlig åtkomst Gateway (UAG). Använd den här artikeln toounderstand hur lösningarna fyra jämföra tooeach andra. För de som du fortfarande använder hello föråldrad TMG eller UAG lösningar kan använda den här artikeln toohelp planen din migrering tooone av hello Application Proxy. 


## <a name="feature-comparison"></a>Jämför funktioner

Använd den här tabellen toounderstand hur Threat Management Gateway (TMG), enhetlig åtkomst Gateway (UAG), Webbprogramproxy (WAP) och Azure AD Application Proxy (AP) jämför tooeach andra.

| Funktion | TMG | UAG | WAP | AP |
| ------- | --- | --- | --- | --- |
| Certifikatautentisering | Ja | Ja | - | - |
| Selektivt publicera webbläsarbaserade appar | Ja | Ja | Ja | Ja |
| Förautentisering och enkel inloggning | Ja | Ja | Ja | Ja | 
| Nivå 2/3 brandväggen | Ja | Ja | - | - |
| Vidarebefordra proxyfunktioner | Ja | - | - | - |
| VPN-funktioner | Ja | Ja | - | - |
| Omfattande protokollstöd | - | Ja | Ja, om du kör via HTTP | Ja, om du kör via HTTP eller via fjärrskrivbordsgateway |
| Fungerar som AD FS-proxyservern | - | Ja | Ja | - |
| En portal för programåtkomst | - | Ja | - | Ja |
| Svaret brödtext länköversättning | Ja | Ja | - | Ja | 
| Autentisering med rubriker | - | Ja | - | Ja, med PingAccess | 
| Skalbar molnlagring säkerhet | - | - | - | Ja | 
| Villkorlig åtkomst | - | Ja | - | Ja |
| Inga komponenter i hello demilitariserad zon (DMZ) | - | - | - | Ja |
| Inga inkommande anslutningar | - | - | - | Ja |

För de flesta fall rekommenderar vi Azure AD-program som hello moderna lösning. Du kan inte använda anpassade domäner i Azure Active Directory Web Application Proxy är bara önskade i scenarier som kräver en proxyserver för AD FS. 

Azure AD Application Proxy erbjuder unika fördelar när jämfört med toosimilar produkter, inklusive:

- Utöka Azure AD tooon lokala resurser
   - Skalbar molnlagring säkerhet och skydd
   - Funktioner som villkorlig åtkomst och Multi-Factor Authentication är enkelt tooenable
- Ingen Component i hello demilitariserad zon
- Inga inkommande anslutningar som krävs
- En åtkomstpanelen att användarna kan gå toofor alla sina program, inklusive O365, Azure AD-integrerade SaaS-appar och dina lokala web apps. 


## <a name="next-steps"></a>Nästa steg

- [Använda Azure AD-program tooprovide säker fjärråtkomst tooon lokala program](active-directory-application-proxy-get-started.md)
- [Övergången från Forefront TMG och UAG tooApplication Proxy](https://blogs.technet.microsoft.com/isablog/2015/06/30/modernizing-microsoft-application-access-with-web-application-proxy-and-azure-active-directory-application-proxy/).
