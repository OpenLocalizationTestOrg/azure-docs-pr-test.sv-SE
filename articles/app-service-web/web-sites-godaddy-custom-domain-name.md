---
title: "aaaConfigure ett anpassat domännamn i Azure App Service (GoDaddy)"
description: "Lär dig hur toouse en domän namn från GoDaddy med Azure Webbappar"
services: app-service
documentationcenter: 
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 33233e30-5846-488f-83f3-b32e5c114564
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 48158ee752f9833249bbf85adf80f572d1c68486
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-custom-domain-name-in-azure-app-service-purchased-directly-from-godaddy"></a>Konfigurera ett anpassat domännamn i Azure Apptjänsten (köps direkt från GoDaddy)
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

Om du har köpt domänen med hjälp av Azure App Service Web Apps finns toohello sista steget i [köpa domän för Web Apps](custom-dns-web-site-buydomains-web-app.md).

Den här artikeln innehåller instruktioner om hur du använder ett anpassat domännamn som har köpts direkt från [GoDaddy](https://godaddy.com) med [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Förstå DNS-poster
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Lägga till en DNS-post för den anpassade domänen
tooassociate din domän med en webbapp i App Service, du måste lägga till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av verktygen i GoDaddy. Använd följande steg toolocate hello DNS-verktyg för GoDaddy.com hello

1. Inloggningskonto tooyour med GoDaddy.com och välj **mitt konto** och sedan **Hantera min domäner**. Välj hello nedrullningsbara menyn för hello domännamn som du vill toouse med din Azure webbapp och markera **hantera DNS-**.
   
    ![anpassad domän-sidan för GoDaddy](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)
2. Från hello **domän information** , bläddra toohello **DNS-zonfilen** fliken. Detta är hello avsnitt används för att lägga till och ändra DNS-posterna för domännamnet.
   
    ![DNS-zonfilen fliken](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)
   
    Välj **lägga till posten** tooadd en befintlig post.
   
    för**redigera** en befintlig post, Välj hello penna och papper ikonen bredvid hello-post.
   
   > [!NOTE]
   > Innan du lägger till nya poster, Observera att GoDaddy redan har skapat DNS-poster för populära underdomäner (kallas **värden** i Redigeraren för) som **e-post**, **filer**, **e**, med mera. Om hello-namn som du vill toouse redan finns kan du ändra hello befintlig post i stället för att skapa en ny.
   > 
   > 
3. När du lägger till en post, måste du först välja hello posttyp.
   
    ![Välj typ av post](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)
   
    Därefter måste du ange hello **värden** (hello domänen eller underordnade domän) och IT-avdelningen **pekar på**.
   
    ![lägga till zonen](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)
   
   * När du lägger till en **A-posten (värd)** -måste du ange hello **värden** fältet tooeither  **@**  (Detta motsvarar rotdomännamn, exempelvis  **Contoso.com**,) * (jokertecken för att matcha flera underdomäner) eller hello underordnade domän som du vill toouse (till exempel **www**.) Du måste ange hello **pekar på** fältet toohello IP-adressen för din Azure-webbapp.
   * När du lägger till en **(alias) för CNAME-post** -du måste ange hello **värden** fältet toohello underordnade domän du vill toouse. Till exempel **www**. Du måste ange hello **pekar på** fältet toohello **. azurewebsites.net** domännamnet för din Azure-webbapp. Till exempel **contoso.azurewebsites.net**.
4. Klicka på **lägga till en annan**.
5. Välj **TXT** som hello-posttypen, ange en **värden** värdet för  **@**  och en **pekar på** värdet för  **&lt;yourwebappname&gt;. azurewebsites.net**.
   
   > [!NOTE]
   > Den här TXT-posten används av Azure toovalidate som du äger hello domän som beskrivs av hello en post eller hello första TXT-post. När hello domän har mappade toohello webbprogram i hello Azure-portalen, kan den här posten TXT-posten tas bort.
   > 
   > 
6. När du har lagt till eller ändra poster, klickar du på **Slutför** toosave ändringar.

<a name="enabledomain"></a>

## <a name="enable-hello-domain-name-on-your-web-app"></a>Aktivera hello domännamn i ditt webbprogram
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

