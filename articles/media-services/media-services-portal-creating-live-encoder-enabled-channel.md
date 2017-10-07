---
title: "aaaHow tooperform direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar med hello Azure-portalen | Microsoft Docs"
description: "Den här kursen får du hello steg som krävs för att skapa en kanal som tar emot en direktsänd dataström med enkel bithastighet och kodar den dataström med multibithastighet toomulti med hello Azure-portalen."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 504f74c2-3103-42a0-897b-9ff52f279e23
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 963a25b8ba4683a2ce34d9fb0e19499874b4707c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooperform-live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams-with-hello-azure-portal"></a>Hur tooperform direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar med hello Azure-portalen
> [!div class="op_single_selector"]
> * [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [REST-API](https://docs.microsoft.com/rest/api/media/operations/channel)
> 
> 

Den här självstudiekursen vägleder dig genom hello stegen för att skapa en **kanal** som tar emot en direktsänd dataström med enkel bithastighet och kodar den dataström med multibithastighet toomulti.

> [!NOTE]
> Mer information finns i Närliggande tooChannels som är aktiverade för live encoding [direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar](media-services-manage-live-encoder-enabled-channels.md).
> 
> 

## <a name="common-live-streaming-scenario"></a>Vanligt scenario för direktsänd strömning
hello följande är allmänna steg som ingår i att skapa vanliga program för direktsänd strömning.

> [!NOTE]
> Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar. Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder.
> 
> 

1. Anslut en videokamera tooa dator. Starta och konfigurera en lokal livekodare som kan mata ut en dataström med enkel bithastighet i något av hello följande protokoll: RTMP, Smooth Streaming eller RTP (MPEG-TS). Mer information finns i [Support och livekodare för Azure Media Services RTMP](http://go.microsoft.com/fwlink/?LinkId=532824).
   
    Det här steget kan också utföras när du har skapat din kanal.
2. Skapa och starta en kanal. 
3. Hämta hello kanal infognings-URL. 
   
    hello infognings-URL som används av hello livekodaren toosend hello dataströmmen toohello kanal.
4. Hämta hello kanalens förhandsgransknings-URL. 
   
    Använd den här URL: en tooverify att din kanal är tar emot hello direktsänd dataström.
5. Skapa en händelse/ett program (som också kommer att skapa en tillgång). 
6. Publicera hello-händelse (som skapar en positionerare för hello associerade tillgången).    
7. Starta hello händelsen när du är klar toostart strömning och arkivering.
8. Du kan också kan hello livekodare vara signalerat toostart en annons. hello annonsen infogas i utdataströmmen hello.
9. Stoppa hello händelsen när du vill toostop strömningen och arkiveringen hello händelsen.
10. Ta bort hello händelser (och ta eventuellt bort tillgången hello).   

## <a name="in-this-tutorial"></a>I den här självstudien
I den här kursen är hello Azure-portalen används tooaccomplish hello följande uppgifter: 

1. Skapa en kanal som är aktiverade tooperform live encoding.
2. Get hello infognings-URL: en i ordning toosupply den toolive kodare. hello live Encoding använder denna URL tooingest hello dataström till hello kanalen.
3. Skapa en händelse/ett program (och en tillgång).
4. Publicera hello tillgången och hämta strömnings-URL: er.  
5. Spela upp ditt innehåll.
6. Rensa.

## <a name="prerequisites"></a>Krav
hello följande är obligatoriska toocomplete hello kursen.

* toocomplete den här självstudiekursen kommer du behöver ett Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. 
  Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).
* Ett Media Services-konto. toocreate Media Services-konto finns [skapa konto](media-services-portal-create-account.md).
* En webbkamera och en kodare som kan skicka en direktsänd dataström i enkel bithastighet.

## <a name="create-a-channel"></a>Skapa en kanal
1. I hello [Azure-portalen](https://portal.azure.com/), Välj Media Services och klicka sedan på namnet på ditt Media Services.
2. Välj **Liveuppspelning**.
3. Välj **Skapa anpassad**. Det här alternativet gör att du kan skapa en kanal som är aktiverad för Live Encoding.
   
    ![Skapa en kanal](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
4. Klicka på **Inställningar**.
   
   1. Välj hello **Live Encoding** kanaltyp. Den här typen anger att du vill toocreate en kanal som är aktiverad för live encoding. Att innebär hello inkommande enkel bithastighet dataströmmen skickas toohello kanalen och kodas till en dataström med multibithastighet med hjälp av livekodarens angivna inställningar. Mer information finns i [direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar](media-services-manage-live-encoder-enabled-channels.md). Klicka på OK.
   2. Ange ett kanalnamn.
   3. Klicka på OK längst ned hello hello-skärmen.
5. Välj hello **infognings** fliken.
   
   1. På den här sidan kan du välja ett strömningsprotokoll. För hello **Live Encoding** kanaltypen, giltigt protokollalternativ är:
      
      * Fragmenterad MP4 med enkel bithastighet (Smooth Streaming)
      * RTMP med enkel bithastighet
      * RTP (MPEG-TS): MPEG-2-transportström över RTP.
        
        Detaljerad information om varje protokoll finns [direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar](media-services-manage-live-encoder-enabled-channels.md).
        
        Du kan inte ändra hello protokollet alternativet när hello kanalen eller dess associerade händelser eller program körs. Om du behöver olika protokoll får du skapa separata kanaler för varje strömningsprotokoll.  
   2. Du kan använda en IP-begränsning på hello mata in. 
      
       Du kan definiera hello IP-adresser som är tillåtna tooingest en video toothis-kanal. Tillåtna IP-adresser kan anges som antingen en enskild IP-adress (t.ex. ”10.0.0.1”), ett IP-intervall med en IP-adress och en CIDR-undernätsmask (t.ex. ”10.0.0.1/22”) eller ett IP-intervall med en IP-adress och en CIDR-undernätsmask med punktavgränsad decimalform (t.ex. '10.0.0.1(255.255.252.0)').
      
       Om inga IP-adresser har angetts och det saknas regeldefinitioner kommer ingen IP-adress att tillåtas. tooallow IP-adresser, skapa en regel och ange 0.0.0.0/0.
6. På hello **Preview** fliken gäller IP-begränsning på hello preview.
7. På hello **kodning** anger hello kodning förinställda. 
   
    För närvarande hello endast system kan du välja förinställda är **Default 720p**. toospecify en anpassad förinställningen, öppna ett supportärende för Microsoft. Ange sedan hello namnet på hello förinställning som skapas automatiskt. 

> [!NOTE]
> För närvarande kan hello kanalstarten in too30 i minuter. Kanalåterställning kan ta upp too5 minuter.
> 
> 

När du har skapat hello kanal kan du klicka på hello kanal och välja **inställningar** där du kan visa dina kanalkonfigurationer. 

Mer information finns i [direktsänd strömning med Azure Media Services toocreate multibithastighet dataströmmar](media-services-manage-live-encoder-enabled-channels.md).

## <a name="get-ingest-urls"></a>Hämta infognings-URL:er
När hello kanalen har skapats kan du få infognings-URL: er som du kommer att ge toohello livekodaren. hello kodaren använder dessa URL: er tooinput en direktsänd dataström.

![ingesturls](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)

## <a name="create-and-manage-events"></a>Skapa och hantera händelser
### <a name="overview"></a>Översikt
En kanal är associerad med händelser och program som gör att du toocontrol hello publicering och lagring av segment i en direktsänd dataström. Kanaler hanterar händelser/program. hello är relationen mellan kanal och Program mycket lik tootraditional media där en kanal har en konstant ström av innehåll och ett program är begränsade toosome tidsinställd händelse på kanalen.

Du kan ange hello antal timmar som du vill att tooretain hello registreras innehåll för hello händelsen genom att ange hello **Arkivfönster** längd. Det här värdet kan anges från minst 5 minuter tooa högst 25 timmar. Längd avgör också hello hur lång tid som klienter kan söka bakåt i tiden från hello aktuella direktsända positionen. Händelser kan köras under hello angiven tidsperiod, men innehåll som ligger bakom hello längd ignoreras kontinuerligt. Det här värdet för den här egenskapen avgör också hur länge hello klient manifest kan växa.

Varje händelse är associerad till en tillgång. toopublish hello händelse måste du skapa en positionerare för hello associerade tillgången. Med den här positioneraren kan toobuild en strömnings-URL som du kan tillhandahålla tooyour klienter.

En kanal har stöd för upp toothree samtidigt med händelser så att du kan skapa flera Arkiv för hello samma inkommande dataström. Detta ger dig toopublish och arkivera olika delar av en händelse efter behov. Till exempel är dina affärsbehov tooarchive 6 timmar av händelsen, men toobroadcast sista 10 minuter. tooaccomplish detta, behöver du toocreate två samtidigt köra händelsen. En händelse är tooarchive 6 timmar av händelsen hello men hello programmet publiceras inte. hello andra händelsen är uppsättningen tooarchive i 10 minuter och det här programmet har publicerats.

Du bör inte återanvända befintliga program för nya händelser. Skapa och starta istället ett nytt program för varje händelse.

Starta en händelsen eller programmet när du är klar toostart strömning och arkivering. Stoppa hello händelsen när du vill toostop strömningen och arkiveringen hello händelsen. 

toodelete arkiverat innehåll, stoppa och ta bort hello händelsen och ta sedan bort hello associerade tillgången. En tillgång kan inte tas bort om den används av hello händelse. hello händelsen måste tas bort först. 

När du stoppar och ta bort hello händelsen hello användare skulle vara kan toostream ditt arkiverade innehåll som en video på begäran för så länge du inte ta bort hello tillgången.

Om du vill tooretain hello arkiverat innehåll, men inte har den tillgänglig för strömning, tar du bort hello strömning lokaliserare.

### <a name="createstartstop-events"></a>Skapa/Starta/Stoppa händelser
När du har hello dataströmmen kan väl flödar till kanalen hello du börja hello strömning händelsen genom att skapa en tillgång, ett Program och en Strömningspositionerare. Detta arkiverar dataströmmen hello och göra den tillgänglig tooviewers via hello Streaming slutpunkt. 

>[!NOTE]
>När AMS-kontot skapas en **standard** strömningsslutpunkt har lagts till tooyour konto i hello **stoppad** tillstånd. toostart strömning ditt innehåll och dra nytta av dynamisk paketering och dynamisk kryptering hello strömningsslutpunkt som du vill toostream innehåll har toobe i hello **kör** tillstånd. 

Det finns två sätt toostart händelse: 

1. Från hello **kanal** sidan genom att trycka på **direktsänd händelse** tooadd en ny händelse.
   
    Ange: händelsens namn, tillgångsnamn, arkivfönster och krypteringsalternativ.
   
    ![createprogram](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)
   
    Om du lämnar **publicera den här direktsänd händelse nu** markerad hello händelse hello PUBLISHING-URL: er skapas.
   
    Du kan trycka på **starta**när du är klar toostream hello händelse.
   
    När du startar hello händelsen du kan trycka på **titta på** toostart spela upp hello innehåll.
2. Du kan också använda en genväg och trycka på **gå Live** hello-knappen **kanal** sidan. Detta skapar en standardtillgång, ett program och en strömningspositionerare.
   
    hello händelse heter **standard** och hello arkivfönstret har angetts too8 timmar.

Du kan titta på hello publicerade händelsedata från hello **direktsänd händelse** sidan. 

Om du klickar på **Off Air**, stoppas alla live-händelser. 

## <a name="watch-hello-event"></a>Titta på hello händelse
toowatch hello-händelse, klickar du på **titta på** i hello Azure-portalen eller kopiera hello strömnings-URL och använder en valfri spelare önskat. 

![Skapad](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

Direktsänd händelse konverteras automatiskt händelser tooon begäran innehåll när stoppades.

## <a name="clean-up"></a>Rensa
Följ hello följande procedur om du är klar med strömningen av händelser och vill tooclean hello resurser som etablerades tidigare.

* Stoppa sändningen hello dataströmmen från kodaren hello.
* Stoppa hello-kanal. När hello kanalen har stoppats kan kommer det inga avgifter. När du behöver toostart igen, det har hello samma infognings-URL så du inte behöver tooreconfigure din kodare.
* Du kan avbryta din Strömningsslutpunkt om du inte vill toocontinue tooprovide hello arkivet för din direktsända händelse som en dataström på begäran. Om hello kanalen har stoppats, kommer det inga avgifter.

## <a name="view-archived-content"></a>Visa arkiverat innehåll
När du stoppar och ta bort hello händelsen hello användare skulle vara kan toostream ditt arkiverade innehåll som en video på begäran för så länge du inte ta bort hello tillgången. En tillgång kan inte tas bort om den används av en händelse; hello händelsen måste tas bort först. 

toomanage dina tillgångar väljer **inställningen** och på **tillgångar**.

![Tillgångar](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

## <a name="considerations"></a>Överväganden
* Hej max är för närvarande rekommenderas varaktighet för en direktsänd händelse 8 timmar. Kontakta amslived på Microsoft.com om du behöver toorun en kanal under längre perioder.
* Se till att hello strömningsslutpunkt som du vill toostream ditt innehåll hello **kör** tillstånd.

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

