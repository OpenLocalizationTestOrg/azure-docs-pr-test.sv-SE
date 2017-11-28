---
title: "aaa ”aktivera HTTPS på en anpassad domän för Azure CDN | Microsoft Docs ”"
description: "Lär dig hur tooenable HTTPS på Azure CDN-slutpunkten med en anpassad domän."
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
ms.openlocfilehash: 93746222616c9ed7977ec3b22c38ac1d43b118f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a><span data-ttu-id="9d310-103">Aktivera HTTPS på en anpassad domän för Azure CDN</span><span class="sxs-lookup"><span data-stu-id="9d310-103">Enable HTTPS on an Azure CDN custom domain</span></span>

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="9d310-104">HTTPS-stöd för Azure CDN anpassade domäner möjliggör toodeliver skyddat innehåll via SSL med hjälp av en egen domän namn tooimprove hello säkerheten för data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="9d310-104">HTTPS support for Azure CDN custom domains enables you toodeliver secure content via SSL using your own domain name tooimprove hello security of data while in transit.</span></span> <span data-ttu-id="9d310-105">hello slutpunkt till slutpunkt-arbetsflöde tooenable HTTPS för den anpassade domänen är förenklad via en enda klickning aktivering, fullständig certifikathantering och alla med utan extra kostnad.</span><span class="sxs-lookup"><span data-stu-id="9d310-105">hello end-to-end workflow tooenable HTTPS for your custom domain is simplified via one-click enablement, complete certificate management, and all with no additional cost.</span></span>

<span data-ttu-id="9d310-106">Det är kritiska tooensure hello sekretess och dataintegriteten för alla program web känsliga data under överföringen.</span><span class="sxs-lookup"><span data-stu-id="9d310-106">It's critical tooensure hello privacy and data integrity of all of your web applications sensitive data while in transit.</span></span> <span data-ttu-id="9d310-107">Med hjälp av HTTPS-protokollet garanterar att dina känsliga data krypteras när de skickas över hello hello internet.</span><span class="sxs-lookup"><span data-stu-id="9d310-107">Using hello HTTPS protocol ensures that your sensitive data is encrypted when it is sent across hello internet.</span></span> <span data-ttu-id="9d310-108">Den ger litar på autentisering och skyddar ditt webbprogram från attacker.</span><span class="sxs-lookup"><span data-stu-id="9d310-108">It provides trust, authentication and protects your web applications from attacks.</span></span> <span data-ttu-id="9d310-109">För närvarande stöder Azure CDN HTTPS på en CDN-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="9d310-109">Currently, Azure CDN supports HTTPS on a CDN endpoint.</span></span> <span data-ttu-id="9d310-110">Till exempel om du skapar en CDN-slutpunkt från Azure CDN (t.ex. https://contoso.azureedge.net) är HTTPS aktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="9d310-110">For example, if you create a CDN endpoint from Azure CDN (e.g. https://contoso.azureedge.net), HTTPS is enabled by default.</span></span> <span data-ttu-id="9d310-111">Nu med anpassad domän HTTPS, kan du aktivera säker leverans för en anpassad domän (t.ex. https://www.contoso.com) samt.</span><span class="sxs-lookup"><span data-stu-id="9d310-111">Now, with custom domain HTTPS, you can enable secure delivery for a custom domain (e.g. https://www.contoso.com) as well.</span></span> 

<span data-ttu-id="9d310-112">Några av hello nyckelattribut för HTTPS-funktionen är:</span><span class="sxs-lookup"><span data-stu-id="9d310-112">Some of hello key attributes of HTTPS feature are:</span></span>

- <span data-ttu-id="9d310-113">Utan extra kostnad: det finns inga kostnader för certifikatanskaffningen eller förnyelse och utan extra kostnad för HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="9d310-113">No additional cost: There are no costs for certificate acquisition or renewal and no additional cost for HTTPS traffic.</span></span> <span data-ttu-id="9d310-114">Du betalar bara för GB utgående trafik från hello CDN.</span><span class="sxs-lookup"><span data-stu-id="9d310-114">You just pay for GB egress from hello CDN.</span></span>

- <span data-ttu-id="9d310-115">Enkel aktivering: en Klicka etablering är tillgänglig från hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9d310-115">Simple enablement: One click provisioning is available from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="9d310-116">Du kan också använda REST API eller någon annan developer tools tooenable hello-funktion.</span><span class="sxs-lookup"><span data-stu-id="9d310-116">You can also use REST API or other developer tools tooenable hello feature.</span></span>

- <span data-ttu-id="9d310-117">Slutföra certifikathantering: alla certifikat inköp och hanteras åt dig.</span><span class="sxs-lookup"><span data-stu-id="9d310-117">Complete certificate management: All certificate procurement and management is handled for you.</span></span> <span data-ttu-id="9d310-118">Certifikat tillhandahålls automatiskt och förnyas tidigare tooexpiration.</span><span class="sxs-lookup"><span data-stu-id="9d310-118">Certificates are automatically provisioned and renewed prior tooexpiration.</span></span> <span data-ttu-id="9d310-119">Detta tar bort hello riskerna med avbrott i tjänsten på grund av ett certifikat upphör att gälla helt.</span><span class="sxs-lookup"><span data-stu-id="9d310-119">This completely removes hello risks of service interruption as a result of a certificate expiring.</span></span>

>[!NOTE] 
><span data-ttu-id="9d310-120">HTTPS-stöd för tidigare tooenabling, du måste har upprättat en [Azure CDN domänen](./cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="9d310-120">Prior tooenabling HTTPS support, you must have already established an [Azure CDN custom domain](./cdn-map-content-to-custom-domain.md).</span></span>

## <a name="step-1-enabling-hello-feature"></a><span data-ttu-id="9d310-121">Steg 1: Aktivera hello-funktionen</span><span class="sxs-lookup"><span data-stu-id="9d310-121">Step 1: Enabling hello feature</span></span> 

1. <span data-ttu-id="9d310-122">I hello [Azure-portalen](https://portal.azure.com), bläddra tooyour Verizon standard eller premium CDN-profilen.</span><span class="sxs-lookup"><span data-stu-id="9d310-122">In hello [Azure portal](https://portal.azure.com), browse tooyour Verizon standard or premium CDN profile.</span></span>

2. <span data-ttu-id="9d310-123">Hello-slutpunkt som innehåller den anpassade domänen på hello listan över slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="9d310-123">In hello list of endpoints, click hello endpoint containing your custom domain.</span></span>

3. <span data-ttu-id="9d310-124">Klicka på hello anpassad domän för vilken du vill tooenable HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9d310-124">Click hello custom domain for which you want tooenable HTTPS.</span></span>

    ![Slutpunkten bladet](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. <span data-ttu-id="9d310-126">Klicka på **på** tooenable HTTPS och spara hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="9d310-126">Click **On** tooenable HTTPS and save hello change.</span></span>

    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a><span data-ttu-id="9d310-128">Steg 2: Verifiering av domän</span><span class="sxs-lookup"><span data-stu-id="9d310-128">Step 2: Domain validation</span></span>

>[!IMPORTANT] 
><span data-ttu-id="9d310-129">Du måste slutföra verifiering av domän innan HTTPS kommer att vara aktiv på den anpassade domänen.</span><span class="sxs-lookup"><span data-stu-id="9d310-129">You must complete domain validation before HTTPS will be active on your custom domain.</span></span> <span data-ttu-id="9d310-130">Du har 6 företag dagar tooapprove hello domän.</span><span class="sxs-lookup"><span data-stu-id="9d310-130">You have 6 business days tooapprove hello domain.</span></span> <span data-ttu-id="9d310-131">Begäran avbryts med ingen godkännande inom 6 arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="9d310-131">Request will be canceled with no approval within 6 business days.</span></span>  

<span data-ttu-id="9d310-132">När du har aktiverat HTTPS på den anpassade domänen verifierar DigiCert för providern våra HTTPS-certifikat ägarskap för domänen genom att kontakta hello registrant för din domän, baserat på WHOIS registrant information via e-post (som standard) eller telefon.</span><span class="sxs-lookup"><span data-stu-id="9d310-132">After enabling HTTPS on your custom domain, our HTTPS certificate provider DigiCert will validate ownership of your domain by contacting hello registrant for your domain, based on WHOIS registrant information, via email (by default) or phone.</span></span> <span data-ttu-id="9d310-133">DigiCert skickar också hello verifiering e-toohello nedan adresser.</span><span class="sxs-lookup"><span data-stu-id="9d310-133">DigiCert will also send hello verification email toohello below addresses.</span></span> <span data-ttu-id="9d310-134">Om WHOIS registrant information är privat kan kontrollera att du kan godkänna direkt från någon av dessa adresser.</span><span class="sxs-lookup"><span data-stu-id="9d310-134">If WHOIS registrant information is private, make sure you can approve directly from one of these addresses.</span></span>

><span data-ttu-id="9d310-135">Admin @< din-domain-name.com > administratör @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="9d310-135">admin@<your-domain-name.com> administrator@<your-domain-name.com></span></span>  
><span data-ttu-id="9d310-136">webbadministratör @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="9d310-136">webmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="9d310-137">hostmaster @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="9d310-137">hostmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="9d310-138">postmaster @< din-domain-name.com ></span><span class="sxs-lookup"><span data-stu-id="9d310-138">postmaster@<your-domain-name.com></span></span>


<span data-ttu-id="9d310-139">Vid hello e-postmeddelanden, har du två alternativ för verifiering:</span><span class="sxs-lookup"><span data-stu-id="9d310-139">Upon receiving hello email, you have two verification options:</span></span>

1. <span data-ttu-id="9d310-140">Du kan godkänna alla framtida beställningar via hello samma konto för hello samma rotdomänen, t.ex. consoto.com. Det här är en rekommenderad metod om du planerar tooadd ytterligare anpassade domäner i hello framtida för hello samma rotdomänen.</span><span class="sxs-lookup"><span data-stu-id="9d310-140">You can approve all future orders placed through hello same account for hello same root domain, e.g. consoto.com. This is a recommended approach if you are planning tooadd additional custom domains in hello future for hello same root domain.</span></span>
 
2. <span data-ttu-id="9d310-141">Du kan bara godkänna hello specifika värdnamn används i denna begäran.</span><span class="sxs-lookup"><span data-stu-id="9d310-141">You can just approve hello specific host name used in this request.</span></span> <span data-ttu-id="9d310-142">Ytterligare godkännande krävas för efterföljande förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="9d310-142">Additional approval will be required for subsequent requests.</span></span>

    <span data-ttu-id="9d310-143">Exempel e-post:</span><span class="sxs-lookup"><span data-stu-id="9d310-143">Example email:</span></span>
    
    ![Dialogrutan för anpassade HTTPS](./media/cdn-custom-ssl/domain-validation-email-example.png)

<span data-ttu-id="9d310-145">Efter godkännande och DigiCert lägger till dina anpassade domäner namnet toohello SAN-certifikat.</span><span class="sxs-lookup"><span data-stu-id="9d310-145">After approval, DigiCert will add your custom domain name toohello SAN certificate.</span></span> <span data-ttu-id="9d310-146">hello certifikatet är giltigt i ett år och automatisk förnyas innan den har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="9d310-146">hello certificate will be valid for one year and will be auto renewed before it's expired.</span></span>

## <a name="step-3-wait-for-hello-propagation-then-start-using-your-feature"></a><span data-ttu-id="9d310-147">Steg 3: Vänta tills hello spridningen och starta sedan din funktionen</span><span class="sxs-lookup"><span data-stu-id="9d310-147">Step 3: Wait for hello propagation then start using your feature</span></span>

<span data-ttu-id="9d310-148">När hello domännamnet har verifierats tar upp hello domänen HTTPS funktionen toobe active too6 8 timmar.</span><span class="sxs-lookup"><span data-stu-id="9d310-148">After hello domain name is validated it will take up too6-8 hours for hello custom domain HTTPS feature toobe active.</span></span> <span data-ttu-id="9d310-149">När hello processen är slutförd anges hello ”anpassade HTTPS” status i hello Azure-portalen för ”Enabled”.</span><span class="sxs-lookup"><span data-stu-id="9d310-149">After hello process is complete, hello "custom HTTPS" status in hello Azure portal will be set too"Enabled".</span></span> <span data-ttu-id="9d310-150">HTTPS med den anpassade domänen är nu klar att användas.</span><span class="sxs-lookup"><span data-stu-id="9d310-150">HTTPS with your custom domain is now ready for your use.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="9d310-151">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="9d310-151">Frequently asked questions</span></span>

1. <span data-ttu-id="9d310-152">*Vem är hello certifikatutfärdaren och vilken typ av certifikat som ska användas?*</span><span class="sxs-lookup"><span data-stu-id="9d310-152">*Who is hello certificate provider and what type of certificate is used?*</span></span>

    <span data-ttu-id="9d310-153">Vi använder alternativa namn på CERTIFIKATMOTTAGARE certifikat som tillhandahålls av DigiCert.</span><span class="sxs-lookup"><span data-stu-id="9d310-153">We use Subject Alternative Names (SAN) certificate provided by DigiCert.</span></span> <span data-ttu-id="9d310-154">Ett SAN-certifikat kan skydda flera fullständigt kvalificerade domännamn med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="9d310-154">A SAN certificate can secure multiple fully qualifIed domain names with one certificate.</span></span>

2. <span data-ttu-id="9d310-155">*Kan jag använda mitt dedikerade certifikat?*</span><span class="sxs-lookup"><span data-stu-id="9d310-155">*Can I use my dedicated certificate?*</span></span>
    
    <span data-ttu-id="9d310-156">För närvarande inte, men dess på hello Översikt.</span><span class="sxs-lookup"><span data-stu-id="9d310-156">Not currently, but it's on hello roadmap.</span></span>

3. <span data-ttu-id="9d310-157">*Vad händer om jag får inte hello domän e-postmeddelandet från DigiCert?*</span><span class="sxs-lookup"><span data-stu-id="9d310-157">*What if I don't receive hello domain verification email from DigiCert?*</span></span>

    <span data-ttu-id="9d310-158">Kontakta Microsoft om du inte får ett e-postmeddelande inom 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="9d310-158">Please contact Microsoft if you don't receive an email within 24 hours.</span></span>

4. <span data-ttu-id="9d310-159">*Använder ett SAN-certifikat som är mindre säker än ett dedikerat certifikat?*</span><span class="sxs-lookup"><span data-stu-id="9d310-159">*Is using a SAN certificate less secure than a dedicated certificate?*</span></span>
    
    <span data-ttu-id="9d310-160">Ett SAN-certifikat som följer hello samma standarder som en dedikerad cert som kryptering och säkerhet. Alla utfärdade SSL-certifikat använder SHA-256 för förbättrad säkerhet.</span><span class="sxs-lookup"><span data-stu-id="9d310-160">A SAN cert follows hello same encryption and security standards as a dedicated cert. All issued SSL certificates are using SHA-256 for enhanced server security.</span></span>

5. <span data-ttu-id="9d310-161">*Kan jag använda domänen HTTPS med Azure CDN från Akamai?*</span><span class="sxs-lookup"><span data-stu-id="9d310-161">*Can I use custom domain HTTPS with Azure CDN from Akamai?*</span></span>

    <span data-ttu-id="9d310-162">Den här funktionen är för närvarande bara tillgänglig med Azure CDN från Verizon.</span><span class="sxs-lookup"><span data-stu-id="9d310-162">Currently, this feature is only available with Azure CDN from Verizon.</span></span> <span data-ttu-id="9d310-163">Vi arbetar på stöder den här funktionen med Azure CDN från Akamai hello kommande månaderna.</span><span class="sxs-lookup"><span data-stu-id="9d310-163">We are working on supporting this feature with Azure CDN from Akamai in hello coming months.</span></span>


## <a name="next-steps"></a><span data-ttu-id="9d310-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9d310-164">Next steps</span></span>

- <span data-ttu-id="9d310-165">Lär dig hur tooset upp en [domänen på Azure CDN-slutpunkten](./cdn-map-content-to-custom-domain.md)</span><span class="sxs-lookup"><span data-stu-id="9d310-165">Learn how tooset up a [custom domain on your Azure CDN endpoint](./cdn-map-content-to-custom-domain.md)</span></span>


