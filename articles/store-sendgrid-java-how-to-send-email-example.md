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
# <a name="how-toosend-email-using-sendgrid-from-java-in-an-azure-deployment"></a>Hur tooSend e-post med hjälp av SendGrid från Java i en Azure-distribution
hello följande exempel visar hur du kan använda SendGrid toosend e-post från en webbsida som finns på Azure. hello resulterande program frågar hello användare för e-värden som visas i följande skärmbild som visar hello.

![E-formulär][emailform]

hello resulterande e-post ser liknande toohello följande skärmdump.

![E-postmeddelande][emailsent]

Du behöver toodo hello följande toouse hello koden i det här avsnittet:

1. Hämta hello javax.mail burkar, till exempel från <http://www.oracle.com/technetwork/java/javamail/index.html>.
2. Lägg till hello burkar tooyour Java byggsökväg.
3. Om du använder Eclipse toocreate Java-program kan inkludera du hello SendGrid bibliotek i din distribution programfilen (WAR) funktionen Eclipses distribution sammansättningen. Om du inte använder Eclipse toocreate Java-program, kontrollera hello bibliotek ingår i hello samma Azure roll som Java program och tillagda toohello klass-sökväg i ditt program.

Du måste också ha egna SendGrid användarnamn och lösenord, toobe kan toosend hello e-post. tooget igång med SendGrid, se [hur toosend e-post med SendGrid från Java](store-sendgrid-java-how-to-send-email.md).

Dessutom förtrogenhet med hello information [skapar en Hello World-program för Azure i Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), eller med andra tekniker som värd för Java-program i Azure om du inte använder Eclipse rekommenderas starkt.

## <a name="create-a-web-form-for-sending-email"></a>Skapa ett webbformulär för att skicka e-post
hello följande kod visar hur toocreate en webbplats formulärdata tooretrieve användaren för att skicka e-post. För det här innehållet är hello JSP-fil med namnet **emailform.jsp**.

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

## <a name="create-hello-code-toosend-hello-email"></a>Skapa hello kod toosend hello e-post
hello följande kod som anropas när du slutför hello formulär i emailform.jsp, skapar hello e-postmeddelande och skickar den. För det här innehållet är hello JSP-fil med namnet **sendemail.jsp**.

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

Dessutom toosending hello e-post, emailform.jsp ger ett resultat för hello användaren. följande skärmbild som visar hello är ett exempel:

![Skicka e-post-resultat][emailresult]

## <a name="next-steps"></a>Nästa steg
Distribuera ditt program toohello beräkningsemulatorn och ange värden i en webbläsare som kör emailform.jsp i hello formuläret, klickar du på **skicka e-postmeddelandet**, och sedan visa resultatet i sendemail.jsp.

Den här koden har angetts tooshow du hur toouse SendGrid i Java i Azure. Du kanske vill tooadd mer felhantering eller andra funktioner innan du distribuerar tooAzure i produktion. Exempel: 

* Du kan använda Azure storage-blobbar eller SQL-databas toostore e-postadresser och e-postmeddelanden, istället för att använda ett webbformulär. Information om hur du använder Azure storage-blobbar i Java finns [hur tooUse hello Blob Storage-tjänst från Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/). Information om hur du använder SQL-databas i Java finns [med hjälp av SQL-databas i Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).
* Du kan använda `RoleEnvironment.getConfigurationSettings` tooretrieve hello SendGrid användarnamn och lösenord från din distribution konfigurationsinställningar, i stället för hello web formuläret tooretrieve dessa värden. Information om hello `RoleEnvironment` klassen, se [Using hello Azure Service Runtime Library i JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) och hello Azure körtid paketet dokumentation på <http://dl.windowsazure.com/javadoc>.
* Mer information om hur du använder SendGrid i Java finns [hur toosend e-post med SendGrid från Java](store-sendgrid-java-how-to-send-email.md).

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
