---
title: "Med hjälp av omdirigering i Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur du konfigurerar och använder omdirigering i RemoteApp"
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
ms.openlocfilehash: b5a65d129225fde46e3b090bc3cd9427989005ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a>Med hjälp av omdirigering i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Omdirigering av enheter gör att användare kan interagera med fjärråtkomst appar som använder enheter som är anslutna till deras lokala datorn, telefon eller surfplatta. Om du har angett Skype via Azure RemoteApp, måste användaren kameran installerad på en dator att arbeta med Skype. Detta gäller även för skrivare, högtalare, Övervakare och ett intervall av USB-anslutna kringutrustning.

RemoteApp utnyttjar Remote Desktop Protocol (RDP) och RemoteFX för omdirigering.

## <a name="what-redirection-is-enabled-by-default"></a>Vilka omdirigering är aktiverat som standard?
När du använder RemoteApp är följande omdirigeringar aktiverade som standard. Informationen i parenteser visa RDP-inställningen.

* Spela upp ljud på den lokala datorn (**spela upp på den här datorn**). (audiomode:i:0)
* Fånga ljud från den lokala datorn och skickas till fjärrdatorn (**poster från den här datorn**). (audiocapturemode:i:1)
* Skriva ut till lokala skrivare (redirectprinters:i:1)
* COM-portar (redirectcomports:i:1)
* Smartkortenhet (redirectsmartcards:i:1)
* Urklipp (möjligheten att kopiera och klistra in) (redirectclipboard:i:1)
* Avmarkera typen kantutjämning (Tillåt kantutjämning: i:1)
* Dirigera alla kompatibla Plug and Play-enheter. (devicestoredirect:s: *)

## <a name="what-other-redirection-is-available"></a>Vilka andra omdirigering är tillgängligt?
Två alternativ för mappomdirigering är inaktiverade som standard:

* Omdirigering (enhetsmappning): den lokala datorn enheter bli mappade enheter i fjärrsessionen. Detta kan du spara eller öppna filer från de lokala enheterna när du arbetar i fjärrsessionen.
* USB-omdirigering: du kan använda USB-enheter som är anslutna till den lokala datorn i fjärrsessionen.

## <a name="change-your-redirection-settings-in-remoteapp"></a>Ändra inställningarna för omdirigering i RemoteApp
Du kan ändra enhetens omdirigeringsinställningar för en samling med hjälp av Microsoft Azure PowerShell med SDK. När du har installerat den nya PowerShell och SDK först konfigurera det för att hantera din prenumeration enligt beskrivningen i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).

Använd sedan ett kommando som liknar följande för att ange anpassade RDP-egenskaper:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

(Observera att  *`n*  används som avgränsare mellan enskilda egenskaper.)

Om du vill hämta en lista över vilka anpassade RDP-egenskaper har konfigurerats, kör du följande cmdlet. Observera att endast anpassade egenskaper visas som utdataresultat och inte standardegenskaperna:  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

När du ställer in egenskaperna måste du ange alla anpassade egenskaper varje gång; Annars återgår inställningen till inaktiverat.   

### <a name="common-examples"></a>Vanliga exempel
Använd följande cmdlet för att aktivera omdirigering:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

Använd denna cmdlet för att aktivera både USB- och enheten omdirigering:

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

Använd denna cmdlet för att inaktivera delning av Urklipp:  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> Se till att helt logga ut alla användare i samlingen (och inte bara koppla från dem) innan du testar ändringen. För att säkerställa att användarna är utloggade helt, gå till den **sessioner** i samlingen i Azure-portalen och logga ut alla användare som har kopplats från eller loggat in. Ibland kan det ta flera sekunder för lokala enheter ska visas i Utforskaren i sessionen.
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a>Ändra inställningar för USB-omdirigering på Windows-klienter
Om du vill använda USB-omdirigering på en dator som ansluter till RemoteApp finns 2 åtgärder som måste utföras. 1 - administratören måste aktivera USB-omdirigering på samlingsnivå med hjälp av Azure PowerShell. 2 - du måste aktivera en grupprincip som tillåter det på varje enhet där du vill använda USB-omdirigering. Det här steget måste göras för varje användare som vill använda USB-omdirigering.

> [!NOTE]
> USB-omdirigering med Azure RemoteApp stöds endast för Windows-datorer.
> 
> 

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a>Aktivera USB-omdirigering för RemoteApp-samling
Använd följande cmdlet för att aktivera USB-omdirigering på samlingsnivå:

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a>Aktivera USB-omdirigering för klientdatorn
Konfigurera inställningar för USB-omdirigering på datorn:

1. Öppna i Redigeraren för lokala grupprinciper (GPEDIT. MSC). (Kör gpedit.msc från en kommandotolk.)
2. Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.
3. Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.
4. Välj **aktiverad**, och välj sedan **administratörer och användare i åtkomstbehörigheter för RemoteFX USB-omdirigering**.
5. Öppna en kommandotolk med administrativ behörighet och kör följande kommando:
   
        gpupdate /force
6. Starta om datorn.

Du kan också använda verktyget hantering av Grupprincip för att skapa och använda USB-omdirigering av principen för alla datorer i domänen:

1. Logga in på domänkontrollanten som domänadministratör.
2. Öppna hanteringskonsolen för Grupprincip. (Klicka på **Start > administrativa verktyg > hantering av Grupprincip**.)
3. Navigera till den domän eller organisationsenhet som du vill skapa principen.
4. Högerklicka på **Standarddomänprincip**, och klicka sedan på **redigera**.
5. Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.
6. Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.
7. Välj **aktiverad**, och välj sedan **administratörer och användare i åtkomstbehörigheter för RemoteFX USB-omdirigering**.
8. Klicka på **OK**.  

