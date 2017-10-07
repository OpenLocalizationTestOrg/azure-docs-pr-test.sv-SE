---
title: "aaaApplication sidan visas inte korrekt för ett program med Application Proxy | Microsoft Docs"
description: "Vägledning när hello sidan inte visas korrekt i ett Application Proxy-program som du har integrerat med Azure AD"
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
ms.openlocfilehash: f4abaa4e94c512868f2085affe59cac443784a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-page-does-not-display-correctly-for-an-application-proxy-application"></a>Sidan program visas inte korrekt för ett program med Application Proxy

Den här artikeln hjälpa tootroubleshoot problem med Azure Active Directory Application Proxy-program när du navigerar toohello sidan, men något på hello sidan ser inte rätt.

## <a name="overview"></a>Översikt
När du publicerar en Application Proxy-app är bara sidor under din roten tillgängliga vid åtkomst till programmet hello. Om hello sidan inte visas korrekt, kan hello rot Intern URL som används för programmet hello saknas några resurser för sidan. tooresolve, kontrollera att du har publicerat *alla* hello resurser för hello sida som en del av ditt program.

Du kan kontrollera detta är hello problemet genom att öppna nätverk-spårning (till exempel Fiddler eller F12 verktyg i Internet Explorer/kant) ladda hello sida och söker efter 404-fel. Värde som anger hello sidor som för närvarande går inte att hitta och du kan fortfarande toobe publiceras.

Som ett exempel på det här fallet förutsätter att du har publicerat ett utgifter program med hjälp av en intern URL för <http://myapps/expenses>, men hello appen använder hello stylesheet <http://myapps/style.css>. I det här fallet publiceras inte hello stylesheet i ditt program så inläsning hello utgifter app utlösa ett 404-fel vid försök tooload style.css. I det här exemplet hello skulle lösa problem genom att publicera hello program med en intern URL <http://myapp/> i stället.

## <a name="problems-with-publishing-as-one-application"></a>Problem med publicering som ett program

Om det inte är möjligt toopublish alla resurser inom Hej samma program, du behöver toopublish flera program och aktivera länkar mellan dem.

toodo så vi rekommenderar att du använder hello [anpassade domäner](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) lösning. Denna lösning kräver dock att du äger hello certifikat för din domän och ditt program använder fullständigt kvalificerade domännamn (FQDN). Andra alternativ finns hello [felsöka brutna länkar dokumentationen](https://microsoft-my.sharepoint.com/personal/harshja_microsoft_com/_layouts/15/guestaccess.aspx?guestaccesstoken=IxuG3mFVbnPWI3Yn4Qi7wCNi8VIfHS5mwPt5quh8DMw%3d&docid=2_14558cd6ddea34c1c9887dc640feb5831&rev=1).

## <a name="next-steps"></a>Nästa steg
[Publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md)
