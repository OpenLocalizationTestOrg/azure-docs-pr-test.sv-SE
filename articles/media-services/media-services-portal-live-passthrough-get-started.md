---
title: "Liveuppspelning med lokala kodare med hjälp av Azure Portal | Microsoft Docs"
description: "Den här vägledningen visar dig stegen för att skapa en kanal som är konfigurerad för en genomströmningsleverans."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 6f4acd95-cc64-4dd9-9e2d-8734707de326
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 6939e3b31c3c1b514df4c559c2d9408fce122a4e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-perform-live-streaming-with-on-premises-encoders-using-the-azure-portal"></a>Så här utför du liveuppspelning med lokala kodare med hjälp av Azure Portal
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-live-passthrough-get-started.md)
> * [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

Den här vägledningen visar dig stegen för att använda Azure-portalen för att skapa en **kanal** som är konfigurerad för en genomströmningsleverans. 

## <a name="prerequisites"></a>Krav
Följande krävs för att kunna genomföra vägledningen:

* Ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Ett Media Services-konto. Information om hur du skapar ett Media Services-konto finns i [Så här skapar du ett Media Services-konto](media-services-portal-create-account.md).
* En webbkamera. Till exempel [Telestream Wirecast-kodaren](http://www.telestream.net/wirecast/overview.htm).

Vi rekommenderar att du tittar närmare på följande artiklar:

* [Azure Media Services RTMP-support och live-kodare](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
* [Översikt över liveuppspelning med Azure Media Services](media-services-manage-channels-overview.md)
* [Liveuppspelning med lokala kodare som skapar strömmar med flera bithastigheter](media-services-live-streaming-with-onprem-encoders.md)

## <a id="scenario"></a>Vanligt scenario för liveuppspelning
Följande steg beskriver uppgifter som ingår i att skapa vanliga appar för direktsänd strömning som använder kanaler som har konfigurerats för genomströmningsleverans. Den här vägledningen visar hur du skapar och hanterar en genomströmningskanal och direktsända händelser.

>[!NOTE]
>Kontrollera att slutpunkten för direktuppspelning som du vill spela upp innehåll från har tillståndet **Körs**. 
    
1. Anslut en videokamera till en dator. Starta och konfigurera en lokal direktsänd kodare som matar ut en RTMP- eller fragmenterad MP4-dataström i multibithastighet. Mer information finns i [Support och direktsända kodare för Azure Media Services RTMP](http://go.microsoft.com/fwlink/?LinkId=532824).
   
    Det här steget kan också utföras när du har skapat din kanal.
2. Skapa och starta en genomströmningskanal
3. Hämta kanalens infognings-URL. 
   
    Infognings-URL:en används av livekodaren för att skicka dataströmmen till kanalen.
4. Hämta kanalens förhandsgransknings-URL. 
   
    Använd denna URL för att kontrollera att din kanal tar emot den direktsända dataströmmen korrekt.
5. Skapa en direktsänd händelse eller ett direktsänt program. 
   
    När du använder Azure-portalen, skapas även en tillgång då du skapar en direktsänd händelse. 

6. Starta händelsen eller programmet när du är redo att påbörja strömning och arkivering.
7. Som alternativ kan den direktsända kodaren få signal om att starta en annons. Annonsen infogas i utdataströmmen.
8. Stoppa händelsen eller programmet när du vill stoppa strömningen och arkiveringen av händelsen.
9. Ta bort händelsen eller programmet (och ta eventuellt bort tillgången).     

> [!IMPORTANT]
> Titta närmare på [Liveuppspelning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md) för att lära dig mer om koncept och överväganden som rör liveuppspelning med lokala kodare och genomströmningskanaler.
> 
> 

## <a name="to-view-notifications-and-errors"></a>Visa meddelanden och fel
Klicka på meddelandeikonen om du vill visa meddelanden och fel som genereras av Azure-portalen.

![Meddelanden](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a>Skapa och starta genomströmningskanaler och händelser
En kanal är associerad med händelser och program som gör att du kan styra publicering och lagring av segment i en direktsänd dataström. Kanaler hanterar händelser. 

Du kan ange det antal timmar som du vill behålla inspelat innehåll för programmet genom att ställa in längden för **Arkivfönster**. Det här värdet kan anges från minst 5 minuter till högst 25 timmar. Även arkivfönstrets längd påverkar den maximala tid som klienter kan söka bakåt i tiden från den aktuella direktsända positionen. Händelser kan köras under den angivna tidsperioden men innehåll som understiger fönsterlängden ignoreras kontinuerligt. Värdet för den här egenskapen avgör också hur länge klientmanifesten kan växa.

Varje händelse är associerad till en tillgång. För att publicera händelsen måste du skapa en OnDemand-lokaliserare för den associerade tillgången. Med den här lokaliseraren kan du skapa en strömnings-URL som du kan tillhandahålla till dina klienter.

En kanal har stöd för upp till tre händelser som körs samtidigt så du kan skapa flera arkiv för samma inkommande dataström. På så sätt kan du publicera och arkivera olika delar av en händelse efter behov. Ditt verksamhetsbehov kan till exempel vara att arkivera 6 timmar av ett program, men bara sända 10 minuter. För att åstadkomma detta måste du skapa två program som körs samtidigt. Ett program ställs in för att arkivera 6 timmar av händelsen, men programmet publiceras inte. Det andra programmet ställs in för att arkivera i 10 minuter och det här programmet publiceras.

Du bör inte återanvända befintliga direktsända händelser. Skapa och starta istället en ny händelse för varje händelse.

Starta händelsen när du är redo att påbörja strömning och arkivering. Stoppa programmet när du vill stoppa strömningen och arkiveringen av händelsen. 

Om du vill ta bort arkiverat innehåll, stoppar du och tar bort händelsen och tar sedan bort associerade tillgången. En tillgång kan inte tas bort om den används av en händelse. Händelsen måste tas bort först. 

Även efter att du stoppat och tagit bort händelsen skulle användarna kunna strömma ditt arkiverade innehåll som en video på begäran så länge du inte tar bort tillgången.

Om du vill behålla det arkiverade innehållet, men inte att det ska vara tillgängligt för strömning, tar du bort strömningslokaliseraren.

### <a name="to-use-the-portal-to-create-a-channel"></a>Använda portalen för att skapa en kanal
Detta avsnitt visar hur du använder alternativet **Snabbregistrering** för att skapa en genomströmningskanal.

Mer information om genomströmningskanaler finns i [Liveuppspelning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).

1. Välj ditt Azure Media Services-konto i [Azure-portalen](https://portal.azure.com/).
2. I fönstret **Inställningar** klickar du på **Direktsänd strömning**. 
   
    ![Komma igång](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    Fönstret **Direktsänd strömning** visas.
3. Klicka på **Snabbregistrering** för att skapa en genomströmningskanal med RTMP-infogningsprotokollet.
   
    Fönstret **SKAPA EN NY KANAL** visas.
4. Namnge den nya kanalen och klicka på **Skapa**. 
   
    Detta skapar en genomströmningskanal med RTMP-infogningsprotokollet.

## <a name="create-events"></a>Skapa händelser
1. Välj en kanal till vilken du vill lägga till en händelse.
2. Tryck på knappen **Direktsänd händelse**.

![Händelse](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a>Hämta infognings-URL:er
När kanalen har skapats kan du få infognings-URL:er som du tillhandahåller till livekodaren. Kodaren använder dessa URL:er för att mata in en direktsänd dataström.

![Skapad](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-the-event"></a>Titta på händelsen
För att titta på händelsen klickar du på **Titta på** i Azure-portalen eller kopierar strömnings-URL:en och använder en valfri spelare. 

![Skapad](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Direktsända händelser konverteras automatiskt till innehåll på begäran när de stoppas.

## <a name="clean-up"></a>Rensa
Mer information om genomströmningskanaler finns i [Liveuppspelning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).

* En kanal kan stoppas endast när alla händelser eller program i kanalen har stoppats.  När kanalen har stoppats medför den inga avgifter. När du vill starta den igen har den samma infognings-URL så att du inte behöver konfigurera om din kodare.
* En kanal kan bara tas bort när alla direktsända händelser i kanalen har tagits bort.

## <a name="view-archived-content"></a>Visa arkiverat innehåll
Även efter att du stoppat och tagit bort händelsen skulle användarna kunna strömma ditt arkiverade innehåll som en video på begäran så länge du inte tar bort tillgången. En tillgång kan inte tas bort om den används av en händelse. Händelsen måste tas bort först. 

För att hantera dina tillgångar väljer du **Inställning** och klickar på **Tillgångar**.

![Tillgångar](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

