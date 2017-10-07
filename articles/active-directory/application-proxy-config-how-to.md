---
title: aaaHow tooconfigure ett program med Application Proxy | Microsoft Docs
description: "Lär dig hur toocreate en konfigurera ett program för APplication Proxy i några enkla steg"
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
ms.openlocfilehash: c64019098fc124e4fe10b8288830bcd2b7239d3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application"></a>Hur tooconfigure ett program med Application Proxy

Den här artikeln hjälper dig toounderstand hur tooconfigure ett Application Proxy-program i Azure AD tooexpose din lokala program toohello cloud.

## <a name="recommended-documents"></a>Rekommenderade dokument 

toolearn om hello inledande konfiguration och skapande av ett program för Application Proxy via hello Admin Portal följer hello [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Mer information om hur du konfigurerar kopplingar finns [aktivera Application Proxy hello Azure-portalen](active-directory-application-proxy-enable.md).

Information om överföring av certifikat och använder anpassade domäner finns i [arbeta med anpassade domäner i Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).

## <a name="create-hello-applicationsetting-hello-urls"></a>Skapa hello programinställning/hello URL: er

Om du följer hello stegen i hello [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) dokumentation och kan hämta ett fel när programmet hello, se hello felinformationen och förslag på hur toofix hello program. De vanligaste felmeddelanden som innehåller en föreslagen åtgärd. tooavoid vanliga fel, kontrollera:

-   Du är administratör med behörigheten toocreate ett program med Application Proxy

-   hello Intern URL är unikt

-   hello externa URL: en är unikt

-   Hej URL: er starta med http eller https och sluta med en ”/”

-   hello Webbadress måste vara ett domännamn, inte en IP-adress

hello felmeddelande ska visas i hello övre högra hörnet när du skapar hello program. Du kan också välja hello ikonen toosee hello felmeddelanden.

   ![Meddelande-fråga](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Konfigurera grupper kopplingar-koppling

Om du har problem med hur du konfigurerar programmet på grund av varning om hello kopplingar och grupper för anslutningstjänsten finns instruktioner för information om hur du aktiverar Application Proxy toodownload kopplingar. Om du vill toolearn mer om kopplingar finns hello [kopplingar dokumentationen](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

Om din kopplingar är inaktiva, betyder det att de är tooreach hello-tjänsten. Detta är ofta eftersom inte alla hello krävs portar är öppna. toosee en lista över portar som krävs i avsnittet hello förutsättningar för hello aktivera Application Proxy-dokumentationen.

## <a name="upload-certificates-for-custom-domains"></a>Överför certifikat för anpassade domäner

Anpassade domäner kan du toospecify hello domänen för de externa URL: er. toouse anpassade domäner, behöver du tooupload hello certifikat för domänen. Information om hur du använder anpassade domäner och certifikat finns i [arbeta med anpassade domäner i Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains). 

Om du upptäcker problem överföra ditt certifikat, leta efter hello felmeddelanden i hello portal för ytterligare information om hello problem med hello certifikat. Vanliga problem med säkerhetscertifikat inkluderar:

-   Certifikatet har upphört att gälla

-   Certifikatet är självsignerat

-   Certifikatet saknar privat nyckel för hello

hello felmeddelande visas i hello övre högra hörnet försök tooupload hello certifikat. Du kan också välja hello ikonen toosee hello felmeddelanden.

   ![Meddelande-fråga](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Nästa steg
[Publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md)
