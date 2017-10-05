---
title: "Aktivera HTTPS på en anpassad domän för Azure CDN | Microsoft Docs"
description: "Lär dig mer om att aktivera HTTPS på Azure CDN-slutpunkten med en anpassad domän."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: b334ba6bbec1d0a7e23a514174bffae01c7fff05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a><span data-ttu-id="d4ca7-103">Aktivera HTTPS på en anpassad domän för Azure CDN</span><span class="sxs-lookup"><span data-stu-id="d4ca7-103">Enable HTTPS on an Azure CDN custom domain</span></span>

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="d4ca7-104">HTTPS-stöd för Azure CDN anpassade domäner kan du leverera skyddat innehåll via SSL med hjälp av ditt eget domännamn för att förbättra säkerheten för data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-104">HTTPS support for Azure CDN custom domains enables you to deliver secure content via SSL using your own domain name to improve the security of data while in transit.</span></span> <span data-ttu-id="d4ca7-105">Slutpunkt till slutpunkt-arbetsflöde för att aktivera HTTPS för den anpassade domänen är förenklad via en enda klickning aktivering, fullständig certifikathantering och alla med utan extra kostnad.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-105">The end-to-end workflow to enable HTTPS for your custom domain is simplified via one-click enablement, complete certificate management, and all with no additional cost.</span></span>

<span data-ttu-id="d4ca7-106">Det är viktigt att säkerställa sekretess och dataintegriteten för alla program web känsliga data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-106">It's critical to ensure the privacy and data integrity of all of your web applications sensitive data while in transit.</span></span> <span data-ttu-id="d4ca7-107">Med hjälp av HTTPS-protokollet garanterar att dina känsliga data krypteras när de skickas över internet.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-107">Using the HTTPS protocol ensures that your sensitive data is encrypted when it is sent across the internet.</span></span> <span data-ttu-id="d4ca7-108">Den ger litar på autentisering och skyddar ditt webbprogram från attacker.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-108">It provides trust, authentication and protects your web applications from attacks.</span></span> <span data-ttu-id="d4ca7-109">För närvarande stöder Azure CDN HTTPS på en CDN-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-109">Currently, Azure CDN supports HTTPS on a CDN endpoint.</span></span> <span data-ttu-id="d4ca7-110">Till exempel om du skapar en CDN-slutpunkt från Azure CDN (t.ex. https://contoso.azureedge.net) är HTTPS aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-110">For example, if you create a CDN endpoint from Azure CDN (e.g. https://contoso.azureedge.net), HTTPS is enabled by default.</span></span> <span data-ttu-id="d4ca7-111">Nu med anpassad domän HTTPS, kan du aktivera säker leverans för en anpassad domän (t.ex. https://www.contoso.com) samt.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-111">Now, with custom domain HTTPS, you can enable secure delivery for a custom domain (e.g. https://www.contoso.com) as well.</span></span> 

<span data-ttu-id="d4ca7-112">Några viktiga attribut för HTTPS-funktionen är:</span><span class="sxs-lookup"><span data-stu-id="d4ca7-112">Some of the key attributes of HTTPS feature are:</span></span>

- <span data-ttu-id="d4ca7-113">Utan extra kostnad: det finns inga kostnader för certifikatanskaffningen eller förnyelse och utan extra kostnad för HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-113">No additional cost: There are no costs for certificate acquisition or renewal and no additional cost for HTTPS traffic.</span></span> <span data-ttu-id="d4ca7-114">Du betalar bara för GB utgående trafik från CDN.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-114">You just pay for GB egress from the CDN.</span></span>

- <span data-ttu-id="d4ca7-115">Enkel aktivering: en Klicka etablering är tillgänglig från den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d4ca7-115">Simple enablement: One click provisioning is available from the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d4ca7-116">Du kan också använda REST API eller andra verktyg för utvecklare för att aktivera funktionen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-116">You can also use REST API or other developer tools to enable the feature.</span></span>

- <span data-ttu-id="d4ca7-117">Slutföra certifikathantering: alla certifikat inköp och hanteras åt dig.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-117">Complete certificate management: All certificate procurement and management is handled for you.</span></span> <span data-ttu-id="d4ca7-118">Certifikaten etableras automatiskt och förnyas innan upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-118">Certificates are automatically provisioned and renewed prior to expiration.</span></span> <span data-ttu-id="d4ca7-119">Detta tar bort riskerna med avbrott i tjänsten på grund av ett certifikat upphör att gälla helt.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-119">This completely removes the risks of service interruption as a result of a certificate expiring.</span></span>

>[!NOTE] 
><span data-ttu-id="d4ca7-120">Innan du aktiverar stöd för HTTPS, du måste har upprättat en [Azure CDN domänen](./cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d4ca7-120">Prior to enabling HTTPS support, you must have already established an [Azure CDN custom domain](./cdn-map-content-to-custom-domain.md).</span></span>

## <a name="step-1-enabling-the-feature"></a><span data-ttu-id="d4ca7-121">Steg 1: Aktivera funktionen</span><span class="sxs-lookup"><span data-stu-id="d4ca7-121">Step 1: Enabling the feature</span></span> 

1. <span data-ttu-id="d4ca7-122">I den [Azure-portalen](https://portal.azure.com), bläddra till Verizon standard eller premium CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-122">In the [Azure portal](https://portal.azure.com), browse to your Verizon standard or premium CDN profile.</span></span>

2. <span data-ttu-id="d4ca7-123">Klicka på den slutpunkt som innehåller din anpassade domän i listan över slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-123">In the list of endpoints, click the endpoint containing your custom domain.</span></span>

3. <span data-ttu-id="d4ca7-124">Klicka på den anpassade domänen som du vill aktivera HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-124">Click the custom domain for which you want to enable HTTPS.</span></span>

    ![Slutpunkten bladet](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. <span data-ttu-id="d4ca7-126">Klicka på **på** att aktivera HTTPS och spara ändringen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-126">Click **On** to enable HTTPS and save the change.</span></span>

    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a><span data-ttu-id="d4ca7-128">Steg 2: Verifiering av domän</span><span class="sxs-lookup"><span data-stu-id="d4ca7-128">Step 2: Domain validation</span></span>

>[!IMPORTANT] 
><span data-ttu-id="d4ca7-129">Du måste slutföra verifiering av domän innan HTTPS kommer att vara aktiv på den anpassade domänen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-129">You must complete domain validation before HTTPS will be active on your custom domain.</span></span> <span data-ttu-id="d4ca7-130">Du har 6 arbetsdagar att godkänna domänen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-130">You have 6 business days to approve the domain.</span></span> <span data-ttu-id="d4ca7-131">Begäran avbryts med ingen godkännande inom 6 arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-131">Request will be canceled with no approval within 6 business days.</span></span>  

<span data-ttu-id="d4ca7-132">När du har aktiverat HTTPS på den anpassade domänen verifierar DigiCert för providern våra HTTPS-certifikat ägarskap för domänen genom att kontakta registrant för din domän, baserat på WHOIS registrant information via e-post (som standard) eller telefon.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-132">After enabling HTTPS on your custom domain, our HTTPS certificate provider DigiCert will validate ownership of your domain by contacting the registrant for your domain, based on WHOIS registrant information, via email (by default) or phone.</span></span> <span data-ttu-id="d4ca7-133">DigiCert kommer också att skicka e-postmeddelandet till den nedan adresser.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-133">DigiCert will also send the verification email to the below addresses.</span></span> <span data-ttu-id="d4ca7-134">Om WHOIS registrant information är privat kan kontrollera att du kan godkänna direkt från någon av dessa adresser.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-134">If WHOIS registrant information is private, make sure you can approve directly from one of these addresses.</span></span>

><span data-ttu-id="d4ca7-135">Admin @< din-domain-name.com > administratör @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="d4ca7-135">admin@<your-domain-name.com> administrator@<your-domain-name.com></span></span>  
><span data-ttu-id="d4ca7-136">webbadministratör @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="d4ca7-136">webmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="d4ca7-137">hostmaster @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="d4ca7-137">hostmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="d4ca7-138">postmaster @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="d4ca7-138">postmaster@<your-domain-name.com></span></span>


<span data-ttu-id="d4ca7-139">Vid mottagning av e-postmeddelandet kan ha två alternativ för verifiering:</span><span class="sxs-lookup"><span data-stu-id="d4ca7-139">Upon receiving the email, you have two verification options:</span></span>

1. <span data-ttu-id="d4ca7-140">Du kan godkänna alla framtida beställningar via samma konto för samma rotdomänen, t.ex. consoto.com.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-140">You can approve all future orders placed through the same account for the same root domain, e.g. consoto.com.</span></span> <span data-ttu-id="d4ca7-141">Detta är en rekommenderad metod om du planerar att lägga till ytterligare anpassade domäner i framtiden för samma rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-141">This is a recommended approach if you are planning to add additional custom domains in the future for the same root domain.</span></span>
 
2. <span data-ttu-id="d4ca7-142">Du kan bara godkänna specifika värdnamnet som används i denna begäran.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-142">You can just approve the specific host name used in this request.</span></span> <span data-ttu-id="d4ca7-143">Ytterligare godkännande krävas för efterföljande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-143">Additional approval will be required for subsequent requests.</span></span>

    <span data-ttu-id="d4ca7-144">Exempel e-post:</span><span class="sxs-lookup"><span data-stu-id="d4ca7-144">Example email:</span></span>
    
    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

<span data-ttu-id="d4ca7-146">Efter godkännande och läggs DigiCert ditt domännamn till SAN-certifikat.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-146">After approval, DigiCert will add your custom domain name to the SAN certificate.</span></span> <span data-ttu-id="d4ca7-147">Certifikatet ska vara giltigt för ett år och automatisk förnyas innan den har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-147">The certificate will be valid for one year and will be auto renewed before it's expired.</span></span>

## <a name="step-3-wait-for-the-propagation-then-start-using-your-feature"></a><span data-ttu-id="d4ca7-148">Steg 3: Vänta tills spridning och starta sedan din funktionen</span><span class="sxs-lookup"><span data-stu-id="d4ca7-148">Step 3: Wait for the propagation then start using your feature</span></span>

<span data-ttu-id="d4ca7-149">När domännamnet har verifierats tar upp till 6 – 8 timmar för anpassade domäner HTTPS-funktionen ska vara aktiv.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-149">After the domain name is validated it will take up to 6-8 hours for the custom domain HTTPS feature to be active.</span></span> <span data-ttu-id="d4ca7-150">När processen är klar sätts ”anpassade HTTPS” status i Azure portal till ”aktiverad”.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-150">After the process is complete, the "custom HTTPS" status in the Azure portal will be set to "Enabled".</span></span> <span data-ttu-id="d4ca7-151">HTTPS med den anpassade domänen är nu klar att användas.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-151">HTTPS with your custom domain is now ready for your use.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="d4ca7-152">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="d4ca7-152">Frequently asked questions</span></span>

1. <span data-ttu-id="d4ca7-153">*Vem är certifikat-providern och vilken typ av certifikat används?*</span><span class="sxs-lookup"><span data-stu-id="d4ca7-153">*Who is the certificate provider and what type of certificate is used?*</span></span>

    <span data-ttu-id="d4ca7-154">Vi använder alternativa namn på CERTIFIKATMOTTAGARE certifikat som tillhandahålls av DigiCert.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-154">We use Subject Alternative Names (SAN) certificate provided by DigiCert.</span></span> <span data-ttu-id="d4ca7-155">Ett SAN-certifikat kan skydda flera fullständigt kvalificerade domännamn med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-155">A SAN certificate can secure multiple fully qualifIed domain names with one certificate.</span></span>

2. <span data-ttu-id="d4ca7-156">*Kan jag använda mitt dedikerade certifikat?*</span><span class="sxs-lookup"><span data-stu-id="d4ca7-156">*Can I use my dedicated certificate?*</span></span>
    
    <span data-ttu-id="d4ca7-157">För närvarande inte, men det är på Översikt.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-157">Not currently, but it's on the roadmap.</span></span>

3. <span data-ttu-id="d4ca7-158">*Vad händer om jag får inte domänen e-postmeddelandet från DigiCert?*</span><span class="sxs-lookup"><span data-stu-id="d4ca7-158">*What if I don't receive the domain verification email from DigiCert?*</span></span>

    <span data-ttu-id="d4ca7-159">Kontakta Microsoft om du inte får ett e-postmeddelande inom 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-159">Please contact Microsoft if you don't receive an email within 24 hours.</span></span>

4. <span data-ttu-id="d4ca7-160">*Använder ett SAN-certifikat som är mindre säker än ett dedikerat certifikat?*</span><span class="sxs-lookup"><span data-stu-id="d4ca7-160">*Is using a SAN certificate less secure than a dedicated certificate?*</span></span>
    
    <span data-ttu-id="d4ca7-161">Ett SAN-certifikat följer samma kryptering och säkerhet standarder som en dedikerad certifikat.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-161">A SAN cert follows the same encryption and security standards as a dedicated cert.</span></span> <span data-ttu-id="d4ca7-162">Alla utfärdade SSL-certifikat använder SHA-256 för förbättrad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-162">All issued SSL certificates are using SHA-256 for enhanced server security.</span></span>

5. <span data-ttu-id="d4ca7-163">*Kan jag använda domänen HTTPS med Azure CDN från Akamai?*</span><span class="sxs-lookup"><span data-stu-id="d4ca7-163">*Can I use custom domain HTTPS with Azure CDN from Akamai?*</span></span>

    <span data-ttu-id="d4ca7-164">Den här funktionen är för närvarande bara tillgänglig med Azure CDN från Verizon.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-164">Currently, this feature is only available with Azure CDN from Verizon.</span></span> <span data-ttu-id="d4ca7-165">Vi arbetar på stöder den här funktionen med Azure CDN från Akamai under de kommande månaderna.</span><span class="sxs-lookup"><span data-stu-id="d4ca7-165">We are working on supporting this feature with Azure CDN from Akamai in the coming months.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d4ca7-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4ca7-166">Next steps</span></span>

- <span data-ttu-id="d4ca7-167">Lär dig hur du ställer in en [domänen på Azure CDN-slutpunkten](./cdn-map-content-to-custom-domain.md)</span><span class="sxs-lookup"><span data-stu-id="d4ca7-167">Learn how to set up a [custom domain on your Azure CDN endpoint](./cdn-map-content-to-custom-domain.md)</span></span>


