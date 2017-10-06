---
title: "aaaAzure Active Directory PoC Playbook beståndsdelar | Microsoft Docs"
description: "Utforska och snabbt implementera scenarier för identitets- och åtkomsthantering"
services: active-directory
keywords: Azure active directory, playbook, konceptbevis, PoC
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 0a7f5cd659b9d62ac86e3c27e5727294d481f4a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a>Azure Active Directory bevis på koncept Playbook komponenter 

## <a name="theme"></a>Tema
Azure AD innehåller identitets- och lösningar i flera områden i hello enterprise. Vi klassificera hello scenarier i hello följande områden: 

* [Många appar, en identitet](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [Öka säkerheten](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [Skala med självbetjäning](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

Definiera ett tema tooframe hello PoC hjälper toofocus hello arbete som får respons med verksamhetsmål som ofta är hello utlösare hello intresse för ett konceptbevis hello första plats. 

## <a name="environment"></a>Miljö

Det är viktigt toodetermine hello information om hello miljö där du levererar hello PoC. Det bästa du kan bygga vidare på den efter hello PoC har slutförts. hello målmiljön är avgörande bör du hitta hello rätt balans mellan att göra det som verkliga som möjligt och extra hello begränsningar eller ytterligare överväganden. hello miljöer för POC är:
* **Produktion:** hello scenarier kommer att genomföras i live-miljön och redan har distribuerats Microsoft Cloud-tjänsterna (produktion AD, Office 365, Azure AD-klient/SSO lösning). 
* **Användaren godkänner testa (UAT) / utvecklingsmiljö:** du har test-infrastruktur (parallell AD och eventuellt Azure AD-klient/SSO-lösning) med testdata som liknar produktion. Normalt delas hello testmiljö mellan flera projekt i hello företag.

De flesta scenarier i den här handboken är additiva till sin natur. De kan därför distribueras i hello produktion klienten utan att påverka användare utanför hello PoC. I det här dokumentet ringer vi ut vilka scenarier som skulle ha klient hela effekt. I sådana fall kanske du vill tooconsider en icke-produktionsmiljö. 


## <a name="target-users"></a>Målgruppsanvändare

Det är viktigt toodetermine hello måluppsättningen med användare som kommer utöva hello scenarier, särskilt när hello-miljö är produktions- och. hello användarkategorier mål för PoC är:
* **Pilotanvändarna:** verkliga användare i hello-miljö som kommer att använda hello lösning med hello konto de använder för sina dag tooday jobbfunktioner
* **Testanvändare:** testa konton som har skapats i hello-miljö 

De flesta scenarier i den här guiden kan utnyttjas av pilotanvändare. I det här dokumentet kommer vi ringer ut mål användaren överväganden om det behövs.


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]