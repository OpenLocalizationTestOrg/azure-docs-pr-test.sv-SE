---
title: "aaaHow toouse hello SendGrid e-posttjänst (.NET) | Microsoft Docs"
description: "Lär dig hur skicka e-post med hello SendGrid e-posttjänst på Azure. Kodexempel som skrivits i C# och använder hello .NET-API."
services: app-service-web
documentationcenter: .net
author: thinkingserious
manager: erikre
editor: 
ms.assetid: 21bf4028-9046-476b-9799-3d3082a0f84c
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2017
ms.author: dx@sendgrid.com
ms.openlocfilehash: b3d77bb67898b991c7293e6b9086b263f6bcb755
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-with-azure"></a>Hur tooSend e-post med hjälp av SendGrid med Azure
## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga programmeringsuppgifter med SendGrid e-tjänsten på Azure. hello prover är skrivna i C\# och stöder .NET Standard 1.3. hello Lär dig bland annat hur du skapar e-post, skicka e-post, lägga till bilagor, och aktivera olika e-post och spåra inställningar. Mer information om SendGrid och skicka e-post finns hello [nästa steg] [ Next steps] avsnitt.

## <a name="what-is-hello-sendgrid-email-service"></a>Vad är hello SendGrid e-posttjänst?
SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering. Vanliga användningsområden för SendGrid är:

* Skicka automatiskt inleveranser eller köp bekräftelser toocustomers.
* Administrera distribution visas för att skicka kunder månatliga reklamblad och kampanjer.
* Samlar in realtid mätvärden för sådant som blockerade e-post och engagera kunder.
* Vidarebefordran av kundfrågor.
* Bearbetning av inkommande e-postmeddelanden.

Mer information finns [https://sendgrid.com](https://sendgrid.com) eller Sendgrid's [C#-biblioteket] [ sendgrid-csharp] GitHub-lagringsplatsen.

## <a name="create-a-sendgrid-account"></a>Skapa ett SendGrid-konto
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-net-class-library"></a>Referens hello SendGrid .NET-klassbibliotek
Hej [SendGrid NuGet-paketet](https://www.nuget.org/packages/Sendgrid) är hello enklaste sättet tooget hello SendGrid API och tooconfigure ditt program med alla beroenden. NuGet är en Visual Studio tillägg som ingår i Microsoft Visual Studio 2015 och ovan som gör det enkelt tooinstall och uppdatera bibliotek och verktyg.

> [!NOTE]
> tooinstall NuGet om du kör en tidigare version av Visual Studio än Visual Studio 2015 besöker [http://www.nuget.org](http://www.nuget.org), och klicka på hello **installera NuGet** knappen.
>
>

tooinstall hello SendGrid NuGet-paketet i ditt program hello följande:

1. Klicka på **nytt projekt** och välj en **mallen**.

   ![Skapa ett nytt projekt][create-new-project]
2. I **Solution Explorer**, högerklicka på **referenser**, klicka på **hantera NuGet-paket**.

   ![SendGrid NuGet-paketet][SendGrid-NuGet-package]
3. Sök efter **SendGrid** och välj hello **SendGrid** objekt i resultatlistan.
4. Välj hello senaste stabil version av hello Nuget-paketet hello version listrutan toobe kan toowork med hello objektmodellen och API: er som visas i den här artikeln.

   ![SendGrid-paket][sendgrid-package]
5. Klicka på **installera** toocomplete hello installationen och stäng sedan dialogrutan.

Sendgrid's .NET-klassbibliotek kallas **SendGrid**. Den innehåller hello följande namnområden:

* **SendGrid** för att kommunicera med Sendgrid's API.
* **SendGrid.Helpers.Mail** för helper metoder tooeasily skapa SendGridMessage-objekt som anger hur toosend e-post.

Lägg till följande kod namnområde deklarationer toohello upp av en C#-fil som du vill tooprogrammatically åtkomst hello SendGrid e-posttjänst hello.

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a>Så här: skapa ett e-postmeddelande
Använd hello **SendGridMessage** objekt toocreate ett e-postmeddelande. Du kan ange egenskaper och metoder, inklusive hello e-postavsändaren, hello e-postmottagare och hello ämne och en brödtext av hello e-post när hello meddelandeobjektet har skapats.

hello exemplet nedan visar hur toocreate ett fullständigt ifyllda e-objekt:

    var msg = new SendGridMessage();

    msg.SetFrom(new EmailAddress("dx@example.com", "SendGrid DX Team"));

    var recipients = new List<EmailAddress>
    {
        new EmailAddress("jeff@example.com", "Jeff Smith"),
        new EmailAddress("anna@example.com", "Anna Lidman"),
        new EmailAddress("peter@example.com", "Peter Saddow")
    };
    msg.AddTos(recipients);

    msg.SetSubject("Testing hello SendGrid C# Library");

    msg.AddContent(MimeType.Text, "Hello World plain text!");
    msg.AddContent(MimeType.Html, "<p>Hello World!</p>");

Mer information om alla egenskaper och metoder som stöds av den **SendGrid** skriver, se [sendgrid csharp] [ sendgrid-csharp] på GitHub.

## <a name="how-to-send-an-email"></a>Så här: skicka ett e-postmeddelande
Du kan skicka den med hjälp av Sendgrid's API när du har skapat ett e-postmeddelande. Du kan också använda [. NETS inbyggd i biblioteket][NET-library].

Skicka e-post måste du ange din SendGrid API-nyckel. Om du vill ha information om hur tooconfigure API-nycklar finns Sendgrid's API-nycklar [dokumentationen][documentation].

Du kan lagra dessa autentiseringsuppgifter via Azure-portalen genom att klicka på Programinställningar och lägga till hello nyckel/värde-par under App-inställningar.

 ![Azure app-inställningar][azure_app_settings]

 Sedan kan du kan komma åt dem på följande sätt:

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

hello som följande exempel visar hur ett meddelande med toosend hello webb-API.

    using System;
    using System.Threading.Tasks;
    using SendGrid;
    using SendGrid.Helpers.Mail;

    namespace Example
    {
        internal class Example
        {
            private static void Main()
            {
                Execute().Wait();
            }

            static async Task Execute()
            {
                var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
                var client = new SendGridClient(apiKey);
                var msg = new SendGridMessage()
                {
                    From = new EmailAddress("test@example.com", "DX Team"),
                    Subject = "Hello World from hello SendGrid CSharp SDK!",
                    PlainTextContent = "Hello, Email!",
                    HtmlContent = "<strong>Hello, Email!</strong>"
                };
                msg.AddTo(new EmailAddress("test@example.com", "Test User"));
                var response = await client.SendEmailAsync(msg);
            }
        }
    }

## <a name="how-to-add-an-attachment"></a>Så här: Lägg till en bifogad fil
Bifogade filer kan du lägga till tooa meddelande anropa hello **AddAttachment** metod och ange minimalt hello filnamnet och Base64-kodade innehåll du vill tooattach. Du kan innehålla flera bifogade filer genom att anropa den här metoden när för varje fil som du vill tooattach eller genom att använda hello **AddAttachments** metod. hello som följande exempel visar att lägga till en bifogad fil tooa meddelande:

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-tooenable-footers-tracking-and-analytics"></a>Så här: använda e-post inställningar tooenable sidfötter, spårning och analys
SendGrid ger ytterligare e-postfunktioner via hello postinställningar för e-och spårning. De här inställningarna kan läggas till tooan e-postmeddelande tooenable specifika funktioner, till exempel klicka på spårning, Google analytics, prenumeration spårning och så vidare. En fullständig lista över appar finns hello [inställningar dokumentationen][settings-documentation].

Appar kan användas för**SendGrid** e-postmeddelanden med metoder som implementeras som en del av hello **SendGridMessage** klass. hello följande exempel visar hello sidfoten och på Spåra filter:

hello följande exempel visar hello sidfoten och på Spåra filter:

### <a name="footer-settings"></a>Inställningar för sidfot
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a>Klicka på spårning
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Så här: använda ytterligare SendGrid tjänster
SendGrid innehåller flera API: er och webhooks som du kan använda tooleverage ytterligare funktioner i Azure-program. Mer information finns i hello [SendGrid API-referens][SendGrid API documentation].

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.

* SendGrid C\# biblioteket lagringsplatsen: [sendgrid csharp][sendgrid-csharp]
* SendGrid API-dokumentationen: <https://sendgrid.com/docs>

[Next steps]: #next-steps
[What is hello SendGrid Email Service?]: #whatis
[Create a SendGrid Account]: #createaccount
[Reference hello SendGrid .NET Class Library]: #reference
[How to: Create an Email]: #createemail
[How to: Send an Email]: #sendemail
[How to: Add an Attachment]: #addattachment
[How to: Use Filters tooEnable Footers, Tracking, and Analytics]: #usefilters
[How to: Use Additional SendGrid Services]: #useservices

[create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/new-project.png
[SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/reference.png
[sendgrid-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid-package.png
[azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/azure-app-settings.png
[sendgrid-csharp]: https://github.com/sendgrid/sendgrid-csharp
[SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
[App Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs/API_Reference/api_v3.html
[NET-library]: https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html#-Using-NETs-Builtin-SMTP-Library
[documentation]: https://sendgrid.com/docs/Classroom/Send/api_keys.html
[settings-documentation]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html

[molnbaserade e-posttjänst]: https://sendgrid.com/solutions
[transaktionella e-postleverans]: https://sendgrid.com/use-cases/transactional-email

