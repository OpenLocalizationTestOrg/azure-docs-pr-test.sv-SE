---
title: "Hur du gör ett telefonsamtal från Twilio (Java) | Microsoft Docs"
description: "Lär dig att ringa ett samtal från en webbsida med Twilio i ett Java-program på Azure."
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
ms.openlocfilehash: 04ecb80a2a9e15b549b47138caf71c7e64bda500
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-make-a-phone-call-using-twilio-in-a-java-application-on-azure"></a><span data-ttu-id="4b8c5-103">Hur du gör ett telefonsamtal med Twilio i ett Java-program på Azure</span><span class="sxs-lookup"><span data-stu-id="4b8c5-103">How to Make a Phone Call Using Twilio in a Java Application on Azure</span></span>
<span data-ttu-id="4b8c5-104">I följande exempel visas hur du kan använda Twilio ringa ett samtal från en webbsida som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-104">The following example shows you how you can use Twilio to make a call from a web page hosted in Azure.</span></span> <span data-ttu-id="4b8c5-105">Exempelprogrammet uppmanas användaren för telefonsamtal värden som visas i följande skärmbild visar.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-105">The resulting application will prompt the user for phone call values, as shown in the following screen shot.</span></span>

![Azure anropet formuläret med hjälp av Twilio och Java][twilio_java]

<span data-ttu-id="4b8c5-107">Du behöver göra följande för att använda koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="4b8c5-107">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="4b8c5-108">Skaffa ett Twilio-konto och autentisering token.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-108">Acquire a Twilio account and authentication token.</span></span> <span data-ttu-id="4b8c5-109">Kom igång med Twilio, utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-109">To get started with Twilio, evaluate pricing at [http://www.twilio.com/pricing][twilio_pricing].</span></span> <span data-ttu-id="4b8c5-110">Du kan registrera dig på [https://www.twilio.com/try-twilio][try_twilio].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-110">You can sign up at [https://www.twilio.com/try-twilio][try_twilio].</span></span> <span data-ttu-id="4b8c5-111">Mer information om API som tillhandahålls av Twilio finns [http://www.twilio.com/api][twilio_api].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-111">For information about the API provided by Twilio, see [http://www.twilio.com/api][twilio_api].</span></span>
2. <span data-ttu-id="4b8c5-112">Hämta Twilio JAR.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-112">Obtain the Twilio JAR.</span></span> <span data-ttu-id="4b8c5-113">Vid [https://github.com/twilio/twilio-java][twilio_java_github], du kan hämta GitHub-datakällor och skapa egna JAR eller hämta en förskapad JAR (med eller utan beroenden).</span><span class="sxs-lookup"><span data-stu-id="4b8c5-113">At [https://github.com/twilio/twilio-java][twilio_java_github], you can download the GitHub sources and create your own JAR, or download a pre-built JAR (with or without dependencies).</span></span>
   <span data-ttu-id="4b8c5-114">Koden i det här avsnittet har skrivits med hjälp av fördefinierade TwilioJava 3.3.8 med beroenden JAR.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-114">The code in this topic was written using the pre-built TwilioJava-3.3.8-with-dependencies JAR.</span></span>
3. <span data-ttu-id="4b8c5-115">Lägg till JAR din Java byggsökväg.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-115">Add the JAR to your Java build path.</span></span>
4. <span data-ttu-id="4b8c5-116">Om du använder Eclipse för att skapa den här Java-program, lägger Twilio JAR i programmet distributionsfilen (WAR) funktionen Eclipses distribution sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-116">If you are using Eclipse to create this Java application, include the Twilio JAR in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="4b8c5-117">Om du inte använder Eclipse för att skapa Java-program, kontrollera Twilio JAR är ingår i samma Azure rollen som Java-programmet, och lägga till klassen sökvägen till programmet.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-117">If you are not using Eclipse to create this Java application, ensure the Twilio JAR is included within the same Azure role as your Java application, and added to the class path of your application.</span></span>
5. <span data-ttu-id="4b8c5-118">Kontrollera din cacerts keystore innehåller Equifax certifikatutfärdare för säkra-certifikat med MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (serienumret är 35:DE:F4:CF och SHA1 fingeravtrycket D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A).</span><span class="sxs-lookup"><span data-stu-id="4b8c5-118">Ensure your cacerts keystore contains the Equifax Secure Certificate Authority certificate with MD5 fingerprint 67:CB:9D:C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (the serial number is 35:DE:F4:CF and the SHA1 fingerprint is D2:32:09:AD:23:D3:14:23:21:74:E4:0D:7F:9D:62:13:97:86:63:3A).</span></span> <span data-ttu-id="4b8c5-119">Det här är certifikatet certifikatutfärdarens certifikat för den [https://api.twilio.com] [ twilio_api_service] -tjänsten som anropas när du använder Twilio APIs.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-119">This is the certificate authority (CA) certificate for the [https://api.twilio.com][twilio_api_service] service, which is called when you use Twilio APIs.</span></span> <span data-ttu-id="4b8c5-120">Information om att lägga till den här Certifikatutfärdarens certifikat till din JDK cacert store finns i [att lägga till ett certifikat i certifikatarkivet för Certifikatutfärdaren i Java][add_ca_cert].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-120">For information about adding this CA certificate to your JDK's cacert store, see [Adding a Certificate to the Java CA Certificate Store][add_ca_cert].</span></span>

<span data-ttu-id="4b8c5-121">Dessutom förtrogenhet med informationen på [skapar en Hello World program med hjälp av Azure-verktygen för Eclipse][azure_java_eclipse_hello_world], eller med andra tekniker för om du är värd för Java-program i Azure Vi rekommenderar starkt att du inte använder Eclipse.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-121">Additionally, familiarity with the information at [Creating a Hello World Application Using the Azure Toolkit for Eclipse][azure_java_eclipse_hello_world], or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-making-a-call"></a><span data-ttu-id="4b8c5-122">Skapa ett webbformulär för ett samtal</span><span class="sxs-lookup"><span data-stu-id="4b8c5-122">Create a web form for making a call</span></span>
<span data-ttu-id="4b8c5-123">Följande kod visar hur du skapar ett webbformulär för att hämta användardata för ett samtal.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-123">The following code shows how to create a web form to retrieve user data for making a call.</span></span> <span data-ttu-id="4b8c5-124">Det här exemplet, ett nytt dynamiskt webbprojekt heter **TwilioCloud**, har skapats och **callform.jsp** har lagts till som en JSP-fil.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-124">For purposes of this example, a new dynamic web project, named **TwilioCloud**, was created, and **callform.jsp** was added as a JSP file.</span></span>

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
           <td><input type="text" size=400 name="callText" value="Hello. This is the call text. Good bye." />
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

## <a name="create-the-code-to-make-the-call"></a><span data-ttu-id="4b8c5-125">Skapa koden för att göra anrop</span><span class="sxs-lookup"><span data-stu-id="4b8c5-125">Create the code to make the call</span></span>
<span data-ttu-id="4b8c5-126">Följande kod, som kallas när användaren slutför formuläret som visas av callform.jsp, skapar anropet meddelandet och genererar anropet.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-126">The following code, which is called when the user completes the form displayed by callform.jsp, creates the call message and generates the call.</span></span> <span data-ttu-id="4b8c5-127">För det här exemplet JSP-fil med namnet **makecall.jsp** och lades till i **TwilioCloud** projekt.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-127">For purposes of this example, the JSP file is named **makecall.jsp** and was added to the **TwilioCloud** project.</span></span> <span data-ttu-id="4b8c5-128">(Använd ditt Twilio-konto och autentisering token i stället för platshållarvärdena för **accountSID** och **authToken** i koden nedan.)</span><span class="sxs-lookup"><span data-stu-id="4b8c5-128">(Use your Twilio account and authentication token instead of the placeholder values assigned to **accountSID** and **authToken** in the code below.)</span></span>

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
         // of the placeholders shown here.
         String accountSID = "your_twilio_account";
         String authToken = "your_twilio_authentication_token";

         // Instantiate an instance of the Twilio client.     
         TwilioRestClient client;
         client = new TwilioRestClient(accountSID, authToken);

         // Retrieve the account, used later to retrieve the CallFactory.
         Account account = client.getAccount();

         // Display the client endpoint. 
         out.println("<p>Using Twilio endpoint " + client.getEndpoint() + ".</p>");

         // Display the API version.
         String APIVERSION = TwilioRestClient.DEFAULT_VERSION;
         out.println("<p>Twilio client API version is " + APIVERSION + ".</p>");

         // Retrieve the values entered by the user.
         String callTo = request.getParameter("callTo");  
         // The Outgoing Caller ID, used for the From parameter,
         // must have previously been verified with Twilio.
         String callFrom = request.getParameter("callFrom");
         String userText = request.getParameter("callText");

         // Replace spaces in the user's text with '%20', 
         // to make the text suitable for a URL.
         userText = userText.replace(" ", "%20");

         // Create a URL using the Twilio message and the user-entered text.
         String Url="http://twimlets.com/message";
         Url = Url + "?Message%5B0%5D=" + userText;

         // Display the message URL.
         out.println("<p>");
         out.println("The URL is " + Url);
         out.println("</p>");

         // Place the call From, To and URL values into a hash map. 
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

<span data-ttu-id="4b8c5-129">Förutom att göra anropet visar makecall.jsp Twilio-slutpunkten och API-versionen av samtalsstatus.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-129">In addition to making the call, makecall.jsp displays the Twilio endpoint, API version, and the call status.</span></span> <span data-ttu-id="4b8c5-130">Följande skärmdump är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="4b8c5-130">An example is the following screen shot:</span></span>

![Azure anropet svaret med hjälp av Twilio och Java][twilio_java_response]

## <a name="run-the-application"></a><span data-ttu-id="4b8c5-132">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="4b8c5-132">Run the application</span></span>
<span data-ttu-id="4b8c5-133">Följande är de övergripande stegen för att köra programmet; information för de här stegen finns på [skapar en Hello World program med hjälp av Azure-verktygen för Eclipse][azure_java_eclipse_hello_world].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-133">Following are the high-level steps to run your application; details for these steps can be found at [Creating a Hello World Application Using the Azure Toolkit for Eclipse][azure_java_eclipse_hello_world].</span></span>

1. <span data-ttu-id="4b8c5-134">Exportera din TwilioCloud WAR till Azure **approot** mapp.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-134">Export your TwilioCloud WAR to the Azure **approot** folder.</span></span> 
2. <span data-ttu-id="4b8c5-135">Ändra **startup.cmd** att packa TwilioCloud-WAR.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-135">Modify **startup.cmd** to unzip your TwilioCloud WAR.</span></span>
3. <span data-ttu-id="4b8c5-136">Kompilera programmet för beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-136">Compile your application for the compute emulator.</span></span>
4. <span data-ttu-id="4b8c5-137">Starta distributionen i beräkningsemulatorn.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-137">Start your deployment in the compute emulator.</span></span>
5. <span data-ttu-id="4b8c5-138">Öppna en webbläsare och kör **http://localhost:8080/TwilioCloud/callform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-138">Open a browser, and run **http://localhost:8080/TwilioCloud/callform.jsp**.</span></span>
6. <span data-ttu-id="4b8c5-139">Ange värden i formuläret, klickar du på **gör det här anropet**, och sedan visa resultaten i makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-139">Enter values in the form, click **Make this call**, and then see the results in makecall.jsp.</span></span>

<span data-ttu-id="4b8c5-140">När du är redo att distribuera till Azure, recompile för distribution till molnet, distribuera till Azure och kör http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp i webbläsaren (Ersätt värdet för  *your_hosted_name*).</span><span class="sxs-lookup"><span data-stu-id="4b8c5-140">When you are ready to deploy to Azure, recompile for deployment to the cloud, deploy to Azure, and run http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp in the browser (substitute your value for *your_hosted_name*).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b8c5-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b8c5-141">Next steps</span></span>
<span data-ttu-id="4b8c5-142">Den här koden har angetts för att visa grundläggande funktioner med hjälp av Twilio i Java i Azure.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-142">This code was provided to show you basic functionality using Twilio in Java on Azure.</span></span> <span data-ttu-id="4b8c5-143">Innan du distribuerar till Azure i produktion, kanske du vill lägga till flera felhantering eller andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-143">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="4b8c5-144">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4b8c5-144">For example:</span></span>

* <span data-ttu-id="4b8c5-145">Istället för att använda ett webbformulär, kunde du använda Azure storage-blobbar eller SQL-databas för att lagra telefonnummer och anropa text.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-145">Instead of using a web form, you could use Azure storage blobs or SQL Database to store phone numbers and call text.</span></span> <span data-ttu-id="4b8c5-146">Information om hur du använder Azure storage-blobbar i Java finns [hur du använder tjänsten Blob Storage från Java][howto_blob_storage_java].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-146">For information about using Azure storage blobs in Java, see [How to Use the Blob Storage Service from Java][howto_blob_storage_java].</span></span> <span data-ttu-id="4b8c5-147">Information om hur du använder SQL-databas i Java finns [med hjälp av SQL-databas i Java][howto_sql_azure_java].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-147">For information about using SQL Database in Java, see [Using SQL Database in Java][howto_sql_azure_java].</span></span>
* <span data-ttu-id="4b8c5-148">Du kan använda **RoleEnvironment.getConfigurationSettings** för att hämta Twilio-konto-ID och autentisering token från din distribution konfigurationsinställningar, i stället för att hårdkoda värdena i makecall.jsp.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-148">You could use **RoleEnvironment.getConfigurationSettings** to retrieve the Twilio account ID and authentication token from your deployment's configuration settings, instead of hard-coding the values in makecall.jsp.</span></span> <span data-ttu-id="4b8c5-149">Information om den **RoleEnvironment** klassen, se [med hjälp av Azure Service Runtime Library i JSP] [ azure_runtime_jsp] och dokumentation för körtid för Azure-paketet på [http://dl.windowsazure.com/javadoc][azure_javadoc].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-149">For information about the **RoleEnvironment** class, see [Using the Azure Service Runtime Library in JSP][azure_runtime_jsp] and the Azure Service Runtime package documentation at [http://dl.windowsazure.com/javadoc][azure_javadoc].</span></span>
* <span data-ttu-id="4b8c5-150">Koden makecall.jsp tilldelar en URL som anges Twilio [http://twimlets.com/message][twimlet_message_url], i den **Url** variabeln.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-150">The makecall.jsp code assigns a Twilio-provided URL, [http://twimlets.com/message][twimlet_message_url], to the **Url** variable.</span></span> <span data-ttu-id="4b8c5-151">URL: en innehåller ett Twilio Markup Language (TwiML)-svar som informerar Twilio hur du fortsätter med anropet.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-151">This URL provides a Twilio Markup Language (TwiML) response that informs Twilio how to proceed with the call.</span></span> <span data-ttu-id="4b8c5-152">Till exempel TwiML som returneras kan innehålla en  **&lt;säg&gt;**  verb som resulterar i text som talas till mottagaren anrop.</span><span class="sxs-lookup"><span data-stu-id="4b8c5-152">For example, the TwiML that is returned can contain a **&lt;Say&gt;** verb that results in text being spoken to the call recipient.</span></span> <span data-ttu-id="4b8c5-153">I stället för med den angivna Twilio-URL, kan du skapa tjänsten för att svara på Twilios begäran. Mer information finns i [så Använd Twilio för röst- och SMS-funktioner i Java][howto_twilio_voice_sms_java].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-153">Instead of using the Twilio-provided URL, you could build your own service to respond to Twilio's request; for more information, see [How to Use Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java].</span></span> <span data-ttu-id="4b8c5-154">Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml], och mer information om  **&lt;säg&gt;**  och andra Twilio-verb finns på [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-154">More information about TwiML can be found at [http://www.twilio.com/docs/api/twiml][twiml], and more information about **&lt;Say&gt;** and other Twilio verbs can be found at [http://www.twilio.com/docs/api/twiml/say][twilio_say].</span></span>
* <span data-ttu-id="4b8c5-155">Läs Twilio-riktlinjer för säkerhet på [https://www.twilio.com/docs/security][twilio_docs_security].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-155">Read the Twilio security guidelines at [https://www.twilio.com/docs/security][twilio_docs_security].</span></span>

<span data-ttu-id="4b8c5-156">Mer information om Twilio finns [https://www.twilio.com/docs][twilio_docs].</span><span class="sxs-lookup"><span data-stu-id="4b8c5-156">For additional information about Twilio, see [https://www.twilio.com/docs][twilio_docs].</span></span>

## <a name="see-also"></a><span data-ttu-id="4b8c5-157">Se även</span><span class="sxs-lookup"><span data-stu-id="4b8c5-157">See Also</span></span>
* <span data-ttu-id="4b8c5-158">[Hur du använder Twilio för röst- och SMS-funktioner i Java][howto_twilio_voice_sms_java]</span><span class="sxs-lookup"><span data-stu-id="4b8c5-158">[How to Use Twilio for Voice and SMS Capabilities in Java][howto_twilio_voice_sms_java]</span></span>
* <span data-ttu-id="4b8c5-159">[Lägga till ett certifikat i certifikatarkivet för Java-Certifikatutfärdare][add_ca_cert]</span><span class="sxs-lookup"><span data-stu-id="4b8c5-159">[Adding a Certificate to the Java CA Certificate Store][add_ca_cert]</span></span>

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
