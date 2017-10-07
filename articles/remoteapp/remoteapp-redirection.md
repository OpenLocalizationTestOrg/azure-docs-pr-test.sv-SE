---
title: aaaUsing omdirigering i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur tooconfigure och använda omdirigering i RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 2c8c867f-4907-4f2e-9ccd-2eb82bb5b837
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d5739a75cf606bd971268da67b2c5ff0fe5fe19b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a>Med hjälp av omdirigering i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Omdirigering av enheter gör att användare kan interagera med fjärråtkomst appar som använder hello enheter anslutna tootheir lokala datorn, telefon eller surfplatta. Om du har angett Skype via Azure RemoteApp, måste användaren hello kamera installerad på sin dator toowork med Skype. Detta gäller även för skrivare, högtalare, Övervakare och ett intervall av USB-anslutna kringutrustning.

RemoteApp utnyttjar hello Remote Desktop Protocol (RDP) och RemoteFX tooprovide omdirigering.

## <a name="what-redirection-is-enabled-by-default"></a>Vilka omdirigering är aktiverat som standard?
När du använder RemoteApp är hello följande omdirigeringar aktiverade som standard. hello information inom parentes visar hello RDP-inställningen.

* Spela upp ljud på hello lokal dator (**spela upp på den här datorn**). (audiomode:i:0)
* Spela in ljud från hello lokala datorn och skicka toohello fjärrdatorn (**poster från den här datorn**). (audiocapturemode:i:1)
* Skriva ut toolocal skrivare (redirectprinters:i:1)
* COM-portar (redirectcomports:i:1)
* Smartkortenhet (redirectsmartcards:i:1)
* Urklipp (möjlighet toocopy och klistra in) (redirectclipboard:i:1)
* Avmarkera typen kantutjämning (Tillåt kantutjämning: i:1)
* Dirigera alla kompatibla Plug and Play-enheter. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Vilka andra omdirigering är tillgängligt?
Två alternativ för mappomdirigering är inaktiverade som standard:

* Omdirigering (enhetsmappning): den lokala datorn enheter bli mappade enheter i hello fjärrsession. Detta kan du spara eller öppna filer från de lokala enheterna när du arbetar i hello fjärrsessionen.
* USB-omdirigering: du kan använda hello USB-enheter anslutna tooyour lokal dator inom hello fjärrsessionen.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Ändra inställningarna för omdirigering i RemoteApp
Du kan ändra hello enhetens omdirigeringsinställningar för en samling med hello Microsoft Azure PowerShell med SDK. När du har installerat Hej nya PowerShell och SDK, först konfigurera din prenumeration som beskrivs i toomanage [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

Använd sedan en liknande toohello för kommandot efter tooset hello anpassade RDP egenskaper:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Observera att  *`n*  används som avgränsare mellan enskilda egenskaper.)

tooget en lista över vilka anpassade RDP-egenskaper har konfigurerats, kör hello följande cmdlet. Observera att endast anpassade egenskaper visas som utdataresultat och inte hello standardegenskaperna:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

När du ställer in egenskaperna måste du ange alla anpassade egenskaper varje gång; Annars återställs hello inställningen toodisabled.   

### <a name="common-examples"></a>Vanliga exempel
Använd följande cmdlet tooenable omdirigering hello:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

Använd den här cmdlet-tooenable omdirigering av såväl USB-enhet:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Använd denna cmdlet toodisable delning av Urklipp:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> Vara att toocompletely logga ut alla användare i samlingen hello (och inte bara koppla från dem) innan du testar hello ändringen. tooensure användare har helt loggat ut gå toohello **sessioner** i hello-samlingen i hello Azure-portalen och logga ut alla användare som har kopplats från eller loggat in. Ibland kan det ta flera sekunder innan hello lokala enheter tooshow i Explorer hello-sessionen.
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Ändra inställningar för USB-omdirigering på Windows-klienter
Om du vill toouse USB-omdirigering på en dator som ansluter tooRemoteApp finns 2 åtgärder som behöver toohappen. 1 - administratören måste tooenable USB-omdirigering på hello samlingsnivå med hjälp av Azure PowerShell. 2 - på varje enhet där du vill att toouse USB-omdirigering, behöver du tooenable en grupprincip som tillåter den. Det här steget behöver toobe för varje användare som vill toouse USB-omdirigering.

> [!NOTE]
> USB-omdirigering med Azure RemoteApp stöds endast för Windows-datorer.
> 
> 

### <a name="enable-usb-redirection-for-hello-remoteapp-collection"></a>Aktivera USB-omdirigering för hello RemoteApp-samling
Använd följande cmdlet tooenable USB-omdirigering på samlingsnivå hello hello:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-hello-client-computer"></a>Aktivera USB-omdirigering för hello klientdator
tooconfigure USB-inställningar för mappomdirigering på datorn:

1. Öppna hello Redigeraren för lokala grupprinciper (GPEDIT. MSC). (Kör gpedit.msc från en kommandotolk.)
2. Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.
3. Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.
4. Välj **aktiverad**, och välj sedan **administratörer och användare i hello RemoteFX USB-omdirigering åtkomsträttigheter**.
5. Öppna en kommandotolk med administrativ behörighet och kör följande kommando hello:
   
        gpupdate /force
6. Starta om datorn hello.

Du kan också använda hello Grupprinciphantering verktyget toocreate och tillämpa hello USB-omdirigering av principen för alla datorer i domänen:

1. Logga in på hello-domänkontrollant som domänadministratör för hello.
2. Öppna hello konsolen Grupprinciphantering. (Klicka på **Start > administrativa verktyg > hantering av Grupprincip**.)
3. Navigera toohello domän eller organisationsenhet som du vill toocreate hello princip.
4. Högerklicka på **Standarddomänprincip**, och klicka sedan på **redigera**.
5. Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.
6. Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.
7. Välj **aktiverad**, och välj sedan **administratörer och användare i hello RemoteFX USB-omdirigering åtkomsträttigheter**.
8. Klicka på **OK**.  

