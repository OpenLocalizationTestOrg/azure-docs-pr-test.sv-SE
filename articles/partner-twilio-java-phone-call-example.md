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
# <a name="how-toomake-a-phone-call-using-twilio-in-a-java-application-on-azure"></a>Hur tooMake ett telefonsamtal med Twilio i ett Java-program på Azure
hello följande exempel visar hur du kan använda Twilio toomake ett samtal från en webbsida som finns på Azure. hello resulterande program frågar hello användaren för telefonsamtal värden som visas i följande skärmbild som visar hello.

![Azure anropet formuläret med hjälp av Twilio och Java][twilio_java]

Du behöver toodo hello följande toouse hello koden i det här avsnittet:

1. Skaffa ett Twilio-konto och autentisering token. tooget igång med Twilio, utvärdera priser på [http://www.twilio.com/pricing][twilio_pricing]. Du kan registrera dig på [https://www.twilio.com/try-twilio][try_twilio]. Information om hello API som tillhandahålls av Twilio finns [http://www.twilio.com/api][twilio_api].
2. Hämta hello Twilio JAR. Vid [https://github.com/twilio/twilio-java][twilio_java_github], kan du hämta hello GitHub källor och skapa egna JAR eller hämta en förskapad JAR (med eller utan beroenden).
   hello koden i den här artikeln skrevs med hello fördefinierade JAR TwilioJava 3.3.8 med beroenden.
3. Lägg till hello JAR tooyour Java byggsökväg.
4. Om du använder Eclipse toocreate Java-program lägger hello Twilio JAR i programmet distributionsfilen (WAR) funktionen Eclipses distribution sammansättningen. Om du inte använder Eclipse toocreate Java-program, kontrollera hello Twilio JAR ingår i hello samma Azure roll som Java program och tillagda toohello klass-sökväg i ditt program.
5. Kontrollera din cacerts keystore innehåller hello Equifax certifikatutfärdare för säkra-certifikat med MD5 fingeravtryck 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (hello seriella nummer är 35:DE:F4:CF och hello SHA1 fingeravtryck D2:32:09:AD:23:D 3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Det här är hello certifikat certifikatutfärdarens certifikat för hello [https://api.twilio.com] [ twilio_api_service] -tjänsten som anropas när du använder Twilio APIs. Information om att lägga till den här Certifikatutfärdarens certifikat tooyour JDK'S cacert store finns i [att lägga till ett certifikat toohello certifikatarkiv för Java CA][add_ca_cert].

Dessutom förtrogenhet med hello information [skapar Hello World använder hello Azure Toolkit för Eclipse][azure_java_eclipse_hello_world], eller med andra tekniker för Java-program i Azure om du använder inte Eclipse, rekommenderas starkt.

## <a name="create-a-web-form-for-making-a-call"></a>Skapa ett webbformulär för ett samtal
hello följande kod visar hur toocreate en webbplats formuläret tooretrieve användardata för ett samtal. Det här exemplet, ett nytt dynamiskt webbprojekt heter **TwilioCloud**, har skapats och **callform.jsp** har lagts till som en JSP-fil.

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

## <a name="create-hello-code-toomake-hello-call"></a>Skapa hello kod toomake hello anrop
hello följande kod som anropas när hello användaren Slutför hello formuläret som visas av callform.jsp, skapar anropet hälsningsmeddelande och genererar hello-anrop. För det här exemplet hello JSP-fil med namnet **makecall.jsp** och lades toohello **TwilioCloud** projekt. (Använd ditt Twilio-konto och autentisering token i stället för hello platshållare för värden som har tilldelats för**accountSID** och **authToken** i hello koden nedan.)

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

Dessutom toomaking Hej anrop, makecall.jsp visar hello Twilio slutpunkt, API-versionen och hello samtalsstatus. Följande skärmbild som visar hello är ett exempel:

![Azure anropet svaret med hjälp av Twilio och Java][twilio_java_response]

## <a name="run-hello-application"></a>Kör programmet hello
Följande är hello anvisningar toorun din ansökan. information för de här stegen finns på [skapar Hello World använder hello Azure Toolkit för Eclipse][azure_java_eclipse_hello_world].

1. Exportera din TwilioCloud WAR-toohello Azure **approot** mapp. 
2. Ändra **startup.cmd** toounzip TwilioCloud-WAR.
3. Kompilera programmet för hello beräkningsemulatorn.
4. Starta distributionen i hello beräkningsemulatorn.
5. Öppna en webbläsare och kör **http://localhost:8080/TwilioCloud/callform.jsp**.
6. Ange värden i formuläret hello klickar du på **gör det här anropet**, och sedan visa hello resultatet i makecall.jsp.

När du är klar toodeploy tooAzure kompilera om för distributionen toohello molnet, distribuera tooAzure och kör http://*your_hosted_name*.cloudapp.net/TwilioCloud/callform.jsp i hello webbläsare (Ersätt värdet för *your_hosted_name*).

## <a name="next-steps"></a>Nästa steg
Den här koden har angetts tooshow du grundläggande funktioner med hjälp av Twilio i Java i Azure. Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion. Exempel:

* Istället för att använda ett webbformulär, kan du använda Azure storage-blobbar eller SQL-databas toostore telefonnummer och anropa text. Information om hur du använder Azure storage-blobbar i Java finns [hur tooUse hello Blob Storage-tjänst från Java][howto_blob_storage_java]. Information om hur du använder SQL-databas i Java finns [med hjälp av SQL-databas i Java][howto_sql_azure_java].
* Du kan använda **RoleEnvironment.getConfigurationSettings** tooretrieve hello Twilio-konto-ID och autentisering token från din distribution konfigurationsinställningar, i stället för att hårdkoda hello värdena i makecall.jsp. Information om hello **RoleEnvironment** klassen, se [Using hello Azure Service Runtime Library i JSP] [ azure_runtime_jsp] och hello Azure körtid paketet dokumentationen på [http://dl.windowsazure.com/javadoc][azure_javadoc].
* Hej makecall.jsp kod tilldelar en Twilio-tillhandahållna URL [http://twimlets.com/message][twimlet_message_url], toohello **Url** variabeln. URL: en innehåller ett Twilio Markup Language (TwiML)-svar som informerar Twilio hur tooproceed med hello anropa. Till exempel hello TwiML som returneras kan innehålla en  **&lt;säg&gt;**  verb som resulterar i texten är talade toohello anrop mottagaren. Istället för att använda hello Twilio-tillhandahållna URL kan du bygga egna service toorespond tooTwilio begäran. Mer information finns i [hur tooUse Twilio för röst- och SMS-funktioner i Java][howto_twilio_voice_sms_java]. Mer information om TwiML kan hittas på [http://www.twilio.com/docs/api/twiml][twiml], och mer information om  **&lt;säg&gt;**  och andra Twilio-verb finns på [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Läsa hello Twilio säkerhetsriktlinjer på [https://www.twilio.com/docs/security][twilio_docs_security].

Mer information om Twilio finns [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Se även
* [Hur tooUse Twilio för röst- och SMS-funktioner i Java][howto_twilio_voice_sms_java]
* [Lägga till ett certifikat toohello Java CA-certifikatet i lagret][add_ca_cert]

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
