---
title: "Hur du för att dirigera USB-enheter i Azure RemoteApp? | Microsoft Docs"
description: "Lär dig hur du använder omdirigering för USB-enheter i Azure RemoteApp."
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
ms.openlocfilehash: 3d7165d2c3dafe87b829e588b9e7f2c377552a35
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Hur du för att dirigera USB-enheter i Azure RemoteApp?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Omdirigering av enheter kan användarna använda USB-enheter som är anslutna till deras dator eller surfplatta med appar i Azure RemoteApp. Till exempel om du har delat Skype via Azure RemoteApp behöver användarna för att kunna använda sina enheter kameror.

Innan du går vidare kontrollerar du du läsa USB-omdirigering av informationen i [med hjälp av omdirigering i Azure RemoteApp](remoteapp-redirection.md). Men det rekommenderas nusbdevicestoredirect:s: * fungerar inte för USB-webbkameror och fungerar inte för vissa USB-skrivare eller multifunktionella USB-enheter. Inbyggt och av säkerhetsskäl har Azure RemoteApp-administratören vill aktivera omdirigering genom enhetsklass GUID eller instans-ID för enheten innan användarna kan använda dessa enheter.

Även om den här artikeln handlar om web kamera omdirigering, du kan använda ett liknande sätt för att omdirigera USB-skrivare och andra multifunktionella USB-enheter som inte dirigeras genom den **nusbdevicestoredirect:s:*** kommando.

## <a name="redirection-options-for-usb-devices"></a>Alternativ för mappomdirigering för USB-enheter
Azure RemoteApp använder mycket lik mekanismer för omdirigering av USB-enheter som de som är tillgängliga för Fjärrskrivbordstjänster. Den underliggande tekniken kan du välja rätt omdirigering av-metoden för en given enhet att få bästa för både den övergripande och RemoteFX USB-enhet med hjälp av omdirigering av **usbdevicestoredirect:s:** kommando. Det finns fyra element för det här kommandot:

| Bearbetningsordningen | Parameter | Beskrivning |
| --- | --- | --- |
| 1 |* |Markerar alla enheter som inte tas upp av övergripande omdirigering. Obs: avsiktligt * fungerar inte för USB-webbkameror. |
| {Enhetsklass GUID} |Markerar alla enheter som matchar den angivna klassen. | |
| USB\InstanceID |Väljer en USB-enhet som har angetts för den angivna instans-ID. | |
| 2 |-USB\Instance ID |Tar bort inställningar för mappomdirigering för den angivna enheten. |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Omdirigera en USB-enhet med hjälp av enhetsklass GUID
Det finns två sätt att hitta enhetsklass GUID som kan användas för omdirigering. 

Det första alternativet är att använda den [systemdefinierade enheten installationsprogrammet klasser kan leverantörer](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Välj den klass som bäst motsvarar den enhet som är ansluten till den lokala datorn. För digitalkameror kan en Imaging enhetsklass eller avbilda videoenhet klass. Du behöver göra vissa experiment med enhetsklasser att hitta klassen rätt GUID som fungerar med lokalt anslutna USB-enhet (i vårt fall webbkamera).

Ett bättre sätt eller det andra alternativet är att följa dessa steg för att hitta enhetsklasser GUID:

1. Öppna Enhetshanteraren, leta upp den enhet som omdirigeras och högerklicka på den och öppna egenskaperna.
   ![Öppna Enhetshanteraren](./media/remoteapp-usbredir/ra-devicemanager.png)
2. På den **information** väljer egenskapen **klass-Guid**. Värdet som visas är klass-GUID för den typ av enhet.
   ![Kameraegenskaper](./media/remoteapp-usbredir/ra-classguid.png)
3. Använd klassen Guid-värde för att dirigera om enheter som matchar den.

Exempel:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Du kan kombinera flera enheten omdirigeringar i samma cmdlet. Exempel: Om du vill omdirigera lokala lagring och en USB-webbkamera cmdlet ser ut så här:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

När du ställer in enhetsomdirigering av klassen GUID omdirigeras alla enheter som matchar den klassen GUID i den angivna samlingen. Om det finns flera datorer i det lokala nätverket som har samma USB-webbkameror, kan du köra en enda cmdlet för att dirigera om alla webbkameror.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Omdirigera en USB-enhet med hjälp av enheten instans-ID
Om du vill ha mer detaljerad kontroll och vill styra omdirigering per enhet kan du använda den **USB\InstanceID** omdirigering av parametern.

Den svåraste delen av den här metoden är att hitta USB-enheten instansens ID. Du behöver åtkomst till datorn och USB-enheten. Följ dessa steg:

1. Aktivera omdirigering av enheter i Remote Desktop Session enligt beskrivningen i [hur kan jag använda mina enheter och resurser i en fjärrskrivbordssession?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Öppna en anslutning till fjärrskrivbord och klicka på **Visa alternativ**.
3. Klicka på **Spara som** att spara aktuella anslutningsinställningar till en RDP-fil.  
    ![Spara inställningarna som en RDP-fil](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Välj ett filnamn och en plats, till exempel MyConnection.rdp och den här PC\Documents och spara filen.
5. Öppna filen MyConnection.rdp med en textredigerare och hitta instans-ID för enheten som du vill omdirigera.

Nu kan du använda instans-ID i följande cmdlet:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Vi vill bli bättre på att hjälpa dig
Visste du att du förutom att betygsätta den här artikeln och skriva kommentarer nedan även kan göra ändringar i själva artikeln? Saknar du något i artikeln? Är det något som är fel? Är det något i artikeln som gör dig förvirrad? Då kan du gå tillbaka till början av sidan och klicka på **Redigera på GitHub** och ändra i texten. Ändringarna skickas till oss och när vi har godkänt dem kommer du att kunna se dina ändringar och förbättringar här.

