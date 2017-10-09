---
title: "aaaHow toouse hello SendGrid e-posttjänst (Java) | Microsoft Docs"
description: "Lär dig hur skicka e-post med hello SendGrid e-posttjänst på Azure. Kodexempel som skrivits i Java."
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
ms.openlocfilehash: 542ce0003e94fc8f5551487d5a3cd6f75d27e8cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-java"></a><span data-ttu-id="86081-104">Hur tooSend e-post med hjälp av SendGrid från Java</span><span class="sxs-lookup"><span data-stu-id="86081-104">How tooSend Email Using SendGrid from Java</span></span>
<span data-ttu-id="86081-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med SendGrid e-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="86081-105">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="86081-106">hello exempel skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="86081-106">hello samples are written in Java.</span></span> <span data-ttu-id="86081-107">hello beskrivs scenarier där **konstruera e-post**, **skicka e-post**, **lägga till bilagor**, **med hjälp av filter**, och **uppdatera egenskaperna för**.</span><span class="sxs-lookup"><span data-stu-id="86081-107">hello scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="86081-108">Mer information om SendGrid och skicka e-post finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="86081-108">For more information on SendGrid and sending email, see hello [Next steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="86081-109">Vad är hello SendGrid e-posttjänst?</span><span class="sxs-lookup"><span data-stu-id="86081-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="86081-110">SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="86081-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="86081-111">Vanliga Användningsscenarier för SendGrid är:</span><span class="sxs-lookup"><span data-stu-id="86081-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="86081-112">Automatiskt skickar kvitton toocustomers</span><span class="sxs-lookup"><span data-stu-id="86081-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="86081-113">Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="86081-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="86081-114">Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider</span><span class="sxs-lookup"><span data-stu-id="86081-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="86081-115">Generera rapporter toohelp identifiera trender</span><span class="sxs-lookup"><span data-stu-id="86081-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="86081-116">Vidarebefordran av kundfrågor</span><span class="sxs-lookup"><span data-stu-id="86081-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="86081-117">E-postaviseringar från ditt program</span><span class="sxs-lookup"><span data-stu-id="86081-117">Email notifications from your application</span></span>

<span data-ttu-id="86081-118">Mer information finns i <http://sendgrid.com>.</span><span class="sxs-lookup"><span data-stu-id="86081-118">For more information, see <http://sendgrid.com>.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="86081-119">Skapa ett SendGrid-konto</span><span class="sxs-lookup"><span data-stu-id="86081-119">Create a SendGrid account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-hello-javaxmail-libraries"></a><span data-ttu-id="86081-120">Så här: använda hello javax.mail bibliotek</span><span class="sxs-lookup"><span data-stu-id="86081-120">How to: Use hello javax.mail libraries</span></span>
<span data-ttu-id="86081-121">Hämta hello javax.mail bibliotek, till exempel från <http://www.oracle.com/technetwork/java/javamail> och importera dem till din kod.</span><span class="sxs-lookup"><span data-stu-id="86081-121">Obtain hello javax.mail libraries, for example from <http://www.oracle.com/technetwork/java/javamail> and import them into your code.</span></span> <span data-ttu-id="86081-122">Vid en hög nivå är hello processen för att använda hello javax.mail biblioteket toosend e-post via SMTP toodo hello följande:</span><span class="sxs-lookup"><span data-stu-id="86081-122">At a high-level, hello process for using hello javax.mail library toosend email using SMTP is toodo hello following:</span></span>

1. <span data-ttu-id="86081-123">Ange hello SMTP-värden, inklusive hello SMTP-servern som för SendGrid smtp.sendgrid.net.</span><span class="sxs-lookup"><span data-stu-id="86081-123">Specify hello SMTP values, including hello SMTP server, which for SendGrid is smtp.sendgrid.net.</span></span>

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

1. <span data-ttu-id="86081-124">Utöka hello *javax.mail.Authenticator* klass, och i din implementering av den *getPasswordAuthentication* -metoden returnerar din SendGrid användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="86081-124">Extend hello *javax.mail.Authenticator* class, and in your implementation of the *getPasswordAuthentication* method, return your SendGrid user name and password.</span></span>  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. <span data-ttu-id="86081-125">Skapa en session autentiserade e-post via en *javax.mail.Session* objekt.</span><span class="sxs-lookup"><span data-stu-id="86081-125">Create an authenticated email session through a *javax.mail.Session* object.</span></span>  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. <span data-ttu-id="86081-126">Skapa ditt meddelande och tilldela **till**, **från**, **ämne** och innehåll värden.</span><span class="sxs-lookup"><span data-stu-id="86081-126">Create your message and assign **To**, **From**, **Subject** and content values.</span></span> <span data-ttu-id="86081-127">Detta framgår hello [hur man: skapa ett e-postmeddelande](#how-to-create-an-email) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="86081-127">This is shown in hello [How To: Create an Email](#how-to-create-an-email) section.</span></span>
4. <span data-ttu-id="86081-128">Skicka hello-meddelande via en *javax.mail.Transport* objekt.</span><span class="sxs-lookup"><span data-stu-id="86081-128">Send hello message through a *javax.mail.Transport* object.</span></span> <span data-ttu-id="86081-129">Detta framgår hello [hur man: skickar ett e-] [så här: skicka ett e-postmeddelande] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="86081-129">This is shown in hello [How To: Send an Email][How to: Send an Email] section.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="86081-130">Så här: skapa ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="86081-130">How to: Create an email</span></span>
<span data-ttu-id="86081-131">hello nedan visar hur toospecify värden för ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="86081-131">hello following shows how toospecify values for an email.</span></span>

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

## <a name="how-to-send-an-email"></a><span data-ttu-id="86081-132">Så här: skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="86081-132">How to: Send an email</span></span>
<span data-ttu-id="86081-133">Hej följande visar hur toosend ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="86081-133">hello following shows how toosend an email.</span></span>

    Transport transport = mailSession.getTransport();
    // Connect hello transport object.
    transport.connect();
    // Send hello message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close hello connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="86081-134">Så här: Lägg till en bifogad fil</span><span class="sxs-lookup"><span data-stu-id="86081-134">How to: Add an attachment</span></span>
<span data-ttu-id="86081-135">hello följande kod visar hur tooadd bifogad fil.</span><span class="sxs-lookup"><span data-stu-id="86081-135">hello following code shows you how tooadd an attachment.</span></span>

    // Local file name and path.
    String attachmentName = "myfile.zip";
    String attachmentPath = "c:\\myfiles\\";
    MimeBodyPart attachmentPart = new MimeBodyPart();
    // Specify hello local file tooattach.
    DataSource source = new FileDataSource(attachmentPath + attachmentName);
    attachmentPart.setDataHandler(new DataHandler(source));
    // This example uses hello local file name as hello attachment name.
    // They could be different if you prefer.
    attachmentPart.setFileName(attachmentName);
    multipart.addBodyPart(attachmentPart);

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="86081-136">Så här: använda filter tooenable sidfötter, spårning och analytics</span><span class="sxs-lookup"><span data-stu-id="86081-136">How to: Use filters tooenable footers, tracking, and analytics</span></span>
<span data-ttu-id="86081-137">SendGrid ger ytterligare e-postfunktioner via hello *filter*.</span><span class="sxs-lookup"><span data-stu-id="86081-137">SendGrid provides additional email functionality through hello use of *filters*.</span></span> <span data-ttu-id="86081-138">Dessa är inställningar som kan läggas till tooan e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera klickar du på spårning, Google analytics, prenumerationen spårning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="86081-138">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="86081-139">En fullständig lista över filter finns [filterinställningar][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="86081-139">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

* <span data-ttu-id="86081-140">hello nedan visar hur tooinsert en sidfot filtrera som resulterar i HTML-text som visas längst ned hello hello e-post som skickas.</span><span class="sxs-lookup"><span data-stu-id="86081-140">hello following shows how tooinsert a footer filter that results in HTML text appearing at hello bottom of hello email being sent.</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* <span data-ttu-id="86081-141">Ett annat exempel på ett filter är klickar du på spårning.</span><span class="sxs-lookup"><span data-stu-id="86081-141">Another example of a filter is click tracking.</span></span> <span data-ttu-id="86081-142">Anta att e-postmeddelandet innehåller en hyperlänk klickar du på som hello följande och du vill tootrack hello hastighet:</span><span class="sxs-lookup"><span data-stu-id="86081-142">Let’s say that your email text contains a hyperlink, such as hello following, and you want tootrack hello click rate:</span></span>

      messagePart.setContent(
          "Hello,
          <p>This is hello body of hello message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* <span data-ttu-id="86081-143">tooenable hello på spårning, Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="86081-143">tooenable hello click tracking, use hello following code:</span></span>

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a><span data-ttu-id="86081-144">Så här: uppdatera e-egenskaper</span><span class="sxs-lookup"><span data-stu-id="86081-144">How to: Update email properties</span></span>
<span data-ttu-id="86081-145">Vissa egenskaper för e-post kan skrivas över med  **ange*egenskapen*** eller läggas till med hjälp av  **lägga till*egenskapen***.</span><span class="sxs-lookup"><span data-stu-id="86081-145">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span>

<span data-ttu-id="86081-146">Till exempel toospecify **ReplyTo** adresser, använder hello följande:</span><span class="sxs-lookup"><span data-stu-id="86081-146">For example, toospecify **ReplyTo** addresses, use hello following:</span></span>

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

<span data-ttu-id="86081-147">tooadd en **kopia** mottagaren, Använd hello följande:</span><span class="sxs-lookup"><span data-stu-id="86081-147">tooadd a **Cc** recipient, use hello following:</span></span>

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="86081-148">Så här: använda ytterligare SendGrid-tjänster</span><span class="sxs-lookup"><span data-stu-id="86081-148">How to: Use additional SendGrid services</span></span>
<span data-ttu-id="86081-149">SendGrid erbjuder webbaserade API: er som du kan använda tooleverage ytterligare SendGrid funktioner från din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="86081-149">SendGrid offers web-based APIs that you can use tooleverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="86081-150">Fullständig information finns i hello [SendGrid API-dokumentationen][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="86081-150">For full details, see hello [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="86081-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="86081-151">Next steps</span></span>
<span data-ttu-id="86081-152">Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="86081-152">Now that you’ve learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="86081-153">Exempel som visar med SendGrid i Azure-distribution: [hur toosend e-post med SendGrid från Java i Azure-distribution](store-sendgrid-java-how-to-send-email-example.md)</span><span class="sxs-lookup"><span data-stu-id="86081-153">Sample that demonstrates using SendGrid in an Azure deployment: [How toosend email using SendGrid from Java in an Azure deployment](store-sendgrid-java-how-to-send-email-example.md)</span></span>
* <span data-ttu-id="86081-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span><span class="sxs-lookup"><span data-stu-id="86081-154">SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html></span></span>
* <span data-ttu-id="86081-155">SendGrid API-dokumentationen: <https://sendgrid.com/docs/API_Reference/index.html></span><span class="sxs-lookup"><span data-stu-id="86081-155">SendGrid API documentation: <https://sendgrid.com/docs/API_Reference/index.html></span></span>
* <span data-ttu-id="86081-156">SendGrid specialerbjudande för Azure-kunder: <https://sendgrid.com/windowsazure.html></span><span class="sxs-lookup"><span data-stu-id="86081-156">SendGrid special offer for Azure customers: <https://sendgrid.com/windowsazure.html></span></span>

[http://sendgrid.com]: https://sendgrid.com
[http://sendgrid.com/pricing.html]: http://sendgrid.com/pricing.html
[http://www.sendgrid.com/azure.html]: https://www.sendgrid.com/windowsazure.html
[http://sendgrid.com/features]: https://sendgrid.com/features
[http://www.oracle.com/technetwork/java/javamail]: http://www.oracle.com/technetwork/java/javamail/index.html
[Filter Settings]: https://sendgrid.com/docs/API_Reference/Web_API/filter_settings.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/index.html
[http://sendgrid.com/azure.html]: https://sendgrid.com/windowsazure.html
[molnbaserade e-posttjänst]: https://sendgrid.com/email-solutions
[transaktionella e-postleverans]: https://sendgrid.com/transactional-email
