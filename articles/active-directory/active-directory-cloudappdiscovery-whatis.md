---
title: aaaFinding ohanterad molnprogram med Cloud App Discovery | Microsoft Docs
description: "Innehåller information om att söka efter och hantera program med Cloud App Discovery, vilka är fördelarna med hello och hur det fungerar."
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
ms.openlocfilehash: 50c24af9bb400e4be11f4ad2d1de13d26f5467bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a>Hitta ohanterad molnprogram med Cloud App Discovery
## <a name="overview"></a>Översikt
Moderna företag, IT-avdelningar ofta inte är medveten om alla hello molnprogram att medlemmar i organisationen använder toodo sitt arbete. Det är enkelt toosee varför administratörer skulle oroar obehörig åtkomst toocorporate data, möjliga dataläckage och andra säkerhetsrisker. Den här bristen på medvetenhet kan göra genom att skapa en plan för hantering av dessa säkerhetsrisker verka avskräckande.

Cloud App Discovery är en funktion i Azure Active Directory (AD) Premium som gör att du toodiscover molnprogram som används av hello personer i din organisation.

**Med Cloud App Discovery kan du:**

* Hitta hello molnprogram som används och mäta denna användning av antalet användare, datavolym trafik eller antalet begäranden toohello för webbprogram.
* Identifiera hello-användare som använder ett program.
* Exportera data för offlineanalys.
* Se dessa program under IT-kontroll och aktivera enkel inloggning på för användarhantering.

## <a name="how-it-works"></a>Hur det fungerar
1. Programmet användning agenter är installerade på användarens dator.
2. hello programmet användningsinformation som avbildas av hello agenter skickas via en säker och krypterad kanal toohello cloud app discovery-tjänsten.
3. hello Cloud App Discovery-tjänsten utvärderar hello data och genererar rapporter.

![Cloud App Discovery-diagram](./media/active-directory-cloudappdiscovery/cad01.png)

tooget igång med Cloud App Discovery finns [komma igång med Cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

## <a name="related-articles"></a>Relaterade artiklar
* [Cloud App Discovery säkerhets- och överväganden för sekretess](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Distributionsguide för cloud App Discovery grupp princip](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [Distributionsguide för cloud App Discovery System Center](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [Cloud App Discovery-registerinställningar för proxyservrar med anpassade portar](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Cloud App Discovery-agenten Changelog](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [Vanliga och frågor svar om cloud App Discovery](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)

