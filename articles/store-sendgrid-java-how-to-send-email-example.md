---
title: aaastore-sendgrid-Java-How-to-Send-email-example
description: "Hur toosend e-post med SendGrid från Java i en Azure-distribution"
services: 
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: af096a73-6985-4350-92e4-ce1722c8d72f
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: vibhork;dominic.may@sendgrid.com;elmer.thomas@sendgrid.com
ms.openlocfilehash: 51fde1fc71467f8252532b30d2f87856ec25067b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-java-in-an-azure-deployment"></a><span data-ttu-id="1d813-103">Hur tooSend e-post med hjälp av SendGrid från Java i en Azure-distribution</span><span class="sxs-lookup"><span data-stu-id="1d813-103">How tooSend Email Using SendGrid from Java in an Azure Deployment</span></span>
<span data-ttu-id="1d813-104">hello följande exempel visar hur du kan använda SendGrid toosend e-post från en webbsida som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="1d813-104">hello following example shows you how you can use SendGrid toosend emails from a web page hosted in Azure.</span></span> <span data-ttu-id="1d813-105">hello resulterande program frågar hello användare för e-värden som visas i följande skärmbild som visar hello.</span><span class="sxs-lookup"><span data-stu-id="1d813-105">hello resulting application will prompt hello user for email values, as shown in hello following screen shot.</span></span>

![E-formulär][emailform]

<span data-ttu-id="1d813-107">hello resulterande e-post ser liknande toohello följande skärmdump.</span><span class="sxs-lookup"><span data-stu-id="1d813-107">hello resulting email will look similar toohello following screen shot.</span></span>

![E-postmeddelande][emailsent]

<span data-ttu-id="1d813-109">Du behöver toodo hello följande toouse hello koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="1d813-109">You'll need toodo hello following toouse hello code in this topic:</span></span>

1. <span data-ttu-id="1d813-110">Hämta hello javax.mail burkar, till exempel från <http://www.oracle.com/technetwork/java/javamail/index.html>.</span><span class="sxs-lookup"><span data-stu-id="1d813-110">Obtain hello javax.mail JARs, for example from <http://www.oracle.com/technetwork/java/javamail/index.html>.</span></span>
2. <span data-ttu-id="1d813-111">Lägg till hello burkar tooyour Java byggsökväg.</span><span class="sxs-lookup"><span data-stu-id="1d813-111">Add hello JARs tooyour Java build path.</span></span>
3. <span data-ttu-id="1d813-112">Om du använder Eclipse toocreate Java-program kan inkludera du hello SendGrid bibliotek i din distribution programfilen (WAR) funktionen Eclipses distribution sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="1d813-112">If you are using Eclipse toocreate this Java application, you can include hello SendGrid libraries in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="1d813-113">Om du inte använder Eclipse toocreate Java-program, kontrollera hello bibliotek ingår i hello samma Azure roll som Java program och tillagda toohello klass-sökväg i ditt program.</span><span class="sxs-lookup"><span data-stu-id="1d813-113">If you are not using Eclipse toocreate this Java application, ensure hello libraries are included within hello same Azure role as your Java application, and added toohello class path of your application.</span></span>

<span data-ttu-id="1d813-114">Du måste också ha egna SendGrid användarnamn och lösenord, toobe kan toosend hello e-post.</span><span class="sxs-lookup"><span data-stu-id="1d813-114">You must also have your own SendGrid username and password, toobe able toosend hello email.</span></span> <span data-ttu-id="1d813-115">tooget igång med SendGrid, se [hur toosend e-post med SendGrid från Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="1d813-115">tooget started with SendGrid, see [How toosend email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

<span data-ttu-id="1d813-116">Dessutom förtrogenhet med hello information [skapar en Hello World-program för Azure i Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), eller med andra tekniker som värd för Java-program i Azure om du inte använder Eclipse rekommenderas starkt.</span><span class="sxs-lookup"><span data-stu-id="1d813-116">Additionally, familiarity with hello information at [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-sending-email"></a><span data-ttu-id="1d813-117">Skapa ett webbformulär för att skicka e-post</span><span class="sxs-lookup"><span data-stu-id="1d813-117">Create a web form for sending email</span></span>
<span data-ttu-id="1d813-118">hello följande kod visar hur toocreate en webbplats formulärdata tooretrieve användaren för att skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="1d813-118">hello following code shows how toocreate a web form tooretrieve user data for sending email.</span></span> <span data-ttu-id="1d813-119">För det här innehållet är hello JSP-fil med namnet **emailform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="1d813-119">For purposes of this content, hello JSP file is named **emailform.jsp**.</span></span>

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email form</title>
    </head>
    <body>
     <p>Fill in all fields and click <b>Send this email</b>.</p>
     <br/>
      <form action="sendemail.jsp" method="post">
       <table>
         <tr>
           <td>To:</td>
           <td><input type="text" size=50 name="emailTo">
           </td>
         </tr>
         <tr>
           <td>From:</td>
           <td><input type="text" size=50 name="emailFrom">
           </td>
         </tr>
         <tr>
           <td>Subject:</td>
           <td><input type="text" size=100 name="emailSubject" value="My email subject">
           </td>
         </tr>
         <tr>
           <td>Text:</td>
           <td><input type="text" size=400 name="emailText" value="Hello,<p>This is my message.</p>Thank you." />
           </td>
         </tr>
         <tr>
           <td>SendGrid user name:</td>
           <td><input type="text" name="sendGridUser">
           </td>
         </tr>
         <tr>
           <td>SendGrid password:</td>
           <td><input type="password" name="sendGridPassword">
           </td>
         </tr>
         <tr>
           <td colspan=2><input type="submit" value="Send this email">
           </td>
         </tr>
       </table>
     </form>
     <br/>
    </body>
    </html>

## <a name="create-hello-code-toosend-hello-email"></a><span data-ttu-id="1d813-120">Skapa hello kod toosend hello e-post</span><span class="sxs-lookup"><span data-stu-id="1d813-120">Create hello code toosend hello email</span></span>
<span data-ttu-id="1d813-121">hello följande kod som anropas när du slutför hello formulär i emailform.jsp, skapar hello e-postmeddelande och skickar den.</span><span class="sxs-lookup"><span data-stu-id="1d813-121">hello following code, which is called when you complete hello form in emailform.jsp, creates hello email message and sends it.</span></span> <span data-ttu-id="1d813-122">För det här innehållet är hello JSP-fil med namnet **sendemail.jsp**.</span><span class="sxs-lookup"><span data-stu-id="1d813-122">For purposes of this content, hello JSP file is named **sendemail.jsp**.</span></span>

    <%@ page language="java" contentType="text/html; charset=ISO-8859-1"
        pageEncoding="ISO-8859-1" import="javax.activation.*, javax.mail.*, javax.mail.internet.*, java.util.Date, java.util.Properties" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Email processing happens here</title>
    </head>
    <body>
        <b>This is my send mail page.</b><p/>
     <%

     final String sendGridUser = request.getParameter("sendGridUser");
     final String sendGridPassword = request.getParameter("sendGridPassword");

     class SMTPAuthenticator extends Authenticator
     {
       public PasswordAuthentication getPasswordAuthentication()
       {
            String username = sendGridUser;
            String password = sendGridPassword;

            return new PasswordAuthentication(username, password);   
       }
     }
     try
     {

         // hello SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";

         Properties properties;

         properties = new Properties();

         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");

         // Display hello email fields entered by hello user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");

         // Create hello authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();

         // Create hello mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);

         // Display debug information toostdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);

         // Create hello message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 

         message = new MimeMessage(mailSession);

         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            

         // Specify hello email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);

         // Uncomment hello following if you want tooadd a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");

         // Uncomment hello following if you want tooenable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");

         Transport transport;
         transport = mailSession.getTransport();
         // Connect hello transport object.
         transport.connect();
         // Send hello message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close hello connection.
         transport.close();

        out.println("<p>Email processing completed.</p>");

     }
     catch (Exception e)
     {
         out.println("<p>Exception encountered: " + 
                            e.getMessage()     +
                            "</p>");   
     }
    %>

    </body>
    </html>

<span data-ttu-id="1d813-123">Dessutom toosending hello e-post, emailform.jsp ger ett resultat för hello användaren. följande skärmbild som visar hello är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="1d813-123">In addition toosending hello email, emailform.jsp provides a result for hello user; an example is hello following screen shot:</span></span>

![Skicka e-post-resultat][emailresult]

## <a name="next-steps"></a><span data-ttu-id="1d813-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1d813-125">Next steps</span></span>
<span data-ttu-id="1d813-126">Distribuera ditt program toohello beräkningsemulatorn och ange värden i en webbläsare som kör emailform.jsp i hello formuläret, klickar du på **skicka e-postmeddelandet**, och sedan visa resultatet i sendemail.jsp.</span><span class="sxs-lookup"><span data-stu-id="1d813-126">Deploy your application toohello compute emulator and within a browser run emailform.jsp, enter values in hello form, click **Send this email**, and then see results in sendemail.jsp.</span></span>

<span data-ttu-id="1d813-127">Den här koden har angetts tooshow du hur toouse SendGrid i Java i Azure.</span><span class="sxs-lookup"><span data-stu-id="1d813-127">This code was provided tooshow you how toouse SendGrid in Java on Azure.</span></span> <span data-ttu-id="1d813-128">Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion.</span><span class="sxs-lookup"><span data-stu-id="1d813-128">Before deploying tooAzure in production, you may want tooadd more error handling or other features.</span></span> <span data-ttu-id="1d813-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="1d813-129">For example:</span></span> 

* <span data-ttu-id="1d813-130">Du kan använda Azure storage-blobbar eller SQL-databas toostore e-postadresser och e-postmeddelanden, istället för att använda ett webbformulär.</span><span class="sxs-lookup"><span data-stu-id="1d813-130">You could use Azure storage blobs or SQL Database toostore email addresses and email messages, instead of using a web form.</span></span> <span data-ttu-id="1d813-131">Information om hur du använder Azure storage-blobbar i Java finns [hur tooUse hello Blob Storage-tjänst från Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span><span class="sxs-lookup"><span data-stu-id="1d813-131">For information about using Azure storage blobs in Java, see [How tooUse hello Blob Storage Service from Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span></span> <span data-ttu-id="1d813-132">Information om hur du använder SQL-databas i Java finns [med hjälp av SQL-databas i Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span><span class="sxs-lookup"><span data-stu-id="1d813-132">For information about using SQL Database in Java, see [Using SQL Database in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span></span>
* <span data-ttu-id="1d813-133">Du kan använda `RoleEnvironment.getConfigurationSettings` tooretrieve hello SendGrid användarnamn och lösenord från din distribution konfigurationsinställningar, i stället för hello web formuläret tooretrieve dessa värden.</span><span class="sxs-lookup"><span data-stu-id="1d813-133">You could use `RoleEnvironment.getConfigurationSettings` tooretrieve hello SendGrid username and password from your deployment's configuration settings, instead of using hello web form tooretrieve those values.</span></span> <span data-ttu-id="1d813-134">Information om hello `RoleEnvironment` klassen, se [Using hello Azure Service Runtime Library i JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) och hello Azure körtid paketet dokumentation på <http://dl.windowsazure.com/javadoc>.</span><span class="sxs-lookup"><span data-stu-id="1d813-134">For information about hello `RoleEnvironment` class, see [Using hello Azure Service Runtime Library in JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) and hello Azure Service Runtime package documentation at <http://dl.windowsazure.com/javadoc>.</span></span>
* <span data-ttu-id="1d813-135">Mer information om hur du använder SendGrid i Java finns [hur toosend e-post med SendGrid från Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="1d813-135">For more information about using SendGrid in Java, see [How toosend email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
