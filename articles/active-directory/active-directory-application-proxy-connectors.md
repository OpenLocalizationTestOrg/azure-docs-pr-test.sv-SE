---
title: aaaClassic portal Azure AD App Proxy kopplingar | Microsoft Docs
description: Omfattar hur toocreate och hantera grupper av kopplingar i Azure AD Application Proxy.
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 43559b0f4ffc3c7dbbf00901e89ac276d01796e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Publicera program på separata nätverk och platser med hjälp av connector-grupper
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Klassisk Azure-portal](active-directory-application-proxy-connectors.md)
>
>

Kopplingen grupper är användbara för olika scenarier, inklusive:

* Webbplatser med flera sammankopplade datacenter. I detta fall vill du tookeep så mycket trafik i hello datacenter som möjligt eftersom länkar mellan datacenter är dyrt och långsamt. Du kan distribuera kopplingar i varje datacenter tooserve endast hello program som finns inom hello datacenter. Den här metoden minimerar länkar mellan datacenter och ger en helt transparent upplevelse tooyour användare.
* Hantera program på isolerade nätverk som inte är del av hello huvudsakliga företagsnätverket. Du kan använda anslutningstjänsten grupper tooinstall dedikerad kopplingar på isolerade nätverk tooalso isolera program toohello nätverk.
* För program som installerats på IaaS för molnåtkomst anger connector grupper du en gemensam toosecure hello åtkomst tooall hello-appar. Kopplingen grupper inte skapa ytterligare beroende på företagets nätverk eller Fragmentera hello app upplevelse. Kopplingar kan installeras på varje molndatacenter och hantera program som finns i det här nätverket. Du kan installera flera kopplingar tooachieve hög tillgänglighet.
* Stöd för miljöer med flera skogar där specifika kopplingar kan distribueras per skog och ange tooserve specifika program.
* Kopplingen grupper kan användas i Disaster Recovery platser tooeither identifiera växling vid fel eller hello huvudsakliga som säkerhetskopia för platsen.
* Kopplingen grupper kan också vara används tooserve flera företag från en enskild klient.

## <a name="prerequisite-create-your-connectors"></a>Förutsättning: Skapa kopplingar
toogroup din kopplingar [installera flera kopplingar](active-directory-application-proxy-enable.md), namnge och gruppera dem. Slutligen du har tooassign dem toospecific appar.

## <a name="step-1-create-connector-groups"></a>Steg 1: Skapa grupper för anslutningstjänsten
Du kan skapa så många connector-grupper som du vill använda. Skapa en koppling sker i hello klassiska Azure-portalen.

1. Välj din katalog och klickar på **konfigurera**.  
    ![Programproxy, konfigurera skärmbild - Klicka på Hantera connector grupper](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. Klicka på under Application Proxy **hantera Connector grupper** och skapa en koppling grupp genom att ge hello gruppen ett namn.  
    ![Application proxy connector grupper skärmbild - namn på ny grupp](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-tooyour-groups"></a>Steg 2: Tilldela kopplingar tooyour grupper
När hello connector grupper skapas bör du flytta hello kopplingar toohello lämplig grupp.

1. Under **Application Proxy**, klickar du på **hantera kopplingar**.
2. Under **grupp**väljer hello-grupp som du vill använda för varje koppling. Det kan ta hello kopplingar in too10 minuter toobecome aktiv i hello ny grupp.  
    ![Application proxy kopplingar skärmbild – Välj grupp från listrutan](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-tooyour-connector-groups"></a>Steg 3: Tilldela program tooyour connector grupper
hello sista steget är tooset varje toohello connector programgrupp som hanterar den.

1. I hello klassiska Azure-portalen i din katalog väljer hello program du vill tooassign toohello gruppen och klicka på **konfigurera**.
2. Under **Connector grupp**väljer hello-grupp som du vill hello programmet toouse. Den här ändringen tillämpas omedelbart.  
    ![Application proxy connector grupp skärmbild - Välj grupp från listrutan](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Se även
* [Aktivera Application Proxy](active-directory-application-proxy-enable.md)
* [Aktivera enkel inloggning](active-directory-application-proxy-sso-using-kcd.md)
* [Aktivera villkorlig åtkomst](active-directory-application-proxy-conditional-access.md)
* [Felsökning av problem med Application Proxy](active-directory-application-proxy-troubleshoot.md)

Hello senaste nyheterna och uppdateringarna finns på hello [bloggen om Application Proxy](http://blogs.technet.com/b/applicationproxyblog/)
