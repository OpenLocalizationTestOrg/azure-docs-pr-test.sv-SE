---
title: "aaaTroubleshooting guide för direktsänd strömning | Microsoft Docs"
description: "Det här avsnittet innehåller förslag på hur tootroubleshoot bor strömmande problem."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 3a7f6c1d-ce57-4fa4-a7a6-edb526b3ffbf
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 8549bae947ff3b225ce624220d1e48b63f90208c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-live-streaming"></a>Felsökningsguide för liveuppspelning
Det här avsnittet innehåller förslag på hur tootroubleshoot vissa direktsänd strömning problem.

## <a name="issues-related-tooon-premises-encoders"></a>Problem med tooon lokala kodare
Det här avsnittet innehåller förslag på hur tootroubleshoot problem relaterade tooon lokala kodare som är konfigurerat toosend en enkel bithastighet dataströmmen tooAMS kanaler som är aktiverade för live encoding.

### <a name="problem-would-like-toosee-logs"></a>Problem: Vill toosee loggar
* **Potentiella problem**: Det går inte att hitta kodare loggar som kan bidra till felsökning av problem.
  
  * **Telestream Wirecast**: vanligtvis finns det loggar under C:\Users\{username} \AppData\Roaming\Wirecast\ 
  * **Grundämne Live**: du kan hitta har länkar toologs på hello-hanteringsportalen. Klicka på **Stats**, sedan **loggar**. På hello **loggfiler** sidan visas en lista över loggar för alla hello LiveEvent objekt; Välj hello en matchande den aktuella sessionen. 
  * **Flash Live mediekodare**: du kan hitta hello **loggkatalogen...**  genom att gå toohello **kodning loggen** fliken.

### <a name="problem-there-is-no-option-for-outputting-a-progressive-stream"></a>Problem: Det finns inget alternativ för att mata ut en progressiv dataström
* **Potentiella problem**: hello kodare används inte automatiskt Ej sammanflätning. 
  
    **Felsökningssteg**: Leta efter en de-interlacing alternativ i hello kodare gränssnittet. Kontrollera igen för inställningar för progressiv utdata när Frigör sammanflätning har aktiverats. 

### <a name="problem-tried-several-encoder-output-settings-and-still-unable-tooconnect"></a>Problem: Försökte flera kodare utdata inställningar och fortfarande inte tooconnect.
* **Potentiella problem**: Azure kodning kanal har inte återställts korrekt. 
  
    **Felsökningssteg**: se till att hello kodare inte längre trycka tooAMS, stoppa och återställa hello-kanalen. Kör en gång igen och försök att ansluta din kodare med hello nya inställningar. Om du fortfarande inte kan lösa problemet hello, försök att skapa en ny kanal helt, ibland kanaler kan bli skadad efter flera misslyckade försök.  
* **Potentiella problem**: hello GOP storlek eller nyckelbilden inställningar inte är optimala. 
  
    **Felsökningssteg**: rekommenderas GOP storlek eller keyframe är 2 sekunder. Vissa kodare beräkna den här inställningen i antal bildrutor, medan andra använder sekunder. Exempel: när mata ut 30fps hello GOP storlek är 60 ramar som är likvärdiga too2 sekunder.  
* **Potentiella problem**: stängda portar blockerar hello dataströmmen. 
  
    **Felsökningssteg**: när direktuppspelning via RTMP, kontrollera brandväggen och/eller proxy inställningar tooconfirm att utgående portar 1935 och 1936 är öppna. När du använder RTP strömning, bekräfta att utgående port 2010 är öppen. 

### <a name="problem-when-configuring-hello-encoder-toostream-with-hello-rtp-protocol-there-is-no-place-tooenter-a-host-name"></a>Problem: När du konfigurerar hello kodare toostream med hello RTP-protokollet är tooenter ett värdnamn.
* **Potentiella problem**: Tillåt inte många RTP kodare för värdnamn och en IP-adress måste toobe förvärvade.  
  
    **Felsökningssteg**: toofind Hej IP-adress, öppna en kommandotolk på varje dator. toodo detta i Windows, öppna hello kör programstart (WIN + R) och Skriv ”cmd” tooopen.  
  
    När hello kommandotolk är öppen skriver du ”Ping [AMS värdnamn]”. 
  
    hello värdnamn kan härledas genom att utelämna hello portnumret från hello Azure infognings-URL, som i följande exempel hello: 
  
    RTP://Test2-amstest009.RTP.Channel.mediaservices.Windows.NET:2010 / 
  
    ![fmle](./media/media-services-fmle-live-encoder/media-services-fmle10.png)

> [!NOTE]
> Skicka ett supportärende med hello Azure-portalen om du efter följande hello du fortfarande inte kan kunna strömma för felsökning.
> 
> 

## <a name="media-services-learning-paths"></a>Sökvägar för Media Services-utbildning
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Ge feedback
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

