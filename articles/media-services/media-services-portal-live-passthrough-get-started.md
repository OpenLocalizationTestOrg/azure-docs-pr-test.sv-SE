---
title: "aaaLive ström med lokala kodare med hjälp av hello Azure-portalen | Microsoft Docs"
description: "Den här självstudiekursen vägleder dig genom hello stegen för att skapa en kanal som är konfigurerad för en genomströmningsleverans."
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
ms.openlocfilehash: 1fb341e022f66f33903e13e07d3e84c0216cad77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-with-on-premises-encoders-using-hello-azure-portal"></a>Hur tooperform direktsänd strömning med lokala kodare med hjälp av hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-live-passthrough-get-started.md)
> * [.NET](media-services-dotnet-live-encode-with-onpremises-encoders.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

Den här självstudiekursen vägleder dig genom hello stegen för att använda hello Azure portal toocreate en **kanal** som är konfigurerad för en genomströmningsleverans. 

## <a name="prerequisites"></a>Krav
hello följande är obligatoriska toocomplete hello kursen:

* Ett Azure-konto. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/). 
* Ett Media Services-konto. toocreate Media Services-konto finns [hur tooCreate Media Services-konto](media-services-portal-create-account.md).
* En webbkamera. Till exempel [Telestream Wirecast-kodaren](http://www.telestream.net/wirecast/overview.htm).

Det rekommenderas starkt tooreview hello följande artiklar:

* [Azure Media Services RTMP-support och live-kodare](https://azure.microsoft.com/blog/2014/09/18/azure-media-services-rtmp-support-and-live-encoders/)
* [Översikt över liveuppspelning med Azure Media Services](media-services-manage-channels-overview.md)
* [Liveuppspelning med lokala kodare som skapar strömmar med flera bithastigheter](media-services-live-streaming-with-onprem-encoders.md)

## <a id="scenario"></a>Vanligt scenario för liveuppspelning
hello beskriver följande steg uppgifter som ingår i att skapa vanliga program för direktsänd strömning som använder kanaler som har konfigurerats för genomströmningsleverans. Den här kursen visar hur toocreate och hanterar en genomströmningskanal och direktsända händelser.

>[!NOTE]
>Se till att hello strömningsslutpunkt från vilken du vill att toostream innehåll hello **kör** tillstånd. 
    
1. Anslut en videokamera tooa dator. Starta och konfigurera en lokal direktsänd kodare som matar ut en RTMP- eller fragmenterad MP4-dataström i multibithastighet. Mer information finns i [Support och direktsända kodare för Azure Media Services RTMP](http://go.microsoft.com/fwlink/?LinkId=532824).
   
    Det här steget kan också utföras när du har skapat din kanal.
2. Skapa och starta en genomströmningskanal
3. Hämta hello kanal infognings-URL. 
   
    hello infognings-URL som används av hello livekodaren toosend hello dataströmmen toohello kanal.
4. Hämta hello kanalens förhandsgransknings-URL. 
   
    Använd den här URL: en tooverify att din kanal är tar emot hello direktsänd dataström.
5. Skapa en direktsänd händelse eller ett direktsänt program. 
   
    När du använder hello Azure-portalen, skapar skapar en direktsänd händelse även en tillgång. 

6. Starta hello händelsen eller programmet när du är klar toostart strömning och arkivering.
7. Du kan också kan hello livekodare vara signalerat toostart en annons. hello annonsen infogas i utdataströmmen hello.
8. Stoppa hello händelsen eller programmet när du vill toostop strömningen och arkiveringen hello händelsen.
9. Ta bort hello händelsen eller programmet (och ta eventuellt bort tillgången hello).     

> [!IMPORTANT]
> Granska [direktsänd strömning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md) toolearn om koncept och överväganden relaterade toolive strömning med lokala kodare och genomströmningskanaler.
> 
> 

## <a name="tooview-notifications-and-errors"></a>tooview meddelanden och fel
Om du vill tooview meddelanden och fel som genereras av hello Azure-portalen, klicka på ikonen för hello-meddelande.

![Meddelanden](./media/media-services-portal-passthrough-get-started/media-services-notifications.png)

## <a name="create-and-start-pass-through-channels-and-events"></a>Skapa och starta genomströmningskanaler och händelser
En kanal är associerad med händelser och program som gör att du toocontrol hello publicering och lagring av segment i en direktsänd dataström. Kanaler hanterar händelser. 

Du kan ange hello antal timmar som du vill att tooretain hello registreras innehåll för programmet hello genom att ange hello **Arkivfönster** längd. Det här värdet kan anges från minst 5 minuter tooa högst 25 timmar. Längd avgör också hello hur lång tid som klienter kan söka bakåt i tiden från hello aktuella direktsända positionen. Händelser kan köras under hello angiven tidsperiod, men innehåll som ligger bakom hello längd ignoreras kontinuerligt. Det här värdet för den här egenskapen avgör också hur länge hello klient manifest kan växa.

Varje händelse är associerad till en tillgång. toopublish hello-händelse, måste du skapa en positionerare för hello associerade tillgången. Med den här positioneraren kan toobuild en strömnings-URL som du kan tillhandahålla tooyour klienter.

En kanal har stöd för upp toothree samtidigt med händelser så att du kan skapa flera Arkiv för hello samma inkommande dataström. Detta ger dig toopublish och arkivera olika delar av en händelse efter behov. Till exempel är dina affärsbehov tooarchive 6 timmar av ett program, men toobroadcast sista 10 minuter. tooaccomplish detta, behöver du toocreate två program som körs samtidigt. Ett program är tooarchive 6 timmar av händelsen hello men hello programmet publiceras inte. hello kan inte ange tooarchive i 10 minuter och det här programmet har publicerats.

Du bör inte återanvända befintliga direktsända händelser. Skapa och starta istället en ny händelse för varje händelse.

Starta hello händelsen när du är klar toostart strömning och arkivering. Stoppa programmet hello när du vill toostop strömningen och arkiveringen hello händelsen. 

toodelete arkiverat innehåll, stoppa och ta bort hello händelsen och ta sedan bort hello associerade tillgången. En tillgång kan inte tas bort om den används av en händelse; hello händelsen måste tas bort först. 

När du stoppar och ta bort hello händelsen hello användare skulle vara kan toostream ditt arkiverade innehåll som en video på begäran för så länge du inte ta bort hello tillgången.

Om du vill tooretain hello arkiverat innehåll, men inte har den tillgänglig för strömning, tar du bort hello strömning lokaliserare.

### <a name="toouse-hello-portal-toocreate-a-channel"></a>toouse hello portal toocreate en kanal
Det här avsnittet visas hur toouse hello **Snabbregistrering** alternativet toocreate en genomströmningskanal.

Mer information om genomströmningskanaler finns i [Liveuppspelning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).

1. I hello [Azure-portalen](https://portal.azure.com/), Välj Azure Media Services-konto.
2. I hello **inställningar** -fönstret klickar du på **direktsänd strömning**. 
   
    ![Komma igång](./media/media-services-portal-passthrough-get-started/media-services-getting-started.png)
   
    Hej **direktsänd strömning** visas.
3. Klicka på **Snabbregistrering** toocreate en genomströmningskanal med hello RTMP-infogningsprotokollet.
   
    Hej **skapa en ny KANAL** visas.
4. Namnge hello ny kanal och klickar på **skapa**. 
   
    Detta skapar en genomströmningskanal med hello RTMP-infogningsprotokollet.

## <a name="create-events"></a>Skapa händelser
1. Välj en kanal toowhich som du vill tooadd en händelse.
2. Tryck på knappen **Direktsänd händelse**.

![Händelse](./media/media-services-portal-passthrough-get-started/media-services-create-events.png)

## <a name="get-ingest-urls"></a>Hämta infognings-URL:er
När hello kanalen har skapats kan du få infognings-URL: er som du kommer att ge toohello livekodaren. hello kodaren använder dessa URL: er tooinput en direktsänd dataström.

![Skapad](./media/media-services-portal-passthrough-get-started/media-services-channel-created.png)

## <a name="watch-hello-event"></a>Titta på hello händelse
toowatch hello-händelse, klickar du på **titta på** i hello Azure-portalen eller kopiera hello strömnings-URL och använder en valfri spelare önskat. 

![Skapad](./media/media-services-portal-passthrough-get-started/media-services-default-event.png)

Direktsänd händelse automatiskt hämta innehåll för konverterade tooon begäran när stoppades.

## <a name="clean-up"></a>Rensa
Mer information om genomströmningskanaler finns i [Liveuppspelning med lokala kodare som skapar dataströmmar i multibithastighet](media-services-live-streaming-with-onprem-encoders.md).

* En kanal kan stoppas endast när alla händelser eller program på hello kanalen har stoppats.  När hello kanalen har stoppats kan det inga avgifter. När du behöver toostart igen, det har hello samma infognings-URL så du inte behöver tooreconfigure din kodare.
* En kanal kan tas bort bara när alla direktsända händelser i hello kanalen har tagits bort.

## <a name="view-archived-content"></a>Visa arkiverat innehåll
När du stoppar och ta bort hello händelsen hello användare skulle vara kan toostream ditt arkiverade innehåll som en video på begäran för så länge du inte ta bort hello tillgången. En tillgång kan inte tas bort om den används av en händelse; hello händelsen måste tas bort först. 

toomanage dina tillgångar väljer **inställningen** och på **tillgångar**.

![Tillgångar](./media/media-services-portal-passthrough-get-started/media-services-assets.png)

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

