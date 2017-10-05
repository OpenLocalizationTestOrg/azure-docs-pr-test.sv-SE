---
title: Hitta ohanterade molnprogram med Cloud App Discovery | Microsoft Docs
description: "Innehåller information om att söka efter och hantera program med Cloud App Discovery, vilka är fördelarna och hur det fungerar."
services: active-directory
keywords: cloud app discovery, hantera program
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 6284ff5bac8edbc19561d0916adef153526dfbe3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Hitta ohanterad molnprogram med Cloud App Discovery
## <a name="overview"></a>Översikt
Moderna företag, IT-avdelningar ofta inte är medveten om alla molnprogram som medlemmar i organisationen använder för att utföra sitt arbete. Det är enkelt att se varför administratörer skulle oroar obehörig åtkomst till företagsdata, möjliga dataläckage och andra säkerhetsrisker. Den här bristen på medvetenhet kan göra genom att skapa en plan för hantering av dessa säkerhetsrisker verka avskräckande.

Cloud App Discovery är en funktion i Azure Active Directory (AD) Premium där du kan identifiera molnprogram som används av personer i din organisation.

**Med Cloud App Discovery kan du:**

* Hitta molnprogram som används och mäta denna användning av antalet användare, datavolym trafik eller antalet webbegäranden till programmet.
* Identifiera de användare som använder ett program.
* Exportera data för offlineanalys.
* Se dessa program under IT-kontroll och aktivera enkel inloggning på för användarhantering.

## <a name="how-it-works"></a>Hur det fungerar
1. Programmet användning agenter är installerade på användarens dator.
2. Programmet användningsinformation som avbildas av agenterna skickas via en säker och krypterad kanal till cloud app discovery-tjänsten.
3. Cloud App Discovery-tjänsten utvärderar data och genererar rapporter.

![Cloud App Discovery-diagram](./media/active-directory-cloudappdiscovery/cad01.png)

Om du vill komma igång med Cloud App Discovery finns [komma igång med Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

## <a name="related-articles"></a>Relaterade artiklar
* [Cloud App Discovery säkerhets- och överväganden för sekretess](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Distributionsguide för cloud App Discovery grupp princip](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [Distributionsguide för cloud App Discovery System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [Cloud App Discovery-registerinställningar för proxyservrar med anpassade portar](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Cloud App Discovery-agenten Changelog](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Vanliga och frågor svar om cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

