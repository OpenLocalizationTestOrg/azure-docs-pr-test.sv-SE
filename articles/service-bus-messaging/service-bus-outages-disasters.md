---
title: aaaInsulating Azure Service Bus program mot avbrott och katastrofer | Microsoft Docs
description: "Här beskrivs tekniker som du kan använda tooprotect program mot ett potentiella avbrott i Service Bus."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: tysonn
ms.assetid: fd9fa8ab-f4c4-43f7-974f-c876df1614d4
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2017
ms.author: sethm
ms.openlocfilehash: 349b4968456c9f15375753d83495246f5a3ddfdb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Metodtips för program mot Service Bus-avbrott och katastrofer isolering
Verksamhetskritiska program måste köras kontinuerligt, även i hello förekomst av oplanerade avbrott eller katastrofer. Det här avsnittet beskrivs metoder du kan använda tooprotect Service Bus program mot en potentiella avbrott eller katastrof.

Ett avbrott har definierats som hello temporär otillgänglighet Azure Service Bus. hello avbrott kan påverka vissa komponenter av Service Bus, till exempel ett meddelandearkiv eller även hello hela datacentret. När hello problemet har åtgärdats, aktiveras Service Bus igen. Normalt orsakar avbrott inte förlorade meddelanden eller andra data. Ett exempel på ett komponentfel är hello otillgänglighet en viss meddelandearkiv. Ett exempel på ett datacenter hela avbrott är strömavbrott hello datacenter eller en felaktig datacenter nätverksväxel. Ett avbrott kan från några få minuter tooa sista några dagar.

En katastrof definieras som hello permanent förlorade en Service Bus-skalningsenhet eller datacenter. hello datacenter kan eller kan inte bli tillgänglig igen. Vanligtvis förlorar en katastrof åtkomsten till vissa eller alla meddelanden eller andra data. Exempel på katastrofer är brand, överbelasta eller jordbävning.

## <a name="current-architecture"></a>Aktuella arkitektur
Service Bus använder flera meddelanden lagrar toostore meddelanden som skickas tooqueues eller avsnitt. En icke-partitionerat kö eller ett ämne tilldelas tooone messaging store. Om den här meddelandearkiv är tillgänglig, misslyckas alla åtgärder på den kö eller ett ämne.

Alla Service Bus meddelandeentiteter (köer, ämnen, reläer) finns i ett namnområde för tjänsten som är kopplad till ett datacenter. Service Bus Aktivera inte automatisk geo-replikering av data och inte heller det tillåter en namnområde toospan flera datacenter.

## <a name="protecting-against-acs-outages"></a>Skydd mot avbrott för ACS
Om du använder autentiseringsuppgifter för ACS och ACS är tillgänglig, kan klienter inte längre hämta token. Klienter som har en token för närvarande hello ACS kraschar kan fortsätta toouse Service Bus tills hello-token upphör att gälla. livslängd för token hello standard är tre timmar.

tooprotect mot ACS-avbrott, Använd delade signatur åtkomst (SAS)-token. I det här fallet autentiserar klienten hello direkt med Service Bus genom att registrera en egen minted token med en hemlig nyckel. Anrop tooACS inte längre behövs. Mer information om SAS-token finns [Service Bus autentisering][Service Bus authentication].

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Skydda köer och ämnen mot messaging store-fel
En icke-partitionerat kö eller ett ämne tilldelas tooone messaging store. Om den här meddelandearkiv är tillgänglig, misslyckas alla åtgärder på den kö eller ett ämne. En partitionerad kö på hello andra sidan, består av flera fragment. Varje fragment lagras i en annan meddelandearkiv. När ett meddelande skickas tooa partitionerade kö eller ett ämne, tilldelar Service Bus hello-meddelande tooone av hello-fragment. Om hello motsvarande meddelandearkiv är tillgänglig, skriver Service Bus hello meddelandet tooa olika fragment, om möjligt. Mer information om partitionerade enheter finns [partitionerade meddelandeentiteter][Partitioned messaging entities].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Skydd mot datacenteravbrott eller katastrofer
tooallow för växling mellan två datacenter, kan du skapa ett namnområde för Service Bus-tjänsten i varje datacenter. Till exempel hello namnområde för Service Bus-tjänsten **contosoPrimary.servicebus.windows.net** kan finnas i hello USA Nord/centrala region och **contosoSecondary.servicebus.windows.net**kan finnas i hello oss söder/centrala region. Om en Service Bus-meddelanden entiteten måste vara tillgänglig i hello förekomsten av ett avbrott i datacenter, kan du skapa den entiteten i båda namnområden.

Mer information finns i ”fel i Service Bus i ett Azure-datacenter” i avsnittet hello [asynkrona meddelanden mönster och hög tillgänglighet][Asynchronous messaging patterns and high availability].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Skyddar relay slutpunkter mot datacenteravbrott eller katastrofer
GEO-replikering relay slutpunkter kan en tjänst som Exponerar en relay endpoint toobe kan nås i hello förekomst av Service Bus avbrott. tooachieve geo-replikering, hello service måste skapa två relay-slutpunkter i olika namnområden. hello namnområden måste finnas i olika datacenter och hello två slutpunkter måste ha olika namn. Till exempel en primär slutpunkten kan nås **contosoPrimary.servicebus.windows.net/myPrimaryService**, medan motparten sekundära kan nås **contosoSecondary.servicebus.windows.net/mySecondaryService**.

hello tjänsten lyssnar på båda slutpunkter och en klient kan anropa hello-tjänsten via antingen slutpunkt. Ett klientprogram slumpmässigt tar en hello reläer som hello primära slutpunkt och skickar dess begäran toohello aktiv slutpunkt. Om hello åtgärden misslyckas med felkoden betyder misslyckande att hello relay slutpunkt inte är tillgänglig. hello-programmet öppnas en kanal toohello säkerhetskopierad slutpunkt och kör hello-begäran. Då hello active och hello säkerhetskopiering slutpunkter växla roll: hello klientprogrammet anser hello gamla aktiv slutpunkt toobe hello nya säkerhetskopierad slutpunkt och hello gamla säkerhetskopierad slutpunkt toobe hello nya aktiv slutpunkt. Om både skicka operations misslyckas hello roller i hello två entiteter förblir oförändrade och returneras ett fel.

Hej [Geo-replikering med Service Bus vidarebefordrade meddelanden] [ Geo-replication with Service Bus relayed Messages] exemplet visar hur tooreplicate vidarebefordrar.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Skydda köer och ämnen mot datacenteravbrott eller katastrofer
tooachieve återhämtning mot datacenteravbrott när med hjälp av asynkron meddelandetjänst, Service Bus stöder två metoder: *active* och *passiva* replikering. För varje metod om en viss kö eller ett ämne måste vara tillgänglig i hello förekomsten av ett avbrott för datacenter, kan du skapa den i både namnområden. Båda entiteter kan ha hello samma namn. Till exempel en primär kö kan nås **contosoPrimary.servicebus.windows.net/myQueue**, medan motparten sekundära kan nås **contosoSecondary.servicebus.windows.net/myQueue**.

Om hello program inte kräver permanent avsändaren att mottagaren kommunikation kan hello program implementera varaktiga klientsidan kön tooprevent förlust och tooshield hello avsändarens från Service Bus tillfälligt fel.

## <a name="active-replication"></a>Aktiv replikering
Aktiv replikering använder entiteter i båda namnområdena för varje åtgärd. Alla klienter som skickar ett meddelande skickas två kopior av hello samma meddelande. hello första kopian skickas toohello primär entitet (till exempel **contosoPrimary.servicebus.windows.net/sales**), och hello kopia av hello-meddelande skickas toohello sekundär enhet (till exempel  **contosoSecondary.servicebus.windows.net/sales**).

En klient tar emot meddelanden från både köer. hello mottagaren processer hello första kopian av ett meddelande och hello kopia ignoreras. toosuppress dubblerade meddelanden, hello avsändaren tagga varje meddelande med en unik identifierare. Båda kopiorna av hello-meddelande måste taggas med hello samma identifierare. Du kan använda hello [BrokeredMessage.MessageId] [ BrokeredMessage.MessageId] eller [BrokeredMessage.Label] [ BrokeredMessage.Label] egenskaper eller en anpassad egenskap tootag hello meddelande. hello mottagare måste upprätthålla en lista med meddelanden som redan har fått.

Hej [Geo-replikering med asynkrona meddelanden i Service Bus] [ Geo-replication with Service Bus Brokered Messages] exemplet visar aktiv replikering av meddelandeentiteter.

> [!NOTE]
> hello aktiv replikering metoden fördubblar hello antal åtgärder, därför kan den här metoden leda toohigher kostnaden.
> 
> 

## <a name="passive-replication"></a>Passiva replikering
I hello felfri fallet använder passiva replikering endast en av hello två meddelandeentiteter. En klient skickar hello meddelandet toohello active entitet. Om hello hello active entiteten misslyckas med en felkod som visar hello datacenter som värdar hello active entitet kan vara otillgänglig, skickar hello klienten en kopia av hello meddelandet toohello säkerhetskopiering entitet. Då hello active och hello säkerhetskopieringsenheter växla roll: hello skickar klienten anser hello gamla active entiteten toobe hello ny säkerhetskopiering entitet och hello gamla säkerhetskopiering entiteten är hello ny aktiva entitet. Om både skicka operations misslyckas hello roller i hello två entiteter förblir oförändrade och returneras ett fel.

En klient tar emot meddelanden från både köer. Eftersom det finns en risk att hello mottagaren tar emot två kopior av samma meddelande, hello hello mottagare måste förhindra att dubbla meddelanden. Du kan förhindra dubbletter i hello samma sätt som för aktiv replikering.

I allmänhet replikeras passiva mer kostnadseffektiv än aktiv replikering eftersom endast en åtgärd utförs i de flesta fall. Svarstid, genomflöde och kostnaden är identiska toohello icke-replikerade scenario.

När du använder passiva replikering, i hello kan följande scenarier meddelanden tappas bort eller fått två gånger:

* **Meddelandet fördröjning eller förlust**: Anta att hello avsändaren har skickat en m1 toohello primära meddelandekö och sedan hello kön blir otillgänglig innan hello mottagaren tar emot m1. hello avsändare skickar en efterföljande m2 toohello sekundära meddelandekö. Om hello primära kön är inte tillgänglig för tillfället, får hello mottagaren m1 när hello kön har blivit tillgänglig igen. Vid en katastrof får hello mottagaren m1.
* **Duplicera mottagning**: förutsätter att hello avsändare skickar en m toohello primära meddelandekö. Service Bus har bearbetar m men misslyckas toosend ett svar. När hello skicka arbetstiderna ut, skickar hello avsändare en identisk kopia av m toohello sekundär kö. Om hello mottagaren kan tooreceive hello första kopian av m innan hello primära kön blir otillgänglig, hello mottagaren tar emot båda kopiorna av m på cirka hello samtidigt. Om hello mottagaren inte kan tooreceive hello första kopian av m innan hello primära kön blir otillgänglig, hello mottagare får först endast hello kopia av m, men tar emot en andra kopia av m när hello primära kön blir tillgänglig.

Hej [Geo-replikering med Service Bus asynkrona meddelanden] [ Geo-replication with Service Bus Brokered Messages] exemplet visar passiva replikering av meddelandeentiteter.

## <a name="next-steps"></a>Nästa steg
toolearn mer om katastrofåterställning, finns i följande artiklar:

* [Företagskontinuitet för Azure SQL-databas][Azure SQL Database Business Continuity]
* [Utforma flexibel program för Azure][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[Geo-replication with Service Bus Relayed Messages]: http://code.msdn.microsoft.com/Geo-replication-with-16dbfecd
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Brokered Messages]: http://code.msdn.microsoft.com/Geo-replication-with-f5688664
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency
