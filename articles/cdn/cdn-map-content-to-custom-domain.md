---
title: "Azure CDN aaaMap innehåll tooa domänen | Microsoft Docs"
description: "Lär dig hur toomap Azure CDN innehåll tooa anpassade domäner."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: d3ee77297f1dd7dbf31a9391191cc2910fbd2cee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="map-azure-cdn-content-tooa-custom-domain"></a>Mappa Azure CDN innehåll tooa anpassad domän
Du kan mappa en anpassad domän tooa CDN-slutpunkt i ordning toouse ditt eget domännamn i URL: er toocached innehåll i stället för att en underdomän till azureedge.net.

Det finns två sätt toomap domänen tooa CDN-slutpunkten:

1. [Skapa en CNAME-post hos din domänregistrator och mappa anpassade domäner och underdomäner toohello CDN-slutpunkten](#register-a-custom-domain-for-an-azure-cdn-endpoint).
   
    En CNAME-post är en DNS-funktion som mappar ett källdomänen som `www.contosocdn.com` eller `cdn.contoso.com`, tooa måldomänen. I det här fallet hello källdomänen är dina anpassade domäner och underdomäner (en underdomän, till exempel **www** eller **cdn** krävs alltid). hello måldomänen är CDN-slutpunkten.  
   
    hello processen för mappning av anpassad domän tooyour CDN-slutpunkten kan dock leda till en kort period driftstopp för hello domänen medan du registrerar hello domän i hello Azure-portalen.
2. [Lägg till ett mellanliggande registreringssteget med **cdnverify**](#register-a-custom-domain-for-an-azure-cdn-endpoint-using-the-intermediary-cdnverify-subdomain)
   
    Om den anpassade domänen för närvarande stöder ett program med ett servicenivåavtal (SLA) som måste det finnas utan avbrott, så du kan använda hello Azure **cdnverify** underdomän tooprovide en mellanliggande registrering steg så att användare ska kunna tooaccess din domän när hello DNS mappning äger rum.  

När du har registrerat din anpassade domän med någon av hello ovan procedurer ska för[verifiera den anpassa underdomänen hello refererar till CDN-slutpunkten](#verify-that-the-custom-subdomain-references-your-cdn-endpoint).

> [!NOTE]
> Du måste skapa en CNAME-post med din domän registrator toomap domän toohello CDN-slutpunkten. CNAME-poster mappa specifika underdomäner som `www.contoso.com` eller `cdn.contoso.com`. Det är inte möjligt toomap rotdomänen en CNAME-post tooa som `contoso.com`.
> 
> En underdomän kan bara vara kopplad till en CDN-slutpunkten. hello CNAME-post som du skapar dirigerar all trafik adresserad toohello underdomän toohello anges slutpunkt.  Om du kopplar till exempel `www.contoso.com` med din CDN-slutpunkten sedan du kan associera den med andra Azure-slutpunkter, till exempel en slutpunkt för storage-konto eller ett moln tjänstslutpunkten. Du kan dock använda olika underdomäner från hello samma domän för olika slutpunkter. Du kan även mappa olika underdomäner toohello samma CDN-slutpunkten.
> 
> För **Azure CDN från Verizon** (Standard och Premium) slutpunkter, Observera att det tar för**90 minuter** för anpassade domäner ändras toopropagate tooCDN kant noder.
> 
> 

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint"></a>Registrera en anpassad domän för en Azure CDN-slutpunkt
1. Logga in på hello [Azure Portal](https://portal.azure.com/).
2. Klicka på **Bläddra**, sedan **CDN profiler**, hello CDN-profilen med hello slutpunkt som du vill toomap tooa anpassade domäner.  
3. I hello **CDN-profilen** bladet, klickar du på hello CDN-slutpunkt som du vill använda tooassociate hello underdomänen.
4. Hello överkant hello endpoint-bladet, klickar du på hello **Lägg till anpassad domän** knappen.  I hello **lägga till en anpassad domän** bladet visas hello endpoint värdnamn som härletts från din CDN-slutpunkten toouse att skapa en ny CNAME-post. hello format hello värdadress namnet visas som  **&lt;EndpointName >. azureedge.net**.  Du kan kopiera den här värden namnet toouse skapar hello CNAME-post.  
5. Navigera tooyour domän register-webbplats och leta upp hello-avsnittet för att skapa DNS-poster. Du kan hitta det här i ett avsnitt som till exempel **Domännamn**, **DNS**, eller **hantering av namnhantering**.
6. Hitta hello avsnitt för att hantera skapa CNAME-poster. Du kan ha toogo tooan avancerade Inställningssida och leta efter hello ord CNAME, Alias eller underdomäner.
7. Skapa en ny CNAME-post som mappar dina valda underdomänen (till exempel **www** eller **cdn**) toohello värdnamn i hello **lägga till en anpassad domän** bladet. 
8. Returnera toohello **lägga till en anpassad domän** bladet och ange din domän, inklusive hello underdomän hello i dialogrutan. Ange till exempel hello domännamn i hello format `www.contoso.com` eller `cdn.contoso.com`.   
   
   Azure ska kontrollera att det finns hello CNAME-post för hello domännamn som du har angett. Om hello CNAME stämmer har din anpassade domän verifierats.  För **Azure CDN från Verizon** (Standard och Premium) slutpunkter, det kan ta upp too90 minuter för anpassade domäner inställningar toopropagate tooall CDN edge noder, men.  
   
   Observera att i vissa fall det kan ta tid för hello CNAME post toopropagate tooname servrar på hello Internet. Om din domän inte har verifierats omedelbart och du tror hello CNAME-post är korrekt, vänta en stund och försök igen.

## <a name="register-a-custom-domain-for-an-azure-cdn-endpoint-using-hello-intermediary-cdnverify-subdomain"></a>Registrera en anpassad domän för ett Azure CDN-slutpunkten med hjälp av hello mellanliggande cdnverify underdomän
1. Logga in på hello [Azure Portal](https://portal.azure.com/).
2. Klicka på **Bläddra**, sedan **CDN profiler**, hello CDN-profilen med hello slutpunkt som du vill toomap tooa anpassade domäner.  
3. I hello **CDN-profilen** bladet, klickar du på hello CDN-slutpunkt som du vill använda tooassociate hello underdomänen.
4. Hello överkant hello endpoint-bladet, klickar du på hello **Lägg till anpassad domän** knappen.  I hello **lägga till en anpassad domän** bladet visas hello endpoint värdnamn som härletts från din CDN-slutpunkten toouse att skapa en ny CNAME-post. hello format hello värdadress namnet visas som  **&lt;EndpointName >. azureedge.net**.  Du kan kopiera den här värden namnet toouse skapar hello CNAME-post.
5. Navigera tooyour domän register-webbplats och leta upp hello-avsnittet för att skapa DNS-poster. Du kan hitta det här i ett avsnitt som till exempel **Domännamn**, **DNS**, eller **hantering av namnhantering**.
6. Hitta hello avsnitt för att hantera skapa CNAME-poster. Du kan ha toogo tooan avancerade Inställningssida och leta efter hello ord **CNAME**, **Alias**, eller **underdomäner**.
7. Skapa en ny CNAME-post och ange en underdomän alias som innehåller hello **cdnverify** underdomänen. Till exempel hello underdomän som du anger ska ha formatet hello **cdnverify.www** eller **cdnverify.cdn**. Ange hello värdnamnet, vilket är din CDN-slutpunkten i hello format **cdnverify.&lt; EndpointName >. azureedge.net**. DNS-mappning bör se ut som:`cdnverify.www.consoto.com   CNAME   cdnverify.consoto.azureedge.net`  
8. Returnera toohello **lägga till en anpassad domän** bladet och ange din domän, inklusive hello underdomän hello i dialogrutan. Ange till exempel hello domännamn i hello format `www.contoso.com` eller `cdn.contoso.com`. Observera att i det här steget kan du inte behöver toopreface hello underdomän med **cdnverify**.  
   
    Azure ska kontrollera att det finns hello CNAME-post för hello cdnverify domännamn som du har angett.
9. Nu är din anpassade domän har verifierats av Azure, men trafik tooyour domän används inte ännu routade tooyour CDN-slutpunkten. Efter att ha väntat tillräckligt lång tooallow hello domänen inställningar toopropagate toohello CDN kant noder (90 minuter för **Azure CDN från Verizon**, 1 till 2 minuter för **Azure CDN från Akamai**), returnera tooyour DNS registrators webbplats och skapa en annan CNAME-post som mappar underdomän tooyour CDN-slutpunkten. Till exempel ange hello underdomän som **www** eller **cdn**, och hello värdnamn som  **&lt;EndpointName >. azureedge.net**. Hello registreringen av den anpassade domänen är klar med det här steget.
10. Slutligen kan du ta bort hello CNAME-post som du skapade med **cdnverify**eftersom det var nödvändigt endast som en mellanliggande steg.  

## <a name="verify-that-hello-custom-subdomain-references-your-cdn-endpoint"></a>Kontrollera att anpassade underdomän hello refererar till CDN-slutpunkten
* När du har slutfört hello registreringen av den anpassade domänen kan du komma åt innehåll som cachelagras på CDN-slutpunkten med hello anpassade domäner.
  Kontrollera först att du har offentligt innehåll som cachelagras på hello slutpunkt. Om din CDN-slutpunkten är associerad med ett lagringskonto, cachelagrar hello CDN innehållet i offentlig blob-behållare. tootest hello anpassad domän, se till att din behållare är tooallow offentlig åtkomst och att den innehåller minst en blob.
* Navigera toohello adress hello blob med hello anpassade domäner i webbläsaren. Om din anpassade domän är till exempel `cdn.contoso.com`, hello URL tooa cachelagrade blob blir liknande toohello följande URL: http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg

## <a name="see-also"></a>Se även
[Hur tooEnable hello innehåll innehållsleveransnätverk (CDN) för Azure](cdn-create-new-endpoint.md)  

