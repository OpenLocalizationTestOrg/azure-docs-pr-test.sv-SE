---
title: aaaIntegrate Azure DNS med Azure-resurser | Microsoft Docs
description: "Lär dig hur toouse Azure DNS längs tooprovide DNS för din Azure-resurser."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: b9b6f829513f0ad9da510190c75bc60dc7431545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-dns-tooprovide-custom-domain-settings-for-an-azure-service"></a>Använda Azure DNS tooprovide domäninställningarna för en Azure-tjänst

Azure DNS ger DNS för en anpassad domän för någon av dina Azure-resurser stöd för anpassade domäner eller som har ett fullständigt kvalificerat domännamn (FQDN). Ett exempel är du har en Azure webbapp och du vill att dina användare tooaccess den genom att antingen använda contoso.com eller www.contoso.com som ett fullständigt domännamn. Den här artikeln beskriver hur du konfigurerar din Azure-tjänst med Azure DNS för att använda anpassade domäner.

## <a name="prerequisites"></a>Krav

I ordning toouse Azure DNS för din anpassade domän, måste du först Delegera din domän tooAzure DNS. Besök [delegera en domän tooAzure DNS](./dns-delegate-domain-azure-dns.md) anvisningar för hur tooconfigure dina namnservrar för delegering. När din domän är delegerad tooyour Azure DNS-zonen, är du kan tooconfigure hello DNS-poster som behövs.

Du kan konfigurera en alternativa eller en anpassad domän för [Azure funktionen appar](#azure-function-app), [Azure IoT](#azure-iot), [offentliga IP-adresser](#public-ip-address), [Apptjänst (webbprogram)](#app-service-web-apps), [Blob storage](#blob-storage), och [Azure CDN](#azure-cdn).

## <a name="azure-function-app"></a>Azure Funktionsapp

tooconfigure en anpassad domän för appar i Azure-funktion, en CNAME-post skapas samt konfigurationen på hello funktionen själva appen.
 
Navigera för**andra** > **Funktionsapp** och välj appen funktion. Klicka på **plattformsfunktioner** och under **nätverk** klickar du på **anpassade domäner**.

![funktionen app-bladet](./media/dns-custom-domain/functionapp.png)

Observera hello aktuella url på hello **anpassade domäner** bladet för den här adressen används som hello-alias för hello DNS-poster som skapats.

![anpassade domäner-bladet](./media/dns-custom-domain/functionshostname.png)

Navigera tooyour DNS-zon och klicka på **+ postuppsättningen**. Fyll i följande information på hello hello **lägga till postuppsättning** bladet och klicka på **OK** toocreate den.

|Egenskap  |Värde  |Beskrivning  |
|---------|---------|---------|
|Namn     | myfunctionapp        | Det här värdet tillsammans med hello domännamnet är hello FQDN för hello domännamn.        |
|Typ     | CNAME        | Använd en CNAME-post använder ett alias.        |
|TTL-VÄRDE     | 1        | 1 används för 1 timme        |
|TTL-enhet     | Timmar        | Timmar används som hello tidmätning         |
|Alias     | adatumfunction.azurewebsites.NET        | hello DNS-namn som du skapar hello-alias för, i det här exemplet är som standard toohello funktionsapp hello adatumfunction.azurewebsites.net DNS-namnet.        |

Navigerar bakåt tooyour funktionsapp, klickar på **plattformsfunktioner**, och under **nätverk** klickar du på **anpassade domäner**, sedan under **värdnamn**klickar du på **+ Lägg till värdnamnet**.

På hello **lägga till värdnamnet** bladet ange hello CNAME-post i hello **värdnamn** textfält och klicka på **verifiera**. Om hello posten var kan toobe hitta, hello **lägga till värdnamnet** visas knappen. Klicka på **lägga till värdnamnet** tooadd hello-alias.

![värden namnet bladet Lägg till med funktionen appar](./media/dns-custom-domain/functionaddhostname.png)

## <a name="azure-iot"></a>Azure IoT

Azure IoT har inte alla anpassningar som behövs på själva hello-tjänsten. toouse en anpassad domän med en IoT-hubb krävs endast en CNAME-post pekar toohello resurser.

Navigera för**Sakernas Internet** > **IoT-hubb** och välj din IoT-hubb. På hello **översikt** bladet Obs hello FQDN för hello IoT-hubb.

![IoT Hub-bladet](./media/dns-custom-domain/iot.png)

Navigera tooyour DNS-zon och klicka på nästa, **+ postuppsättningen**. Fyll i följande information på hello hello **lägga till postuppsättning** bladet och klicka på **OK** toocreate den.


|Egenskap  |Värde  |Beskrivning  |
|---------|---------|---------|
|Namn     | myiothub        | Det här värdet tillsammans med hello domännamnet är hello FQDN för hello IoT-hubb.        |
|Typ     | CNAME        | Använd en CNAME-post använder ett alias.
|TTL-VÄRDE     | 1        | 1 används för 1 timme        |
|TTL-enhet     | Timmar        | Timmar används som hello tidmätning         |
|Alias     | adatumIOT.azure devices.net        | hello DNS-namn som du skapar hello-alias för, i det här exemplet är det hello adatumIOT.azure devices.net värdnamnet tillhandahålls av hello IoT-hubb.

När hello post har skapats, testa namnmatchning med hello CNAME post`nslookup`

## <a name="public-ip-address"></a>Offentlig IP-adress

tooconfigure en anpassad domän för tjänster som använder en offentlig IP-adressresurs, till exempel Programgateway, belastningsutjämnare, molnbaserad tjänst bör hanteraren för virtuella datorer, och klassiska virtuella datorer, en CNAME-post används.

Navigera för**nätverk** > **offentliga IP-adressen**, Välj hello offentliga IP-resurs och klickar på **Configuration**. Anteckna hello IP-adress visas.

![offentliga ip-bladet](./media/dns-custom-domain/publicip.png)

Navigera tooyour DNS-zon och klicka på **+ postuppsättningen**. Fyll i följande information på hello hello **lägga till postuppsättning** bladet och klicka på **OK** toocreate den.


|Egenskap  |Värde  |Beskrivning  |
|---------|---------|---------|
|Namn     | mywebserver        | Det här värdet tillsammans med hello domännamnet är hello FQDN för hello domännamn.        |
|Typ     | A        | Använd en A-post som hello resursen är en IP-adress.        |
|TTL-VÄRDE     | 1        | 1 används för 1 timme        |
|TTL-enhet     | Timmar        | Timmar används som hello tidmätning         |
|IP-adress     | <your ip address>       | hello offentlig IP-adress.|

![Skapa en A-post](./media/dns-custom-domain/arecord.png)

Köra när hello A-posten skapas `nslookup` toovalidate hello post matchas.

![offentliga IP-DNS-sökning](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>Apptjänst (webbprogram)

hello följande steg leder dig genom att konfigurera en anpassad domän för en app service webbapp.

Navigera för**webb- och Mobile** > **Apptjänst** och markera hello resurs som du konfigurerar ett eget domännamn och klicka på **anpassade domäner**.

Observera hello aktuella url på hello **anpassade domäner** bladet för den här adressen används som hello-alias för hello DNS-poster som skapats.

![anpassade domäner-bladet](./media/dns-custom-domain/url.png)

Navigera tooyour DNS-zon och klicka på **+ postuppsättningen**. Fyll i följande information på hello hello **lägga till postuppsättning** bladet och klicka på **OK** toocreate den.


|Egenskap  |Värde  |Beskrivning  |
|---------|---------|---------|
|Namn     | mywebserver        | Det här värdet tillsammans med hello domännamnet är hello FQDN för hello domännamn.        |
|Typ     | CNAME        | Använd en CNAME-post använder ett alias. Om hello resursen används en IP-adress, används en A-post.        |
|TTL-VÄRDE     | 1        | 1 används för 1 timme        |
|TTL-enhet     | Timmar        | Timmar används som hello tidmätning         |
|Alias     | webserver.azurewebsites.NET        | hello DNS-namn som du skapar hello-alias för, i det här exemplet är som standard toohello webbprogrammet hello webserver.azurewebsites.net DNS-namnet.        |


![Skapa en CNAME-post](./media/dns-custom-domain/createcnamerecord.png)

Gå tillbaka toohello app service som har konfigurerats för hello domännamn. Klicka på **anpassade domäner**, klicka på **värdnamn**. tooadd hello CNAME-post som du har skapat, klicka på **+ Lägg till värdnamnet**.

![bild 1](./media/dns-custom-domain/figure1.png)

När hello processen är klar kör **nslookup** toovalidate namnmatchning fungerar.

![bild 1](./media/dns-custom-domain/finalnslookup.png)

toolearn mer information om hur du mappar en anpassad domän tooApp tjänsten, besök [mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](../app-service-web/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Om du behöver toopurchase en anpassad domän kan du besöka [köpa ett anpassat domännamn för Azure Web Apps](../app-service-web/custom-dns-web-site-buydomains-web-app.md) toolearn mer om Apptjänst-domäner.

## <a name="blob-storage"></a>Blob Storage

hello följande steg leder dig genom att konfigurera en CNAME-post för blob storage-kontot med hello asverify metoden. Den här metoden garanterar det utan avbrott.

Navigera för**lagring** > **Lagringskonton**, Välj ditt lagringskonto och på **anpassad domän**. Anteckna hello FQDN under steg 2, det här värdet används toocreate hello första CNAME-post

![BLOB storage-domänen](./media/dns-custom-domain/blobcustomdomain.png)

Navigera tooyour DNS-zon och klicka på **+ postuppsättningen**. Fyll i följande information på hello hello **lägga till postuppsättning** bladet och klicka på **OK** toocreate den.


|Egenskap  |Värde  |Beskrivning  |
|---------|---------|---------|
|Namn     | asverify.mystorageaccount        | Det här värdet tillsammans med hello domännamnet är hello FQDN för hello domännamn.        |
|Typ     | CNAME        | Använd en CNAME-post använder ett alias.        |
|TTL-VÄRDE     | 1        | 1 används för 1 timme        |
|TTL-enhet     | Timmar        | Timmar används som hello tidmätning         |
|Alias     | asverify.adatumfunctiona9ed.BLOB.Core.Windows.NET        | hello DNS-namn som du skapar hello-alias för, i det här exemplet är det hello asverify.adatumfunctiona9ed.blob.core.windows.net DNS-namn som tillhandahålls av toohello standardkontot för lagring.        |

Gå tillbaka tooyour storage-konto genom att klicka **lagring** > **Lagringskonton**, Välj ditt lagringskonto och på **anpassad domän**. Typen i hello-alias du skapat utan hello asverify prefix kontroll i textrutan hello ** Använd indirekt CNAME-validering och på **spara**. När det här steget är klar kan returnera tooyour DNS-zon och skapa en CNAME-post utan hello asverify prefix.  Efter den, är säker toodelete hello CNAME-post med hello cdnverify prefix.

![BLOB storage-domänen](./media/dns-custom-domain/indirectvalidate.png)

Verifiera DNS-matchning genom att köra`nslookup`

Mer om hur du mappar en anpassad domän tooa blob storage-slutpunkt finns toolearn [konfigurera ett anpassat domännamn för din slutpunkt för Blob-lagring](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

hello följande steg leder dig genom att konfigurera en CNAME-post för en CDN-slutpunkt med hello cdnverify metoden. Den här metoden garanterar det utan avbrott.

Navigera för**nätverk** > **CDN profiler**, Välj CDN-profilen och på **slutpunkter** under **allmänna**.

Välj hello slutpunkt du arbetar med och klicka på **+ anpassad domän**. Obs hello **slutpunktens värdnamn** som det här värdet är hello som hello CNAME-post som pekar på.

![Anpassad domän för Innehållsleveransnätverk](./media/dns-custom-domain/endpointcustomdomain.png)

Navigera tooyour DNS-zon och klicka på **+ postuppsättningen**. Fyll i följande information på hello hello **lägga till postuppsättning** bladet och klicka på **OK** toocreate den.

|Egenskap  |Värde  |Beskrivning  |
|---------|---------|---------|
|Namn     | cdnverify.mycdnendpoint        | Det här värdet tillsammans med hello domännamnet är hello FQDN för hello domännamn.        |
|Typ     | CNAME        | Använd en CNAME-post använder ett alias.        |
|TTL-VÄRDE     | 1        | 1 används för 1 timme        |
|TTL-enhet     | Timmar        | Timmar används som hello tidmätning         |
|Alias     | cdnverify.adatumcdnendpoint.azureedge.NET        | hello DNS-namn som du skapar hello-alias för, i det här exemplet är det hello cdnverify.adatumcdnendpoint.azureedge.net DNS-namn som tillhandahålls av toohello standardkontot för lagring.        |

Gå tillbaka tooyour CDN-slutpunkten genom att klicka **nätverk** > **CDN profiler**, och välj CDN-profilen. Klicka på **+ anpassad domän** och ange CNAME-post-alias utan hello cdnverify prefix och klicka på **Lägg till**.

När det här steget är klar kan returnera tooyour DNS-zon och skapa en CNAME-post utan hello cdnverify prefix.  Efter den, är säker toodelete hello CNAME-post med hello cdnverify prefix. Mer information om CDN och hur tooconfigure en anpassad domän utan hello mellanliggande registreringssteget besöka [anpassad domän för kartan Azure CDN innehåll tooa](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[konfigurerar omvänd DNS för tjänster i Azure](dns-reverse-dns-for-azure-services.md).
