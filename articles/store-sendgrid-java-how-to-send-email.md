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
# <a name="how-toosend-email-using-sendgrid-from-java"></a>Hur tooSend e-post med hjälp av SendGrid från Java
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med SendGrid e-tjänsten på Azure. hello exempel skriven i Java. hello beskrivs scenarier där **konstruera e-post**, **skicka e-post**, **lägga till bilagor**, **med hjälp av filter**, och **uppdatera egenskaperna för**. Mer information om SendGrid och skicka e-post finns hello [nästa steg](#next-steps) avsnitt.

## <a name="what-is-hello-sendgrid-email-service"></a>Vad är hello SendGrid e-posttjänst?
SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering. Vanliga Användningsscenarier för SendGrid är:

* Automatiskt skickar kvitton toocustomers
* Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden
* Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider
* Generera rapporter toohelp identifiera trender
* Vidarebefordran av kundfrågor
* E-postaviseringar från ditt program

Mer information finns i <http://sendgrid.com>.

## <a name="create-a-sendgrid-account"></a>Skapa ett SendGrid-konto
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="how-to-use-hello-javaxmail-libraries"></a>Så här: använda hello javax.mail bibliotek
Hämta hello javax.mail bibliotek, till exempel från <http://www.oracle.com/technetwork/java/javamail> och importera dem till din kod. Vid en hög nivå är hello processen för att använda hello javax.mail biblioteket toosend e-post via SMTP toodo hello följande:

1. Ange hello SMTP-värden, inklusive hello SMTP-servern som för SendGrid smtp.sendgrid.net.

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

1. Utöka hello *javax.mail.Authenticator* klass, och i din implementering av den *getPasswordAuthentication* -metoden returnerar din SendGrid användarnamn och lösenord.  

       private class SMTPAuthenticator extends javax.mail.Authenticator {
       public PasswordAuthentication getPasswordAuthentication() {
          String username = SMTP_AUTH_USER;
          String password = SMTP_AUTH_PWD;
          return new PasswordAuthentication(username, password);
       }
2. Skapa en session autentiserade e-post via en *javax.mail.Session* objekt.  

       Authenticator auth = new SMTPAuthenticator();
       Session mailSession = Session.getDefaultInstance(properties, auth);
3. Skapa ditt meddelande och tilldela **till**, **från**, **ämne** och innehåll värden. Detta framgår hello [hur man: skapa ett e-postmeddelande](#how-to-create-an-email) avsnitt.
4. Skicka hello-meddelande via en *javax.mail.Transport* objekt. Detta framgår hello [hur man: skickar ett e-] [så här: skicka ett e-postmeddelande] avsnitt.

## <a name="how-to-create-an-email"></a>Så här: skapa ett e-postmeddelande
hello nedan visar hur toospecify värden för ett e-postmeddelande.

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

## <a name="how-to-send-an-email"></a>Så här: skicka ett e-postmeddelande
Hej följande visar hur toosend ett e-postmeddelande.

    Transport transport = mailSession.getTransport();
    // Connect hello transport object.
    transport.connect();
    // Send hello message.
    transport.sendMessage(message, message.getAllRecipients());
    // Close hello connection.
    transport.close();

## <a name="how-to-add-an-attachment"></a>Så här: Lägg till en bifogad fil
hello följande kod visar hur tooadd bifogad fil.

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

## <a name="how-to-use-filters-tooenable-footers-tracking-and-analytics"></a>Så här: använda filter tooenable sidfötter, spårning och analytics
SendGrid ger ytterligare e-postfunktioner via hello *filter*. Dessa är inställningar som kan läggas till tooan e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera klickar du på spårning, Google analytics, prenumerationen spårning och så vidare. En fullständig lista över filter finns [filterinställningar][Filter Settings].

* hello nedan visar hur tooinsert en sidfot filtrera som resulterar i HTML-text som visas längst ned hello hello e-post som skickas.

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"footer\":
          {\"settings\":
          {\"enable\":1,\"text/html\":
          \"<html><b>Thank you</b> for your business.</html>\"}}}}");
* Ett annat exempel på ett filter är klickar du på spårning. Anta att e-postmeddelandet innehåller en hyperlänk klickar du på som hello följande och du vill tootrack hello hastighet:

      messagePart.setContent(
          "Hello,
          <p>This is hello body of hello message. Visit
          <a href='http://www.contoso.com'>http://www.contoso.com</a>.</p>
          Thank you.",
          "text/html");
* tooenable hello på spårning, Använd hello följande kod:

      message.addHeader("X-SMTPAPI",
          "{\"filters\":
          {\"clicktrack\":
          {\"settings\":
          {\"enable\":1}}}}");

## <a name="how-to-update-email-properties"></a>Så här: uppdatera e-egenskaper
Vissa egenskaper för e-post kan skrivas över med  **ange*egenskapen*** eller läggas till med hjälp av  **lägga till*egenskapen***.

Till exempel toospecify **ReplyTo** adresser, använder hello följande:

    InternetAddress addresses[] =
        { new InternetAddress("john@contoso.com"),
          new InternetAddress("wendy@contoso.com") };

    message.setReplyTo(addresses);

tooadd en **kopia** mottagaren, Använd hello följande:

    message.addRecipient(Message.RecipientType.CC, new
    InternetAddress("john@contoso.com"));

## <a name="how-to-use-additional-sendgrid-services"></a>Så här: använda ytterligare SendGrid-tjänster
SendGrid erbjuder webbaserade API: er som du kan använda tooleverage ytterligare SendGrid funktioner från din Azure-program. Fullständig information finns i hello [SendGrid API-dokumentationen][SendGrid API documentation].

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.

* Exempel som visar med SendGrid i Azure-distribution: [hur toosend e-post med SendGrid från Java i Azure-distribution](store-sendgrid-java-how-to-send-email-example.md)
* SendGrid Java SDK: <https://sendgrid.com/docs/Code_Examples/java.html>
* SendGrid API-dokumentationen: <https://sendgrid.com/docs/API_Reference/index.html>
* SendGrid specialerbjudande för Azure-kunder: <https://sendgrid.com/windowsazure.html>

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
