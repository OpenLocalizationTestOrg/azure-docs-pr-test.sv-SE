---
title: "Konfigurera ett anpassat domännamn i molntjänster | Microsoft Docs"
description: "Lär dig mer om att exponera dina Azure-program eller data till internet på en anpassad domän genom att konfigurera DNS-inställningar.  De här exemplen använder Azure-portalen."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 139ec6578dc9e76039c5fb13e7a7741aa8ba4e0d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurera ett anpassat domännamn för en Azure-molntjänst
När du skapar en molnbaserad tjänst Azure tilldelar den en underdomän till **cloudapp.net**. Till exempel om Molntjänsten har namnet ”contoso”, kommer användarna att kunna komma åt programmet på en URL som http://contoso.cloudapp.net. Azure ger också en virtuell IP-adress.

Men du kan också exponera dina program på ditt eget domännamn som **contoso.com**. Den här artikeln beskriver hur du reservera eller konfigurera ett anpassat domännamn för Molntjänsten web-roller.

Har du redan att förstå vilka CNAME- och A-poster är? [Hoppa över en förklaring](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> Procedurerna i det här steget gäller för Azure Cloud Services. App-tjänster, se [mappa ett befintligt anpassade DNS-namn till Azure Web Apps](../app-service/app-service-web-tutorial-custom-domain.md). Storage-konton finns [konfigurera ett anpassat domännamn för din Azure Blob storage-slutpunkt](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Komma igång snabbare--Använd nya Azure [guidad genomgång](http://support.microsoft.com/kb/2990804)!  Gör det associera ett eget domännamn och att säkra kommunikationen (SSL) med Azure Cloud Services eller Azure Websites ett ögonblick.
> 
> 

## <a name="understand-cname-and-a-records"></a>Förstå CNAME och A-poster
CNAME-post (eller alias poster) och A-poster båda kan du associera ett domännamn med en viss server (eller tjänst i det här fallet) men de fungerar på olika sätt. Det finns också några särskilda överväganden när du använder en poster med Azure-molntjänster som du bör tänka på innan du bestämmer dig som ska användas.

### <a name="cname-or-alias-record"></a>Posten CNAME eller Alias
En CNAME-post mappar en *specifika* domän, som **contoso.com** eller **www.contoso.com**, till ett kanoniskt domännamn. I så fall måste kanoniskt domännamnet är den **[myapp] .cloudapp .net** domännamnet för din Azure värd för programmet. När du skapat CNAME skapas ett alias för den **[myapp] .cloudapp .net**. CNAME-posten ska matcha IP-adressen för din **[myapp] .cloudapp .net** tjänsten automatiskt, så om IP-adressen för Molntjänsten ändras, inte behöver du vidta några åtgärder.

> [!NOTE]
> Vissa domän-registratorer tillåter endast att mappa underdomäner när du använder en CNAME-post, till exempel www.contoso.com och inte rot namn, t.ex contoso.com. Mer information om CNAME-poster finns i dokumentationen från din registrator [Wikipedia posten CNAME-post](http://en.wikipedia.org/wiki/CNAME_record), eller [IETF domännamn - implementering och specifikation](http://tools.ietf.org/html/rfc1035) dokumentet.
> 
> 

### <a name="a-record"></a>en post
En *A* post som mappar en domän, **contoso.com** eller **www.contoso.com**, *eller en jokerteckendomän med* som  **\*. contoso.com**, en IP-adress. När det gäller ett Azure Cloud Service, virtuella IP-Adressen för tjänsten. Så att den viktigaste fördelen med en A-post via en CNAME-post är att du har en post som använder ett jokertecken som \* **. contoso.com**, som kan hantera förfrågningar för flera underordnade domäner som **mail.contoso.com**, **login.contoso.com**, eller **www.contso.com**.

> [!NOTE]
> Eftersom en A-post är mappad till en statisk IP-adress, kan den automatiskt lösa ändringar till IP-adressen för din tjänst i molnet. IP-adress som används av din molntjänst tilldelas första gången du distribuerar till ett tomt fack (produktion eller mellanlagring.) Om du tar bort distributionen för facket IP-adressen har getts ut av Azure och alla framtida distributioner till facket som får en ny IP-adress.
> 
> IP-adressen för en viss distributionsplats (produktion eller mellanlagring) är ett enkelt sätt, beständiga när växling mellan mellanlagring och produktiondistributioner eller utför en uppgradering på plats av en befintlig distribution. Mer information om hur du utför dessa åtgärder finns [så här hanterar du molntjänster](cloud-services-how-to-manage-portal.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Lägg till en CNAME-post för den anpassade domänen
Om du vill skapa en CNAME-post, du måste lägga till en ny post i DNS-tabellen för den anpassade domänen med hjälp av verktygen i din registrator. Varje register har en liknande metod för att ange en CNAME-post, men konceptet är detsamma.

1. Använd någon av följande metoder för att hitta den **. cloudapp.net** domännamn som tilldelats till Molntjänsten.
   
   * Logga in på den [Azure-portalen], Välj din molntjänst, titta på den **Essentials** avsnittet och leta reda på den **Webbadress** post.
     
       ![snabböversikten avsnitt visar webbplatsens URL][csurl]
     
       **ELLER**
   * Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och Använd sedan följande kommando:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Spara det domännamn som används i URL-Adressen som returneras av antingen metoden som du behöver den när du skapar en CNAME-post.
2. Logga in på din DNS-registratorns webbplats och gå till sidan för hantering av DNS. Leta efter länkar eller områden på webbplatsen med etiketten **domännamn**, **DNS**, eller **namn serverhantering**.
3. Hitta nu där du kan markera eller ange CNAME'S. Du kan behöva väljer du typ av post i en nedrullningsbara ned eller gå till en sida med avancerade inställningar. Du bör titta efter orden **CNAME**, **Alias**, eller **underdomäner**.
4. Du måste också ange den domän eller underdomän alias för CNAME, som **www** om du vill skapa ett alias för **www.customdomain.com**. Om du vill skapa ett alias för rotdomänen den kan visas som den '**@**' symbol i din domänregistrators DNS-verktyg.
5. Måste du ange ett kanoniska värdnamn, vilket är ditt programs **cloudapp.net** domän i det här fallet.

Till exempel följande CNAME-posten vidarebefordrar all trafik från **www.contoso.com** till **contoso.cloudapp.net**, det anpassa domännamnet för ditt distribuerade program:

| Alias/Host namn/underdomän | Kanoniskt domän |
| --- | --- |
| www |Contoso.cloudapp.NET |

> [!NOTE]
> En besökare av **www.contoso.com** visas aldrig true värden (contoso.cloudapp.net), så att vidarebefordran är osynliga för slutanvändaren.
> 
> Exemplet ovan gäller bara för trafik i den **www** underdomänen. Eftersom du inte kan använda jokertecken med CNAME-poster, måste du skapa en CNAME-post för varje domän/underdomänen. Om du vill dirigera trafik från underdomäner, som *. contoso.com, till cloudapp.net-adress kan du konfigurera en **omdirigerings-URL: en** eller **URL vidarebefordra** post i DNS-inställningarna, eller skapa en A-post.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Lägg till en A-post för den anpassade domänen
Om du vill skapa en A-post, måste du först hittas virtuella IP-adressen för din tjänst i molnet. Lägg sedan till en ny post i DNS-tabellen för den anpassade domänen med hjälp av verktygen i din registrator. Varje register har en liknande metod för att ange en A-post, men konceptet är detsamma.

1. Använd någon av följande metoder för att hämta IP-adressen för din molntjänst.
   
   * Logga in på den [Azure-portalen], Välj din molntjänst, titta på den **Essentials** avsnittet och leta reda på den **offentliga IP-adresser** post.
     
       ![snabböversikten avsnitt visar VIP][vip]
     
       **ELLER**
   * Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och Använd sedan följande kommando:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Spara IP-adress som du behöver den när du skapar en A-post.
2. Logga in på din DNS-registratorns webbplats och gå till sidan för hantering av DNS. Leta efter länkar eller områden på webbplatsen med etiketten **domännamn**, **DNS**, eller **namn serverhantering**.
3. Hitta nu där du kan markera eller ange en post. Du kan behöva väljer du typ av post i en nedrullningsbara ned eller gå till en sida med avancerade inställningar.
4. Välj eller ange domän eller underdomän som ska använda den här A-post. Välj exempelvis **www** om du vill skapa ett alias för **www.customdomain.com**. Ange om du vill skapa en post med jokertecken för alla underdomäner ' ***'. Detta kommer att omfatta alla underordnade domäner som **mail.customdomain.com**, **login.customdomain.com**, och **www.customdomain.com**.
   
    Om du vill skapa en A-post för rotdomänen den kan visas som den '**@**' symbol i din domänregistrators DNS-verktyg.
5. Ange IP-adressen för din molntjänst i det angivna fältet. Det här associerar domän-posten som används i en post med IP-adressen för din molndistribution för tjänsten.

Till exempel följande post vidarebefordrar all trafik från **contoso.com** till **137.135.70.239**, IP-adressen för ditt distribuerade program:

| Värden namn/underdomän | IP-adress |
| --- | --- |
| @ |137.135.70.239 |

Det här exemplet visar hur du skapar en A-post för rotdomänen. Om du vill skapa en post med jokertecken för att täcka alla underdomäner anger du ' ***' som underdomänen.

> [!WARNING]
> IP-adresser i Azure är dynamiska som standard. Vill du förmodligen använda ett [reserverad IP-adress](../virtual-network/virtual-networks-reserved-public-ip.md) så att din IP-adress inte ändras.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Så här hanterar du molntjänster](cloud-services-how-to-manage-portal.md)
* [Mappa CDN-innehåll till en anpassad domän](../cdn/cdn-map-content-to-custom-domain.md)
* [Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).
* Lär dig hur du [distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure-portalen]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
