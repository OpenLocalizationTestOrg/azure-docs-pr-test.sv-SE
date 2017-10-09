---
title: "aaaConfigure hello NewTek TriCaster kodare toosend en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur live tooconfigure hello Tricaster kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 8973181a-3059-471a-a6bb-ccda7d3ff297
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkd;anilmur
ms.openlocfilehash: 57dcf62a6a76b04e69f147a738be78ccb3c3ecdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-newtek-tricaster-encoder-toosend-a-single-bitrate-live-stream"></a>Använd hello NewTek TriCaster kodare toosend en direktsänd dataström med enkel bithastighet
> [!div class="op_single_selector"]
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Elemental Live](media-services-configure-elemental-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Det här avsnittet visar hur tooconfigure hello [NewTek TriCaster](http://newtek.com/products/tricaster-40.html) live kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding. Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Den här kursen visar hur toomanage Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget. Det här verktyget körs bara på Windows-dator. Om du är på Mac- eller Linux använda hello Azure portal toocreate [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).

> [!NOTE]
> När du använder Tricaster för att skicka i ett bidrag feed tooAMS kanaler som är aktiverade för live encoding, kan det finnas problem med ljud och i din direktsända händelse om du använder vissa funktioner i Tricaster, till exempel snabb klippa ut mellan feeds eller växla till/från pekdatorer . hello AMS-teamet arbetar på att åtgärda dessa problem fram till dess rekommenderas inte toouse dessa funktioner.
>
>

## <a name="prerequisites"></a>Krav
* [Skapa ett Azure Media Services-konto](media-services-portal-create-account.md)
* Se till att det finns en Strömningsslutpunkt som körs. Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md)
* Installera hello senaste versionen av hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.
* Starta verktyget hello och ansluta tooyour AMS-konto.

## <a name="tips"></a>Tips
* När det är möjligt använda ett inbyggt internet-anslutning.
* En bra tumregel samband med fastställandet av krav på bandbredd är toodouble hello strömning bithastighet. Detta är inte ett obligatoriskt krav, som det kan minska hello effekten av överbelastning.
* När du använder programvara baserat kodare, Stäng alla onödiga program.

## <a name="create-a-channel"></a>Skapa en kanal
1. Navigera i hello AMSE-verktyget toohello **Live** och högerklicka på inom området för hello-kanal. Välj **skapa kanal...** hello-menyn.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster1.png)

2. Ange ett Kanalnamn hello beskrivningsfältet är valfritt. Markera under inställningar för kanal **Standard** för hello Live Encoding alternativet med hello indata protokollet värdet för**RTMP**. Du kan lämna alla andra inställningar som är.

    Se till att hello **Start hello ny kanal nu** är markerad.

3. Klicka på **skapa kanal**.

   ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster2.png)

> [!NOTE]
> hello kanal kan ta upp till 20 minuter toostart.
>
>

Medan hello kanal startar kan du [konfigurera hello kodare](media-services-configure-tricaster-live-encoder.md#configure_tricaster_rtmp).

> [!IMPORTANT]
> Observera att faktureringen påbörjas så snart kanal blir klar att användas. Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_tricaster_rtmp></a>Konfigurera hello NewTek TriCaster kodaren
I den här självstudiekursen hello används följande inställningar för utdata. hello resten av det här avsnittet beskriver konfigurationssteg i detalj.

**Video**:

* Codec: H.264
* Profil: Hög (nivå 4.0)
* Bithastighet: 5000 kbit/s
* Keyframe: 2 sekunder (60 sekunder)
* RAM-hastighet: 30

**Ljud**:

* Codec: AAC (LC)
* Bithastighet: 192 kbit/s
* Samplingsfrekvens: 44.1 kHz

### <a name="configuration-steps"></a>Konfigurationssteg
1. Skapa en ny **NewTek TriCaster** projekt beroende på vilka inkommande videokällan används.
2. Hitta en gång i det aktuella projektet hello **dataströmmen** och klickar på hello växeln ikonen nästa tooit tooaccess hello dataströmmen configuration-menyn.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster3.png)
3. När du har öppnat hello-menyn, klickar du på **ny** under hello anslutning rubrik. När du uppmanas att ange hello anslutningstypen Välj **Adobe Flash**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster4.png)
4. Klicka på **OK**.
5. En profil för FMLE kan nu importeras genom att klicka på hello nedpilen under **strömning profil** och navigera för**Bläddra**.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster5.png)
6. Navigera toowhere hello konfigurerad FMLE profilen sparades.
7. Markera den och tryck på **OK**.

    När hello profil överförs fortsätta toohello nästa steg.
8. Hämta hello kanalens inkommande URL i ordning tooassign den toohello Tricaster **RTMP Endpoint**.

    Gå tillbaka toohello AMSE-verktyget och kontrollera status för slutförande av hello-kanal. När hello tillstånd har ändrats från **Start** för**kör**, du kan hämta hello indata-URL.

    När du kör hello kanal högerklickar du på hello Kanalnamn, Navigera ned toohover över **kopiera indata URL tooclipboard** och välj sedan **primära indata-URL**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster6.png)
9. Klistra in informationen i hello **plats** fältet under **Flash Server** inom hello Tricaster projekt. Tilldela ett stream-namn i hello **dataström-ID** fältet.

    Om strömmen information har lagts till toohello FMLE profil, även importeras toothis avsnittet genom att klicka på **importinställningarna**, navigera toohello sparade FMLE profil och klicka på **OK**. hello relevanta fält i Flash Server bör fyllas i automatiskt med hello information från FMLE.

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster7.png)
10. När du är klar klickar du på **OK** längst hello hello-skärmen. När video och ljud indata till hello Tricaster är klar börjar strömning tooAMS genom att klicka på hello **dataströmmen** knappen.

     ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster11.png)

> [!IMPORTANT]
> Innan du klickar på **dataströmmen**, du **måste** se till att hello kanalen är klar.
> Kontrollera också att inte tooleave hello kanal med tillståndet klart utan ett inkommande bidrag feed för > 15 minuter.
>
>

## <a name="test-playback"></a>Testa uppspelning
Navigera toohello AMSE-verktyget och högerklicka på hello kanal toobe testas. Hovra över hello-menyn **uppspelning hello Preview** och välj **med Azure Media Player**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster8.png)

Om hello dataströmmen visas i hello player har hello kodare varit korrekt konfigurerade tooconnect tooAMS.

Om ett fel tas emot måste hello kanal toobe Återställ kodare inställningar och justeras. Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.  

## <a name="create-a-program"></a>Skapa ett program
1. När kanalen uppspelning bekräftas, skapa ett program. Under hello **Live** i hello AMSE-verktyget, högerklickar du i området för hello-programmet och välj **Skapa nytt Program**.  

    ![tricaster](./media/media-services-tricaster-live-encoder/media-services-tricaster9.png)
2. Namnge programmet hello och, om det behövs, justera hello **längd** (som standard too4 timmar). Du kan också ange en lagringsplats eller lämna som hello standard.  
3. Kontrollera hello **Start hello programmet nu** rutan.
4. Klicka på **skapa Program**.  

    >[!NOTE]
    >Skapa en program tar mindre tid än att skapa en kanal.
        
5. När hello programmet körs kan bekräfta uppspelning genom att högerklicka på programmet hello och navigera för**uppspelning hello program** och sedan välja **med Azure Media Player**.  
6. När bekräftat, högerklicka på programmet hello igen och välj **kopiera hello utdata URL tooClipboard** (eller hämta den här informationen från hello **programmet information och inställningar** alternativet hello-menyn).

hello är nu redo toobe inbäddat i en spelare eller distribuerade tooan målgruppen för live visning.  

## <a name="troubleshooting"></a>Felsökning
Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.

## <a name="next-step"></a>Nästa steg
Granska sökvägarna för Media Services-utbildning.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
