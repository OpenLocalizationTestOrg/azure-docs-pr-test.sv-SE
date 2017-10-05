---
title: store-sendgrid-Java-How-to-Send-email-example
description: "Hur du skickar e-post med SendGrid från Java i en Azure-distribution"
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
ms.openlocfilehash: d80d7d9c54bad12a4d26d8623eeccdf9bc2a743a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-java-in-an-azure-deployment"></a><span data-ttu-id="25d77-103">Hur du skickar e-post med SendGrid från Java i Azure-distribution</span><span class="sxs-lookup"><span data-stu-id="25d77-103">How to Send Email Using SendGrid from Java in an Azure Deployment</span></span>
<span data-ttu-id="25d77-104">I följande exempel visas hur du kan använda SendGrid för att skicka e-post från en webbsida som finns på Azure.</span><span class="sxs-lookup"><span data-stu-id="25d77-104">The following example shows you how you can use SendGrid to send emails from a web page hosted in Azure.</span></span> <span data-ttu-id="25d77-105">Exempelprogrammet uppmanas användaren för e-värden som visas i följande skärmbild visar.</span><span class="sxs-lookup"><span data-stu-id="25d77-105">The resulting application will prompt the user for email values, as shown in the following screen shot.</span></span>

![E-formulär][emailform]

<span data-ttu-id="25d77-107">Resulterande e-postmeddelandet ser ut ungefär som följande skärmbild visar.</span><span class="sxs-lookup"><span data-stu-id="25d77-107">The resulting email will look similar to the following screen shot.</span></span>

![E-postmeddelande][emailsent]

<span data-ttu-id="25d77-109">Du behöver göra följande för att använda koden i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="25d77-109">You'll need to do the following to use the code in this topic:</span></span>

1. <span data-ttu-id="25d77-110">Hämta javax.mail burkar, till exempel från <http://www.oracle.com/technetwork/java/javamail/index.html>.</span><span class="sxs-lookup"><span data-stu-id="25d77-110">Obtain the javax.mail JARs, for example from <http://www.oracle.com/technetwork/java/javamail/index.html>.</span></span>
2. <span data-ttu-id="25d77-111">Lägg till burkar din Java byggsökväg.</span><span class="sxs-lookup"><span data-stu-id="25d77-111">Add the JARs to your Java build path.</span></span>
3. <span data-ttu-id="25d77-112">Om du använder Eclipse för att skapa Java-program kan du inkludera SendGrid-bibliotek i din distribution programfilen (WAR) funktionen Eclipses distribution sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="25d77-112">If you are using Eclipse to create this Java application, you can include the SendGrid libraries in your application deployment file (WAR) using Eclipse's deployment assembly feature.</span></span> <span data-ttu-id="25d77-113">Om du inte använder Eclipse för att skapa Java-program, se till att biblioteken ingår i samma Azure rollen som Java-programmet och lägga till klassen sökvägen till programmet.</span><span class="sxs-lookup"><span data-stu-id="25d77-113">If you are not using Eclipse to create this Java application, ensure the libraries are included within the same Azure role as your Java application, and added to the class path of your application.</span></span>

<span data-ttu-id="25d77-114">Du måste också ha en egen SendGrid användarnamn och lösenord för att kunna skicka e-postmeddelandet.</span><span class="sxs-lookup"><span data-stu-id="25d77-114">You must also have your own SendGrid username and password, to be able to send the email.</span></span> <span data-ttu-id="25d77-115">Kom igång med SendGrid, se [hur du skickar e-post med SendGrid från Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="25d77-115">To get started with SendGrid, see [How to send email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

<span data-ttu-id="25d77-116">Dessutom förtrogenhet med informationen på [skapar en Hello World program för Azure i Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), eller med andra tekniker som värd för Java-program i Azure om du inte använder Eclipse rekommenderas starkt.</span><span class="sxs-lookup"><span data-stu-id="25d77-116">Additionally, familiarity with the information at [Creating a Hello World Application for Azure in Eclipse](http://msdn.microsoft.com/library/windowsazure/hh690944), or with other techniques for hosting Java applications in Azure if you are not using Eclipse, is highly recommended.</span></span>

## <a name="create-a-web-form-for-sending-email"></a><span data-ttu-id="25d77-117">Skapa ett webbformulär för att skicka e-post</span><span class="sxs-lookup"><span data-stu-id="25d77-117">Create a web form for sending email</span></span>
<span data-ttu-id="25d77-118">Följande kod visar hur du skapar ett webbformulär för att hämta användardata för att skicka e-post.</span><span class="sxs-lookup"><span data-stu-id="25d77-118">The following code shows how to create a web form to retrieve user data for sending email.</span></span> <span data-ttu-id="25d77-119">För det här innehållet är JSP-fil med namnet **emailform.jsp**.</span><span class="sxs-lookup"><span data-stu-id="25d77-119">For purposes of this content, the JSP file is named **emailform.jsp**.</span></span>

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

## <a name="create-the-code-to-send-the-email"></a><span data-ttu-id="25d77-120">Skapa koden för att skicka e-postmeddelandet</span><span class="sxs-lookup"><span data-stu-id="25d77-120">Create the code to send the email</span></span>
<span data-ttu-id="25d77-121">Följande kod som anropas när du har slutfört formuläret i emailform.jsp, skapar e-postmeddelande och skickar den.</span><span class="sxs-lookup"><span data-stu-id="25d77-121">The following code, which is called when you complete the form in emailform.jsp, creates the email message and sends it.</span></span> <span data-ttu-id="25d77-122">För det här innehållet är JSP-fil med namnet **sendemail.jsp**.</span><span class="sxs-lookup"><span data-stu-id="25d77-122">For purposes of this content, the JSP file is named **sendemail.jsp**.</span></span>

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

         // The SendGrid SMTP server.
         String SMTP_HOST_NAME = "smtp.sendgrid.net";

         Properties properties;

         properties = new Properties();

         // Specify SMTP values.
         properties.put("mail.transport.protocol", "smtp");
         properties.put("mail.smtp.host", SMTP_HOST_NAME);
         properties.put("mail.smtp.port", 587);
         properties.put("mail.smtp.auth", "true");

         // Display the email fields entered by the user. 
         out.println("Value entered for email Subject: " + request.getParameter("emailSubject") + "<br/>");        
         out.println("Value entered for email      To: " + request.getParameter("emailTo") + "<br/>");
         out.println("Value entered for email    From: " + request.getParameter("emailFrom") + "<br/>");
         out.println("Value entered for email    Text: " + "<br/>" + request.getParameter("emailText") + "<br/>");

         // Create the authenticator object.
         Authenticator authenticator = new SMTPAuthenticator();

         // Create the mail session object.
         Session mailSession;
         mailSession = Session.getDefaultInstance(properties, authenticator);

         // Display debug information to stdout, useful when using the
         // compute emulator during development.
         mailSession.setDebug(true);

         // Create the message and message part objects.
         MimeMessage message;
         Multipart multipart;
         MimeBodyPart messagePart; 

         message = new MimeMessage(mailSession);

         multipart = new MimeMultipart("alternative");
         messagePart = new MimeBodyPart();
         messagePart.setContent(request.getParameter("emailText"), "text/html");
         multipart.addBodyPart(messagePart);            

         // Specify the email To, From, Subject and Content. 
         message.setFrom(new InternetAddress(request.getParameter("emailFrom")));
         message.addRecipient(Message.RecipientType.TO, new InternetAddress(request.getParameter("emailTo")));
         message.setSubject(request.getParameter("emailSubject")); 
         message.setContent(multipart);

         // Uncomment the following if you want to add a footer.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"footer\": {\"settings\": {\"enable\":1,\"text/html\": \"<html>This is my <b>email footer</b>.</html>\"}}}}");

         // Uncomment the following if you want to enable click tracking.
         // message.addHeader("X-SMTPAPI", "{\"filters\": {\"clicktrack\": {\"settings\": {\"enable\":1}}}}");

         Transport transport;
         transport = mailSession.getTransport();
         // Connect the transport object.
         transport.connect();
         // Send the message.
         transport.sendMessage(message,  message.getRecipients(Message.RecipientType.TO));
         // Close the connection.
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

<span data-ttu-id="25d77-123">Förutom att skicka e-postmeddelandet tillhandahåller emailform.jsp ett resultat för användaren. Följande skärmdump är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="25d77-123">In addition to sending the email, emailform.jsp provides a result for the user; an example is the following screen shot:</span></span>

![Skicka e-post-resultat][emailresult]

## <a name="next-steps"></a><span data-ttu-id="25d77-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="25d77-125">Next steps</span></span>
<span data-ttu-id="25d77-126">Distribuera appen till beräkningsemulatorn och kör emailform.jsp i en webbläsare, ange värden i formuläret, klicka på **skicka e-postmeddelandet**, och sedan visa resultatet i sendemail.jsp.</span><span class="sxs-lookup"><span data-stu-id="25d77-126">Deploy your application to the compute emulator and within a browser run emailform.jsp, enter values in the form, click **Send this email**, and then see results in sendemail.jsp.</span></span>

<span data-ttu-id="25d77-127">Den här koden har angetts för att visa dig hur du använder SendGrid i Java i Azure.</span><span class="sxs-lookup"><span data-stu-id="25d77-127">This code was provided to show you how to use SendGrid in Java on Azure.</span></span> <span data-ttu-id="25d77-128">Innan du distribuerar till Azure i produktion, kanske du vill lägga till flera felhantering eller andra funktioner.</span><span class="sxs-lookup"><span data-stu-id="25d77-128">Before deploying to Azure in production, you may want to add more error handling or other features.</span></span> <span data-ttu-id="25d77-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="25d77-129">For example:</span></span> 

* <span data-ttu-id="25d77-130">Du kan använda Azure storage-blobbar eller SQL-databas för att lagra e-postadresser och e-postmeddelanden, istället för att använda ett webbformulär.</span><span class="sxs-lookup"><span data-stu-id="25d77-130">You could use Azure storage blobs or SQL Database to store email addresses and email messages, instead of using a web form.</span></span> <span data-ttu-id="25d77-131">Information om hur du använder Azure storage-blobbar i Java finns [hur du använder tjänsten Blob Storage från Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span><span class="sxs-lookup"><span data-stu-id="25d77-131">For information about using Azure storage blobs in Java, see [How to Use the Blob Storage Service from Java](https://azure.microsoft.com/develop/java/how-to-guides/blob-storage/).</span></span> <span data-ttu-id="25d77-132">Information om hur du använder SQL-databas i Java finns [med hjälp av SQL-databas i Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span><span class="sxs-lookup"><span data-stu-id="25d77-132">For information about using SQL Database in Java, see [Using SQL Database in Java](https://azure.microsoft.com/develop/java/how-to-guides/using-sql-azure-in-java/).</span></span>
* <span data-ttu-id="25d77-133">Du kan använda `RoleEnvironment.getConfigurationSettings` att hämta SendGrid användarnamn och lösenord från din distribution konfigurationsinställningar, i stället för webbformuläret för att hämta värdena.</span><span class="sxs-lookup"><span data-stu-id="25d77-133">You could use `RoleEnvironment.getConfigurationSettings` to retrieve the SendGrid username and password from your deployment's configuration settings, instead of using the web form to retrieve those values.</span></span> <span data-ttu-id="25d77-134">Information om den `RoleEnvironment` klassen, se [med hjälp av Azure Service Runtime Library i JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) och dokumentation för körtid för Azure-paketet på <http://dl.windowsazure.com/javadoc>.</span><span class="sxs-lookup"><span data-stu-id="25d77-134">For information about the `RoleEnvironment` class, see [Using the Azure Service Runtime Library in JSP](http://msdn.microsoft.com/library/windowsazure/hh690948) and the Azure Service Runtime package documentation at <http://dl.windowsazure.com/javadoc>.</span></span>
* <span data-ttu-id="25d77-135">Mer information om hur du använder SendGrid i Java finns [hur du skickar e-post med SendGrid från Java](store-sendgrid-java-how-to-send-email.md).</span><span class="sxs-lookup"><span data-stu-id="25d77-135">For more information about using SendGrid in Java, see [How to send email using SendGrid from Java](store-sendgrid-java-how-to-send-email.md).</span></span>

[emailform]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailform.jpg
[emailsent]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaEmailSent.jpg
[emailresult]: ./media/store-sendgrid-java-how-to-send-email-example/SendGridJavaResult.jpg
