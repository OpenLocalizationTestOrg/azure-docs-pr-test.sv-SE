---
title: "Hur du använder SendGrid e-posttjänst (Java) | Microsoft Docs"
description: "Lär dig hur skicka e-post med SendGrid-e-posttjänsten på Azure. Kodexempel som skrivits i Java."
services: 
documentationcenter: java
author: thinkingserious
manager: sendgrid
editor: mollybos
ms.assetid: cc75c43e-ede9-492b-98c2-9147fcb92c21
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 10/30/2014
ms.author: elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork
ms.openlocfilehash: 85a0e302626ca14ac039ee6f662f372ddbeb62c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-java"></a><span data-ttu-id="473d9-104">Hur du skickar e-post med SendGrid från Java</span><span class="sxs-lookup"><span data-stu-id="473d9-104">How to Send Email Using SendGrid from Java</span></span>
<span data-ttu-id="473d9-105">Den här guiden visar hur du utför vanliga programmeringsuppgifter med SendGrid-e-posttjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="473d9-105">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="473d9-106">Exemplen är skrivna i Java.</span><span class="sxs-lookup"><span data-stu-id="473d9-106">The samples are written in Java.</span></span> <span data-ttu-id="473d9-107">Scenarier som tas upp inkluderar **konstruera e-post**, **skicka e-post**, **lägga till bilagor**, **med hjälp av filter**, och **uppdatera egenskaperna för**.</span><span class="sxs-lookup"><span data-stu-id="473d9-107">The scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="473d9-108">Mer information om SendGrid och skicka e-post finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="473d9-108">For more information on SendGrid and sending email, see the [Next steps](#next-steps) section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="473d9-109">Vad är SendGrid e-posttjänst?</span><span class="sxs-lookup"><span data-stu-id="473d9-109">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="473d9-110">SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="473d9-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="473d9-111">Vanliga Användningsscenarier för SendGrid är:</span><span class="sxs-lookup"><span data-stu-id="473d9-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="473d9-112">Skicka automatiskt kvitton till kunder</span><span class="sxs-lookup"><span data-stu-id="473d9-112">Automatically sending receipts to customers</span></span>
* <span data-ttu-id="473d9-113">Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="473d9-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="473d9-114">Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider</span><span class="sxs-lookup"><span data-stu-id="473d9-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="473d9-115">Generera rapporter för att identifiera trender</span><span class="sxs-lookup"><span data-stu-id="473d9-115">Generating reports to help identify trends</span></span>
* <span data-ttu-id="473d9-116">Vidarebefordran av kundfrågor</span><span class="sxs-lookup"><span data-stu-id="473d9-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="473d9-117">E-postaviseringar från ditt program</span><span class="sxs-lookup"><span data-stu-id="473d9-117">Email notifications from your application</span></span>

<span data-ttu-id="473d9-118">Mer information finns i <http://sendgrid.com>.</span><span class="sxs-lookup"><span data-stu-id="473d9-118">For more information, see <http://sendgrid.com>.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="473d9-119">Skapa ett SendGrid-konto</span><span class="sxs-lookup"><span data-stu-id="473d9-119">Create a SendGrid account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-the-javaxmail-libraries"></a><span data-ttu-id="473d9-120">Så här: använda javax.mail-bibliotek</span><span class="sxs-lookup"><span data-stu-id="473d9-120">How to: Use the javax.mail libraries</span></span>
<span data-ttu-id="473d9-121">Hämta javax.mail bibliotek, till exempel från <http://www.oracle.com/technetwork/java/javamail> och importera dem till din kod.</span><span class="sxs-lookup"><span data-stu-id="473d9-121">Obtain the javax.mail libraries, for example from <http://www.oracle.com/technetwork/java/javamail> and import them into your code.</span></span> <span data-ttu-id="473d9-122">Vid en hög nivå är processen för att använda biblioteket javax.mail för att skicka e-post via SMTP att göra följande:</span><span class="sxs-lookup"><span data-stu-id="473d9-122">At a high-level, the process for using the javax.mail library to send email using SMTP is to do the following:</span></span>

1. <span data-ttu-id="473d9-123">Ange SMTP-värden, inklusive SMTP-servern som för SendGrid smtp.sendgrid.net.</span><span class="sxs-lookup"><span data-stu-id="473d9-123">Specify the SMTP values, including the SMTP server, which for SendGrid is smtp.sendgrid.net.</span></span>

```
        import java.util.Properties;
        import javax.activation.*;
        import javax.mail.*;
        import javax.mail.internet.*;

        public class MyEmailer {
           private static final String SMTP_HOST_NAME = "smtp.sendgrid.net";
           private static final String SMTP_AUTH_USER = "your_sendgrid_username";
           private static final String SMTP_AUTH_PWD = "your_sendgrid_password";

           public static void main(String[] args) throws Exception{
               new MyEmailer().SendMail();
           }

           public void SendMail() throws Exception
           {
              Properties properties = new Properties();
                 properties.put("mail.transport.protocol", "smtp");
                 properties.put("mail.smtp.host", SMTP_HOST_NAME);
                 properties.put("mail.smtp.port", 587);
                 properties.put("mail.smtp.auth", "true");
                 // …
```

1. <span data-ttu-id="473d9-124">Utöka den *javax.mail.Authenticator* klass, och i din implementering av den *getPasswordAuthentication* -metoden returnerar din SendGrid användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="473d9-124">Extend the *javax.mail.Authenticator* class, and in your implementation of the *getPasswordAuthentication* method, return your SendGrid user name and password.</span></span>  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. <span data-ttu-id="473d9-125">Skapa en session autentiserade e-post via en *javax.mail.Session* objekt.</span><span class="sxs-lookup"><span data-stu-id="473d9-125">Create an authenticated email session through a *javax.mail.Session* object.</span></span>  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. <span data-ttu-id="473d9-126">Skapa ditt meddelande och tilldela **till**, **från**, **ämne** och innehåll värden.</span><span class="sxs-lookup"><span data-stu-id="473d9-126">Create your message and assign **To**, **From**, **Subject** and content values.</span></span> <span data-ttu-id="473d9-127">Detta framgår av [hur man: skapa ett e-postmeddelande](#how-to-create-an-email) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="473d9-127">This is shown in the [How To: Create an Email](#how-to-create-an-email) section.</span></span>
4. <span data-ttu-id="473d9-128">Skicka meddelandet via en *javax.mail.Transport* objekt.</span><span class="sxs-lookup"><span data-stu-id="473d9-128">Send the message through a *javax.mail.Transport* object.</span></span> <span data-ttu-id="473d9-129">Detta visas i den [hur man: skickar ett e-] [så här: skicka ett e-postmeddelande] avsnittet.</span><span class="sxs-lookup"><span data-stu-id="473d9-129">This is shown in the [How To: Send an Email][How to: Send an Email] section.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="473d9-130">Så här: skapa ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="473d9-130">How to: Create an email</span></span>
<span data-ttu-id="473d9-131">Nedan visas hur du anger värden för ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="473d9-131">The following shows how to specify values for an email.</span></span>

    MimeMessage message = new MimeMessage(mailSession);
    Multipart multipart = new MimeMultipart("alternative");
    BodyPart part1 = new MimeBodyPart();
    part1.setText("Hello, Your Contoso order has shipped. Thank you, John");
    BodyPart part2 = new MimeBodyPart();
    part2.setContent(
        "<p>Hello,</p>
        <p>Your Contoso order has <b>shipped</b>.</p>
        <p>Thank you,<br>John</br></p>",
        "text/html");
    multipart.addBodyPart(part1);
    multipart.addBodyPart(part2);
    message.setFrom(new InternetAddress("john@contoso.com"));
    message.addRecipient(Message.RecipientType.TO,
       new InternetAddress("someone@example.com"));
    message.setSubject("Your recent order");
    message.setContent(multipart);

## <a name="how-to-send-an-email"></a><span data-ttu-id="473d9-132">Så här: skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="473d9-132">How to: Send an email</span></span>
<span data-ttu-id="473d9-133">Nedan visas hur du skickar ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="473d9-133">The following shows how to send an email.</span></span>

    Transport transport = mailSession.getTransport();
    // Connect the transport object.
    transport.connect();
    // Send the message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close the connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="473d9-134">Så här: Lägg till en bifogad fil</span><span class="sxs-lookup"><span data-stu-id="473d9-134">How to: Add an attachment</span></span>
<span data-ttu-id="473d9-135">Följande kod visar hur du lägger till en bifogad fil.</span><span class="sxs-lookup"><span data-stu-id="473d9-135">The following code shows you how to add an attachment.</span></span>

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify the local file to attach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses the local file name as the attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a><span data-ttu-id="473d9-136">Så här: använda filter för att aktivera sidfötter, spårning och analys</span><span class="sxs-lookup"><span data-stu-id="473d9-136">How to: Use filters to enable footers, tracking, and analytics</span></span>
<span data-ttu-id="473d9-137">SendGrid ger ytterligare e-postfunktioner med *filter*.</span><span class="sxs-lookup"><span data-stu-id="473d9-137">SendGrid provides additional email functionality through the use of *filters*.</span></span> <span data-ttu-id="473d9-138">Dessa finns inställningar som du kan lägga till ett e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera Klicka spårning, Google analytics, prenumeration spårning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="473d9-138">These are settings that can be added to an email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="473d9-139">En fullständig lista över filter finns [filterinställningar][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="473d9-139">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

* <span data-ttu-id="473d9-140">Nedan visas hur du infogar en sidfot filter som resulterar i HTML-text som visas längst ned i e-postmeddelandet som skickas.</span><span class="sxs-lookup"><span data-stu-id="473d9-140">The following shows how to insert a footer filter that results in HTML text appearing at the bottom of the email being sent.</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* <span data-ttu-id="473d9-141">Ett annat exempel på ett filter är klickar du på spårning.</span><span class="sxs-lookup"><span data-stu-id="473d9-141">Another example of a filter is click tracking.</span></span> <span data-ttu-id="473d9-142">Anta att e-postmeddelandet innehåller en hyperlänk, till exempel följande, och du vill spåra Klicka hastighet:</span><span class="sxs-lookup"><span data-stu-id="473d9-142">Let’s say that your email text contains a hyperlink, such as the following, and you want to track the click rate:</span></span>

      messagePart.setContent(
          "Hello,
          <p>This is the body of the message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* <span data-ttu-id="473d9-143">Aktivera klicka spårning genom att använda följande kod:</span><span class="sxs-lookup"><span data-stu-id="473d9-143">To enable the click tracking, use the following code:</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a><span data-ttu-id="473d9-144">Så här: uppdatera e-egenskaper</span><span class="sxs-lookup"><span data-stu-id="473d9-144">How to: Update email properties</span></span>
<span data-ttu-id="473d9-145">Vissa egenskaper för e-post kan skrivas över med  **ange*egenskapen*** eller läggas till med hjälp av  **lägga till*egenskapen***.</span><span class="sxs-lookup"><span data-stu-id="473d9-145">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span>

<span data-ttu-id="473d9-146">Till exempel för att ange **ReplyTo** adresser, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="473d9-146">For example, to specify **ReplyTo** addresses, use the following:</span></span>

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

<span data-ttu-id="473d9-147">Att lägga till en **kopia** mottagaren, använder du följande:</span><span class="sxs-lookup"><span data-stu-id="473d9-147">To add a **Cc** recipient, use the following:</span></span>

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="473d9-148">Så här: använda ytterligare SendGrid-tjänster</span><span class="sxs-lookup"><span data-stu-id="473d9-148">How to: Use additional SendGrid services</span></span>
<span data-ttu-id="473d9-149">SendGrid erbjuder webbaserade API: er som du kan använda för att utnyttja ytterligare funktioner för SendGrid från Azure-program.</span><span class="sxs-lookup"><span data-stu-id="473d9-149">SendGrid offers web-based APIs that you can use to leverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="473d9-150">Fullständig information finns i [SendGrid API-dokumentationen][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="473d9-150">For full details, see the [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="473d9-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="473d9-151">Next steps</span></span>
<span data-ttu-id="473d9-152">Nu när du har lärt dig grunderna om tjänsten SendGrid e-post, kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="473d9-152">Now that you’ve learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="473d9-153">Exempel som visar med SendGrid i Azure-distribution: [hur du skickar e-post med SendGrid från Java i Azure-distribution](store-sendgrid-java-how-to-send-email-example.md)</span><span class="sxs-lookup"><span data-stu-id="473d9-153">Sample that demonstrates using SendGrid in an Azure deployment: [How to send email using SendGrid from Java in an Azure deployment](store-sendgrid-java-how-to-send-email-example.md)</span></span>
* <span data-ttu-id="473d9-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span><span class="sxs-lookup"><span data-stu-id="473d9-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span></span>
* <span data-ttu-id="473d9-155">SendGrid API-dokumentationen: <https://sendgrid.com/docs/API_Reference/index.html></span><span class="sxs-lookup"><span data-stu-id="473d9-155">SendGrid API documentation: <https://sendgrid.com/docs/API_Reference/index.html></span></span>
* <span data-ttu-id="473d9-156">SendGrid specialerbjudande för Azure-kunder: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="473d9-156">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
<span data-ttu-id="473d9-157">[molnbaserade e-posttjänst]: https://sendgrid.com/email-solutions</span><span class="sxs-lookup"><span data-stu-id="473d9-157">[cloud-based email service]: https://sendgrid.com/email-solutions</span></span>
<span data-ttu-id="473d9-158">[transaktionella e-postleverans]: https://sendgrid.com/transactional-email</span><span class="sxs-lookup"><span data-stu-id="473d9-158">[transactional email delivery]: https://sendgrid.com/transactional-email</span></span>
