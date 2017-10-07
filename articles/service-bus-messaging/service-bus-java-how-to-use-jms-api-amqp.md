---
title: aaaHow toouse AMQP 1.0 med hello Java Service Bus-API | Microsoft Docs
description: Hur toouse hello Java meddelandet Service (JMS) med Azure Service Bus och avancerade Message Queuing Protodol (AMQP) 1.0.
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
editor: 
ms.assetid: be766f42-6fd1-410c-b275-8c400c811519
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 3e1d0329f2675a2273e12bb7389d3ce38b156a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-java-message-service-jms-api-with-service-bus-and-amqp-10"></a>Hur toouse hello Java meddelandet Service (JMS) API med Service Bus och AMQP 1.0
hello är avancerade Message Queuing-protokollet (AMQP) 1.0 ett effektivt, tillförlitlig och överföring nivå meddelanden protokoll som du kan använda toobuild robust, plattformsoberoende meddelandeprogram.

Stöd för AMQP 1.0 i Service Bus innebär att du kan använda hello queuing och publicera/prenumerera asynkrona meddelandetjänsten funktioner mellan olika plattformar med en effektiv binär-protokollet. Dessutom kan du skapa program består av komponenter som skapats med en blandning av språk och ramverk operativsystem.

Den här artikeln förklarar hur toouse Service Bus meddelandefunktioner (köer och publicera/prenumerera avsnitt) från Java-program med hjälp av hello populära Java meddelandet Service (JMS) API som standard. Det finns en [tillhörande artikel](service-bus-amqp-dotnet.md) som förklarar hur toodo hello samma med hello Service Bus .NET-API. Du kan använda dessa två guider tillsammans toolearn om plattformar använder AMQP 1.0.

## <a name="get-started-with-service-bus"></a>Kom igång med Service Bus
Den här handboken förutsätts att du redan har ett Service Bus-namnområde som innehåller en kö med namnet **Kö1**. Om du inte gör det, så kan du [skapa hello namnområde och kön](service-bus-create-namespace-portal.md) med hello [Azure-portalen](https://portal.azure.com). Mer information om hur toocreate Service Bus-namnområden och köer, se [komma igång med Service Bus-köer](service-bus-dotnet-get-started-with-queues.md).

> [!NOTE]
> Stöder också AMQP partitionerade köer och ämnen. Mer information finns i [partitionerade meddelandeentiteter](service-bus-partitioning.md) och [AMQP 1.0-stöd för Service Bus partitionerad köer och ämnen](service-bus-partitioned-queues-and-topics-amqp-overview.md).
> 
> 

## <a name="downloading-hello-amqp-10-jms-client-library"></a>Hämta hello AMQP 1.0 JMS klientbibliotek
Information om där toodownload hello senaste versionen av klientbiblioteket hello Apache Qpid JMS AMQP 1.0 finns [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).

Du måste lägga till följande fyra JAR-filer från hello Apache Qpid JMS AMQP 1.0 distribution Arkiv toohello Java-KLASSÖKVÄGEN när du bygger och JMS program som körs med Service Bus hello:

* geronimo jms\_1.1\_spec 1.0.jar
* qpid-amqp-1-0-Client-[version].JAR
* qpid-amqp-1-0-Client-jms-[version].JAR
* qpid-amqp-1-0-Common-[version].JAR

## <a name="coding-java-applications"></a>Kodning Java-program
### <a name="java-naming-and-directory-interface-jndi"></a>Java Naming and Directory Interface JNDI)
JMS använder hello Java Naming and Directory Interface JNDI () toocreate en åtskillnad mellan namn på logisk och fysisk. Två typer av JMS objekt löses med hjälp av JNDI: ConnectionFactory- och målservrar. JNDI använder en providermodell som du kan ansluta annan katalog services toohandle name resolution uppgifter. hello Apache Qpid JMS AMQP 1.0 biblioteket levereras med en enkel egenskaper filbaserade JNDI Provider som har konfigurerats med en egenskapsfil av hello följande format:

```
# servicebus.properties - sample JNDI configuration

# Register a ConnectionFactory in JNDI using hello form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCF = amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net

# Register some queues in JNDI using hello form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
queue.QUEUE = queue1
```

#### <a name="configure-hello-connectionfactory"></a>Konfigurera hello ConnectionFactory
Hej post används toodefine en **ConnectionFactory** i hello Qpid egenskaper filen JNDI provider är hello följande format:

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

Där **[jndi_name]** och **[ConnectionURL]** har hello följande innebörd:

* **[jndi_name]** : hello logiska namnet för hello ConnectionFactory. Detta är hello-namn som kommer att lösas i hello Java-program som använder hello JNDI IntialContext.lookup()-metoden.
* **[ConnectionURL]** : En URL som ger hello JMS biblioteket hello information krävs toohello AMQP broker.

hello format hello **ConnectionURL** är följande:

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
Där **[namnutrymme]**, **[SASPolicyName]** och **[SASPolicyKey]** har hello följande innebörd:

* **[namnutrymme]** : hello Service Bus-namnrymd.
* **[SASPolicyName]** : hello kön signatur för delad åtkomst principnamn.
* **[SASPolicyKey]** : hello kön signatur för delad åtkomst principnyckel.

> [!NOTE]
> Du måste URL-koda hello lösenord manuellt. Det finns ett användbart URL-kodning verktyg på [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).
> 
> 

#### <a name="configure-destinations"></a>Konfigurera mål
hello-posten används toodefine ett mål i hello Qpid egenskaper file JNDI providern är av hello följande format:

```
queue.[jndi_name] = [physical_name]
```

eller

```
topic.[jndi_name] = [physical_name]
```

Där **[jndi\_namn]** och **[fysiska\_namn]** har hello följande innebörd:

* **[jndi_name]** : hello logiska namnet för hello mål. Detta är hello-namn som kommer att lösas i hello Java-program som använder hello JNDI IntialContext.lookup()-metoden.
* **[physical_name]** : hello namnet på hello Service Bus entiteten toowhich hello programmet skickar eller tar emot meddelanden.

> [!NOTE]
> När du får från en prenumeration för Service Bus-avsnittet, bör hello fysiska namnet som angetts i JNDI vara hello namn av hello-avsnittet. hello prenumerationsnamn tillhandahålls när hello varaktiga prenumerationen skapas i hello JMS programkod. Hej [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) innehåller mer information om hur du arbetar med Service Bus-ämnen från JMS.
> 
> 

### <a name="write-hello-jms-application"></a>Skriva hello JMS program
Det finns inga särskilda API: er eller alternativ som krävs när du använder JMS med Service Bus. Det finns dock några begränsningar som beskrivs senare. Som med alla JMS program är hello först öppna krävs för konfigurationen av hello JNDI miljö, toobe kan tooresolve en **ConnectionFactory** och mål.

#### <a name="configure-hello-jndi-initialcontext"></a>Konfigurera hello JNDI InitialContext
Hej JNDI miljö konfigureras genom att skicka en hash-tabell konfigurationsinformationen i hello konstruktor för hello javax.naming.InitialContext klass. hello två obligatoriska element i hello hashtable är hello klassnamnet för hello inledande kontexten fabriken och hello providern URL. hello följande kod visar hur tooconfigure hello JNDI miljö toouse hello Qpid egenskaper filbaserade JNDI providern med en egenskapsfil med namnet **servicebus.properties**.

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a>Ett enkelt JMS program med hjälp av en Service Bus-kö
hello följande exempel programmet skickar JMS TextMessages tooa Service Bus-kö med hello JNDI logiska namnet för KÖN och tar emot hälsningsmeddelande tillbaka.

```java
// SimpleSenderReceiver.java

import javax.jms.*;
import javax.naming.Context;
import javax.naming.InitialContext;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Hashtable;
import java.util.Random;

public class SimpleSenderReceiver implements MessageListener {
    private static boolean runReceiver = true;
    private Connection connection;
    private Session sendSession;
    private Session receiveSession;
    private MessageProducer sender;
    private MessageConsumer receiver;
    private static Random randomGenerator = new Random();

    public SimpleSenderReceiver() throws Exception {
        // Configure JNDI environment
        Hashtable<String, String> env = new Hashtable<String, String>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, 
                   "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory");
        env.put(Context.PROVIDER_URL, "servicebus.properties");
        Context context = new InitialContext(env);

        // Look up ConnectionFactory and Queue
        ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCF");
        Destination queue = (Destination) context.lookup("QUEUE");

        // Create Connection
        connection = cf.createConnection();

        // Create sender-side Session and MessageProducer
        sendSession = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        sender = sendSession.createProducer(queue);

        if (runReceiver) {
            // Create receiver-side Session, MessageConsumer,and MessageListener
            receiveSession = connection.createSession(false, Session.CLIENT_ACKNOWLEDGE);
            receiver = receiveSession.createConsumer(queue);
            receiver.setMessageListener(this);
            connection.start();
        }
    }

    public static void main(String[] args) {
        try {

            if ((args.length > 0) && args[0].equalsIgnoreCase("sendonly")) {
                runReceiver = false;
            }

            SimpleSenderReceiver simpleSenderReceiver = new SimpleSenderReceiver();
            System.out.println("Press [enter] toosend a message. Type 'exit' + [enter] tooquit.");
            BufferedReader commandLine = new java.io.BufferedReader(new InputStreamReader(System.in));

            while (true) {
                String s = commandLine.readLine();
                if (s.equalsIgnoreCase("exit")) {
                    simpleSenderReceiver.close();
                    System.exit(0);
                } else {
                    simpleSenderReceiver.sendMessage();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private void sendMessage() throws JMSException {
        TextMessage message = sendSession.createTextMessage();
        message.setText("Test AMQP message from JMS");
        long randomMessageID = randomGenerator.nextLong() >>>1;
        message.setJMSMessageID("ID:" + randomMessageID);
        sender.send(message);
        System.out.println("Sent message with JMSMessageID = " + message.getJMSMessageID());
    }

    public void close() throws JMSException {
        connection.close();
    }

    public void onMessage(Message message) {
        try {
            System.out.println("Received message with JMSMessageID = " + message.getJMSMessageID());
            message.acknowledge();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}    
```

### <a name="run-hello-application"></a>Kör programmet hello
Kör programmet hello ger utdata hello formuläret:

```
> java SimpleSenderReceiver
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.

Sent message with JMSMessageID = ID:2867600614942270318
Received message with JMSMessageID = ID:2867600614942270318

Sent message with JMSMessageID = ID:7578408152750301483
Received message with JMSMessageID = ID:7578408152750301483

Sent message with JMSMessageID = ID:956102171969368961
Received message with JMSMessageID = ID:956102171969368961
exit
```

## <a name="cross-platform-messaging-between-jms-and-net"></a>Plattformsoberoende meddelanden mellan JMS och .NET
Den här guiden visar hur toosend och ta emot meddelanden tooand från Service Bus med hjälp av JMS. En hello viktiga fördelar med AMQP 1.0 är dock att du kan program toobe skapas från komponenter som skrivits i olika språk med meddelanden som utbyts tillförlitligt och fullständiga återgivning.

Med hjälp av hello JMS exempelprogrammet ovan och ett liknande .NET-program som hämtas från en tillhörande artikel [med hjälp av Service Bus från .NET med AMQP 1.0](service-bus-amqp-dotnet.md), du kan utbyta meddelanden mellan .NET och Java. Den här artikeln för mer information om hello information av plattformsoberoende meddelanden med hjälp av Service Bus och AMQP 1.0.

### <a name="jms-toonet"></a>JMS too.NET
toodemonstrate JMS too.NET meddelanden:

* Starta hello .NET exempelprogrammet utan kommandoradsargument.
* Starta hello Java-exempelprogram med hello ”sendonly” kommandoradsargument. I det här läget skickas hello programmet inte kommer ta emot meddelanden från hello kön, bara.
* Tryck på **RETUR** några gånger i hello Java-program-konsolen som gör att meddelanden toobe skickas.
* Dessa meddelanden tas emot av hello .NET-program.

#### <a name="output-from-jms-application"></a>Utdata från JMS program
```
> java SimpleSenderReceiver sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a>Utdata från .NET-program
```
> SimpleSenderReceiver.exe    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-toojms"></a>.NET tooJMS
toodemonstrate .NET tooJMS meddelanden:

* Starta hello .NET exempelprogrammet med hello ”sendonly” kommandoradsargument. I det här läget skickas hello programmet inte kommer ta emot meddelanden från hello kön, bara.
* Starta hello Java exempelprogrammet utan kommandoradsargument.
* Tryck på **RETUR** några gånger i hello .NET application-konsolen, som gör att meddelanden toobe skickas.
* Dessa meddelanden tas emot av hello Java-program.

#### <a name="output-from-net-application"></a>Utdata från .NET-program
```
> SimpleSenderReceiver.exe sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a>Utdata från JMS program
```
> java SimpleSenderReceiver    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a>Funktioner som inte stöds och begränsningar
hello följande begränsningar gäller när du använder JMS via AMQP 1.0 med Service Bus, nämligen:

* Endast en **MessageProducer** eller **MessageConsumer** tillåts per **Session**. Om du behöver toocreate flera **MessageProducers** eller **MessageConsumers** i ett program, skapa ett dedikerat **Session** för dem.
* Temporär ämnesprenumerationer stöds inte för närvarande.
* **MessageSelectors** stöds inte för närvarande.
* Tillfälligt mål; till exempel **TemporaryQueue**, **TemporaryTopic** stöds inte för närvarande, tillsammans med hello **QueueRequestor** och **TopicRequestor**API: er som använder dem.
* Överförda sessioner och distribuerade transaktioner stöds inte.

## <a name="summary"></a>Sammanfattning
Den här hur tooguide visades hur toouse asynkrona Service Bus meddelandefunktioner (köer och publicera/prenumerera avsnitt) från Java använder hello populära JMS API och AMQP 1.0.

Du kan också använda Service Bus AMQP 1.0 från andra språk, inklusive .NET, C, Python eller PHP. Komponenter som skapats med hjälp av dessa olika språk kan utbyta meddelanden på ett tillförlitligt sätt och bästa använder hello AMQP 1.0-stöd i Service Bus.

## <a name="next-steps"></a>Nästa steg
* [AMQP 1.0-stöd i Azure Service Bus](service-bus-amqp-overview.md)
* [Hur toouse AMQP 1.0 med hello Service Bus .NET-API](service-bus-dotnet-advanced-message-queuing.md)
* [Service Bus AMQP 1.0-Guide för utvecklare](service-bus-amqp-dotnet.md)
* [Komma igång med Service Bus-köer](service-bus-dotnet-get-started-with-queues.md)
* [Java-utvecklingscenter](https://azure.microsoft.com/develop/java/)

