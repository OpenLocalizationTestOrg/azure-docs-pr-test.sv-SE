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
# <a name="how-toosend-email-using-sendgrid-with-azure"></a><span data-ttu-id="10146-104">Hur tooSend e-post med hjälp av SendGrid med Azure</span><span class="sxs-lookup"><span data-stu-id="10146-104">How tooSend Email Using SendGrid with Azure</span></span>
## <a name="overview"></a><span data-ttu-id="10146-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="10146-105">Overview</span></span>
<span data-ttu-id="10146-106">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med SendGrid e-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="10146-106">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="10146-107">hello prover är skrivna i C\# och stöder .NET Standard 1.3.</span><span class="sxs-lookup"><span data-stu-id="10146-107">hello samples are written in C\# and supports .NET Standard 1.3.</span></span> <span data-ttu-id="10146-108">hello Lär dig bland annat hur du skapar e-post, skicka e-post, lägga till bilagor, och aktivera olika e-post och spåra inställningar.</span><span class="sxs-lookup"><span data-stu-id="10146-108">hello scenarios covered include constructing email, sending email, adding attachments, and enabling various mail and tracking settings.</span></span> <span data-ttu-id="10146-109">Mer information om SendGrid och skicka e-post finns hello [nästa steg] [ Next steps] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="10146-109">For more information on SendGrid and sending email, see hello [Next steps][Next steps] section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="10146-110">Vad är hello SendGrid e-posttjänst?</span><span class="sxs-lookup"><span data-stu-id="10146-110">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="10146-111">SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="10146-111">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="10146-112">Vanliga användningsområden för SendGrid är:</span><span class="sxs-lookup"><span data-stu-id="10146-112">Common SendGrid use cases include:</span></span>

* <span data-ttu-id="10146-113">Skicka automatiskt inleveranser eller köp bekräftelser toocustomers.</span><span class="sxs-lookup"><span data-stu-id="10146-113">Automatically sending receipts or purchase confirmations toocustomers.</span></span>
* <span data-ttu-id="10146-114">Administrera distribution visas för att skicka kunder månatliga reklamblad och kampanjer.</span><span class="sxs-lookup"><span data-stu-id="10146-114">Administering distribution lists for sending customers monthly fliers and promotions.</span></span>
* <span data-ttu-id="10146-115">Samlar in realtid mätvärden för sådant som blockerade e-post och engagera kunder.</span><span class="sxs-lookup"><span data-stu-id="10146-115">Collecting real-time metrics for things like blocked email and customer engagement.</span></span>
* <span data-ttu-id="10146-116">Vidarebefordran av kundfrågor.</span><span class="sxs-lookup"><span data-stu-id="10146-116">Forwarding customer inquiries.</span></span>
* <span data-ttu-id="10146-117">Bearbetning av inkommande e-postmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="10146-117">Processing incoming emails.</span></span>

<span data-ttu-id="10146-118">Mer information finns [https://sendgrid.com](https://sendgrid.com) eller Sendgrid's [C#-biblioteket] [ sendgrid-csharp] GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="10146-118">For more information, visit [https://sendgrid.com](https://sendgrid.com) or SendGrid's [C# library][sendgrid-csharp] GitHub repo.</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="10146-119">Skapa ett SendGrid-konto</span><span class="sxs-lookup"><span data-stu-id="10146-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-net-class-library"></a><span data-ttu-id="10146-120">Referens hello SendGrid .NET-klassbibliotek</span><span class="sxs-lookup"><span data-stu-id="10146-120">Reference hello SendGrid .NET Class Library</span></span>
<span data-ttu-id="10146-121">Hej [SendGrid NuGet-paketet](https://www.nuget.org/packages/Sendgrid) är hello enklaste sättet tooget hello SendGrid API och tooconfigure ditt program med alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="10146-121">hello [SendGrid NuGet package](https://www.nuget.org/packages/Sendgrid) is hello easiest way tooget hello SendGrid API and tooconfigure your application with all dependencies.</span></span> <span data-ttu-id="10146-122">NuGet är en Visual Studio tillägg som ingår i Microsoft Visual Studio 2015 och ovan som gör det enkelt tooinstall och uppdatera bibliotek och verktyg.</span><span class="sxs-lookup"><span data-stu-id="10146-122">NuGet is a Visual Studio extension included with Microsoft Visual Studio 2015 and above that makes it easy tooinstall and update libraries and tools.</span></span>

> [!NOTE]
> <span data-ttu-id="10146-123">tooinstall NuGet om du kör en tidigare version av Visual Studio än Visual Studio 2015 besöker [http://www.nuget.org](http://www.nuget.org), och klicka på hello **installera NuGet** knappen.</span><span class="sxs-lookup"><span data-stu-id="10146-123">tooinstall NuGet if you are running a version of Visual Studio earlier than Visual Studio 2015, visit [http://www.nuget.org](http://www.nuget.org), and click hello **Install NuGet** button.</span></span>
>
>

<span data-ttu-id="10146-124">tooinstall hello SendGrid NuGet-paketet i ditt program hello följande:</span><span class="sxs-lookup"><span data-stu-id="10146-124">tooinstall hello SendGrid NuGet package in your application, do hello following:</span></span>

1. <span data-ttu-id="10146-125">Klicka på **nytt projekt** och välj en **mallen**.</span><span class="sxs-lookup"><span data-stu-id="10146-125">Click on **New Project** and select a **Template**.</span></span>

   ![Skapa ett nytt projekt][create-new-project]
2. <span data-ttu-id="10146-127">I **Solution Explorer**, högerklicka på **referenser**, klicka på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="10146-127">In **Solution Explorer**, right-click **References**, then click **Manage NuGet Packages**.</span></span>

   ![SendGrid NuGet-paketet][SendGrid-NuGet-package]
3. <span data-ttu-id="10146-129">Sök efter **SendGrid** och välj hello **SendGrid** objekt i resultatlistan.</span><span class="sxs-lookup"><span data-stu-id="10146-129">Search for **SendGrid** and select hello **SendGrid** item in the results list.</span></span>
4. <span data-ttu-id="10146-130">Välj hello senaste stabil version av hello Nuget-paketet hello version listrutan toobe kan toowork med hello objektmodellen och API: er som visas i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="10146-130">Select hello latest stable version of hello Nuget package from hello version dropdown toobe able toowork with hello object model and APIs demonstrated in this article.</span></span>

   ![SendGrid-paket][sendgrid-package]
5. <span data-ttu-id="10146-132">Klicka på **installera** toocomplete hello installationen och stäng sedan dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="10146-132">Click **Install** toocomplete hello installation, and then close this dialog.</span></span>

<span data-ttu-id="10146-133">Sendgrid's .NET-klassbibliotek kallas **SendGrid**.</span><span class="sxs-lookup"><span data-stu-id="10146-133">SendGrid's .NET class library is called **SendGrid**.</span></span> <span data-ttu-id="10146-134">Den innehåller hello följande namnområden:</span><span class="sxs-lookup"><span data-stu-id="10146-134">It contains hello following namespaces:</span></span>

* <span data-ttu-id="10146-135">**SendGrid** för att kommunicera med Sendgrid's API.</span><span class="sxs-lookup"><span data-stu-id="10146-135">**SendGrid** for communicating with SendGrid’s API.</span></span>
* <span data-ttu-id="10146-136">**SendGrid.Helpers.Mail** för helper metoder tooeasily skapa SendGridMessage-objekt som anger hur toosend e-post.</span><span class="sxs-lookup"><span data-stu-id="10146-136">**SendGrid.Helpers.Mail** for helper methods tooeasily create SendGridMessage objects that specify how toosend emails.</span></span>

<span data-ttu-id="10146-137">Lägg till följande kod namnområde deklarationer toohello upp av en C#-fil som du vill tooprogrammatically åtkomst hello SendGrid e-posttjänst hello.</span><span class="sxs-lookup"><span data-stu-id="10146-137">Add hello following code namespace declarations toohello top of any C# file in which you want tooprogrammatically access hello SendGrid email service.</span></span>

    using SendGrid;
    using SendGrid.Helpers.Mail;

## <a name="how-to-create-an-email"></a><span data-ttu-id="10146-138">Så här: skapa ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="10146-138">How to: Create an Email</span></span>
<span data-ttu-id="10146-139">Använd hello **SendGridMessage** objekt toocreate ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="10146-139">Use hello **SendGridMessage** object toocreate an email message.</span></span> <span data-ttu-id="10146-140">Du kan ange egenskaper och metoder, inklusive hello e-postavsändaren, hello e-postmottagare och hello ämne och en brödtext av hello e-post när hello meddelandeobjektet har skapats.</span><span class="sxs-lookup"><span data-stu-id="10146-140">Once hello message object is created, you can set properties and methods, including hello email sender, hello email recipient, and hello subject and body of hello email.</span></span>

<span data-ttu-id="10146-141">hello exemplet nedan visar hur toocreate ett fullständigt ifyllda e-objekt:</span><span class="sxs-lookup"><span data-stu-id="10146-141">hello following example demonstrates how toocreate a fully populated email object:</span></span>

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

<span data-ttu-id="10146-142">Mer information om alla egenskaper och metoder som stöds av den **SendGrid** skriver, se [sendgrid csharp] [ sendgrid-csharp] på GitHub.</span><span class="sxs-lookup"><span data-stu-id="10146-142">For more information on all properties and methods supported by the **SendGrid** type, see [sendgrid-csharp][sendgrid-csharp] on GitHub.</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="10146-143">Så här: skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="10146-143">How to: Send an Email</span></span>
<span data-ttu-id="10146-144">Du kan skicka den med hjälp av Sendgrid's API när du har skapat ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="10146-144">After creating an email message, you can send it using SendGrid's API.</span></span> <span data-ttu-id="10146-145">Du kan också använda [. NETS inbyggd i biblioteket][NET-library].</span><span class="sxs-lookup"><span data-stu-id="10146-145">Alternatively, you may use [.NET's built in library][NET-library].</span></span>

<span data-ttu-id="10146-146">Skicka e-post måste du ange din SendGrid API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="10146-146">Sending email requires that you supply your SendGrid API Key.</span></span> <span data-ttu-id="10146-147">Om du vill ha information om hur tooconfigure API-nycklar finns Sendgrid's API-nycklar [dokumentationen][documentation].</span><span class="sxs-lookup"><span data-stu-id="10146-147">If you need details about how tooconfigure API Keys, please visit SendGrid's API Keys [documentation][documentation].</span></span>

<span data-ttu-id="10146-148">Du kan lagra dessa autentiseringsuppgifter via Azure-portalen genom att klicka på Programinställningar och lägga till hello nyckel/värde-par under App-inställningar.</span><span class="sxs-lookup"><span data-stu-id="10146-148">You may store these credentials via your Azure Portal by clicking Application settings and adding hello key/value pairs under App settings.</span></span>

 ![Azure app-inställningar][azure_app_settings]

 <span data-ttu-id="10146-150">Sedan kan du kan komma åt dem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="10146-150">Then, you may access them as follows:</span></span>

    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");
    var client = new SendGridClient(apiKey);

<span data-ttu-id="10146-151">hello som följande exempel visar hur ett meddelande med toosend hello webb-API.</span><span class="sxs-lookup"><span data-stu-id="10146-151">hello following examples show how toosend a message using hello Web API.</span></span>

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

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="10146-152">Så här: Lägg till en bifogad fil</span><span class="sxs-lookup"><span data-stu-id="10146-152">How to: Add an attachment</span></span>
<span data-ttu-id="10146-153">Bifogade filer kan du lägga till tooa meddelande anropa hello **AddAttachment** metod och ange minimalt hello filnamnet och Base64-kodade innehåll du vill tooattach.</span><span class="sxs-lookup"><span data-stu-id="10146-153">Attachments can be added tooa message by calling hello **AddAttachment** method and minimally specifying hello file name and Base64 encoded content you want tooattach.</span></span> <span data-ttu-id="10146-154">Du kan innehålla flera bifogade filer genom att anropa den här metoden när för varje fil som du vill tooattach eller genom att använda hello **AddAttachments** metod.</span><span class="sxs-lookup"><span data-stu-id="10146-154">You can include multiple attachments by calling this method once for each file you wish tooattach or by using hello **AddAttachments** method.</span></span> <span data-ttu-id="10146-155">hello som följande exempel visar att lägga till en bifogad fil tooa meddelande:</span><span class="sxs-lookup"><span data-stu-id="10146-155">hello following example demonstrates adding an attachment tooa message:</span></span>

    var banner2 = new Attachment()
    {
        Content = Convert.ToBase64String(raw_content),
        Type = "image/png",
        Filename = "banner2.png",
        Disposition = "inline",
        ContentId = "Banner 2"
    };
    msg.AddAttachment(banner2);

## <a name="how-to-use-mail-settings-tooenable-footers-tracking-and-analytics"></a><span data-ttu-id="10146-156">Så här: använda e-post inställningar tooenable sidfötter, spårning och analys</span><span class="sxs-lookup"><span data-stu-id="10146-156">How to: Use mail settings tooenable footers, tracking, and analytics</span></span>
<span data-ttu-id="10146-157">SendGrid ger ytterligare e-postfunktioner via hello postinställningar för e-och spårning.</span><span class="sxs-lookup"><span data-stu-id="10146-157">SendGrid provides additional email functionality through hello use of mail settings and tracking settings.</span></span> <span data-ttu-id="10146-158">De här inställningarna kan läggas till tooan e-postmeddelande tooenable specifika funktioner, till exempel klicka på spårning, Google analytics, prenumeration spårning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="10146-158">These settings can be added tooan email message tooenable specific functionality such as click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="10146-159">En fullständig lista över appar finns hello [inställningar dokumentationen][settings-documentation].</span><span class="sxs-lookup"><span data-stu-id="10146-159">For a full list of apps, see hello [Settings Documentation][settings-documentation].</span></span>

<span data-ttu-id="10146-160">Appar kan användas för**SendGrid** e-postmeddelanden med metoder som implementeras som en del av hello **SendGridMessage** klass.</span><span class="sxs-lookup"><span data-stu-id="10146-160">Apps can be applied too**SendGrid** email messages using methods implemented as part of hello **SendGridMessage** class.</span></span> <span data-ttu-id="10146-161">hello följande exempel visar hello sidfoten och på Spåra filter:</span><span class="sxs-lookup"><span data-stu-id="10146-161">hello following examples demonstrate hello footer and click tracking filters:</span></span>

<span data-ttu-id="10146-162">hello följande exempel visar hello sidfoten och på Spåra filter:</span><span class="sxs-lookup"><span data-stu-id="10146-162">hello following examples demonstrate hello footer and click tracking filters:</span></span>

### <a name="footer-settings"></a><span data-ttu-id="10146-163">Inställningar för sidfot</span><span class="sxs-lookup"><span data-stu-id="10146-163">Footer settings</span></span>
    msg.SetFooterSetting(
                         true,
                         "Some Footer HTML",
                         "<strong>Some Footer Text</strong>");

### <a name="click-tracking"></a><span data-ttu-id="10146-164">Klicka på spårning</span><span class="sxs-lookup"><span data-stu-id="10146-164">Click tracking</span></span>
    msg.SetClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="10146-165">Så här: använda ytterligare SendGrid tjänster</span><span class="sxs-lookup"><span data-stu-id="10146-165">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="10146-166">SendGrid innehåller flera API: er och webhooks som du kan använda tooleverage ytterligare funktioner i Azure-program.</span><span class="sxs-lookup"><span data-stu-id="10146-166">SendGrid offers several APIs and webhooks that you can use tooleverage additional functionality within your Azure application.</span></span> <span data-ttu-id="10146-167">Mer information finns i hello [SendGrid API-referens][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="10146-167">For more details, see hello [SendGrid API Reference][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="10146-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10146-168">Next steps</span></span>
<span data-ttu-id="10146-169">Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="10146-169">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="10146-170">SendGrid C\# biblioteket lagringsplatsen: [sendgrid csharp][sendgrid-csharp]</span><span class="sxs-lookup"><span data-stu-id="10146-170">SendGrid C\# library repo: [sendgrid-csharp][sendgrid-csharp]</span></span>
* <span data-ttu-id="10146-171">SendGrid API-dokumentationen: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="10146-171">SendGrid API documentation: <https://sendgrid.com/docs></span></span>

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

