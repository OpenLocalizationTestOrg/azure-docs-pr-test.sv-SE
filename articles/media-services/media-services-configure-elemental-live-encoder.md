---
title: "aaaConfigure hello grundämne Live-kodaren toosend en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur hello tooconfigure grundämne Live-kodaren toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding."
services: media-services
documentationcenter: 
author: cenkdin
manager: cfowler
editor: 
ms.assetid: 9c6bf6a9-6273-4fdd-9477-f0e565280b5b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: cenkd;anilmur;juliako
ms.openlocfilehash: 9a5de6189bfb123768a9da038b8c8db69cf85e91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-elemental-live-encoder-toosend-a-single-bitrate-live-stream"></a>Använd hello grundämne Live-kodaren toosend en direktsänd dataström med enkel bithastighet
> [!div class="op_single_selector"]
> * [Elemental Live](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Det här avsnittet visar hur tooconfigure hello [grundämne Live](http://www.elementaltechnologies.com/products/elemental-live) kodare toosend enkel bithastighet strömma tooAMS kanaler som är aktiverade för live encoding.  Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Den här kursen visar hur toomanage Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget. Det här verktyget körs bara på Windows-dator. Om du är på Mac- eller Linux använda hello Azure portal toocreate [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).

## <a name="prerequisites"></a>Krav
* Måste ha kunskaper för att använda grundämne Live web interface toocreate direktsända händelser.
* [Skapa ett Azure Media Services-konto](media-services-portal-create-account.md)
* Se till att det finns en Strömningsslutpunkt som körs. Mer information finns i [hanterar Strömningsslutpunkter i ett Media Services-konto](media-services-portal-manage-streaming-endpoints.md).
* Installera hello senaste versionen av hello [AMSE](https://github.com/Azure/Azure-Media-Services-Explorer) verktyget.
* Starta verktyget hello och ansluta tooyour AMS-konto.

## <a name="tips"></a>Tips
* När det är möjligt använda ett inbyggt internet-anslutning.
* En bra tumregel samband med fastställandet av krav på bandbredd är toodouble hello strömning bithastighet. Detta är inte ett obligatoriskt krav, som det kan minska hello effekten av överbelastning.
* När du använder programvara baserat kodare, Stäng alla onödiga program.

## <a name="elemental-live-with-rtp-ingest"></a>Mata in grundämne Live med RTP
Det här avsnittet visar hur tooconfigure hello grundämne Live kodare som skickar en enkel bithastighet direktsänd dataström över RTP.  Mer information finns i [MPEG-TS dataströmmen över RTP](media-services-manage-live-encoder-enabled-channels.md#channel).

### <a name="create-a-channel"></a>Skapa en kanal

1. Navigera i hello AMSE-verktyget toohello **Live** och högerklicka på inom området för hello-kanal. Välj **skapa kanal...** hello-menyn.

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental1.png)

2. Ange ett Kanalnamn hello beskrivningsfältet är valfritt. Markera under inställningar för kanal **Standard** för hello Live Encoding alternativet med hello indata protokollet värdet för**RTP (MPEG-TS)**. Du kan lämna alla andra inställningar som är.

    Se till att hello **Start hello ny kanal nu** är markerad.

3. Klicka på **skapa kanal**.

   ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental12.png)

> [!NOTE]
> hello kanal kan ta upp till 20 minuter toostart.
>
>

Medan hello kanal startar kan du [konfigurera hello kodare](media-services-configure-elemental-live-encoder.md#configure_elemental_rtp).

> [!IMPORTANT]
> Observera att faktureringen påbörjas så snart kanal blir klar att användas. Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

### <a id=configure_elemental_rtp></a>Konfigurera hello grundämne Live kodaren
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

#### <a name="configuration-steps"></a>Konfigurationssteg
1. Navigera toohello **grundämne Live** webbgränssnittet och ställa in hello kodare för **UDP/TS** strömning.
2. När du har skapat en ny händelse rulla nedåt toohello utdata grupper och Lägg till hello **UDP/TS** utdata grupp.
3. Skapa en ny utdata genom att välja **ny ström** och sedan klicka på **lägga till utdata**.  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental13.png)

   > [!NOTE]
   > Det rekommenderas att grundämne hello-händelsen har hello tidskod som angetts för ”systemklockan” toohelp hello kodare återansluta i hello fall av ett stream-fel.
   >
   >
4. Nu när hello utdata har skapats, klicka på **lägga till ett flöde**. inställningar för hello utdata kan nu konfigureras.
5. Rulla nedåt toohello ”dataströmmen 1” just har skapat, klicka på hello **Video** fliken hello vänster och expanderar hello **Avancerat** inställningar.

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental4.png)

    Medan grundämne Live har ett stort antal tillgängliga anpassa, hello följande inställningar rekommenderas för att komma igång med direktuppspelning tooAMS.

   * Lösning: minst 1 280 x 720
   * Ramhastighet: 30
   * GOP storlek: 60 ramar
   * Sammanfläta läge: progressiv
   * Bithastighet: 5000000 bit/s (Detta kan ställas in baserat på begränsningar)

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental5.png)

1. Hämta hello kanal indata-URL.

    Gå tillbaka toohello AMSE-verktyget och kontrollera status för slutförande av hello-kanal. När hello tillstånd har ändrats från **Start** för**kör**, du kan hämta hello indata-URL.

    När du kör hello kanal högerklickar du på hello Kanalnamn, Navigera ned toohover över **kopiera indata URL tooclipboard** och välj sedan **primära indata-URL**.  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental6.png)
2. Klistra in informationen i hello **primära mål** i hello Elemental. Alla andra inställningar kan förbli hello standard.

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental14.png)

    Upprepa dessa steg med hello sekundära indata-URL genom att skapa en separat flik ”utdata” för direktuppspelning av UDP/TS för extra redundans.
3. Klicka på **skapa** (om en ny händelse skapades) eller **uppdatering** (om du redigerar en befintlig händelse) och gå sedan vidare toostart hello kodare.

> [!IMPORTANT]
> Innan du klickar på **starta** på grundämne Live Webbgränssnitt för hello du **måste** se till att hello kanalen är klar.
> Kontrollera också att inte tooleave hello kanal i klar att användas utan en händelse för > 15 minuter.
>
>

När hello dataströmmen har körts i 30 sekunder, gå tillbaka toohello AMSE-verktyget och testa uppspelning.  

### <a name="test-playback"></a>Testa uppspelning

Navigera toohello AMSE-verktyget och högerklicka på hello kanal toobe testas. Hovra över hello-menyn **uppspelning hello Preview** och välj **med Azure Media Player**.  

    ![Elemental](./media/media-services-elemental-live-encoder/media-services-elemental8.png)

Om hello dataströmmen visas i hello player har hello kodare varit korrekt konfigurerade tooconnect tooAMS.

Om ett fel tas emot måste hello kanal toobe Återställ kodare inställningar och justeras. Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.   

### <a name="create-a-program"></a>Skapa ett program
1. När kanalen uppspelning bekräftas, skapa ett program. Under hello **Live** i hello AMSE-verktyget, högerklickar du i området för hello-programmet och välj **Skapa nytt Program**.  

    ![Grundämne](./media/media-services-elemental-live-encoder/media-services-elemental9.png)
2. Namnge programmet hello och, om det behövs, justera hello **längd** (som standard too4 timmar). Du kan också ange en lagringsplats eller lämna som hello standard.  
3. Kontrollera hello **Start hello programmet nu** rutan.
4. Klicka på **skapa Program**.  

    >[!NOTE]
    > Skapa en program tar mindre tid än att skapa en kanal.   
      
5. När hello programmet körs kan bekräfta uppspelning genom att högerklicka på programmet hello och navigera för**uppspelning hello program** och sedan välja **med Azure Media Player**.  
6. När bekräftat, högerklicka på programmet hello igen och välj **kopiera hello utdata URL tooClipboard** (eller hämta den här informationen från hello **programmet information och inställningar** alternativet hello-menyn).

hello är nu redo toobe inbäddat i en spelare eller distribuerade tooan målgruppen för live visning.  

## <a name="troubleshooting"></a>Felsökning
Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
