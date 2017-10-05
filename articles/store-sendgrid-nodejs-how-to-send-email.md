---
title: "Hur du använder SendGrid e-posttjänst (Node.js) | Microsoft Docs"
description: "Lär dig hur skicka e-post med SendGrid-e-posttjänsten på Azure. Kodexempel som skrivits med Node.js-API."
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
ms.openlocfilehash: 327cea3a24cc47a9cc463b37cc2346ebc475ef7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-send-email-using-sendgrid-from-nodejs"></a><span data-ttu-id="bd589-104">Hur du skickar e-post med SendGrid från Node.js</span><span class="sxs-lookup"><span data-stu-id="bd589-104">How to Send Email Using SendGrid from Node.js</span></span>
<span data-ttu-id="bd589-105">Den här guiden visar hur du utför vanliga programmeringsuppgifter med SendGrid-e-posttjänsten på Azure.</span><span class="sxs-lookup"><span data-stu-id="bd589-105">This guide demonstrates how to perform common programming tasks with the SendGrid email service on Azure.</span></span> <span data-ttu-id="bd589-106">Exemplen är skrivna med Node.js-API.</span><span class="sxs-lookup"><span data-stu-id="bd589-106">The samples are written using the Node.js API.</span></span> <span data-ttu-id="bd589-107">Scenarier som tas upp inkluderar **konstruera e-post**, **skicka e-post**, **lägga till bilagor**, **med hjälp av filter**, och **uppdatera egenskaperna för**.</span><span class="sxs-lookup"><span data-stu-id="bd589-107">The scenarios covered include **constructing email**, **sending email**, **adding attachments**, **using filters**, and **updating properties**.</span></span> <span data-ttu-id="bd589-108">Mer information om SendGrid och skicka e-post finns i [nästa steg](#next-steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bd589-108">For more information on SendGrid and sending email, see the [Next Steps](#next-steps) section.</span></span>

## <a name="what-is-the-sendgrid-email-service"></a><span data-ttu-id="bd589-109">Vad är SendGrid e-posttjänst?</span><span class="sxs-lookup"><span data-stu-id="bd589-109">What is the SendGrid Email Service?</span></span>
<span data-ttu-id="bd589-110">SendGrid är en [molnbaserade e-posttjänst] som ger tillförlitliga [transaktionella e-postleverans], skalbarhet och analys i realtid tillsammans med flexibel API: er som gör det enkelt anpassad integrering.</span><span class="sxs-lookup"><span data-stu-id="bd589-110">SendGrid is a [cloud-based email service] that provides reliable [transactional email delivery], scalability, and real-time analytics along with flexible APIs that make custom integration easy.</span></span> <span data-ttu-id="bd589-111">Vanliga Användningsscenarier för SendGrid är:</span><span class="sxs-lookup"><span data-stu-id="bd589-111">Common SendGrid usage scenarios include:</span></span>

* <span data-ttu-id="bd589-112">Skicka automatiskt kvitton till kunder</span><span class="sxs-lookup"><span data-stu-id="bd589-112">Automatically sending receipts to customers</span></span>
* <span data-ttu-id="bd589-113">Administrera distribution visas för att skicka kunder månatliga e-reklamblad och specialerbjudanden</span><span class="sxs-lookup"><span data-stu-id="bd589-113">Administering distribution lists for sending customers monthly e-fliers and special offers</span></span>
* <span data-ttu-id="bd589-114">Samla in realtid mätvärden för sådant som blockerade e-post och kunden svarstider</span><span class="sxs-lookup"><span data-stu-id="bd589-114">Collecting real-time metrics for things like blocked e-mail, and customer responsiveness</span></span>
* <span data-ttu-id="bd589-115">Generera rapporter för att identifiera trender</span><span class="sxs-lookup"><span data-stu-id="bd589-115">Generating reports to help identify trends</span></span>
* <span data-ttu-id="bd589-116">Vidarebefordran av kundfrågor</span><span class="sxs-lookup"><span data-stu-id="bd589-116">Forwarding customer inquiries</span></span>
* <span data-ttu-id="bd589-117">E-postaviseringar från ditt program</span><span class="sxs-lookup"><span data-stu-id="bd589-117">Email notifications from your application</span></span>

<span data-ttu-id="bd589-118">Mer information finns i [https://sendgrid.com](https://sendgrid.com).</span><span class="sxs-lookup"><span data-stu-id="bd589-118">For more information, see [https://sendgrid.com](https://sendgrid.com).</span></span>

## <a name="create-a-sendgrid-account"></a><span data-ttu-id="bd589-119">Skapa ett SendGrid-konto</span><span class="sxs-lookup"><span data-stu-id="bd589-119">Create a SendGrid Account</span></span>
[!INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-nodejs-module"></a><span data-ttu-id="bd589-120">Referera till SendGrid Node.js-modul</span><span class="sxs-lookup"><span data-stu-id="bd589-120">Reference the SendGrid Node.js Module</span></span>
<span data-ttu-id="bd589-121">Modulen SendGrid för Node.js kan installeras via noden package manager (npm) med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="bd589-121">The SendGrid module for Node.js can be installed through the node package manager (npm) by using the following command:</span></span>

    npm install sendgrid

<span data-ttu-id="bd589-122">Efter installationen kan kräva du modulen i ditt program med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="bd589-122">After installation, you can require the module in your application by using the following code:</span></span>

    var sendgrid = require('sendgrid')(sendgrid_username, sendgrid_password);

<span data-ttu-id="bd589-123">Modulen SendGrid exporterar den **SendGrid** och **e-post** funktioner.</span><span class="sxs-lookup"><span data-stu-id="bd589-123">The SendGrid module exports the **SendGrid** and **Email** functions.</span></span>
<span data-ttu-id="bd589-124">**SendGrid** ansvarar för att skicka e-post via Web API medan **e-post** kapslar in ett e-postmeddelande.</span><span class="sxs-lookup"><span data-stu-id="bd589-124">**SendGrid** is responsible for sending email through Web API, while **Email** encapsulates an email message.</span></span>

## <a name="how-to-create-an-email"></a><span data-ttu-id="bd589-125">Så här: skapa ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="bd589-125">How to: Create an Email</span></span>
<span data-ttu-id="bd589-126">Skapa ett e-postmeddelande med hjälp av modulen SendGrid innebär att först skapa ett e-postmeddelande med hjälp av funktionen för e-post och skicka det med hjälp av funktionen SendGrid.</span><span class="sxs-lookup"><span data-stu-id="bd589-126">Creating an email message using the SendGrid module involves first creating an email message using the Email function, and then sending it using the SendGrid function.</span></span> <span data-ttu-id="bd589-127">Följande är ett exempel på hur du skapar ett nytt meddelande med hjälp av funktionen för e-post:</span><span class="sxs-lookup"><span data-stu-id="bd589-127">The following is an example of creating a new message using the Email function:</span></span>

    var email = new sendgrid.Email({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.'
    });

<span data-ttu-id="bd589-128">Du kan också ange ett HTML-meddelande för klienter som stöder html-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bd589-128">You can also specify an HTML message for clients that support it by setting the html property.</span></span> <span data-ttu-id="bd589-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bd589-129">For example:</span></span>

    html: This is a sample <b>HTML<b> email message.

<span data-ttu-id="bd589-130">Ange egenskaper för både text och html ger korrekt användning av textinnehåll för klienter som inte stöder HTML-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bd589-130">Setting both the text and html properties provides graceful fallback to text content for clients that cannot support HTML messages.</span></span>

<span data-ttu-id="bd589-131">Mer information om alla egenskaper som stöds av funktionen för e-post finns [sendgrid nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="bd589-131">For more information on all properties supported by the Email function, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-send-an-email"></a><span data-ttu-id="bd589-132">Så här: skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="bd589-132">How to: Send an Email</span></span>
<span data-ttu-id="bd589-133">Du kan skicka den med hjälp av Web-API som tillhandahålls av SendGrid när du har skapat ett e-postmeddelande med hjälp av funktionen för e-post.</span><span class="sxs-lookup"><span data-stu-id="bd589-133">After creating an email message using the Email function, you can send it using the Web API provided by SendGrid.</span></span> 

### <a name="web-api"></a><span data-ttu-id="bd589-134">Webb-API</span><span class="sxs-lookup"><span data-stu-id="bd589-134">Web API</span></span>
    sendgrid.send(email, function(err, json){
        if(err) { return console.error(err); }
        console.log(json);
    });

> [!NOTE]
> <span data-ttu-id="bd589-135">Du kan också direkt anropa send-funktionen genom att ange egenskaper för e-post direkt när ovanstående exempel visa passera i en e-objektet och motringning funktion.</span><span class="sxs-lookup"><span data-stu-id="bd589-135">While the above examples show passing in an email object and callback function, you can also directly invoke the send function by directly specifying email properties.</span></span> <span data-ttu-id="bd589-136">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bd589-136">For example:</span></span>  
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

## <a name="how-to-add-an-attachment"></a><span data-ttu-id="bd589-137">Så här: Lägg till en bifogad fil</span><span class="sxs-lookup"><span data-stu-id="bd589-137">How to: Add an Attachment</span></span>
<span data-ttu-id="bd589-138">Bifogade filer kan läggas till ett meddelande genom att ange filnamn och sökväg i den **filer** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bd589-138">Attachments can be added to a message by specifying the file name(s) and path(s) in the **files** property.</span></span> <span data-ttu-id="bd589-139">Exemplet nedan visar skicka bifogade filer:</span><span class="sxs-lookup"><span data-stu-id="bd589-139">The following example demonstrates sending an attachment:</span></span>

    sendgrid.send({
        to: 'john@contoso.com',
        from: 'anna@contoso.com',
        subject: 'test mail',
        text: 'This is a sample email message.',
        files: [
            {
                filename:     '',           // required only if file.content is used.
                contentType:  '',           // optional
                cid:          '',           // optional, used to specify cid for inline content
                path:         '',           //
                url:          '',           // == One of these three options is required
                content:      ('' | Buffer) //
            }
        ],
    });

> [!NOTE]
> <span data-ttu-id="bd589-140">När du använder den **filer** egenskapen filen måste vara tillgängligt via [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span><span class="sxs-lookup"><span data-stu-id="bd589-140">When using the **files** property, the file must be accessible through [fs.readFile](http://nodejs.org/docs/v0.6.7/api/fs.html#fs.readFile).</span></span> <span data-ttu-id="bd589-141">Om filen som du vill ansluta finns i Azure Storage, exempelvis en Blob-behållare, måste du först kopiera filen till lokal lagring eller en Azure-enheten innan den kan skickas som en bifogad fil med hjälp av den **filer** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bd589-141">If the file you wish to attach is hosted in Azure Storage, such as in a Blob container, you must first copy the file to local storage or to an Azure drive before it can be sent as an attachment using the **files** property.</span></span>
> 
> 

## <a name="how-to-use-filters-to-enable-footers-and-tracking"></a><span data-ttu-id="bd589-142">Så här: använda filter för att aktivera sidfötter och spårning</span><span class="sxs-lookup"><span data-stu-id="bd589-142">How to: Use Filters to Enable Footers and Tracking</span></span>
<span data-ttu-id="bd589-143">SendGrid ger ytterligare e-postfunktioner genom att använda filter.</span><span class="sxs-lookup"><span data-stu-id="bd589-143">SendGrid provides additional email functionality through the use of filters.</span></span> <span data-ttu-id="bd589-144">Dessa finns inställningar som du kan lägga till ett e-postmeddelande för att aktivera vissa funktioner, till exempel aktivera Klicka spårning, Google analytics, prenumeration spårning och så vidare.</span><span class="sxs-lookup"><span data-stu-id="bd589-144">These are settings that can be added to an email message to enable specific functionality such as enabling click tracking, Google analytics, subscription tracking, and so on.</span></span> <span data-ttu-id="bd589-145">En fullständig lista över filter finns [filterinställningar][Filter Settings].</span><span class="sxs-lookup"><span data-stu-id="bd589-145">For a full list of filters, see [Filter Settings][Filter Settings].</span></span>

<span data-ttu-id="bd589-146">Filter kan tillämpas på ett meddelande med hjälp av den **filter** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bd589-146">Filters can be applied to a message by using the **filters** property.</span></span>
<span data-ttu-id="bd589-147">Varje filter som anges av ett hash-värde som innehåller filter-specifika inställningar.</span><span class="sxs-lookup"><span data-stu-id="bd589-147">Each filter is specified by a hash containing filter-specific settings.</span></span>
<span data-ttu-id="bd589-148">Följande exempel visar sidfoten och på Spåra filter:</span><span class="sxs-lookup"><span data-stu-id="bd589-148">The following examples demonstrate the footer and click tracking filters:</span></span>

### <a name="footer"></a><span data-ttu-id="bd589-149">Sidfot</span><span class="sxs-lookup"><span data-stu-id="bd589-149">Footer</span></span>
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

### <a name="click-tracking"></a><span data-ttu-id="bd589-150">Klicka på spårning</span><span class="sxs-lookup"><span data-stu-id="bd589-150">Click Tracking</span></span>
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

## <a name="how-to-update-email-properties"></a><span data-ttu-id="bd589-151">Så här: uppdatera egenskaper för e-post</span><span class="sxs-lookup"><span data-stu-id="bd589-151">How to: Update Email Properties</span></span>
<span data-ttu-id="bd589-152">Vissa egenskaper för e-post kan skrivas över med  **ange*egenskapen*** eller läggas till med hjälp av  **lägga till*egenskapen***.</span><span class="sxs-lookup"><span data-stu-id="bd589-152">Some email properties can be overwritten using **set*Property*** or appended using **add*Property***.</span></span> <span data-ttu-id="bd589-153">Du kan till exempel lägga till ytterligare mottagare med hjälp av</span><span class="sxs-lookup"><span data-stu-id="bd589-153">For example, you can add additional recipients by using</span></span>

    email.addTo('jeff@contoso.com');

<span data-ttu-id="bd589-154">eller ange ett filter med hjälp av</span><span class="sxs-lookup"><span data-stu-id="bd589-154">or set a filter by using</span></span>

    email.addFilter('footer', 'enable', 1);
    email.addFilter('footer', 'text/html', '<strong>boo</strong>');

<span data-ttu-id="bd589-155">Mer information finns i [sendgrid nodejs][sendgrid-nodejs].</span><span class="sxs-lookup"><span data-stu-id="bd589-155">For more information, see [sendgrid-nodejs][sendgrid-nodejs].</span></span>

## <a name="how-to-use-additional-sendgrid-services"></a><span data-ttu-id="bd589-156">Så här: använda ytterligare SendGrid tjänster</span><span class="sxs-lookup"><span data-stu-id="bd589-156">How to: Use Additional SendGrid Services</span></span>
<span data-ttu-id="bd589-157">SendGrid erbjuder webbaserade API: er som du kan använda för att utnyttja ytterligare funktioner för SendGrid från Azure-program.</span><span class="sxs-lookup"><span data-stu-id="bd589-157">SendGrid offers web-based APIs that you can use to leverage additional SendGrid functionality from your Azure application.</span></span> <span data-ttu-id="bd589-158">Fullständig information finns i [SendGrid API-dokumentationen][SendGrid API documentation].</span><span class="sxs-lookup"><span data-stu-id="bd589-158">For full details, see the [SendGrid API documentation][SendGrid API documentation].</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd589-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd589-159">Next Steps</span></span>
<span data-ttu-id="bd589-160">Nu när du har lärt dig grunderna om tjänsten SendGrid e-post, kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="bd589-160">Now that you've learned the basics of the SendGrid Email service, follow these links to learn more.</span></span>

* <span data-ttu-id="bd589-161">SendGrid Node.js modulen databasen: [sendgrid nodejs][sendgrid-nodejs]</span><span class="sxs-lookup"><span data-stu-id="bd589-161">SendGrid Node.js module repository: [sendgrid-nodejs][sendgrid-nodejs]</span></span>
* <span data-ttu-id="bd589-162">SendGrid API-dokumentationen: <https://sendgrid.com/docs></span><span class="sxs-lookup"><span data-stu-id="bd589-162">SendGrid API documentation: <https://sendgrid.com/docs></span></span>
* <span data-ttu-id="bd589-163">SendGrid specialerbjudande för Azure-kunder: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span><span class="sxs-lookup"><span data-stu-id="bd589-163">SendGrid special offer for Azure customers: [http://sendgrid.com/azure.html](https://sendgrid.com/windowsazure.html)</span></span>

[special offer]: https://sendgrid.com/windowsazure.html
[sendgrid-nodejs]: https://github.com/sendgrid/sendgrid-nodejs
[Filter Settings]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
[SendGrid API documentation]: https://sendgrid.com/docs
<span data-ttu-id="bd589-164">[molnbaserade e-posttjänst]: https://sendgrid.com/email-solutions</span><span class="sxs-lookup"><span data-stu-id="bd589-164">[cloud-based email service]: https://sendgrid.com/email-solutions</span></span>
<span data-ttu-id="bd589-165">[transaktionella e-postleverans]: https://sendgrid.com/transactional-email</span><span class="sxs-lookup"><span data-stu-id="bd589-165">[transactional email delivery]: https://sendgrid.com/transactional-email</span></span>
