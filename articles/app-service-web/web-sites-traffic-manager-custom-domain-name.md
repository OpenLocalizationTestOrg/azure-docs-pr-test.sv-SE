---
title: "aaaConfigure ett anpassat domännamn för en webbapp i Azure App Service som använder Traffic Manager för belastningsutjämning."
description: "Använd ett anpassat domännamn för en en webbapp i Azure App Service med Traffic Manager för belastningsutjämning."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: dfde5fc6b445b30b10e03dcb03e8d072130d9377
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurera ett anpassat domännamn för en webbapp i Azure App Service med Traffic Manager
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Den här artikeln innehåller allmänna anvisningar för att använda ett anpassat domännamn med Azure App Service som använder Traffic Manager för belastningsutjämning.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Förstå DNS-poster
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurera dina webbprogram för standardläge
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Lägga till en DNS-post för den anpassade domänen
> [!NOTE]
> Om du har köpt domänen med hjälp av Azure App Service Web Apps och hoppa över följande steg och hänvisa toohello sista steget i [köpa domän för Web Apps](custom-dns-web-site-buydomains-web-app.md) artikel.
> 
> 

tooassociate din domän med en webbapp i Azure App Service, du måste lägga till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av verktygen i hello domänregistrator som du har köpt domännamnet från. Använd följande steg toolocate hello och hello DNS-verktyg.

1. Logga in tooyour konto hos din domänregistrator och leta efter en sida för att hantera DNS-poster. Sök efter länkar eller delar av hello plats anges som **domännamn**, **DNS**, eller **namn serverhantering**. Ofta en länk toothis sidan finns visa din kontoinformation och söker efter en länk som **min domäner**.
2. När du har hittat sidan för hantering av hello för ditt domännamn kan du leta efter en länk som du kan använda tooedit hello DNS-poster. Detta kan anges som en **zonfilen**, **DNS-poster**, eller som en **Avancerat** konfigurationslänken.
   
   * hello sidan har antagligen några poster som redan har skapats, till exempel en post som kopplar '**@**'eller'\*' med en 'domän parkering'-sida. Den kan även innehålla poster för vanliga underdomäner som **www**.
   * hello sidan kommer nämnt **CNAME-poster**, eller ange en nedrullningsbar tooselect en posttyp. Det kan också anges andra poster som **A-poster** och **MX-poster**. I vissa fall CNAME-poster kommer att anropas av andra namn som en **aliaspost**.
   * hello sidan måste även ha fält som gör det möjligt för**kartan** från en **värdnamn** eller **domännamn** tooanother domännamn.
3. Medan hello detaljerna i varje register varierar i allmänhet du mappa *från* ditt domännamn (t.ex **contoso.com**,) *till* hello Traffic Manager-domännamn (**contoso.trafficmanager.net**) som används för ditt webbprogram.
   
   > [!NOTE]
   > Du kan också om en post används redan och du behöver toopreemptively binda tooit dina appar, kan du skapa en ytterligare CNAME-post. Till exempel toopreemptively bind **www.contoso.com** tooyour webbapp, skapa en CNAME-post från **awverify.www** för**contoso.trafficmanager.net**. Du kan sedan lägga till ”www.contoso.com” tooyour Web App utan att ändra hello ”www” CNAME-post. Mer information finns i [skapa DNS-poster för ett webbprogram i en anpassad domän][CREATEDNS].
   > 
   > 
4. När du har lagt till eller ändra DNS-poster hos din registrator spara hello ändringar.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Aktivera Traffic Manager
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [Node.js Developer Center](/develop/nodejs/).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md
