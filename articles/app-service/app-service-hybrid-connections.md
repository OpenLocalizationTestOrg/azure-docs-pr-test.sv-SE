---
title: "aaa ”Hybridanslutningar i Azure App Service | Microsoft Docs ”"
description: "Hur toocreate och använda Hybridanslutningar tooaccess resurser i olika nätverk"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: 61d58068ab0a7c803019e3f0e92bde4273d1a053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure Apptjänst Hybridanslutningar #

## <a name="overview"></a>Översikt ##

Hybridanslutningar är både en tjänst i Azure som en funktion i hello Azure App Service.  Som en tjänst har användning och funktioner utöver de som utnyttjas i hello Azure App Service.  Mer om Hybridanslutningar och deras användning utanför hello Azure App Service kan du börja här toolearn [Azure Relay Hybridanslutningar][HCService]

I hello Azure App Service vara hybridanslutningar används tooaccess resurser i andra nätverk. Det ger åtkomst från din app tooan programmet slutpunkt.  Det kan inte ett alternativt kapaciteten tooaccess ditt program.  Som används i hello Apptjänst korrelerar varje hybridanslutning tooa enda TCP-värd och port kombination.  Det innebär att hello hybrid Anslutningens slutpunkt kan på ett annat operativsystem och alla program ges du träffa en lyssnande TCP-port. Hybridanslutningar inte vet och se vilka hello protokoll är eller vad du ansluter till.  Den bara ger åtkomst till nätverket.  


## <a name="how-it-works"></a>Hur det fungerar ##
hello hybrid anslutningar funktionen består av två utgående samtal tooService Bus Relay.  Det finns en anslutning från ett bibliotek på hello värd där appen körs i hello app service och det finns en anslutning från hello Hybrid anslutning Manager(HCM) tooService Bus relay.  hello HCM är en relay-tjänst som du distribuerar i hello nätverket värd 

Via hello fast två kopplade anslutningar som din app har en TCP-tunnel tooa värdnamn: port-kombination på hello andra sidan av hello HCM.  hello-anslutning använder TLS 1.2 för säkerhets- och SAS-nycklar för autentisering/auktorisering.    

![][1]

När appen skickar en DNS-begäran som matchar en konfigurera Hybridanslutning slutpunkt sedan hello utgående TCP-trafik dirigeras ned hello hybridanslutning.  

> [!NOTE]
> Det innebär att du borde testa tooalways används ett DNS-namn för din Hybridanslutning.  Vissa klientprogrammet utför inte i en DNS-sökning om hello slutpunkten används en IP-adress i stället.
>
>

Det finns två typer av hybridanslutningar hello nya hybridanslutningar som erbjuds som en tjänst med Azure Relay och hello äldre BizTalk-Hybridanslutningar.  hello är äldre BizTalk Hybridanslutningar refererad tooas klassiska Hybridanslutningar hello-portalen.  Det finns mer information i det här dokumentet senare om dem.

### <a name="app-service-hybrid-connection-benefits"></a>Fördelar med Apptjänst hybrid-anslutning ###

Det finns ett antal fördelar toohello hybrid anslutningar kapaciteten inklusive

- Appar kan på ett säkert sätt komma åt på lokala datorer och tjänster på ett säkert sätt
- hello-funktionen kräver inte en tillgänglig internet-slutpunkt
- Det går snabbt och enkelt tooset in  
- varje hybridanslutning matchar tooa enda värdnamn: port kombinationen som är en utmärkt säkerhet aspekt
- Normalt behöver inte brandvägg hål som hello anslutningar är alla utgående över standard webbportar
- eftersom hello-funktionen är nätverksnivån som också innebär att det är storleksoberoende toohello språk som används av din app och hello teknik som används av hello slutpunkt
- Det kan vara används tooprovide åtkomst i flera nätverk från en enda app 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Saker du kan göra med Hybridanslutningar ###

Det finns några saker som du inte kan göra med hybridanslutningar och de inkluderar:

- montera en enhet
- med hjälp av UDP
- åtkomst till TCP baserade tjänster som använder dynamiska portar, till exempel FTP passivt läge eller utökad passivt läge
- LDAP-stöd, som ibland kräver UDP
- Active Directory-support

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Lägga till och skapa en Hybridanslutning i din App ##

Hybridanslutningar kan skapas via hello app portalen eller från hello Hybridanslutning tjänstportalen.  Vi rekommenderar starkt att du använder hello appen företagsportal toocreate hello Hybridanslutningar som du vill toouse med din app.  toocreate en Hybridanslutning gå toohello [Azure-portalen] [ portal] och gå till hello Användargränssnittet för din app.  Välj **nätverk > Konfigurera slutpunkter för din hybridanslutning**.  Här hittar du hello Hybridanslutningar som är konfigurerade med din app  

![][2]

tooadd en ny Hybridanslutning, klicka på Lägg till Hybrid-anslutning.  hello-användargränssnitt som öppnas visar hello Hybridanslutningar som du redan har skapat.  tooadd en eller flera av dem tooyour appen, klicka på hello som du vill använda och tryck på **Lägg till valda hybridanslutning**.  

![][3]

Om du vill toocreate en ny Hybridanslutning **Skapa ny hybridanslutning**.  Här anger du det: 

- namnet på slutpunkten
- slutpunktens värdnamn
- port för slutpunkt
- servicebus-namnområde som du vill att toouse

![][4]

Varje hybridanslutning är bundet tooa service bus-namnrymd och varje service bus-namnrymd är i en Azure-region.  Det är viktigt tootry och använder en service bus-namnområde i hello samma region som din app så tooavoid framkallas Nätverksfördröjningen.

Om du vill tooremove din hybridanslutning från din app, högerklicka på den och välj **frånkoppling**.  

När en hybridanslutning läggs tooyour webbprogrammet kan se du information på den genom att klicka på den.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Hybridanslutningar och Apptjänstplaner ##

hello endast hybridanslutningar du nu är hello nya hybridanslutningar.  De är bara tillgängliga i Basic, Standard, Premium och isolerad priser SKU: er.  Det finns begränsningar knutna toohello priser plan.  

| Priser för planen | Planera för antalet hybridanslutningar som kan användas i hello |
|----|----|
| Basic | 5 |
| Standard | 25 |
| Premium | 200 |
| Isolerad | 200 |

Eftersom det finns begränsningar för App Service-Plan finns även Användargränssnittet i hello App Service-Plan som visar hur många hybridanslutningar används och vilka appar.  

![][6]

Precis som med hello app vy, kan du se information på din hybridanslutning genom att klicka på den.  I hello egenskaperna som visas här och du kan se alla hello information som du hade hello app vy men kan också se hur många andra appar i hello använder samma App Service-Plan hybridanslutningen.

![][7]

När det finns en gräns på hello antal slutpunkter för hybridanslutning som kan användas i en Apptjänstplan, kan varje hybridanslutning används användas till alla appar i den App Service-Plan.  Det är toosay om jag hade 1 hybridanslutning som jag använde i 5 separata appar i min App Service-Plan som inte är fortfarande bara 1 hybridanslutning.

Det finns ett extra kostnad toohybrid anslutningar utöver som endast kan användas i en Basic, Standard, Premium eller isolerade SKU.  För information om priser för hybrid anslutning finns här: [priser för Service Bus][sbpricing].

## <a name="hybrid-connection-manager"></a>Hybridanslutningshanterare ##

För hybrid anslutningar toowork måste en relay agent i hello-nätverk som är värd för din hybrid Anslutningens slutpunkt.  Den relay agenten kallas hello Manager för Hybrid-anslutning (HCM).  Det här verktyget kan hämtas från hello **nätverk > Konfigurera slutpunkter för din hybridanslutning** användargränssnitt som är tillgängliga från din app i hello [Azure-portalen][portal].  

Det här verktyget körs på Windows server 2008 R2 och senare versioner av Windows.  När du installerade hello HCM körs som en tjänst.  Den här tjänsten ansluter tooAzure servicebus relay baserat på konfigurerad hello slutpunkter.  hello anslutningar från hello HCM är utgående tooports 80 och 443.    

hello HCM har ett användargränssnitt tooconfigure den.  Efter hello HCM har installerats kan du ta fram hello Användargränssnittet genom att köra hello HybridConnectionManagerUi.exe som placeras i installationskatalogen för hello Hybridanslutningshanteraren.  Uppnås också enkelt på Windows 10 genom att skriva *Hybridanslutningshanteraren UI* i sökrutan.  

När hello HCM UI startas hello först öppna du är en tabell med alla hello hybridanslutningar som är konfigurerade med den här instansen av hello HCM finns.  Om du inte vill toomake ändringar behöver tooauthenticate med Azure. 

tooadd en eller flera Hybridanslutningar tooyour HCM:

1. Starta hello HCM UI
1. Klicka på Konfigurera en annan hybridanslutning![][8]

1. Logga in med ditt Azure-konto
1. Välj en prenumeration
1. Klicka på hello hybridanslutningar du vill använda den här HCM toorelay![][9]

1. Klicka på Spara

Nu visas hello hybridanslutningar som du har lagt till.  Du kan också klicka på hello konfigurerad hybridanslutning och se information om hello-anslutning.

![][10]

För HCM toobe kan toosupport hello hybrid anslutningarna konfigureras den med, måste den:

- TCP åtkomst tooAzure via portarna 80 och 443
- TCP åtkomst toohello hybrid Anslutningens slutpunkt
- möjlighet toodo DNS-sökningar på hello endpoint värd och hello azure servicebus-namnområde

hello HCM stöder både nya hybridanslutningar samt hello äldre BizTalk hybridanslutningar.

### <a name="redundancy"></a>Redundans ###

Varje HCM kan stödja flera hybridanslutningar.  Alla angivna hybridanslutning stöds också av flera HCMs.  hello standardbeteendet är tooround robin trafiken över hello konfigurerade HCMs för alla angivna slutpunkten.  Om du vill hög tillgänglighet på hybridanslutningar från ditt nätverk kan bara skapa en instans av flera HCMs på separata datorer. 

### <a name="manually-adding-a-hybrid-connection"></a>Att lägga till en hybridanslutning manuellt ###

Om du vill att någon utanför din prenumeration toohost en HCM-instans för en given hybridanslutning, kan du dela med dem hello anslutningssträng för gateway för hello hybridanslutning.  Du kan se detta i hello egenskaperna för en hybridanslutning i hello [Azure-portalen][portal]. toouse sträng, klicka på hello **konfigurera manuellt** i hello HCM och klistra in i anslutningssträngen för hello gateway.


## <a name="troubleshooting"></a>Felsökning ##

När din hybridanslutning ställs in med ett program som körs och det finns minst en HCM som har konfigurerats hybridanslutningen och sedan hello status står **ansluten** hello-portalen.  Om meddelandet inte anger **ansluten** innebär att din app är nere eller din HCM kan inte ansluta ut tooAzure på port 80 eller 443.  

hello främsta skälet att klienter inte kan ansluta tootheir slutpunkten är eftersom hello endpoint angavs med en IP-adress i stället för ett DNS-namn.  Om din app kan inte nå hello önskad slutpunkt och du använde en IP-adress, växla toousing ett DNS-namn som är giltiga på hello värd där hello HCM körs.  Andra saker toocheck är att hello DNS-namn matchas korrekt på hello värd där hello HCM körs och att det finns en anslutning från hello värden där hello HCM körs toohello hybrid Anslutningens slutpunkt.  

Det finns ett verktyg i hello App Service som kan anropas från hello-konsolen som kallas tcpping.  Det här verktyget kan berätta om du har åtkomst tooa TCP-slutpunkt men inte berättar om du har åtkomst tooa hybrid Anslutningens slutpunkt.  När den används i hello konsolen mot en hybrid Anslutningens slutpunkt, talar en lyckad ping endast om att du har en hybridanslutning som konfigurerats för din app som använder som värdnamn: port-kombination.  

## <a name="biztalk-hybrid-connections"></a>BizTalks hybridanslutningar ##

hello äldre BizTalk Hybridanslutningar kapaciteten har stängts av toofurther BizTalk Hybridanslutning skapande.  Du kan fortsätta använda din befintliga BizTalk-Hybridanslutningar med dina appar, men du bör migrera toohello ny tjänst.  Bland hello finns fördelarna hello ny tjänst över hello BizTalk-version:

- Det krävs inga ytterligare BizTalk-konto
- TLS är 1.2 i stället för 1.0 som BizTalk Hybridanslutningar
- Kommunikation är via portarna 80 och 443 med hjälp av en DNS-namnet tooreach Azure i stället för IP-adresser och ett antal ytterligare andra portar.  

tooadd en BizTalk hybrid anslutning tooyour app, gå tooyour app i hello [Azure-portalen] [ portal] och på **nätverk > Konfigurera slutpunkter för din hybridanslutning**.  Klicka i hello klassisk hybrid Anslutningstabell **lägga till klassiska hybridanslutning**.  Här visas en lista över dina BizTalk hybrid-anslutningar.  


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/

