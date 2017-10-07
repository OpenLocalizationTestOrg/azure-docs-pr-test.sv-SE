---
title: "aaaConfigure hello Telestream Wirecast-kodaren toosend en direktsänd dataström med enkel bithastighet | Microsoft Docs"
description: "Det här avsnittet visar hur live tooconfigure hello Wirecast kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding. "
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 0d2f1e81-51a6-4ca9-894a-6dfa51ce4c70
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/05/2017
ms.author: juliako;cenkdin;anilmur
ms.openlocfilehash: e373f6c08232c652e65db584ded409c405d8cffe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-wirecast-encoder-toosend-a-single-bitrate-live-stream"></a>Använd hello Wirecast-kodaren toosend en direktsänd dataström med enkel bithastighet
> [!div class="op_single_selector"]
> * [Wirecast](media-services-configure-wirecast-live-encoder.md)
> * [Elemental Live](media-services-configure-elemental-live-encoder.md)
> * [Tricaster](media-services-configure-tricaster-live-encoder.md)
> * [FMLE](media-services-configure-fmle-live-encoder.md)
>
>

Det här avsnittet visar hur tooconfigure hello [Telestream Wirecast](http://www.telestream.net/wirecast/overview.htm) live kodare toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding.  Mer information finns i [arbeta med kanaler som är aktiverad tooPerform Live Encoding med Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

Den här kursen visar hur toomanage Azure Media Services (AMS) med Azure Media Services Explorer (AMSE)-verktyget. Det här verktyget körs bara på Windows-dator. Om du är på Mac- eller Linux använda hello Azure portal toocreate [kanaler](media-services-portal-creating-live-encoder-enabled-channel.md#create-a-channel) och [program](media-services-portal-creating-live-encoder-enabled-channel.md).

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

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast1.png)

2. Ange ett Kanalnamn hello beskrivningsfältet är valfritt. Markera under inställningar för kanal **Standard** för hello Live Encoding alternativet med hello indata protokollet värdet för**RTMP**. Du kan lämna alla andra inställningar som är.

    Se till att hello **Start hello ny kanal nu** är markerad.

3. Klicka på **skapa kanal**.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast2.png)

> [!NOTE]
> hello kanal kan ta upp till 20 minuter toostart.
>
>

Medan hello kanal startar kan du [konfigurera hello kodare](media-services-configure-wirecast-live-encoder.md#configure_wirecast_rtmp).

> [!IMPORTANT]
> Observera att faktureringen påbörjas så snart kanal blir klar att användas. Mer information finns i [kanalens tillstånd](media-services-manage-live-encoder-enabled-channels.md#states).
>
>

## <a id=configure_wirecast_rtmp></a>Konfigurera hello Telestream Wirecast-kodaren
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
1. Öppna hello Telestream Wirecast programmet hello datorn som används och konfigurera för RTMP strömning.
2. Konfigurera hello utdata genom att gå toohello **utdata** fliken och markera **utdata inställningar...** .

    Se till att hello **utdata mål** har angetts för**RTMP-Server**.
3. Klicka på **OK**.
4. Ange hello hello på sidan Inställningar **mål** fältet toobe **Azure Media Services**.

    hello kodning profil är markerat för**Azure H.264 720 p 16:9 (minst 1 280 x 720)**. toocustomize dessa inställningar väljer hello växeln ikonen toohello höger i hello listrutan och välj sedan **nya förinställningen**.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast3.png)
5. Konfigurera kodare förinställningar.

    Namnet hello förinställningen och söka efter hello följande rekommenderade inställningar:

    **Video**

   * Kodaren: MainConcept H.264
   * Bildrutor per sekund: 30
   * Genomsnittlig bithastighet: 5000 kbits per sekund (kan justeras utifrån nätverksbegränsningar)
   * Profil: Main
   * Nyckelbilden var: 60 ramar

    **Ljud**

   * Mål bithastighet: 192 kbits per sekund
   * Samplingsfrekvens: 44 100 kHz

     ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast4.png)
6. Tryck på **spara**.

    fältet för avkodning av hello har nu hello nyskapad profil kan väljas.

    Kontrollera att hello ny profil är markerad.
7. Hämta hello kanalens inkommande URL i ordning tooassign den toohello Wirecast **RTMP Endpoint**.

    Gå tillbaka toohello AMSE-verktyget och kontrollera status för slutförande av hello-kanal. När hello tillstånd har ändrats från **Start** för**kör**, du kan hämta hello indata-URL.

    När du kör hello kanal högerklickar du på hello Kanalnamn, Navigera ned toohover över **kopiera indata URL tooclipboard** och välj sedan **primära indata-URL**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast6.png)
8. I hello Wirecast **inställningar utdata** och klistra in informationen i hello **adress** i hello utdataavsnitt och tilldela ett namn på dataströmmen.

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast5.png)

1. Välj **OK**.
2. På hello huvudsakliga **Wirecast** skärmen, bekräfta indatakällor för video och ljud är klar och tryck sedan på **dataströmmen** i hello övre vänstra hörnet.

   ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast7.png)

> [!IMPORTANT]
> Innan du klickar på **dataströmmen**, du **måste** se till att hello kanalen är klar.
> Kontrollera också att inte tooleave hello kanal med tillståndet klart utan ett inkommande bidrag feed för > 15 minuter.
>
>

## <a name="test-playback"></a>Testa uppspelning

Navigera toohello AMSE-verktyget och högerklicka på hello kanal toobe testas. Hovra över hello-menyn **uppspelning hello Preview** och välj **med Azure Media Player**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast8.png)

Om hello dataströmmen visas i hello player har hello kodare varit korrekt konfigurerade tooconnect tooAMS.

Om ett fel tas emot måste hello kanal toobe Återställ kodare inställningar och justeras. Se hello [felsökning](media-services-troubleshooting-live-streaming.md) avsnittet som vägledning.  

## <a name="create-a-program"></a>Skapa ett program
1. När kanalen uppspelning bekräftas, skapa ett program. Under hello **Live** i hello AMSE-verktyget, högerklickar du i området för hello-programmet och välj **Skapa nytt Program**.  

    ![wirecast](./media/media-services-wirecast-live-encoder/media-services-wirecast9.png)
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

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
