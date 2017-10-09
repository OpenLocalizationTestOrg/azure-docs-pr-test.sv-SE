---
title: "aaaConfigure ett anpassat domännamn i molntjänster | Microsoft Docs"
description: "Lär dig hur tooexpose din Azure-program eller data toohello internet på en anpassad domän genom att konfigurera DNS-inställningar.  Dessa exempel används hello Azure-portalen."
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
ms.openlocfilehash: a0f3186b6022fbc4570ef1ce4b921426842bde76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfigurera ett anpassat domännamn för en Azure-molntjänst
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-custom-domain-name-portal.md)
> * [Klassisk Azure-portal](cloud-services-custom-domain-name.md)
> 
> 

När du skapar en molnbaserad tjänst Azure tilldelar den tooa underdomän **cloudapp.net**. Till exempel om Molntjänsten har namnet ”contoso”, användarna kommer att kunna tooaccess programmet på en URL som http://contoso.cloudapp.net. Azure ger också en virtuell IP-adress.

Men du kan också exponera dina program på ditt eget domännamn som **contoso.com**. Den här artikeln förklarar hur tooreserve eller konfigurera ett anpassat domännamn för Molntjänsten web-roller.

Har du redan att förstå vilka CNAME- och A-poster är? [Hoppa över hello förklaring](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> hello procedurerna i det här steget gäller tooAzure molntjänster. App-tjänster, se [detta](../app-service-web/web-sites-custom-domain-name.md). Storage-konton finns [detta](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Komma igång snabbare--Använd hello nya Azure [guidad genomgång](http://support.microsoft.com/kb/2990804)!  Gör det associera ett eget domännamn och att säkra kommunikationen (SSL) med Azure Cloud Services eller Azure Websites ett ögonblick.
> 
> 

## <a name="understand-cname-and-a-records"></a>Förstå CNAME och A-poster
CNAME-post (eller alias poster) och A-poster både tillåter du tooassociate ett domännamn med en viss server (eller tjänst i det här fallet) men de fungerar på olika sätt. Det finns också några särskilda överväganden när du använder en poster med Azure-molntjänster som du bör tänka på innan du bestämmer vilka toouse.

### <a name="cname-or-alias-record"></a>Posten CNAME eller Alias
En CNAME-post mappar en *specifika* domän, som **contoso.com** eller **www.contoso.com**, tooa kanoniska domännamn. I det här fallet hello kanoniska domännamnet är hello **[myapp] .cloudapp .net** domännamnet för din Azure värd för programmet. När du skapat hello CNAME skapas ett alias för hello **[myapp] .cloudapp .net**. hello CNAME-post ska matcha toohello IP-adressen för din **[myapp] .cloudapp .net** tjänsten automatiskt, så om hello IP-adressen för hello Molntjänsten ändras, behöver du inte tootake någon åtgärd.

> [!NOTE]
> Vissa domän-registratorer endast tillåter att du toomap underdomäner när du använder en CNAME-post, till exempel www.contoso.com och inte rot namn, t.ex contoso.com. Mer information om CNAME-poster i dokumentationen hello tillhandahålls av din registrator [hello Wikipedia transaktionen på CNAME-post](http://en.wikipedia.org/wiki/CNAME_record), eller hello [IETF domännamn - implementering och specifikation](http://tools.ietf.org/html/rfc1035) dokumentet.
> 
> 

### <a name="a-record"></a>en post
En *A* post som mappar en domän, **contoso.com** eller **www.contoso.com**, *eller en jokerteckendomän med* som  **\*. contoso.com**, tooan IP-adress. Hello virtuella IP-Adressen för hello-tjänsten i hello fallet för en Azure-Molntjänsten. Så hello viktigaste fördelen med en A-post via en CNAME-post är att du har en post som använder ett jokertecken som \* **. contoso.com**, som kan hantera förfrågningar för flera underordnade domäner som  **Mail.contoso.com**, **login.contoso.com**, eller **www.contso.com**.

> [!NOTE]
> Eftersom en A-post mappas tooa statisk IP-adress, den kan inte automatiskt lösa ändringar toohello IP-adressen för din tjänst i molnet. hello IP-adress som används av din molntjänst tilldelas hello första gången du distribuerar tooan tomt fack (produktion eller mellanlagring.) Om du tar bort hello distribution hello platsens hello IP-adress har getts ut av Azure och alla framtida distributioner toohello fack får en ny IP-adress.
> 
> Enkelt, sparas hello IP-adressen för en viss distributionsplats (produktion eller mellanlagring) när växling mellan mellanlagring och produktiondistributioner eller utför en uppgradering på plats av en befintlig distribution. Mer information om hur du utför dessa åtgärder finns [hur toomanage molntjänster](cloud-services-how-to-manage.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Lägg till en CNAME-post för den anpassade domänen
toocreate en CNAME-post, du måste lägga till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av hello verktyg som tillhandahålls av din registrator. Varje register har en liknande metod för att ange en CNAME-post, men hello principerna är hello samma.

1. Använd någon av dessa metoder toofind hello **. cloudapp.net** domännamn som tilldelats tooyour Molntjänsten.
   
   * Inloggningen toohello [Azure-portalen], Välj din molntjänst, titta på hello **Essentials** avsnittet och hitta hello **Webbadress** post.
     
       ![snabböversikten avsnitt som visar hello Webbadress][csurl]
     
       **ELLER**
   * Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och sedan använda hello följande kommando:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Spara hello domännamn som används i hello-URL som returnerades av dessa metoder som du behöver den när du skapar en CNAME-post.
2. Logga in tooyour DNS registrators webbplats och gå toohello sidan för hantering av DNS. Sök efter länkar eller delar av hello plats anges som **domännamn**, **DNS**, eller **namn serverhantering**.
3. Hitta nu där du kan markera eller ange CNAME'S. Du kan ha tooselect hello post från en nedrullningsbar, eller gå tooan avancerade inställningar. Du bör titta efter hello ord **CNAME**, **Alias**, eller **underdomäner**.
4. Du måste också ange hello domän eller underdomän alias för hello CNAME, t.ex **www** om du vill toocreate ett alias för **www.customdomain.com**. Om du vill toocreate ett alias för hello rotdomän, den kan visas som hello '**@**' symbol i din domänregistrators DNS-verktyg.
5. Måste du ange ett kanoniska värdnamn, vilket är ditt programs **cloudapp.net** domän i det här fallet.

Till exempel följande CNAME-post hello vidarebefordrar all trafik från **www.contoso.com** för**contoso.cloudapp.net**, hello domännamn för ditt distribuerade program:

| Alias/Host namn/underdomän | Kanoniskt domän |
| --- | --- |
| www |Contoso.cloudapp.NET |

> [!NOTE]
> En besökare av **www.contoso.com** visas aldrig hello true värden (contoso.cloudapp.net), så hello processer för vidarebefordring är osynliga toothe slutanvändaren.
> 
> hello exemplet ovan gäller bara tootraffic på hello **www** underdomänen. Eftersom du inte kan använda jokertecken med CNAME-poster, måste du skapa en CNAME-post för varje domän/underdomänen. Om du vill ha toodirect trafik från underdomäner, exempel *. contoso.com, tooyour cloudapp.net adress, kan du konfigurera en **omdirigerings-URL: en** eller **URL vidarebefordra** post i DNS-inställningarna, eller skapa en A-post.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Lägg till en A-post för den anpassade domänen
toocreate en A-post, måste du först hittas hello virtuella IP-adressen för din tjänst i molnet. Lägg sedan till en ny post i hello DNS-tabellen för den anpassade domänen med hjälp av hello verktyg som tillhandahålls av din registrator. Varje register har en liknande metod för att ange en A-post, men hello principerna är hello samma.

1. Använd någon av följande metoder tooget hello IP-adressen för din molntjänst hello.
   
   * Inloggningen toohello [Azure-portalen], Välj din molntjänst, titta på hello **Essentials** avsnittet och hitta hello **offentliga IP-adresser** post.
     
       ![snabböversikten avsnitt som visar hello VIP][vip]
     
       **ELLER**
   * Installera och konfigurera [Azure Powershell](/powershell/azure/overview), och sedan använda hello följande kommando:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Spara hello IP-adress, eftersom du behöver den när du skapar en A-post.
2. Logga in tooyour DNS registrators webbplats och gå toohello sidan för hantering av DNS. Sök efter länkar eller delar av hello plats anges som **domännamn**, **DNS**, eller **namn serverhantering**.
3. Hitta nu där du kan markera eller ange en post. Du kan ha tooselect hello post från en nedrullningsbar, eller gå tooan avancerade inställningar.
4. Välj eller ange hello domän eller underdomän som ska använda den här A-post. Välj exempelvis **www** om du vill toocreate ett alias för **www.customdomain.com**. Om du vill toocreate en post med jokertecken för alla underdomäner ange ' ***'. Detta kommer att omfatta alla underordnade domäner som **mail.customdomain.com**, **login.customdomain.com**, och **www.customdomain.com**.
   
    Om du vill toocreate en A-post för hello rotdomän, den kan visas som hello '**@**' symbol i din domänregistrators DNS-verktyg.
5. Ange hello IP-adressen för din molntjänst i hello angivna fält. Det här associerar hello domän posten som används i hello A-post med hello IP-adressen för din molndistribution för tjänsten.

Till exempel efter en post hello vidarebefordrar all trafik från **contoso.com** för**137.135.70.239**, hello IP-adressen för ditt distribuerade program:

| Värden namn/underdomän | IP-adress |
| --- | --- |
| @ |137.135.70.239 |

Det här exemplet visar att skapa en A-post för hello rotdomän. Om du inte vill toocreate ett jokertecken post toocover alla underdomäner som du vill ange ' ***' som hello underdomänen.

> [!WARNING]
> IP-adresser i Azure är dynamiska som standard. Vill du förmodligen toouse en [reserverad IP-adress](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure som din IP-adress inte ändras.
> 
> 

## <a name="next-steps"></a>Nästa steg
* [Hur tooManage Cloud Services](cloud-services-how-to-manage.md)
* [Hur tooMap innehåll till CDN tooa anpassad domän](../cdn/cdn-map-content-to-custom-domain.md)
* [Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).
* Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).
* Konfigurera [ssl-certifikat](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates hello subdomain with hello storage account]: #create-cname
[Azure-portalen]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
