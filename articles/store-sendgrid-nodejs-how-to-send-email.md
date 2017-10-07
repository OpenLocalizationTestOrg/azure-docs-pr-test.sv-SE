---
title: "aaaHow toouse hello SendGrid e-posttjänst (Node.js) | Microsoft Docs"
description: "Lär dig hur skicka e-post med hello SendGrid e-posttjänst på Azure. Kodexempel som skrivits med hello Node.js API."
services: 
documentationcenter: nodejs
author: erikre
manager: wpickett
editor: 
ms.assetid: cac444b4-26b0-45ea-9c3d-eca28d57dacb
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 01/05/2016
ms.author: erikre
ms.openlocfilehash: fd617b6aaa656e7b5dd51c51ebb0db1e848450f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosend-email-using-sendgrid-from-nodejs"></a><span data-ttu-id="bbc78-104">Hur tooSend e-post med hjälp av SendGrid från Node.js</span><span class="sxs-lookup"><span data-stu-id="bbc78-104">How tooSend Email Using SendGrid from Node.js</span></span>
<span data-ttu-id="bbc78-105">Den här guiden visar hur tooperform vanliga programmeringsuppgifter med SendGrid e-tjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="bbc78-105">This guide demonstrates how tooperform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="bbc78-106">hello exempel skrivs med hello Node.js API.</span><span class="sxs-lookup"><span data-stu-id="bbc78-106">hello samples are written using hello Node.js API.</span></span> <span data-ttu-id="bbc78-107">hello beskrivs scenarier där **konstruera e-post**, **skicka e-post**, **lägga till bilagor**, **med hjälp av filter**, och **uppdatera egenskaperna för**.</span><span class="sxs-lookup"><span data-stu-id="bbc78-107">hello scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="bbc78-108">Mer information om SendGrid och skicka e-post finns hello [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bbc78-108">For more information on SendGrid and sending email, see hello [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-hello-sendgrid-email-service"></a><span data-ttu-id="bbc78-109">Vad är hello SendGrid e-posttjänst?</span><span class="sxs-lookup"><span data-stu-id="bbc78-109">What is hello SendGrid Email Service?</span></span>
<span data-ttu-id="bbc78-110">SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="bbc78-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="bbc78-111">Vanliga Användningsscenarier för SendGrid är:</span><span class="sxs-lookup"><span data-stu-id="bbc78-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="bbc78-112">Automatiskt skickar kvitton toocustomers</span><span class="sxs-lookup"><span data-stu-id="bbc78-112">Automatically sending receipts toocustomers</span></span>
* <span data-ttu-id="bbc78-113">Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="bbc78-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="bbc78-114">Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider</span><span class="sxs-lookup"><span data-stu-id="bbc78-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="bbc78-115">Generera rapporter toohelp identifiera trender</span><span class="sxs-lookup"><span data-stu-id="bbc78-115">Generating reports toohelp identify trends</span></span>
* <span data-ttu-id="bbc78-116">Vidarebefordran av kundfrågor</span><span class="sxs-lookup"><span data-stu-id="bbc78-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="bbc78-117">E-postaviseringar från ditt program</span><span class="sxs-lookup"><span data-stu-id="bbc78-117">Email notifications from your application</span></span>

<span data-ttu-id="bbc78-118">Mer information finns i [https://sendgrid.com](https://sendgrid.com).</span><span class="sxs-lookup"><span data-stu-id="bbc78-118">For more information, see [https://sendgrid.com](https://sendgrid.com).</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="bbc78-119">Skapa ett SendGrid-konto</span><span class="sxs-lookup"><span data-stu-id="bbc78-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-hello-sendgrid-nodejs-module"></a><span data-ttu-id="bbc78-120">Referens hello SendGrid Node.js-modul</span><span class="sxs-lookup"><span data-stu-id="bbc78-120">Reference hello SendGrid Node.js Module</span></span>
<span data-ttu-id="bbc78-121">Hej SendGrid-modul för Node.js kan installeras via hello nod Pakethanteraren (npm) med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bbc78-121">hello SendGrid module for Node.js can be installed through hello node package manager (npm) by using hello following command:</span></span>

    npm install sendgrid

<span data-ttu-id="bbc78-122">Efter installationen kan kräva du hello modulen i ditt program med hjälp av hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="bbc78-122">After installation, you can require hello module in your application by using hello following code:</span></span>

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

<span data-ttu-id="bbc78-123">Hej SendGrid modulen exporterar hello **SendGrid** och **e-post** funktioner.</span><span class="sxs-lookup"><span data-stu-id="bbc78-123">hello SendGrid module exports hello **SendGrid** and **Email** functions.</span></span>
<span data-ttu-id="bbc78-124">**SendGrid** ansvarar för att skicka e-post via Web API medan **e-post** kapslar in ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="bbc78-124">**SendGrid** is responsible for sending email through Web API, while **Email** encapsulates an email message.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="bbc78-125">Så här: skapa ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="bbc78-125">How to: Create an Email</span></span>
<span data-ttu-id="bbc78-126">Skapa ett e-postmeddelande med hello SendGrid modulen innebär att först skapa ett e-postmeddelande hello e-funktionen, och skickar den med hjälp av hello SendGrid-funktionen.</span><span class="sxs-lookup"><span data-stu-id="bbc78-126">Creating an email message using hello SendGrid module involves first creating an email message using hello Email function, and then sending it using hello SendGrid function.</span></span> <span data-ttu-id="bbc78-127">hello följande är ett exempel på hur du skapar ett nytt meddelande hello e-funktionen:</span><span class="sxs-lookup"><span data-stu-id="bbc78-127">hello following is an example of creating a new message using hello Email function:</span></span>

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

<span data-ttu-id="bbc78-128">Du kan också ange ett HTML-meddelande för klienter som stöder genom att ange hello HTML-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bbc78-128">You can also specify an HTML message for clients that support it by setting hello html property.</span></span> <span data-ttu-id="bbc78-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bbc78-129">For example:</span></span>

    html: This is a sample <b>HTML<b> email message.

<span data-ttu-id="bbc78-130">Ange egenskaper för båda hello text och html ger korrekt användning av textinnehåll för klienter som inte stöder HTML-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bbc78-130">Setting both hello text and html properties provides graceful fallback to text content for clients that cannot support HTML messages.</span></span>

<span data-ttu-id="bbc78-131">Mer information om alla egenskaper som stöds av hello e-funktionen finns [sendgrid nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="bbc78-131">For more information on all properties supported by hello Email function, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="bbc78-132">Så här: skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="bbc78-132">How to: Send an Email</span></span>
<span data-ttu-id="bbc78-133">När du har skapat ett e-postmeddelande med hello e-funktionen kan du skicka den via hello webb-API som tillhandahålls av SendGrid.</span><span class="sxs-lookup"><span data-stu-id="bbc78-133">After creating an email message using hello Email function, you can send it using hello Web API provided by SendGrid.</span></span> 

### <a name="web-api"></a><span data-ttu-id="bbc78-134">Webb-API</span><span class="sxs-lookup"><span data-stu-id="bbc78-134">Web API</span></span>
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> <span data-ttu-id="bbc78-135">Medan hello exemplen ovan visas skicka i ett e-objektet och motringning funktionen måste anropa du också direkt hello skicka funktionen genom att ange egenskaper för e-post direkt.</span><span class="sxs-lookup"><span data-stu-id="bbc78-135">While hello above examples show passing in an email object and callback function, you can also directly invoke hello send function by directly specifying email properties.</span></span> <span data-ttu-id="bbc78-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bbc78-136">For example:</span></span>  
> 
> `````
> sendgrid.send({
> to: 'john@contoso.com',
> from: 'anna@contoso.com',
> subject: 'test mail',
> text: 'This is a sample email message.'
> });
> `````
> 
> 

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="bbc78-137">Så här: Lägg till en bifogad fil</span><span class="sxs-lookup"><span data-stu-id="bbc78-137">How to: Add an Attachment</span></span>
<span data-ttu-id="bbc78-138">Bifogade filer kan läggas till tooa meddelandet genom att ange hello filnamn och sökväg i hello **filer** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bbc78-138">Attachments can be added tooa message by specifying hello file name(s) and path(s) in hello **files** property.</span></span> <span data-ttu-id="bbc78-139">hello som följande exempel visar skicka bifogade filer:</span><span class="sxs-lookup"><span data-stu-id="bbc78-139">hello following example demonstrates sending an attachment:</span></span>

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used toospecify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> <span data-ttu-id="bbc78-140">När du använder hello **filer** egenskapen hello-filen måste vara tillgängligt via [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span><span class="sxs-lookup"><span data-stu-id="bbc78-140">When using hello **files** property, hello file must be accessible through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span></span> <span data-ttu-id="bbc78-141">Om hello-fil som du vill tooattach finns i Azure Storage, exempelvis en Blob-behållare, måste du först kopiera hello fillagring toolocal eller tooan Azure enheten innan den kan skickas som en bifogad fil med hello **filer** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bbc78-141">If hello file you wish tooattach is hosted in Azure Storage, such as in a Blob container, you must first copy hello file toolocal storage or tooan Azure drive before it can be sent as an attachment using hello **files** property.</span></span>
> 
> 

## <a name="how-to-use-filters-tooenable-footers-and-tracking"></a><span data-ttu-id="bbc78-142">Så här: filter tooEnable sidfötter och spårning</span><span class="sxs-lookup"><span data-stu-id="bbc78-142">How to: Use Filters tooEnable Footers and Tracking</span></span>
<span data-ttu-id="bbc78-143">SendGrid ger ytterligare e-postfunktioner via hello filter.</span><span class="sxs-lookup"><span data-stu-id="bbc78-143">SendGrid provides additional email functionality through hello use of filters.</span></span> <span data-ttu-id="bbc78-144">Dessa är inställningar som kan läggas till tooan e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera klickar du på spårning, Google analytics, prenumerationen spårning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="bbc78-144">These are settings that can be added tooan email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="bbc78-145">En fullständig lista över filter finns [filterinställningar][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="bbc78-145">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

<span data-ttu-id="bbc78-146">Filter kan vara tillämpade tooa meddelande med hjälp av hello **filter** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bbc78-146">Filters can be applied tooa message by using hello **filters** property.</span></span>
<span data-ttu-id="bbc78-147">Varje filter som anges av ett hash-värde som innehåller filter-specifika inställningar.</span><span class="sxs-lookup"><span data-stu-id="bbc78-147">Each filter is specified by a hash containing filter-specific settings.</span></span>
<span data-ttu-id="bbc78-148">hello följande exempel visar hello sidfoten och på Spåra filter:</span><span class="sxs-lookup"><span data-stu-id="bbc78-148">hello following examples demonstrate hello footer and click tracking filters:</span></span>

### <a name="footer"></a><span data-ttu-id="bbc78-149">Sidfot</span><span class="sxs-lookup"><span data-stu-id="bbc78-149">Footer</span></span>
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'footer': {
            'settings': {
                'enable': 1,
                'text/plain': 'This is a text footer.'
            }
        }
    });

    sendgrid.send(email);

### <a name="click-tracking"></a><span data-ttu-id="bbc78-150">Klicka på spårning</span><span class="sxs-lookup"><span data-stu-id="bbc78-150">Click Tracking</span></span>
    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

    email.setFilters({
        'clicktrack': {
            'settings': {
                'enable': 1
            }
        }
    });

    sendgrid.send(email);

## <a name="how-to-update-email-properties"></a><span data-ttu-id="bbc78-151">Så här: uppdatera egenskaper för e-post</span><span class="sxs-lookup"><span data-stu-id="bbc78-151">How to: Update Email Properties</span></span>
<span data-ttu-id="bbc78-152">Vissa egenskaper för e-post kan skrivas över med  **ange*egenskapen*** eller läggas till med hjälp av  **lägga till*egenskapen***.</span><span class="sxs-lookup"><span data-stu-id="bbc78-152">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span> <span data-ttu-id="bbc78-153">Du kan till exempel lägga till ytterligare mottagare med hjälp av</span><span class="sxs-lookup"><span data-stu-id="bbc78-153">For example, you can add additional recipients by using</span></span>

    email.addTo('jeff@contoso.com');

<span data-ttu-id="bbc78-154">eller ange ett filter med hjälp av</span><span class="sxs-lookup"><span data-stu-id="bbc78-154">or set a filter by using</span></span>

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

<span data-ttu-id="bbc78-155">Mer information finns i [sendgrid nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="bbc78-155">For more information, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="bbc78-156">Så här: använda ytterligare SendGrid tjänster</span><span class="sxs-lookup"><span data-stu-id="bbc78-156">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="bbc78-157">SendGrid erbjuder webbaserade API: er som du kan använda tooleverage ytterligare SendGrid funktioner från din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="bbc78-157">SendGrid offers web-based APIs that you can use tooleverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="bbc78-158">Fullständig information finns i hello [SendGrid API-dokumentationen][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="bbc78-158">For full details, see hello [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbc78-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bbc78-159">Next Steps</span></span>
<span data-ttu-id="bbc78-160">Nu när du har lärt dig hello grunderna i hello SendGrid e-posttjänst, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="bbc78-160">Now that you've learned hello basics of hello SendGrid Email service, follow these links toolearn more.</span></span>

* <span data-ttu-id="bbc78-161">SendGrid Node.js modulen databasen: [sendgrid nodejs][sendgrid-nodejs]</span><span class="sxs-lookup"><span data-stu-id="bbc78-161">SendGrid Node.js module repository: [sendgrid-nodejs][sendgrid-nodejs]</span></span>
* <span data-ttu-id="bbc78-162">SendGrid API-dokumentationen: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="bbc78-162">SendGrid API documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="bbc78-163">SendGrid specialerbjudande för Azure-kunder: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span><span class="sxs-lookup"><span data-stu-id="bbc78-163">SendGrid special offer for Azure customers: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span></span>

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
[molnbaserade e-posttjänst]: https://sendgrid.com/email-solutions
[transaktionella e-postleverans]: https://sendgrid.com/transactional-email
