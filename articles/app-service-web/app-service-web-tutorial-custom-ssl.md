---
title: aaaBind en befintlig anpassad SSL-certifikatet tooAzure Web Apps | Microsoft Docs
description: "Lär dig tootoobind anpassad SSL-certifikat tooyour webbapp, mobilappsserverdel och API-app i Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 3503ba9f96c8ea8d18451e8bf9a9b441797ef44d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-tooazure-web-apps"></a><span data-ttu-id="57bf4-103">Bind ett befintligt anpassat SSL-certifikat tooAzure Web Apps</span><span class="sxs-lookup"><span data-stu-id="57bf4-103">Bind an existing custom SSL certificate tooAzure Web Apps</span></span>

<span data-ttu-id="57bf4-104">Azure Web Apps ger en mycket skalbar, automatisk uppdatering värdtjänst.</span><span class="sxs-lookup"><span data-stu-id="57bf4-104">Azure Web Apps provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="57bf4-105">Den här kursen visar hur toobind anpassat SSL-certifikatet som du köper från en betrodd certifikatutfärdare för[Azure Web Apps](app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="57bf4-105">This tutorial shows you how toobind a custom SSL certificate that you purchased from a trusted certificate authority too[Azure Web Apps](app-service-web-overview.md).</span></span> <span data-ttu-id="57bf4-106">När du är klar, kommer du att kunna tooaccess ditt webbprogram på hello HTTPS-slutpunkten för din anpassade DNS-domän.</span><span class="sxs-lookup"><span data-stu-id="57bf4-106">When you're finished, you'll be able tooaccess your web app at hello HTTPS endpoint of your custom DNS domain.</span></span>

![Webbprogram med anpassade SSL-certifikat](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

<span data-ttu-id="57bf4-108">I den här guiden får du lära dig hur man:</span><span class="sxs-lookup"><span data-stu-id="57bf4-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="57bf4-109">Uppgradera prisnivån för din app</span><span class="sxs-lookup"><span data-stu-id="57bf4-109">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="57bf4-110">Binda din anpassade SSL-certifikat tooApp Service</span><span class="sxs-lookup"><span data-stu-id="57bf4-110">Bind your custom SSL certificate tooApp Service</span></span>
> * <span data-ttu-id="57bf4-111">Använd HTTPS för din app</span><span class="sxs-lookup"><span data-stu-id="57bf4-111">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="57bf4-112">Automatisera SSL-certifikat-bindning med skript</span><span class="sxs-lookup"><span data-stu-id="57bf4-112">Automate SSL certificate binding with scripts</span></span>

> [!NOTE]
> <span data-ttu-id="57bf4-113">Om du behöver tooget ett anpassat SSL-certifikat kan du skaffa en i hello Azure-portalen direkt och binda tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-113">If you need tooget a custom SSL certificate, you can get one in hello Azure portal directly and bind it tooyour web app.</span></span> <span data-ttu-id="57bf4-114">Följ hello [Apptjänstcertifikat kursen](web-sites-purchase-ssl-web-site.md).</span><span class="sxs-lookup"><span data-stu-id="57bf4-114">Follow hello [App Service Certificates tutorial](web-sites-purchase-ssl-web-site.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57bf4-115">Krav</span><span class="sxs-lookup"><span data-stu-id="57bf4-115">Prerequisites</span></span>

<span data-ttu-id="57bf4-116">toocomplete den här kursen:</span><span class="sxs-lookup"><span data-stu-id="57bf4-116">toocomplete this tutorial:</span></span>

- [<span data-ttu-id="57bf4-117">Skapa en Apptjänst-app</span><span class="sxs-lookup"><span data-stu-id="57bf4-117">Create an App Service app</span></span>](/azure/app-service/)
- [<span data-ttu-id="57bf4-118">Mappa en anpassad DNS-namnet tooyour webbapp</span><span class="sxs-lookup"><span data-stu-id="57bf4-118">Map a custom DNS name tooyour web app</span></span>](app-service-web-tutorial-custom-domain.md)
- <span data-ttu-id="57bf4-119">Skaffa ett SSL-certifikat från en betrodd certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="57bf4-119">Acquire an SSL certificate from a trusted certificate authority</span></span>

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a><span data-ttu-id="57bf4-120">Krav för SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="57bf4-120">Requirements for your SSL certificate</span></span>

<span data-ttu-id="57bf4-121">toouse ett certifikat i App Service hello certifikat måste uppfylla alla hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="57bf4-121">toouse a certificate in App Service, hello certificate must meet all hello following requirements:</span></span>

* <span data-ttu-id="57bf4-122">Signerats av en betrodd certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="57bf4-122">Signed by a trusted certificate authority</span></span>
* <span data-ttu-id="57bf4-123">Exporteras som en lösenordsskyddad PFX-fil</span><span class="sxs-lookup"><span data-stu-id="57bf4-123">Exported as a password-protected PFX file</span></span>
* <span data-ttu-id="57bf4-124">Innehåller privat nyckel minst 2048 bitar långt</span><span class="sxs-lookup"><span data-stu-id="57bf4-124">Contains private key at least 2048 bits long</span></span>
* <span data-ttu-id="57bf4-125">Innehåller alla mellanliggande certifikat i certifikatkedjan hello</span><span class="sxs-lookup"><span data-stu-id="57bf4-125">Contains all intermediate certificates in hello certificate chain</span></span>

> [!NOTE]
> <span data-ttu-id="57bf4-126">**Elliptic Curve Cryptography (ECC) certifikat** kan arbeta med App Service men omfattas inte av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="57bf4-126">**Elliptic Curve Cryptography (ECC) certificates** can work with App Service but are not covered by this article.</span></span> <span data-ttu-id="57bf4-127">Arbeta med din certifikatutfärdare på hello hur toocreate ECC-certifikat.</span><span class="sxs-lookup"><span data-stu-id="57bf4-127">Work with your certificate authority on hello exact steps toocreate ECC certificates.</span></span>

## <a name="prepare-your-web-app"></a><span data-ttu-id="57bf4-128">Förbered ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="57bf4-128">Prepare your web app</span></span>

<span data-ttu-id="57bf4-129">toobind anpassat SSL-certifikatet tooyour webbapp din [programtjänstplanen](https://azure.microsoft.com/pricing/details/app-service/) måste vara i hello **grundläggande**, **Standard**, eller **Premium** nivå.</span><span class="sxs-lookup"><span data-stu-id="57bf4-129">toobind a custom SSL certificate tooyour web app, your [App Service plan](https://azure.microsoft.com/pricing/details/app-service/) must be in hello **Basic**, **Standard**, or **Premium** tier.</span></span> <span data-ttu-id="57bf4-130">I det här steget ska kontrollera du att ditt webbprogram i hello stöds prisnivån.</span><span class="sxs-lookup"><span data-stu-id="57bf4-130">In this step, you make sure that your web app is in hello supported pricing tier.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="57bf4-131">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="57bf4-131">Log in tooAzure</span></span>

<span data-ttu-id="57bf4-132">Öppna hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="57bf4-132">Open hello [Azure portal](https://portal.azure.com).</span></span>

### <a name="navigate-tooyour-web-app"></a><span data-ttu-id="57bf4-133">Navigera tooyour webbprogram</span><span class="sxs-lookup"><span data-stu-id="57bf4-133">Navigate tooyour web app</span></span>

<span data-ttu-id="57bf4-134">Hello vänstra menyn klickar du på **Apptjänster**, och klicka sedan på hello namnet på ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-134">From hello left menu, click **App Services**, and then click hello name of your web app.</span></span>

![Välj webbprogram](./media/app-service-web-tutorial-custom-ssl/select-app.png)

<span data-ttu-id="57bf4-136">Du har landat i hello sidan för hantering av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-136">You have landed in hello management page of your web app.</span></span>  

### <a name="check-hello-pricing-tier"></a><span data-ttu-id="57bf4-137">Kontrollera hello prisnivån</span><span class="sxs-lookup"><span data-stu-id="57bf4-137">Check hello pricing tier</span></span>

<span data-ttu-id="57bf4-138">Bläddra i hello vänstra navigeringsfönstret på webbsidan app toohello **inställningar** avsnittet och väljer **skala upp (apptjänstplan)**.</span><span class="sxs-lookup"><span data-stu-id="57bf4-138">In hello left-hand navigation of your web app page, scroll toohello **Settings** section and select **Scale up (App Service plan)**.</span></span>

![Skala-menyn](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

<span data-ttu-id="57bf4-140">Kontrollera toomake till att ditt webbprogram inte hello **lediga** eller **delade** nivå.</span><span class="sxs-lookup"><span data-stu-id="57bf4-140">Check toomake sure that your web app is not in hello **Free** or **Shared** tier.</span></span> <span data-ttu-id="57bf4-141">Ditt webbprogram aktuell nivå markeras med en mörk ruta.</span><span class="sxs-lookup"><span data-stu-id="57bf4-141">Your web app's current tier is highlighted by a dark blue box.</span></span>

![Kontrollera prisnivå](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

<span data-ttu-id="57bf4-143">Anpassat SSL stöds inte i hello **lediga** eller **delade** nivå.</span><span class="sxs-lookup"><span data-stu-id="57bf4-143">Custom SSL is not supported in hello **Free** or **Shared** tier.</span></span> <span data-ttu-id="57bf4-144">Om du behöver tooscale upp åtgärderna hello i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="57bf4-144">If you need tooscale up, follow hello steps in hello next section.</span></span> <span data-ttu-id="57bf4-145">I annat fall stänger hello **Välj din prisnivå** sidan och hoppa över för[överför och binda SSL-certifikat](#upload).</span><span class="sxs-lookup"><span data-stu-id="57bf4-145">Otherwise, close hello **Choose your pricing tier** page and skip too[Upload and bind your SSL certificate](#upload).</span></span>

### <a name="scale-up-your-app-service-plan"></a><span data-ttu-id="57bf4-146">Skala upp din programtjänstplan</span><span class="sxs-lookup"><span data-stu-id="57bf4-146">Scale up your App Service plan</span></span>

<span data-ttu-id="57bf4-147">Välj en av hello **grundläggande**, **Standard**, eller **Premium** nivåer.</span><span class="sxs-lookup"><span data-stu-id="57bf4-147">Select one of hello **Basic**, **Standard**, or **Premium** tiers.</span></span>

<span data-ttu-id="57bf4-148">Klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="57bf4-148">Click **Select**.</span></span>

![Välj prisnivå](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

<span data-ttu-id="57bf4-150">När du ser följande meddelande hello har hello skalningsåtgärden slutförts.</span><span class="sxs-lookup"><span data-stu-id="57bf4-150">When you see hello following notification, hello scale operation is complete.</span></span>

![Skala upp meddelande](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a><span data-ttu-id="57bf4-152">Binda SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="57bf4-152">Bind your SSL certificate</span></span>

<span data-ttu-id="57bf4-153">Du är klar tooupload ditt webbprogram för SSL-certifikat tooyour.</span><span class="sxs-lookup"><span data-stu-id="57bf4-153">You are ready tooupload your SSL certificate tooyour web app.</span></span>

### <a name="merge-intermediate-certificates"></a><span data-ttu-id="57bf4-154">Sammanfoga mellanliggande certifikat</span><span class="sxs-lookup"><span data-stu-id="57bf4-154">Merge intermediate certificates</span></span>

<span data-ttu-id="57bf4-155">Om din certifikatutfärdare ger flera certifikat i certifikatkedjan hello, behöver du toomerge hello certifikat i ordning.</span><span class="sxs-lookup"><span data-stu-id="57bf4-155">If your certificate authority gives you multiple certificates in hello certificate chain, you need toomerge hello certificates in order.</span></span> 

<span data-ttu-id="57bf4-156">toodo detta, öppna varje certifikat du tog emot i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="57bf4-156">toodo this, open each certificate you received in a text editor.</span></span> 

<span data-ttu-id="57bf4-157">Skapa en fil för hello kopplade certifikat, kallat _mergedcertificate.crt_.</span><span class="sxs-lookup"><span data-stu-id="57bf4-157">Create a file for hello merged certificate, called _mergedcertificate.crt_.</span></span> <span data-ttu-id="57bf4-158">Kopiera hello innehållet i varje certifikat till den här filen i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="57bf4-158">In a text editor, copy hello content of each certificate into this file.</span></span> <span data-ttu-id="57bf4-159">hello ordning certifikat bör se ut som följande mall hello:</span><span class="sxs-lookup"><span data-stu-id="57bf4-159">hello order of your certificates should look like hello following template:</span></span>

```
-----BEGIN CERTIFICATE-----
<your Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-toopfx"></a><span data-ttu-id="57bf4-160">Exportera certifikatet tooPFX</span><span class="sxs-lookup"><span data-stu-id="57bf4-160">Export certificate tooPFX</span></span>

<span data-ttu-id="57bf4-161">Exportera sammanfogade SSL-certifikat med hello privat nyckel som genererades certifikatbegäran med.</span><span class="sxs-lookup"><span data-stu-id="57bf4-161">Export your merged SSL certificate with hello private key that your certificate request was generated with.</span></span>

<span data-ttu-id="57bf4-162">Om du genererade din certifikatbegäran med hjälp av OpenSSL har du skapat en fil för privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="57bf4-162">If you generated your certificate request using OpenSSL, then you have created a private key file.</span></span> <span data-ttu-id="57bf4-163">tooexport tooPFX ditt certifikat, kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="57bf4-163">tooexport your certificate tooPFX, run hello following command.</span></span> <span data-ttu-id="57bf4-164">Ersätt platshållarna hello  _&lt;fil för privat nyckel >_ och  _&lt;samman certifikatfil >_.</span><span class="sxs-lookup"><span data-stu-id="57bf4-164">Replace hello placeholders _&lt;private-key-file>_ and _&lt;merged-certificate-file>_.</span></span>

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

<span data-ttu-id="57bf4-165">När du uppmanas ange ett lösenord för export.</span><span class="sxs-lookup"><span data-stu-id="57bf4-165">When prompted, define an export password.</span></span> <span data-ttu-id="57bf4-166">När du överför dina SSL-certifikat tooApp tjänsten senare ska du använda det här lösenordet.</span><span class="sxs-lookup"><span data-stu-id="57bf4-166">You'll use this password when uploading your SSL certificate tooApp Service later.</span></span>

<span data-ttu-id="57bf4-167">Om du använder IIS eller _Certreq.exe_ toogenerate din certifikatbegäran, installera hello certifikat tooyour lokala dator, och sedan [exportera hello certifikat tooPFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span><span class="sxs-lookup"><span data-stu-id="57bf4-167">If you used IIS or _Certreq.exe_ toogenerate your certificate request, install hello certificate tooyour local machine, and then [export hello certificate tooPFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).</span></span>

### <a name="upload-your-ssl-certificate"></a><span data-ttu-id="57bf4-168">Överför SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="57bf4-168">Upload your SSL certificate</span></span>

<span data-ttu-id="57bf4-169">tooupload SSL-certifikat, klickar du på **SSL-certifikat** i hello vänster navigeringsfält av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-169">tooupload your SSL certificate, click **SSL certificates** in hello left navigation of your web app.</span></span>

<span data-ttu-id="57bf4-170">Klicka på **överför certifikat**.</span><span class="sxs-lookup"><span data-stu-id="57bf4-170">Click **Upload Certificate**.</span></span>

<span data-ttu-id="57bf4-171">I **PFX-certifikatsfilen**, Välj din PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="57bf4-171">In **PFX Certificate File**, select your PFX file.</span></span> <span data-ttu-id="57bf4-172">I **certifikatlösenord**, ange hello lösenord som du skapade när du exporterade hello PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="57bf4-172">In **Certificate password**, type hello password that you created when you exported hello PFX file.</span></span>

<span data-ttu-id="57bf4-173">Klicka på **Överför**.</span><span class="sxs-lookup"><span data-stu-id="57bf4-173">Click **Upload**.</span></span>

![Överför certifikat](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

<span data-ttu-id="57bf4-175">När Apptjänst har överförts certifikatet, visas den i hello **SSL-certifikat** sidan.</span><span class="sxs-lookup"><span data-stu-id="57bf4-175">When App Service finishes uploading your certificate, it appears in hello **SSL certificates** page.</span></span>

![Certifikat som har överförts](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a><span data-ttu-id="57bf4-177">Binda SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="57bf4-177">Bind your SSL certificate</span></span>

<span data-ttu-id="57bf4-178">I hello **SSL-bindningar** klickar du på **bindning**.</span><span class="sxs-lookup"><span data-stu-id="57bf4-178">In hello **SSL bindings** section, click **Add binding**.</span></span>

<span data-ttu-id="57bf4-179">I hello **lägger till SSL-bindningen** använder hello nedrullningsbara listorna tooselect hello domain name toosecure och hello certifikat toouse.</span><span class="sxs-lookup"><span data-stu-id="57bf4-179">In hello **Add SSL Binding** page, use hello dropdowns tooselect hello domain name toosecure, and hello certificate toouse.</span></span>

> [!NOTE]
> <span data-ttu-id="57bf4-180">Om du har överfört ett certifikat, men inte ser hello domänens namn i hello **värdnamn** listrutan Försök uppdatera hello Webbläsarsida.</span><span class="sxs-lookup"><span data-stu-id="57bf4-180">If you have uploaded your certificate but don't see hello domain name(s) in hello **Hostname** dropdown, try refreshing hello browser page.</span></span>
>
>

<span data-ttu-id="57bf4-181">I **SSL typen**, Välj om toouse  **[Server Servernamnsindikation (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  eller IP-baserade SSL.</span><span class="sxs-lookup"><span data-stu-id="57bf4-181">In **SSL Type**, select whether toouse **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP-based SSL.</span></span>

- <span data-ttu-id="57bf4-182">**SNI-baserade SSL** -flera SNI-baserade SSL-bindningar kan läggas till.</span><span class="sxs-lookup"><span data-stu-id="57bf4-182">**SNI-based SSL** - Multiple SNI-based SSL bindings may be added.</span></span> <span data-ttu-id="57bf4-183">Det här alternativet kan flera SSL-certifikat toosecure flera domäner på hello samma IP-adress.</span><span class="sxs-lookup"><span data-stu-id="57bf4-183">This option allows multiple SSL certificates toosecure multiple domains on hello same IP address.</span></span> <span data-ttu-id="57bf4-184">De flesta moderna webbläsare (inklusive Internet Explorer, Chrome, Firefox och Opera) stöder SNI (supportinformation finns mer omfattande webbläsare på [Servernamnindikator](http://wikipedia.org/wiki/Server_Name_Indication)).</span><span class="sxs-lookup"><span data-stu-id="57bf4-184">Most modern browsers (including Internet Explorer, Chrome, Firefox, and Opera) support SNI (find more comprehensive browser support information at [Server Name Indication](http://wikipedia.org/wiki/Server_Name_Indication)).</span></span>
- <span data-ttu-id="57bf4-185">**IP-baserade SSL** – en enda IP-baserade SSL-bindning kan läggas till.</span><span class="sxs-lookup"><span data-stu-id="57bf4-185">**IP-based SSL** - Only one IP-based SSL binding may be added.</span></span> <span data-ttu-id="57bf4-186">Det här alternativet kan endast en SSL-certifikat toosecure dedikerade offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="57bf4-186">This option allows only one SSL certificate toosecure a dedicated public IP address.</span></span> <span data-ttu-id="57bf4-187">toosecure flera domäner, du måste skydda dem med alla hello samma SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="57bf4-187">toosecure multiple domains, you must secure them all using hello same SSL certificate.</span></span> <span data-ttu-id="57bf4-188">Detta är hello traditionella alternativ för SSL-bindning.</span><span class="sxs-lookup"><span data-stu-id="57bf4-188">This is hello traditional option for SSL binding.</span></span>

<span data-ttu-id="57bf4-189">Klicka på **bindning**.</span><span class="sxs-lookup"><span data-stu-id="57bf4-189">Click **Add Binding**.</span></span>

![Binda SSL-certifikat](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

<span data-ttu-id="57bf4-191">När Apptjänst har överförts certifikatet, visas den i hello **SSL-bindningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="57bf4-191">When App Service finishes uploading your certificate, it appears in hello **SSL bindings** sections.</span></span>

![Certifikatet bundet tooweb app](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a><span data-ttu-id="57bf4-193">Mappa om en post för IP-SSL</span><span class="sxs-lookup"><span data-stu-id="57bf4-193">Remap A record for IP SSL</span></span>

<span data-ttu-id="57bf4-194">Om du inte använder IP-baserade SSL i ditt webbprogram, hoppar du över för[Test HTTPS för den anpassade domänen](#test).</span><span class="sxs-lookup"><span data-stu-id="57bf4-194">If you don't use IP-based SSL in your web app, skip too[Test HTTPS for your custom domain](#test).</span></span>

<span data-ttu-id="57bf4-195">Som standard används ditt webbprogram delad offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="57bf4-195">By default, your web app uses a shared public IP address.</span></span> <span data-ttu-id="57bf4-196">När du binda ett certifikat med IP-baserade SSL skapar App Service en ny, dedicerade IP-adress för din webbapp.</span><span class="sxs-lookup"><span data-stu-id="57bf4-196">When you bind a certificate with IP-based SSL, App Service creates a new, dedicated IP address for your web app.</span></span>

<span data-ttu-id="57bf4-197">Om du har mappat en A-poster tooyour webbapp, Uppdatera registret domän med den här nya, dedikerade IP-adress.</span><span class="sxs-lookup"><span data-stu-id="57bf4-197">If you have mapped an A record tooyour web app, update your domain registry with this new, dedicated IP address.</span></span>

<span data-ttu-id="57bf4-198">Ditt webbprogram **anpassad domän** sidan uppdateras med hello nya, dedikerade IP-adress.</span><span class="sxs-lookup"><span data-stu-id="57bf4-198">Your web app's **Custom domain** page is updated with hello new, dedicated IP address.</span></span> <span data-ttu-id="57bf4-199">[Kopiera den här IP-adressen](app-service-web-tutorial-custom-domain.md#info), sedan [mappa om hello en post](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis nya IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="57bf4-199">[Copy this IP address](app-service-web-tutorial-custom-domain.md#info), then [remap hello A record](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis new IP address.</span></span>

<a name="test"></a>

## <a name="test-https"></a><span data-ttu-id="57bf4-200">Testa HTTPS</span><span class="sxs-lookup"><span data-stu-id="57bf4-200">Test HTTPS</span></span>

<span data-ttu-id="57bf4-201">Alla som har lämnat toodo nu är toomake att HTTPS fungerar för domänen.</span><span class="sxs-lookup"><span data-stu-id="57bf4-201">All that's left toodo now is toomake sure that HTTPS works for your custom domain.</span></span> <span data-ttu-id="57bf4-202">Olika webbläsare bläddrar för`https://<your.custom.domain>` toosee som den hanterar upp ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-202">In various browsers, browse too`https://<your.custom.domain>` toosee that it serves up your web app.</span></span>

![Portalen navigering tooAzure app](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> <span data-ttu-id="57bf4-204">Om ditt webbprogram ger du certifikat verifieringsfel, använder du förmodligen ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="57bf4-204">If your web app gives you certificate validation errors, you're probably using a self-signed certificate.</span></span>
>
> <span data-ttu-id="57bf4-205">Om detta inte är fallet hello har du lämnat ut mellanliggande certifikat när du exporterar certifikatet toohello PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="57bf4-205">If that's not hello case, you may have left out intermediate certificates when you export your certificate toohello PFX file.</span></span>

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a><span data-ttu-id="57bf4-206">Använda HTTPS</span><span class="sxs-lookup"><span data-stu-id="57bf4-206">Enforce HTTPS</span></span>

<span data-ttu-id="57bf4-207">Apptjänst har *inte* genomdriva HTTPS, så att alla kan fortfarande komma åt ditt webbprogram med hjälp av HTTP.</span><span class="sxs-lookup"><span data-stu-id="57bf4-207">App Service does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="57bf4-208">tooenforce HTTPS för ditt webbprogram, definiera en regel för omarbetning i hello _web.config_ -filen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-208">tooenforce HTTPS for your web app, define a rewrite rule in hello _web.config_ file for your web app.</span></span> <span data-ttu-id="57bf4-209">Apptjänst använder den här filen, oavsett hello språkramverket av ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-209">App Service uses this file, regardless of hello language framework of your web app.</span></span>

> [!NOTE]
> <span data-ttu-id="57bf4-210">Det finns språkspecifika omdirigering av begäranden.</span><span class="sxs-lookup"><span data-stu-id="57bf4-210">There is language-specific redirection of requests.</span></span> <span data-ttu-id="57bf4-211">ASP.NET MVC kan använda hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter i stället för hello omarbetning regeln i _web.config_.</span><span class="sxs-lookup"><span data-stu-id="57bf4-211">ASP.NET MVC can use hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter instead of hello rewrite rule in _web.config_.</span></span>

<span data-ttu-id="57bf4-212">Om du är en .NET-utvecklare kan vara du relativt bekant med den här filen.</span><span class="sxs-lookup"><span data-stu-id="57bf4-212">If you're a .NET developer, you should be relatively familiar with this file.</span></span> <span data-ttu-id="57bf4-213">Det är i hello rot i lösningen.</span><span class="sxs-lookup"><span data-stu-id="57bf4-213">It is in hello root of your solution.</span></span>

<span data-ttu-id="57bf4-214">Alternativt, om du utvecklar med PHP, Node.js, Python eller Java, ökar risken skapade vi den här filen för din räkning i App Service.</span><span class="sxs-lookup"><span data-stu-id="57bf4-214">Alternatively, if you develop with PHP, Node.js, Python, or Java, there is a chance we generated this file on your behalf in App Service.</span></span>

<span data-ttu-id="57bf4-215">Ansluta tooyour webbapp FTP-slutpunkten genom att följa anvisningarna hello på [distribuera din app tooAzure App Service med FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="57bf4-215">Connect tooyour web app's FTP endpoint by following hello instructions at [Deploy your app tooAzure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="57bf4-216">Den här filen måste finnas i _/home/site/wwwroot_.</span><span class="sxs-lookup"><span data-stu-id="57bf4-216">This file should be located in _/home/site/wwwroot_.</span></span> <span data-ttu-id="57bf4-217">Om inte, skapar du en _web.config_ filen i mappen med följande XML hello:</span><span class="sxs-lookup"><span data-stu-id="57bf4-217">If not, create a _web.config_ file in this folder with hello following XML:</span></span>

```xml   
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- BEGIN rule ELEMENT FOR HTTPS REDIRECT -->
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
        <!-- END rule ELEMENT FOR HTTPS REDIRECT -->
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

<span data-ttu-id="57bf4-218">För en befintlig _web.config_ fil, kopiera hello hela `<rule>` element i din _web.config_'s `configuration/system.webServer/rewrite/rules` element.</span><span class="sxs-lookup"><span data-stu-id="57bf4-218">For an existing _web.config_ file, copy hello entire `<rule>` element into your _web.config_'s `configuration/system.webServer/rewrite/rules` element.</span></span> <span data-ttu-id="57bf4-219">Om det finns andra `<rule>` element i din _web.config_, plats hello kopieras `<rule>` elementet innan hello andra `<rule>` element.</span><span class="sxs-lookup"><span data-stu-id="57bf4-219">If there are other `<rule>` elements in your _web.config_, place hello copied `<rule>` element before hello other `<rule>` elements.</span></span>

<span data-ttu-id="57bf4-220">Den här regeln returnerar en HTTP 301 (permanent omdirigering) toohello HTTPS-protokollet när hello användaren gör en HTTP-begäran tooyour webbapp.</span><span class="sxs-lookup"><span data-stu-id="57bf4-220">This rule returns an HTTP 301 (permanent redirect) toohello HTTPS protocol whenever hello user makes an HTTP request tooyour web app.</span></span> <span data-ttu-id="57bf4-221">Till exempel Serverproxyn från `http://contoso.com` för`https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="57bf4-221">For example, it redirects from `http://contoso.com` too`https://contoso.com`.</span></span>

<span data-ttu-id="57bf4-222">Mer information om hello-modulen för omarbetning av IIS-URL finns hello [URL-omskrivning om](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentation.</span><span class="sxs-lookup"><span data-stu-id="57bf4-222">For more information on hello IIS URL Rewrite module, see hello [URL Rewrite](http://www.iis.net/downloads/microsoft/url-rewrite) documentation.</span></span>

## <a name="enforce-https-for-web-apps-on-linux"></a><span data-ttu-id="57bf4-223">Använd HTTPS för webbprogram på Linux</span><span class="sxs-lookup"><span data-stu-id="57bf4-223">Enforce HTTPS for Web Apps on Linux</span></span>

<span data-ttu-id="57bf4-224">App-tjänsten på Linux *inte* genomdriva HTTPS, så att alla kan fortfarande komma åt ditt webbprogram med hjälp av HTTP.</span><span class="sxs-lookup"><span data-stu-id="57bf4-224">App Service on Linux does *not* enforce HTTPS, so anyone can still access your web app using HTTP.</span></span> <span data-ttu-id="57bf4-225">tooenforce HTTPS för ditt webbprogram, definiera en regel för omarbetning i hello _.htaccess_ -filen för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="57bf4-225">tooenforce HTTPS for your web app, define a rewrite rule in hello _.htaccess_ file for your web app.</span></span> 

<span data-ttu-id="57bf4-226">Ansluta tooyour webbapp FTP-slutpunkten genom att följa anvisningarna hello på [distribuera din app tooAzure App Service med FTP/S](app-service-deploy-ftp.md).</span><span class="sxs-lookup"><span data-stu-id="57bf4-226">Connect tooyour web app's FTP endpoint by following hello instructions at [Deploy your app tooAzure App Service using FTP/S](app-service-deploy-ftp.md).</span></span>

<span data-ttu-id="57bf4-227">I _/home/site/wwwroot_, skapa en _.htaccess_ fil med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="57bf4-227">In _/home/site/wwwroot_, create an _.htaccess_ file with hello following code:</span></span>

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

<span data-ttu-id="57bf4-228">Den här regeln returnerar en HTTP 301 (permanent omdirigering) toohello HTTPS-protokollet när hello användaren gör en HTTP-begäran tooyour webbapp.</span><span class="sxs-lookup"><span data-stu-id="57bf4-228">This rule returns an HTTP 301 (permanent redirect) toohello HTTPS protocol whenever hello user makes an HTTP request tooyour web app.</span></span> <span data-ttu-id="57bf4-229">Till exempel Serverproxyn från `http://contoso.com` för`https://contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="57bf4-229">For example, it redirects from `http://contoso.com` too`https://contoso.com`.</span></span>

## <a name="automate-with-scripts"></a><span data-ttu-id="57bf4-230">Automatisera med skript</span><span class="sxs-lookup"><span data-stu-id="57bf4-230">Automate with scripts</span></span>

<span data-ttu-id="57bf4-231">Du kan automatisera SSL-bindningar för ditt webbprogram med skript, med hjälp av hello [Azure CLI](/cli/azure/install-azure-cli) eller [Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="57bf4-231">You can automate SSL bindings for your web app with scripts, using hello [Azure CLI](/cli/azure/install-azure-cli) or [Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="azure-cli"></a><span data-ttu-id="57bf4-232">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="57bf4-232">Azure CLI</span></span>

<span data-ttu-id="57bf4-233">följande kommando hello överför exporterade PFX-filen och hämtar hello tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="57bf4-233">hello following command uploads an exported PFX file and gets hello thumbprint.</span></span>

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

<span data-ttu-id="57bf4-234">hello följande kommando lägger till en SNI-baserade SSL-bindning med hello tumavtryck från hello föregående kommando.</span><span class="sxs-lookup"><span data-stu-id="57bf4-234">hello following command adds an SNI-based SSL binding, using hello thumbprint from hello previous command.</span></span>

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a><span data-ttu-id="57bf4-235">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="57bf4-235">Azure PowerShell</span></span>

<span data-ttu-id="57bf4-236">hello följande kommando överför exporterade PFX-filen och lägger till en SNI-baserade SSL-bindning.</span><span class="sxs-lookup"><span data-stu-id="57bf4-236">hello following command uploads an exported PFX file and adds an SNI-based SSL binding.</span></span>

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a><span data-ttu-id="57bf4-237">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57bf4-237">Next steps</span></span>

<span data-ttu-id="57bf4-238">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="57bf4-238">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="57bf4-239">Uppgradera prisnivån för din app</span><span class="sxs-lookup"><span data-stu-id="57bf4-239">Upgrade your app's pricing tier</span></span>
> * <span data-ttu-id="57bf4-240">Binda din anpassade SSL-certifikat tooApp Service</span><span class="sxs-lookup"><span data-stu-id="57bf4-240">Bind your custom SSL certificate tooApp Service</span></span>
> * <span data-ttu-id="57bf4-241">Använd HTTPS för din app</span><span class="sxs-lookup"><span data-stu-id="57bf4-241">Enforce HTTPS for your app</span></span>
> * <span data-ttu-id="57bf4-242">Automatisera SSL-certifikat-bindning med skript</span><span class="sxs-lookup"><span data-stu-id="57bf4-242">Automate SSL certificate binding with scripts</span></span>

<span data-ttu-id="57bf4-243">I förväg toohello nästa självstudiekurs toolearn hur toouse Azure Content Delivery Network.</span><span class="sxs-lookup"><span data-stu-id="57bf4-243">Advance toohello next tutorial toolearn how toouse Azure Content Delivery Network.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="57bf4-244">Lägg till en innehåll innehållsleveransnätverk (CDN) tooan Azure App Service</span><span class="sxs-lookup"><span data-stu-id="57bf4-244">Add a Content Delivery Network (CDN) tooan Azure App Service</span></span>](app-service-web-tutorial-content-delivery-network.md)
