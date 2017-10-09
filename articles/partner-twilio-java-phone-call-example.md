---
title: "aaaHow tooMake ett telefonsamtal från Twilio (Java) | Microsoft Docs"
description: "Lär dig hur toomake en telefon anropa från en webbsida med Twilio i ett Java-program på Azure."
services: 
documentationcenter: java
author: devinrader
manager: twilio
editor: mollybos
ms.assetid: 0381789e-e775-41a0-a784-294275192b1d
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 11/25/2014
ms.author: microsofthelp@twilio.com
ms.openlocfilehash: 04fe5a78d431a79790dee3ca75c2b004aea4345d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomake-a-phone-call-using-twilio-in-a-java-application-on-azure"></a><span data-ttu-id="24b15-103">Hur tooMake ett telefonsamtal med Twilio i ett Java-program på Azure</span><span class="sxs-lookup"><span data-stu-id="24b15-103">How tooMake a Phone Call Using Twilio in a Java Application on Azure</span></span>
<span data-ttu-id="24b15-104">hello följande exempel visar hur du kan använda Twilio toomake ett samtal från en webbsida som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="24b15-104">hello following example shows you how you can use Twilio toomake a call from a web page hosted in Azure.</span></span> <span data-ttu-id="24b15-105">hello resulterande program frågar hello användaren för telefonsamtal värden som visas i följande skärmbild som visar hello.</span><span class="sxs-lookup"><span data-stu-id="24b15-105">hello resulting application will prompt hello user for phone call values, as shown in hello following screen shot.</span></span>

![Azure anropet formuläret med hjälp av Twilio och Java][twilio_java]

<span data-ttu-id="24b15-107">Du behöver toodo hello följande toouse hello koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="24b15-107">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="24b15-108">Skaffa ett Twilio-konto och autentisering token.</span><span class="sxs-lookup"><span data-stu-id="24b15-108">Acquire a Twilio account and authentication token.</span></span> <span data-ttu-id="24b15-109">tooget igång med Twilio, utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="24b15-109">tooget started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="24b15-110">Du kan registrera dig på [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="24b15-110">You can sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="24b15-111">Information om hello API som tillhandahålls av Twilio finns [http://www.twilio.com/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="24b15-111">For information about hello API provided by Twilio, see [http://www.twilio.com/api][twilio_api].</span></span>
2. <span data-ttu-id="24b15-112">Hämta hello Twilio JAR.</span><span class="sxs-lookup"><span data-stu-id="24b15-112">Obtain hello Twilio JAR.</span></span> <span data-ttu-id="24b15-113">Vid [https://github.com/twilio/twilio-java][twilio_java_github], kan du hämta hello GitHub källor och skapa egna JAR eller hämta en förskapad JAR (med eller utan beroenden).</span><span class="sxs-lookup"><span data-stu-id="24b15-113">At [https://github.com/twilio/twilio-java][twilio_java_github], you can download hello GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
   <span data-ttu-id="24b15-114">hello koden i den här artikeln skrevs med hello fördefinierade JAR TwilioJava 3.3.8 med beroenden.</span><span class="sxs-lookup"><span data-stu-id="24b15-114">hello code in this topic was written using hello pre-built TwilioJava-3.3.8-with-dependencies JAR.</span></span>
3. <span data-ttu-id="24b15-115">Lägg till hello JAR tooyour Java byggsökväg.</span><span class="sxs-lookup"><span data-stu-id="24b15-115">Add hello JAR tooyour Java build path.</span></span>
4. <span data-ttu-id="24b15-116">Om du använder Eclipse toocreate Java-program lägger hello Twilio JAR i programmet distributionsfilen (WAR) funktionen Eclipses distribution sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="24b15-116">If you are using Eclipse toocreate this Java application, include hello Twilio JAR in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="24b15-117">Om du inte använder Eclipse toocreate Java-program, kontrollera hello Twilio JAR ingår i hello samma Azure roll som Java program och tillagda toohello klass-sökväg i ditt program.</span><span class="sxs-lookup"><span data-stu-id="24b15-117">If you are not using Eclipse toocreate this Java application, ensure hello Twilio JAR is included within hello same Azure role as your Java application, and added toohello class path of your application.</span></span>
5. <span data-ttu-id="24b15-118">Kontrollera din cacerts keystore innehåller hello Equifax certifikatutfärdare för säkra-certifikat med MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello seriella nummer är 35:DE:F4:CF och hello SHA1 fingeravtryck D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="24b15-118">Ensure your cacerts keystore contains hello Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello serial number is 35:DE:F4:CF and hello SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="24b15-119">Det här är hello certifikat certifikatutfärdarens certifikat för hello [https://api.twilio.com] [ twilio_api_service] -tjänsten som anropas när du använder Twilio APIs.</span><span class="sxs-lookup"><span data-stu-id="24b15-119">This is hello certificate authority (CA) certificate for hello [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="24b15-120">Information om att lägga till den här Certifikatutfärdarens certifikat tooyour JDK'S cacert store finns i [att lägga till ett certifikat toohello certifikatarkiv för Java CA][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="24b15-120">For information about adding this CA certificate tooyour JDK's cacert store, see [Adding a Certificate toohello Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="24b15-121">Dessutom förtrogenhet med hello information [skapar Hello World använder hello Azure Toolkit för Eclipse][azure_java_eclipse_hello_world], eller med andra tekniker för Java-program i Azure om du använder inte Eclipse, rekommenderas starkt.</span><span class="sxs-lookup"><span data-stu-id="24b15-121">Additionally, familiarity with hello information at [Creating a Hello World Application Using hello Azure Toolkit for Eclipse][azure_java_eclipse_hello_world], or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="24b15-122">Skapa ett webbformulär för ett samtal</span><span class="sxs-lookup"><span data-stu-id="24b15-122">Create a web form for making a call</span></span>
<span data-ttu-id="24b15-123">hello följande kod visar hur toocreate en webbplats formuläret tooretrieve användardata för ett samtal.</span><span class="sxs-lookup"><span data-stu-id="24b15-123">hello following code shows how toocreate a web form tooretrieve user data for making a call.</span></span> <span data-ttu-id="24b15-124">Det här exemplet, ett nytt dynamiskt webbprojekt heter **TwilioCloud**, har skapats och **callform.jsp** har lagts till som en JSP-fil.</span><span class="sxs-lookup"><span data-stu-id="24b15-124">For purposes of this example, a new dynamic web project, named **TwilioCloud**, was created, and **callform.jsp** was added as a JSP file.</span></span>

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Automated call form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Make this call</b>.</p>
     <br/>
      <form action="makecall.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="callTo" value="" />
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="callFrom" value="" />
           </td>
         </tr>
         <tr>
           <td>Call message:</td>
           <td><input type="text" size=400 name="callText" value="Hello. This is hello call text. Good bye." />
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Make this call" />
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-hello-code-toomake-hello-call"></a><span data-ttu-id="24b15-125">Skapa hello kod toomake hello anrop</span><span class="sxs-lookup"><span data-stu-id="24b15-125">Create hello code toomake hello call</span></span>
<span data-ttu-id="24b15-126">hello följande kod som anropas när hello användaren Slutför hello formuläret som visas av callform.jsp, skapar anropet hälsningsmeddelande och genererar hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="24b15-126">hello following code, which is called when hello user completes hello form displayed by callform.jsp, creates hello call message and generates hello call.</span></span> <span data-ttu-id="24b15-127">För det här exemplet hello JSP-fil med namnet **makecall.jsp** och lades toohello **TwilioCloud** projekt.</span><span class="sxs-lookup"><span data-stu-id="24b15-127">For purposes of this example, hello JSP file is named **makecall.jsp** and was added toohello **TwilioCloud** project.</span></span> <span data-ttu-id="24b15-128">(Använd ditt Twilio-konto och autentisering token i stället för hello platshållare för värden som har tilldelats för**accountSID** och **authToken** i hello koden nedan.)</span><span class="sxs-lookup"><span data-stu-id="24b15-128">(Use your Twilio account and authentication token instead of hello placeholder values assigned too**accountSID** and **authToken** in hello code below.)</span></span>

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    import="java.util.*"
    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
    pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Call processing happens here</title>
    </head>
    <body>
        <b>This is my make call page.</b><p/>
     <%
    try 
    {
         // Use your account SID and authentication token instead
         // of hello placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of hello Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve hello account, used later tooretrieve hello CallFactory.
         Account account = client.getAccount();

         // Display hello client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display hello API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve hello values entered by hello user.
         String callTo = request.getParameter("callTo");  
         // hello Outgoing Caller ID, used for hello From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in hello user's text with '%20', 
         // toomake hello text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using hello Twilio message and hello user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display hello message URL.
         out.println("<p>");
         out.println("hello URL is " + Url);
         out.println("</p>");

         // Place hello call From, tooand URL values into a hash map. 
         HashMap<String, String> params = new HashMap<String, String>();
         params.put("From", callFrom);
         params.put("To", callTo);
         params.put("Url", Url);

         CallFactory callFactory = account.getCallFactory();
         Call call = callFactory.create(params);
         out.println("<p>Call status: " + call.getStatus()  + "</p>"); 
    } 
    catch (TwilioRestException e) 
    {
        out.println("<p>TwilioRestException encountered: " + e.getMessage() + "</p>");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    catch (Exception e) 
    {
        out.println("<p>Exception encountered: " + e.getMessage() + "");
        out.println("<p>StackTrace: " + e.getStackTrace().toString() + "</p>");
    }
    %>
    </body>
    </html>

<span data-ttu-id="24b15-129">Dessutom toomaking Hej anrop, makecall.jsp visar hello Twilio slutpunkt, API-versionen och hello samtalsstatus.</span><span class="sxs-lookup"><span data-stu-id="24b15-129">In addition toomaking hello call, makecall.jsp displays hello Twilio endpoint, API version, and hello call status.</span></span> <span data-ttu-id="24b15-130">Följande skärmbild som visar hello är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="24b15-130">An example is hello following screen shot:</span></span>

![Azure anropet svaret med hjälp av Twilio och Java][twilio_java_response]

## <a name="run-hello-application"></a><span data-ttu-id="24b15-132">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="24b15-132">Run hello application</span></span>
<span data-ttu-id="24b15-133">Följande är hello anvisningar toorun din ansökan. information för de här stegen finns på [skapar Hello World använder hello Azure Toolkit för Eclipse][azure_java_eclipse_hello_world].</span><span class="sxs-lookup"><span data-stu-id="24b15-133">Following are hello high-level steps toorun your application; details for these steps can be found at [Creating a Hello World Application Using hello Azure Toolkit for Eclipse][azure_java_eclipse_hello_world].</span></span>

1. <span data-ttu-id="24b15-134">Exportera din TwilioCloud WAR-toohello Azure **approot** mapp.</span><span class="sxs-lookup"><span data-stu-id="24b15-134">Export your TwilioCloud WAR toohello Azure **approot** folder.</span></span> 
2. <span data-ttu-id="24b15-135">Ändra **startup.cmd** toounzip TwilioCloud-WAR.</span><span class="sxs-lookup"><span data-stu-id="24b15-135">Modify **startup.cmd** toounzip your TwilioCloud WAR.</span></span>
3. <span data-ttu-id="24b15-136">Kompilera programmet för hello beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="24b15-136">Compile your application for hello compute emulator.</span></span>
4. <span data-ttu-id="24b15-137">Starta distributionen i hello beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="24b15-137">Start your deployment in hello compute emulator.</span></span>
5. <span data-ttu-id="24b15-138">Öppna en webbläsare och kör **http://localhost:8080/TwilioCloud/callform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="24b15-138">Open a browser, and run **http://localhost:8080/TwilioCloud/callform.jsp**.</span></span>
6. <span data-ttu-id="24b15-139">Ange värden i formuläret hello klickar du på **gör det här anropet**, och sedan visa hello resultatet i makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="24b15-139">Enter values in hello form, click **Make this call**, and then see hello results in makecall.jsp.</span></span>

<span data-ttu-id="24b15-140">När du är klar toodeploy tooAzure kompilera om för distributionen toohello molnet, distribuera tooAzure och kör http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp i hello webbläsare (Ersätt värdet för *your_hosted_name*).</span><span class="sxs-lookup"><span data-stu-id="24b15-140">When you are ready toodeploy tooAzure, recompile for deployment toohello cloud, deploy tooAzure, and run http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in hello browser (substitute your value for *your_hosted_name*).</span></span>

## <a name="next-steps"></a><span data-ttu-id="24b15-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24b15-141">Next steps</span></span>
<span data-ttu-id="24b15-142">Den här koden har angetts tooshow du grundläggande funktioner med hjälp av Twilio i Java i Azure.</span><span class="sxs-lookup"><span data-stu-id="24b15-142">This code was provided tooshow you basic functionality using Twilio in Java on Azure.</span></span> <span data-ttu-id="24b15-143">Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion.</span><span class="sxs-lookup"><span data-stu-id="24b15-143">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="24b15-144">Exempel:</span><span class="sxs-lookup"><span data-stu-id="24b15-144">For example:</span></span>

* <span data-ttu-id="24b15-145">Istället för att använda ett webbformulär, kan du använda Azure storage-blobbar eller SQL-databas toostore telefonnummer och anropa text.</span><span class="sxs-lookup"><span data-stu-id="24b15-145">Instead of using a web form, you could use Azure storage blobs or SQL Database toostore phone numbers and call text.</span></span> <span data-ttu-id="24b15-146">Information om hur du använder Azure storage-blobbar i Java finns [hur tooUse hello Blob Storage-tjänst från Java][howto_blob_storage_java].</span><span class="sxs-lookup"><span data-stu-id="24b15-146">For information about using Azure storage blobs in Java, see [How tooUse hello Blob Storage Service from Java][howto_blob_storage_java].</span></span> <span data-ttu-id="24b15-147">Information om hur du använder SQL-databas i Java finns [med hjälp av SQL-databas i Java][howto_sql_azure_java].</span><span class="sxs-lookup"><span data-stu-id="24b15-147">For information about using SQL Database in Java, see [Using SQL Database in Java][howto_sql_azure_java].</span></span>
* <span data-ttu-id="24b15-148">Du kan använda **RoleEnvironment.getConfigurationSettings** tooretrieve hello Twilio-konto-ID och autentisering token från din distribution konfigurationsinställningar, i stället för att hårdkoda hello värdena i makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="24b15-148">You could use **RoleEnvironment.getConfigurationSettings** tooretrieve hello Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding hello values in makecall.jsp.</span></span> <span data-ttu-id="24b15-149">Information om hello **RoleEnvironment** klassen, se [Using hello Azure Service Runtime Library i JSP] [ azure_runtime_jsp] och hello Azure körtid paketet dokumentationen på [http://dl.windowsazure.com/javadoc][azure_javadoc].</span><span class="sxs-lookup"><span data-stu-id="24b15-149">For information about hello **RoleEnvironment** class, see [Using hello Azure Service Runtime Library in JSP][azure_runtime_jsp] and hello Azure Service Runtime package documentation at [http://dl.windowsazure.com/javadoc][azure_javadoc].</span></span>
* <span data-ttu-id="24b15-150">Hej makecall.jsp kod tilldelar en Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url], toohello **Url** variabeln.</span><span class="sxs-lookup"><span data-stu-id="24b15-150">hello makecall.jsp code assigns a Twilio-provided URL, [http://twimlets.com/message][twimlet_message_url], toohello **Url** variable.</span></span> <span data-ttu-id="24b15-151">URL: en innehåller ett Twilio Markup Language (TwiML)-svar som informerar Twilio hur tooproceed med hello anropa.</span><span class="sxs-lookup"><span data-stu-id="24b15-151">This URL provides a Twilio Markup Language (TwiML) response that informs Twilio how tooproceed with hello call.</span></span> <span data-ttu-id="24b15-152">Till exempel hello TwiML som returneras kan innehålla en  **&lt;säg&gt;**  verb som resulterar i texten är talade toohello anrop mottagaren.</span><span class="sxs-lookup"><span data-stu-id="24b15-152">For example, hello TwiML that is returned can contain a **&lt;Say&gt;** verb that results in text being spoken toohello call recipient.</span></span> <span data-ttu-id="24b15-153">Istället för att använda hello Twilio-tillhandahållna URL kan du bygga egna service toorespond tooTwilio begäran. Mer information finns i [hur tooUse Twilio för röst- och SMS-funktioner i Java][howto_twilio_voice_sms_java].</span><span class="sxs-lookup"><span data-stu-id="24b15-153">Instead of using hello Twilio-provided URL, you could build your own service toorespond tooTwilio's request; for more information, see [How tooUse Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java].</span></span> <span data-ttu-id="24b15-154">Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml], och mer information om  **&lt;säg&gt;**  och andra Twilio-verb finns på [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="24b15-154">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about **&lt;Say&gt;** and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="24b15-155">Läsa hello Twilio säkerhetsriktlinjer på [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="24b15-155">Read hello Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="24b15-156">Mer information om Twilio finns [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="24b15-156">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="24b15-157">Se även</span><span class="sxs-lookup"><span data-stu-id="24b15-157">See Also</span></span>
* <span data-ttu-id="24b15-158">[Hur tooUse Twilio för röst- och SMS-funktioner i Java][howto_twilio_voice_sms_java]</span><span class="sxs-lookup"><span data-stu-id="24b15-158">[How tooUse Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java]</span></span>
* <span data-ttu-id="24b15-159">[Lägga till ett certifikat toohello Java CA-certifikatet i lagret][add_ca_cert]</span><span class="sxs-lookup"><span data-stu-id="24b15-159">[Adding a Certificate toohello Java CA Certificate Store][add_ca_cert]</span></span>

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_java_github]: http://github.com/twilio/twilio-java
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[azure_java_eclipse_hello_world]: http://msdn.microsoft.com/library/windowsazure/hh690944.aspx
[howto_twilio_voice_sms_java]: partner-twilio-java-how-to-use-voice-sms.md
[howto_blob_storage_java]: http://www.windowsazure.com/develop/java/how-to-guides/blob-storage/
[howto_sql_azure_java]: http://msdn.microsoft.com/library/windowsazure/hh749029.aspx
[azure_runtime_jsp]: http://msdn.microsoft.com/library/windowsazure/hh690948.aspx
[azure_javadoc]: http://dl.windowsazure.com/javadoc
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[twilio_java]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaCallForm.jpg
[twilio_java_response]: ./media/partner-twilio-java-phone-call-example/WA_TwilioJavaMakeCall.jpg
