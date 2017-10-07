---
title: "aaaHow gör du omdirigerar USB-enheter i Azure RemoteApp? | Microsoft Docs"
description: "Lär dig hur toouse omdirigering för USB-enheter i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 191d98af-2f5a-4307-9042-aae0e4049f9f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 661b90c0910167d76ac3886b5af7a32d00b3943f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Hur du för att dirigera USB-enheter i Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Omdirigering av enheter kan användarna använda hello USB-enheter anslutna tootheir dator eller surfplatta med hello appar i Azure RemoteApp. Till exempel om du har delat Skype via Azure RemoteApp behöver användarna toobe kan toouse kameror sin enhet.

Innan du går vidare kontrollerar du du läsa hello USB-omdirigering av informationen i [med hjälp av omdirigering i Azure RemoteApp](remoteapp-redirection.md). Men hello rekommenderar nusbdevicestoredirect:s: * fungerar inte för USB-webbkameror och fungerar inte för vissa USB-skrivare eller multifunktionella USB-enheter. Inbyggt och av säkerhetsskäl hello Azure RemoteApp-administratören har tooenable omdirigering genom enhetsklass GUID eller instans-ID för enheten innan användarna kan använda dessa enheter.

Även om den här artikeln handlar om web kamera omdirigering, kan du använda en liknande metoden tooredirect USB-skrivare och andra multifunktionella USB-enheter som inte dirigeras av hello **nusbdevicestoredirect:s:*** kommando.

## <a name="redirection-options-for-usb-devices"></a>Alternativ för mappomdirigering för USB-enheter
Azure RemoteApp använder mycket lik mekanismer för omdirigering av USB-enheter som hello sådana som är tillgängliga för Fjärrskrivbordstjänster. hello underliggande teknik kan du välja hello omdirigering av rätt metod för en given enhet tooget hello bästa för både den övergripande och RemoteFX USB-enhet genom att använda hello **usbdevicestoredirect:s:** kommando. Det finns fyra element toothis kommando:

| Bearbetningsordningen | Parameter | Beskrivning |
| --- | --- | --- |
| 1 |* |Markerar alla enheter som inte tas upp av övergripande omdirigering. Obs: avsiktligt * fungerar inte för USB-webbkameror. |
| {Enhetsklass GUID} |Markerar alla enheter som matchar angivna hello klassen. | |
| USB\InstanceID |Väljer en USB-enhet som har angetts för hello angivna instans-ID. | |
| 2 |-USB\Instance ID |Tar bort hello omdirigeringsinställningar för hello angiven enhet. |

## <a name="redirecting-a-usb-device-by-using-hello-device-class-guid"></a>Omdirigera en USB-enhet med hjälp av hello enhetsklass GUID
Det finns två sätt toofind hello enhetsklass GUID som kan användas för omdirigering. 

hello första alternativet är toouse hello [systemdefinierade enheten installationsprogrammet klasser tillgängliga tooVendors](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Välj hello-klass som närmast matchar hello enheten bifogade toohello lokal dator. För digitalkameror kan en Imaging enhetsklass eller avbilda videoenhet klass. Du behöver toodo vissa experiment med hello enheten klasser toofind hello rätt klass-GUID som fungerar med hello lokalt anslutna USB-enhet (i vårt fall hello webbkamera).

Ett bättre sätt eller hello andra alternativet, är toofollow dessa steg toofind hello enhet klass-GUID:

1. Öppna hello Enhetshanteraren, hitta hello-enhet som omdirigeras och högerklicka på den och sedan öppna hello egenskaper.
   ![Öppna hello Enhetshanteraren](./media/remoteapp-usbredir/ra-devicemanager.png)
2. På hello **information** väljer hello egenskapen **klass-Guid**. hello-värdet som visas är hello klass-GUID för den typ av enhet.
   ![Kameraegenskaper](./media/remoteapp-usbredir/ra-classguid.png)
3. Använd hello klass-Guid-värde tooredirect enheter som matchar den.

Exempel:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Du kan kombinera flera enheten omdirigeringar i hello samma cmdlet. Till exempel: tooredirect lokal lagring och en USB-web kamera, cmdlet ser ut så här:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

När du ställer in enhetsomdirigering av klassen GUID omdirigeras alla enheter som matchar att klassen GUID i hello angiven samling. Till exempel om det finns flera datorer i hello lokala nätverk som har hello samma USB-webbkameror, kan du köra en enda cmdlet tooredirect alla hello webbkameror.

## <a name="redirecting-a-usb-device-by-using-hello-device-instance-id"></a>Omdirigera en USB-enhet med hjälp av hello enheten instans-ID
Om du vill ha mer detaljerad kontroll och vill toocontrol omdirigering per enhet, kan du använda hello **USB\InstanceID** omdirigering av parametern.

hello svåraste en del av den här metoden är att hitta hello USB-enheten instansens ID. Du kommer behöver komma åt toohello datorn och hello specifik USB-enhet. Följ dessa steg:

1. Aktivera hello enhetsomdirigering i fjärrsessionen Desktop enligt beskrivningen i [hur kan jag använda mina enheter och resurser i en fjärrskrivbordssession?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Öppna en anslutning till fjärrskrivbord och klicka på **Visa alternativ**.
3. Klicka på **Spara som** toosave hello aktuella anslutning inställningar tooan RDP-filen.  
    ![Spara hello inställningar som en RDP-fil](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Välj ett namn och en plats, till exempel MyConnection.rdp och den här PC\Documents och spara hello-filen.
5. Öppna hello MyConnection.rdp filen med en textredigerare och hitta hello instans-ID hello enhet som du vill tooredirect.

Nu kan du använda hello instans-ID i hello följande cmdlet:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Vi vill bli bättre på att hjälpa dig
Visste du att i tillägg toorating den här artikeln och skriva kommentarer nedan, kan du göra ändringar toohello själva artikeln? Saknar du något i artikeln? Är det något som är fel? Är det något i artikeln som gör dig förvirrad? Rulla uppåt och klicka på **Redigera på GitHub** toomake ändringar - de kommer toous för granskning och sedan, när vi har godkänt dem, visas dina ändringar och förbättringar här.

