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
# <a name="how-toouse-hello-java-message-service-jms-api-with-service-bus-and-amqp-10"></a><span data-ttu-id="39bc6-103">Hur toouse hello Java meddelandet Service (JMS) API med Service Bus och AMQP 1.0</span><span class="sxs-lookup"><span data-stu-id="39bc6-103">How toouse hello Java Message Service (JMS) API with Service Bus and AMQP 1.0</span></span>
<span data-ttu-id="39bc6-104">hello är avancerade Message Queuing-protokollet (AMQP) 1.0 ett effektivt, tillförlitlig och överföring nivå meddelanden protokoll som du kan använda toobuild robust, plattformsoberoende meddelandeprogram.</span><span class="sxs-lookup"><span data-stu-id="39bc6-104">hello Advanced Message Queuing Protocol (AMQP) 1.0 is an efficient, reliable, wire-level messaging protocol that you can use toobuild robust, cross-platform messaging applications.</span></span>

<span data-ttu-id="39bc6-105">Stöd för AMQP 1.0 i Service Bus innebär att du kan använda hello queuing och publicera/prenumerera asynkrona meddelandetjänsten funktioner mellan olika plattformar med en effektiv binär-protokollet.</span><span class="sxs-lookup"><span data-stu-id="39bc6-105">Support for AMQP 1.0 in Service Bus means that you can use hello queuing and publish/subscribe brokered messaging features from a range of platforms using an efficient binary protocol.</span></span> <span data-ttu-id="39bc6-106">Dessutom kan du skapa program består av komponenter som skapats med en blandning av språk och ramverk operativsystem.</span><span class="sxs-lookup"><span data-stu-id="39bc6-106">Furthermore, you can build applications comprised of components built using a mix of languages, frameworks, and operating systems.</span></span>

<span data-ttu-id="39bc6-107">Den här artikeln förklarar hur toouse Service Bus meddelandefunktioner (köer och publicera/prenumerera avsnitt) från Java-program med hjälp av hello populära Java meddelandet Service (JMS) API som standard.</span><span class="sxs-lookup"><span data-stu-id="39bc6-107">This article explains how toouse Service Bus messaging features (queues and publish/subscribe topics) from Java applications using hello popular Java Message Service (JMS) API standard.</span></span> <span data-ttu-id="39bc6-108">Det finns en [tillhörande artikel](service-bus-amqp-dotnet.md) som förklarar hur toodo hello samma med hello Service Bus .NET-API.</span><span class="sxs-lookup"><span data-stu-id="39bc6-108">There is a [companion article](service-bus-amqp-dotnet.md) that explains how toodo hello same using hello Service Bus .NET API.</span></span> <span data-ttu-id="39bc6-109">Du kan använda dessa två guider tillsammans toolearn om plattformar använder AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="39bc6-109">You can use these two guides together toolearn about cross-platform messaging using AMQP 1.0.</span></span>

## <a name="get-started-with-service-bus"></a><span data-ttu-id="39bc6-110">Kom igång med Service Bus</span><span class="sxs-lookup"><span data-stu-id="39bc6-110">Get started with Service Bus</span></span>
<span data-ttu-id="39bc6-111">Den här handboken förutsätts att du redan har ett Service Bus-namnområde som innehåller en kö med namnet **Kö1**.</span><span class="sxs-lookup"><span data-stu-id="39bc6-111">This guide assumes that you already have a Service Bus namespace containing a queue named **queue1**.</span></span> <span data-ttu-id="39bc6-112">Om du inte gör det, så kan du [skapa hello namnområde och kön](service-bus-create-namespace-portal.md) med hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="39bc6-112">If you do not, then you can [create hello namespace and queue](service-bus-create-namespace-portal.md) using hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="39bc6-113">Mer information om hur toocreate Service Bus-namnområden och köer, se [komma igång med Service Bus-köer](service-bus-dotnet-get-started-with-queues.md).</span><span class="sxs-lookup"><span data-stu-id="39bc6-113">For more information about how toocreate Service Bus namespaces and queues, see [Get started with Service Bus queues](service-bus-dotnet-get-started-with-queues.md).</span></span>

> [!NOTE]
> <span data-ttu-id="39bc6-114">Stöder också AMQP partitionerade köer och ämnen.</span><span class="sxs-lookup"><span data-stu-id="39bc6-114">Partitioned queues and topics also support AMQP.</span></span> <span data-ttu-id="39bc6-115">Mer information finns i [partitionerade meddelandeentiteter](service-bus-partitioning.md) och [AMQP 1.0-stöd för Service Bus partitionerad köer och ämnen](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span><span class="sxs-lookup"><span data-stu-id="39bc6-115">For more information, see [Partitioned messaging entities](service-bus-partitioning.md) and [AMQP 1.0 support for Service Bus partitioned queues and topics](service-bus-partitioned-queues-and-topics-amqp-overview.md).</span></span>
> 
> 

## <a name="downloading-hello-amqp-10-jms-client-library"></a><span data-ttu-id="39bc6-116">Hämta hello AMQP 1.0 JMS klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="39bc6-116">Downloading hello AMQP 1.0 JMS client library</span></span>
<span data-ttu-id="39bc6-117">Information om där toodownload hello senaste versionen av klientbiblioteket hello Apache Qpid JMS AMQP 1.0 finns [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span><span class="sxs-lookup"><span data-stu-id="39bc6-117">For information about where toodownload hello latest version of hello Apache Qpid JMS AMQP 1.0 client library, visit [https://qpid.apache.org/download.html](https://qpid.apache.org/download.html).</span></span>

<span data-ttu-id="39bc6-118">Du måste lägga till följande fyra JAR-filer från hello Apache Qpid JMS AMQP 1.0 distribution Arkiv toohello Java-KLASSÖKVÄGEN när du bygger och JMS program som körs med Service Bus hello:</span><span class="sxs-lookup"><span data-stu-id="39bc6-118">You must add hello following four JAR files from hello Apache Qpid JMS AMQP 1.0 distribution archive toohello Java CLASSPATH when building and running JMS applications with Service Bus:</span></span>

* <span data-ttu-id="39bc6-119">geronimo jms\_1.1\_spec 1.0.jar</span><span class="sxs-lookup"><span data-stu-id="39bc6-119">geronimo-jms\_1.1\_spec-1.0.jar</span></span>
* <span data-ttu-id="39bc6-120">qpid-amqp-1-0-Client-[version].JAR</span><span class="sxs-lookup"><span data-stu-id="39bc6-120">qpid-amqp-1-0-client-[version].jar</span></span>
* <span data-ttu-id="39bc6-121">qpid-amqp-1-0-Client-jms-[version].JAR</span><span class="sxs-lookup"><span data-stu-id="39bc6-121">qpid-amqp-1-0-client-jms-[version].jar</span></span>
* <span data-ttu-id="39bc6-122">qpid-amqp-1-0-Common-[version].JAR</span><span class="sxs-lookup"><span data-stu-id="39bc6-122">qpid-amqp-1-0-common-[version].jar</span></span>

## <a name="coding-java-applications"></a><span data-ttu-id="39bc6-123">Kodning Java-program</span><span class="sxs-lookup"><span data-stu-id="39bc6-123">Coding Java applications</span></span>
### <a name="java-naming-and-directory-interface-jndi"></a><span data-ttu-id="39bc6-124">Java Naming and Directory Interface JNDI)</span><span class="sxs-lookup"><span data-stu-id="39bc6-124">Java Naming and Directory Interface (JNDI)</span></span>
<span data-ttu-id="39bc6-125">JMS använder hello Java Naming and Directory Interface JNDI () toocreate en åtskillnad mellan namn på logisk och fysisk.</span><span class="sxs-lookup"><span data-stu-id="39bc6-125">JMS uses hello Java Naming and Directory Interface (JNDI) toocreate a separation between logical names and physical names.</span></span> <span data-ttu-id="39bc6-126">Två typer av JMS objekt löses med hjälp av JNDI: ConnectionFactory- och målservrar.</span><span class="sxs-lookup"><span data-stu-id="39bc6-126">Two types of JMS objects are resolved using JNDI: ConnectionFactory and Destination.</span></span> <span data-ttu-id="39bc6-127">JNDI använder en providermodell som du kan ansluta annan katalog services toohandle name resolution uppgifter.</span><span class="sxs-lookup"><span data-stu-id="39bc6-127">JNDI uses a provider model into which you can plug different directory services toohandle name resolution duties.</span></span> <span data-ttu-id="39bc6-128">hello Apache Qpid JMS AMQP 1.0 biblioteket levereras med en enkel egenskaper filbaserade JNDI Provider som har konfigurerats med en egenskapsfil av hello följande format:</span><span class="sxs-lookup"><span data-stu-id="39bc6-128">hello Apache Qpid JMS AMQP 1.0 library comes with a simple properties file-based JNDI Provider that is configured using a properties file of hello following format:</span></span>

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

#### <a name="configure-hello-connectionfactory"></a><span data-ttu-id="39bc6-129">Konfigurera hello ConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="39bc6-129">Configure hello ConnectionFactory</span></span>
<span data-ttu-id="39bc6-130">Hej post används toodefine en **ConnectionFactory** i hello Qpid egenskaper filen JNDI provider är hello följande format:</span><span class="sxs-lookup"><span data-stu-id="39bc6-130">hello entry used toodefine a **ConnectionFactory** in hello Qpid properties file JNDI provider is of hello following format:</span></span>

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

<span data-ttu-id="39bc6-131">Där **[jndi_name]** och **[ConnectionURL]** har hello följande innebörd:</span><span class="sxs-lookup"><span data-stu-id="39bc6-131">Where **[jndi_name]** and **[ConnectionURL]** have hello following meanings:</span></span>

* <span data-ttu-id="39bc6-132">**[jndi_name]** : hello logiska namnet för hello ConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="39bc6-132">**[jndi_name]**: hello logical name of hello ConnectionFactory.</span></span> <span data-ttu-id="39bc6-133">Detta är hello-namn som kommer att lösas i hello Java-program som använder hello JNDI IntialContext.lookup()-metoden.</span><span class="sxs-lookup"><span data-stu-id="39bc6-133">This is hello name that will be resolved in hello Java application using hello JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="39bc6-134">**[ConnectionURL]** : En URL som ger hello JMS biblioteket hello information krävs toohello AMQP broker.</span><span class="sxs-lookup"><span data-stu-id="39bc6-134">**[ConnectionURL]**: A URL that provides hello JMS library with hello information required toohello AMQP broker.</span></span>

<span data-ttu-id="39bc6-135">hello format hello **ConnectionURL** är följande:</span><span class="sxs-lookup"><span data-stu-id="39bc6-135">hello format of hello **ConnectionURL** is as follows:</span></span>

```
amqps://[SASPolicyName]:[SASPolicyKey]@[namespace].servicebus.windows.net
```
<span data-ttu-id="39bc6-136">Där **[namnutrymme]**, **[SASPolicyName]** och **[SASPolicyKey]** har hello följande innebörd:</span><span class="sxs-lookup"><span data-stu-id="39bc6-136">Where **[namespace]**, **[SASPolicyName]** and **[SASPolicyKey]** have hello following meanings:</span></span>

* <span data-ttu-id="39bc6-137">**[namnutrymme]** : hello Service Bus-namnrymd.</span><span class="sxs-lookup"><span data-stu-id="39bc6-137">**[namespace]**: hello Service Bus namespace.</span></span>
* <span data-ttu-id="39bc6-138">**[SASPolicyName]** : hello kön signatur för delad åtkomst principnamn.</span><span class="sxs-lookup"><span data-stu-id="39bc6-138">**[SASPolicyName]**: hello Queue Shared Access Signature policy name.</span></span>
* <span data-ttu-id="39bc6-139">**[SASPolicyKey]** : hello kön signatur för delad åtkomst principnyckel.</span><span class="sxs-lookup"><span data-stu-id="39bc6-139">**[SASPolicyKey]**: hello Queue Shared Access Signature policy key.</span></span>

> [!NOTE]
> <span data-ttu-id="39bc6-140">Du måste URL-koda hello lösenord manuellt.</span><span class="sxs-lookup"><span data-stu-id="39bc6-140">You must URL-encode hello password manually.</span></span> <span data-ttu-id="39bc6-141">Det finns ett användbart URL-kodning verktyg på [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="39bc6-141">A useful URL-encoding utility is available at [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
> 
> 

#### <a name="configure-destinations"></a><span data-ttu-id="39bc6-142">Konfigurera mål</span><span class="sxs-lookup"><span data-stu-id="39bc6-142">Configure destinations</span></span>
<span data-ttu-id="39bc6-143">hello-posten används toodefine ett mål i hello Qpid egenskaper file JNDI providern är av hello följande format:</span><span class="sxs-lookup"><span data-stu-id="39bc6-143">hello entry used toodefine a destination in hello Qpid properties file JNDI provider is of hello following format:</span></span>

```
queue.[jndi_name] = [physical_name]
```

<span data-ttu-id="39bc6-144">eller</span><span class="sxs-lookup"><span data-stu-id="39bc6-144">or</span></span>

```
topic.[jndi_name] = [physical_name]
```

<span data-ttu-id="39bc6-145">Där **[jndi\_namn]** och **[fysiska\_namn]** har hello följande innebörd:</span><span class="sxs-lookup"><span data-stu-id="39bc6-145">Where **[jndi\_name]** and **[physical\_name]** have hello following meanings:</span></span>

* <span data-ttu-id="39bc6-146">**[jndi_name]** : hello logiska namnet för hello mål.</span><span class="sxs-lookup"><span data-stu-id="39bc6-146">**[jndi_name]**: hello logical name of hello destination.</span></span> <span data-ttu-id="39bc6-147">Detta är hello-namn som kommer att lösas i hello Java-program som använder hello JNDI IntialContext.lookup()-metoden.</span><span class="sxs-lookup"><span data-stu-id="39bc6-147">This is hello name that will be resolved in hello Java application using hello JNDI IntialContext.lookup() method.</span></span>
* <span data-ttu-id="39bc6-148">**[physical_name]** : hello namnet på hello Service Bus entiteten toowhich hello programmet skickar eller tar emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="39bc6-148">**[physical_name]**: hello name of hello Service Bus entity toowhich hello application sends or receives messages.</span></span>

> [!NOTE]
> <span data-ttu-id="39bc6-149">När du får från en prenumeration för Service Bus-avsnittet, bör hello fysiska namnet som angetts i JNDI vara hello namn av hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="39bc6-149">When receiving from a Service Bus topic subscription, hello physical name specified in JNDI should be hello name of hello topic.</span></span> <span data-ttu-id="39bc6-150">hello prenumerationsnamn tillhandahålls när hello varaktiga prenumerationen skapas i hello JMS programkod.</span><span class="sxs-lookup"><span data-stu-id="39bc6-150">hello subscription name is provided when hello durable subscription is created in hello JMS application code.</span></span> <span data-ttu-id="39bc6-151">Hej [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) innehåller mer information om hur du arbetar med Service Bus-ämnen från JMS.</span><span class="sxs-lookup"><span data-stu-id="39bc6-151">hello [Service Bus AMQP 1.0 Developer's Guide](service-bus-amqp-dotnet.md) provides more details on working with Service Bus topics from JMS.</span></span>
> 
> 

### <a name="write-hello-jms-application"></a><span data-ttu-id="39bc6-152">Skriva hello JMS program</span><span class="sxs-lookup"><span data-stu-id="39bc6-152">Write hello JMS application</span></span>
<span data-ttu-id="39bc6-153">Det finns inga särskilda API: er eller alternativ som krävs när du använder JMS med Service Bus.</span><span class="sxs-lookup"><span data-stu-id="39bc6-153">There are no special APIs or options required when using JMS with Service Bus.</span></span> <span data-ttu-id="39bc6-154">Det finns dock några begränsningar som beskrivs senare.</span><span class="sxs-lookup"><span data-stu-id="39bc6-154">However, there are a few restrictions that will be covered later.</span></span> <span data-ttu-id="39bc6-155">Som med alla JMS program är hello först öppna krävs för konfigurationen av hello JNDI miljö, toobe kan tooresolve en **ConnectionFactory** och mål.</span><span class="sxs-lookup"><span data-stu-id="39bc6-155">As with any JMS application, hello first thing required is configuration of hello JNDI environment, toobe able tooresolve a **ConnectionFactory** and destinations.</span></span>

#### <a name="configure-hello-jndi-initialcontext"></a><span data-ttu-id="39bc6-156">Konfigurera hello JNDI InitialContext</span><span class="sxs-lookup"><span data-stu-id="39bc6-156">Configure hello JNDI InitialContext</span></span>
<span data-ttu-id="39bc6-157">Hej JNDI miljö konfigureras genom att skicka en hash-tabell konfigurationsinformationen i hello konstruktor för hello javax.naming.InitialContext klass.</span><span class="sxs-lookup"><span data-stu-id="39bc6-157">hello JNDI environment is configured by passing a hashtable of configuration information into hello constructor of hello javax.naming.InitialContext class.</span></span> <span data-ttu-id="39bc6-158">hello två obligatoriska element i hello hashtable är hello klassnamnet för hello inledande kontexten fabriken och hello providern URL.</span><span class="sxs-lookup"><span data-stu-id="39bc6-158">hello two required elements in hello hashtable are hello class name of hello Initial Context Factory and hello Provider URL.</span></span> <span data-ttu-id="39bc6-159">hello följande kod visar hur tooconfigure hello JNDI miljö toouse hello Qpid egenskaper filbaserade JNDI providern med en egenskapsfil med namnet **servicebus.properties**.</span><span class="sxs-lookup"><span data-stu-id="39bc6-159">hello following code shows how tooconfigure hello JNDI environment toouse hello Qpid properties file based JNDI Provider with a properties file named **servicebus.properties**.</span></span>

```java
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
InitialContext context = new InitialContext(env);
``` 

### <a name="a-simple-jms-application-using-a-service-bus-queue"></a><span data-ttu-id="39bc6-160">Ett enkelt JMS program med hjälp av en Service Bus-kö</span><span class="sxs-lookup"><span data-stu-id="39bc6-160">A simple JMS application using a Service Bus queue</span></span>
<span data-ttu-id="39bc6-161">hello följande exempel programmet skickar JMS TextMessages tooa Service Bus-kö med hello JNDI logiska namnet för KÖN och tar emot hälsningsmeddelande tillbaka.</span><span class="sxs-lookup"><span data-stu-id="39bc6-161">hello following example program sends JMS TextMessages tooa Service Bus queue with hello JNDI logical name of QUEUE, and receives hello messages back.</span></span>

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

### <a name="run-hello-application"></a><span data-ttu-id="39bc6-162">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="39bc6-162">Run hello application</span></span>
<span data-ttu-id="39bc6-163">Kör programmet hello ger utdata hello formuläret:</span><span class="sxs-lookup"><span data-stu-id="39bc6-163">Running hello application produces output of hello form:</span></span>

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

## <a name="cross-platform-messaging-between-jms-and-net"></a><span data-ttu-id="39bc6-164">Plattformsoberoende meddelanden mellan JMS och .NET</span><span class="sxs-lookup"><span data-stu-id="39bc6-164">Cross-platform messaging between JMS and .NET</span></span>
<span data-ttu-id="39bc6-165">Den här guiden visar hur toosend och ta emot meddelanden tooand från Service Bus med hjälp av JMS.</span><span class="sxs-lookup"><span data-stu-id="39bc6-165">This guide showed how toosend and receive messages tooand from Service Bus using JMS.</span></span> <span data-ttu-id="39bc6-166">En hello viktiga fördelar med AMQP 1.0 är dock att du kan program toobe skapas från komponenter som skrivits i olika språk med meddelanden som utbyts tillförlitligt och fullständiga återgivning.</span><span class="sxs-lookup"><span data-stu-id="39bc6-166">However, one of hello key benefits of AMQP 1.0 is that it enables applications toobe built from components written in different languages, with messages exchanged reliably and at full fidelity.</span></span>

<span data-ttu-id="39bc6-167">Med hjälp av hello JMS exempelprogrammet ovan och ett liknande .NET-program som hämtas från en tillhörande artikel [med hjälp av Service Bus från .NET med AMQP 1.0](service-bus-amqp-dotnet.md), du kan utbyta meddelanden mellan .NET och Java.</span><span class="sxs-lookup"><span data-stu-id="39bc6-167">Using hello sample JMS application described above and a similar .NET application taken from a companion article, [Using Service Bus from .NET with AMQP 1.0](service-bus-amqp-dotnet.md), you can exchange messages between .NET and Java.</span></span> <span data-ttu-id="39bc6-168">Den här artikeln för mer information om hello information av plattformsoberoende meddelanden med hjälp av Service Bus och AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="39bc6-168">Read this article for more information about hello details of cross-platform messaging using Service Bus and AMQP 1.0.</span></span>

### <a name="jms-toonet"></a><span data-ttu-id="39bc6-169">JMS too.NET</span><span class="sxs-lookup"><span data-stu-id="39bc6-169">JMS too.NET</span></span>
<span data-ttu-id="39bc6-170">toodemonstrate JMS too.NET meddelanden:</span><span class="sxs-lookup"><span data-stu-id="39bc6-170">toodemonstrate JMS too.NET messaging:</span></span>

* <span data-ttu-id="39bc6-171">Starta hello .NET exempelprogrammet utan kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="39bc6-171">Start hello .NET sample application without any command-line arguments.</span></span>
* <span data-ttu-id="39bc6-172">Starta hello Java-exempelprogram med hello ”sendonly” kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="39bc6-172">Start hello Java sample application with hello "sendonly" command-line argument.</span></span> <span data-ttu-id="39bc6-173">I det här läget skickas hello programmet inte kommer ta emot meddelanden från hello kön, bara.</span><span class="sxs-lookup"><span data-stu-id="39bc6-173">In this mode, hello application will not receive messages from hello queue, it will only send.</span></span>
* <span data-ttu-id="39bc6-174">Tryck på **RETUR** några gånger i hello Java-program-konsolen som gör att meddelanden toobe skickas.</span><span class="sxs-lookup"><span data-stu-id="39bc6-174">Press **Enter** a few times in hello Java application console, which will cause messages toobe sent.</span></span>
* <span data-ttu-id="39bc6-175">Dessa meddelanden tas emot av hello .NET-program.</span><span class="sxs-lookup"><span data-stu-id="39bc6-175">These messages are received by hello .NET application.</span></span>

#### <a name="output-from-jms-application"></a><span data-ttu-id="39bc6-176">Utdata från JMS program</span><span class="sxs-lookup"><span data-stu-id="39bc6-176">Output from JMS application</span></span>
```
> java SimpleSenderReceiver sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with JMSMessageID = ID:4364096528752411591
Sent message with JMSMessageID = ID:459252991689389983
Sent message with JMSMessageID = ID:1565011046230456854
exit
```

#### <a name="output-from-net-application"></a><span data-ttu-id="39bc6-177">Utdata från .NET-program</span><span class="sxs-lookup"><span data-stu-id="39bc6-177">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with MessageID = 4364096528752411591
Received message with MessageID = 459252991689389983
Received message with MessageID = 1565011046230456854
exit
```

### <a name="net-toojms"></a><span data-ttu-id="39bc6-178">.NET tooJMS</span><span class="sxs-lookup"><span data-stu-id="39bc6-178">.NET tooJMS</span></span>
<span data-ttu-id="39bc6-179">toodemonstrate .NET tooJMS meddelanden:</span><span class="sxs-lookup"><span data-stu-id="39bc6-179">toodemonstrate .NET tooJMS messaging:</span></span>

* <span data-ttu-id="39bc6-180">Starta hello .NET exempelprogrammet med hello ”sendonly” kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="39bc6-180">Start hello .NET sample application with hello "sendonly" command-line argument.</span></span> <span data-ttu-id="39bc6-181">I det här läget skickas hello programmet inte kommer ta emot meddelanden från hello kön, bara.</span><span class="sxs-lookup"><span data-stu-id="39bc6-181">In this mode, hello application will not receive messages from hello queue, it will only send.</span></span>
* <span data-ttu-id="39bc6-182">Starta hello Java exempelprogrammet utan kommandoradsargument.</span><span class="sxs-lookup"><span data-stu-id="39bc6-182">Start hello Java sample application without any command-line arguments.</span></span>
* <span data-ttu-id="39bc6-183">Tryck på **RETUR** några gånger i hello .NET application-konsolen, som gör att meddelanden toobe skickas.</span><span class="sxs-lookup"><span data-stu-id="39bc6-183">Press **Enter** a few times in hello .NET application console, which will cause messages toobe sent.</span></span>
* <span data-ttu-id="39bc6-184">Dessa meddelanden tas emot av hello Java-program.</span><span class="sxs-lookup"><span data-stu-id="39bc6-184">These messages are received by hello Java application.</span></span>

#### <a name="output-from-net-application"></a><span data-ttu-id="39bc6-185">Utdata från .NET-program</span><span class="sxs-lookup"><span data-stu-id="39bc6-185">Output from .NET application</span></span>
```
> SimpleSenderReceiver.exe sendonly
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Sent message with MessageID = d64e681a310a48a1ae0ce7b017bf1cf3    
Sent message with MessageID = 98a39664995b4f74b32e2a0ecccc46bb
Sent message with MessageID = acbca67f03c346de9b7893026f97ddeb
exit
```

#### <a name="output-from-jms-application"></a><span data-ttu-id="39bc6-186">Utdata från JMS program</span><span class="sxs-lookup"><span data-stu-id="39bc6-186">Output from JMS application</span></span>
```
> java SimpleSenderReceiver    
Press [enter] toosend a message. Type 'exit' + [enter] tooquit.
Received message with JMSMessageID = ID:d64e681a310a48a1ae0ce7b017bf1cf3
Received message with JMSMessageID = ID:98a39664995b4f74b32e2a0ecccc46bb
Received message with JMSMessageID = ID:acbca67f03c346de9b7893026f97ddeb
exit
```

## <a name="unsupported-features-and-restrictions"></a><span data-ttu-id="39bc6-187">Funktioner som inte stöds och begränsningar</span><span class="sxs-lookup"><span data-stu-id="39bc6-187">Unsupported features and restrictions</span></span>
<span data-ttu-id="39bc6-188">hello följande begränsningar gäller när du använder JMS via AMQP 1.0 med Service Bus, nämligen:</span><span class="sxs-lookup"><span data-stu-id="39bc6-188">hello following restrictions exist when using JMS over AMQP 1.0 with Service Bus, namely:</span></span>

* <span data-ttu-id="39bc6-189">Endast en **MessageProducer** eller **MessageConsumer** tillåts per **Session**.</span><span class="sxs-lookup"><span data-stu-id="39bc6-189">Only one **MessageProducer** or **MessageConsumer** is allowed per **Session**.</span></span> <span data-ttu-id="39bc6-190">Om du behöver toocreate flera **MessageProducers** eller **MessageConsumers** i ett program, skapa ett dedikerat **Session** för dem.</span><span class="sxs-lookup"><span data-stu-id="39bc6-190">If you need toocreate multiple **MessageProducers** or **MessageConsumers** in an application, create a dedicated **Session** for each of them.</span></span>
* <span data-ttu-id="39bc6-191">Temporär ämnesprenumerationer stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="39bc6-191">Volatile topic subscriptions are not currently supported.</span></span>
* <span data-ttu-id="39bc6-192">**MessageSelectors** stöds inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="39bc6-192">**MessageSelectors** are not currently supported.</span></span>
* <span data-ttu-id="39bc6-193">Tillfälligt mål; till exempel **TemporaryQueue**, **TemporaryTopic** stöds inte för närvarande, tillsammans med hello **QueueRequestor** och **TopicRequestor**API: er som använder dem.</span><span class="sxs-lookup"><span data-stu-id="39bc6-193">Temporary destinations; for example, **TemporaryQueue**, **TemporaryTopic** are not currently supported, along with hello **QueueRequestor** and **TopicRequestor** APIs that use them.</span></span>
* <span data-ttu-id="39bc6-194">Överförda sessioner och distribuerade transaktioner stöds inte.</span><span class="sxs-lookup"><span data-stu-id="39bc6-194">Transacted sessions and distributed transactions are not supported.</span></span>

## <a name="summary"></a><span data-ttu-id="39bc6-195">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="39bc6-195">Summary</span></span>
<span data-ttu-id="39bc6-196">Den här hur tooguide visades hur toouse asynkrona Service Bus meddelandefunktioner (köer och publicera/prenumerera avsnitt) från Java använder hello populära JMS API och AMQP 1.0.</span><span class="sxs-lookup"><span data-stu-id="39bc6-196">This how-tooguide showed how toouse Service Bus brokered messaging features (queues and publish/subscribe topics) from Java using hello popular JMS API and AMQP 1.0.</span></span>

<span data-ttu-id="39bc6-197">Du kan också använda Service Bus AMQP 1.0 från andra språk, inklusive .NET, C, Python eller PHP.</span><span class="sxs-lookup"><span data-stu-id="39bc6-197">You can also use Service Bus AMQP 1.0 from other languages, including .NET, C, Python, and PHP.</span></span> <span data-ttu-id="39bc6-198">Komponenter som skapats med hjälp av dessa olika språk kan utbyta meddelanden på ett tillförlitligt sätt och bästa använder hello AMQP 1.0-stöd i Service Bus.</span><span class="sxs-lookup"><span data-stu-id="39bc6-198">Components built using these different languages can exchange messages reliably and at full fidelity using hello AMQP 1.0 support in Service Bus.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39bc6-199">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39bc6-199">Next steps</span></span>
* [<span data-ttu-id="39bc6-200">AMQP 1.0-stöd i Azure Service Bus</span><span class="sxs-lookup"><span data-stu-id="39bc6-200">AMQP 1.0 support in Azure Service Bus</span></span>](service-bus-amqp-overview.md)
* [<span data-ttu-id="39bc6-201">Hur toouse AMQP 1.0 med hello Service Bus .NET-API</span><span class="sxs-lookup"><span data-stu-id="39bc6-201">How toouse AMQP 1.0 with hello Service Bus .NET API</span></span>](service-bus-dotnet-advanced-message-queuing.md)
* [<span data-ttu-id="39bc6-202">Service Bus AMQP 1.0-Guide för utvecklare</span><span class="sxs-lookup"><span data-stu-id="39bc6-202">Service Bus AMQP 1.0 Developer's Guide</span></span>](service-bus-amqp-dotnet.md)
* [<span data-ttu-id="39bc6-203">Komma igång med Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="39bc6-203">Get started with Service Bus queues</span></span>](service-bus-dotnet-get-started-with-queues.md)
* [<span data-ttu-id="39bc6-204">Java-utvecklingscenter</span><span class="sxs-lookup"><span data-stu-id="39bc6-204">Java Developer Center</span></span>](https://azure.microsoft.com/develop/java/)

