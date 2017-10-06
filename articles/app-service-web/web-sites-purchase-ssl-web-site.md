---
title: aaaAdd en SSL-certifikatet tooyour Azure App Service-appen | Microsoft Docs
description: "Lär dig hur tooadd en SSL-certifikatet tooyour Apptjänst-app."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.openlocfilehash: f4652794ba745790a073264f6a102c64c73e8db0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a><span data-ttu-id="a8e02-103">Köp och konfigurera ett SSL-certifikat för din Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="a8e02-103">Buy and Configure an SSL Certificate for your Azure App Service</span></span>

<span data-ttu-id="a8e02-104">I den här självstudiekursen kommer du skyddar ditt webbprogram genom att köpa ett SSL-certifikat för din  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, på ett säkert sätt lagra det i [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), och associera den med en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="a8e02-104">In this tutorial, you will secure your web app by purchasing an SSL certificate for your **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, securely storing it in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), and associating it with a custom domain.</span></span>

## <a name="step-1---log-in-tooazure"></a><span data-ttu-id="a8e02-105">Steg 1 – Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="a8e02-105">Step 1 - Log in tooAzure</span></span>

<span data-ttu-id="a8e02-106">Logga in toohello Azure-portalen på http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="a8e02-106">Log in toohello Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---place-an-ssl-certificate-order"></a><span data-ttu-id="a8e02-107">Steg 2 – beställning SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="a8e02-107">Step 2 - Place an SSL Certificate order</span></span>

<span data-ttu-id="a8e02-108">Du kan placera en order för SSL-certifikat genom att skapa en ny [App tjänstcertifikat](https://portal.azure.com/#create/Microsoft.SSL) i hello **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="a8e02-108">You can place an SSL Certificate order by creating a new [App Service Certificate](https://portal.azure.com/#create/Microsoft.SSL) In hello **Azure portal**.</span></span>

![Skapande av certifikat](./media/app-service-web-purchase-ssl-web-site/createssl.png)

<span data-ttu-id="a8e02-110">Ange ett eget **namn** för SSL-certifikatet och ange hello **domännamn**</span><span class="sxs-lookup"><span data-stu-id="a8e02-110">Enter a friendly **Name** for your SSL certificate and enter hello **Domain Name**</span></span>

> [!NOTE]
> <span data-ttu-id="a8e02-111">Detta är en hello viktigaste delarna i hello köpet.</span><span class="sxs-lookup"><span data-stu-id="a8e02-111">This is one of hello most critical parts of hello purchase process.</span></span> <span data-ttu-id="a8e02-112">Se till att tooenter korrigera värdnamn (anpassad domän) som du vill tooprotect med det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a8e02-112">Make sure tooenter correct host name (custom domain) that you want tooprotect with this certificate.</span></span> <span data-ttu-id="a8e02-113">**INTE** bifoga hello värdnamn med WWW.</span><span class="sxs-lookup"><span data-stu-id="a8e02-113">**DO NOT** append hello Host name with WWW.</span></span> 
>

<span data-ttu-id="a8e02-114">Välj din **prenumeration**, **resursgruppen**, och **certifikat SKU**</span><span class="sxs-lookup"><span data-stu-id="a8e02-114">Select your **Subscription**, **Resource Group**, and **Certificate SKU**</span></span>

> [!WARNING]
> <span data-ttu-id="a8e02-115">Apptjänstcertifikat kan endast användas av andra App-tjänster inom hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a8e02-115">App Service Certificates can only be used by other App Services within hello same subscription.</span></span>  
>

## <a name="step-3---store-hello-certificate-in-azure-key-vault"></a><span data-ttu-id="a8e02-116">Steg 3 – Store hello certifikat i Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a8e02-116">Step 3 - Store hello certificate in Azure Key Vault</span></span>

> [!NOTE]
> <span data-ttu-id="a8e02-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) är en Azure-tjänst som hjälper dig skydda krypteringsnycklar och hemligheter som används av molnprogram och tjänster.</span><span class="sxs-lookup"><span data-stu-id="a8e02-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) is an Azure service that helps safeguard cryptographic keys and secrets used by cloud applications and services.</span></span>
>

<span data-ttu-id="a8e02-118">När hello SSL-certifikat köpet har slutförts måste tooopen [Apptjänstcertifikat](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) resursbladet.</span><span class="sxs-lookup"><span data-stu-id="a8e02-118">Once hello SSL Certificate purchase is complete, you need tooopen [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) Resource blade.</span></span>

![Infoga en bild av redo toostore i KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

<span data-ttu-id="a8e02-120">Du kommer att märka att certifikatstatus är **”väntande utfärdande”** eftersom det inte finns några fler steg du måste toocomplete innan du kan börja använda det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a8e02-120">You will notice that Certificate status is **“Pending Issuance”** as there are few more steps you need toocomplete before you can start using this certificate.</span></span>

<span data-ttu-id="a8e02-121">Klicka på **Certifikatkonfigureringen** inuti certifikategenskaper bladet och klicka på **steg 1: Store** toostore det här certifikatet i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a8e02-121">Click **Certificate Configuration** inside Certificate Properties blade and Click on **Step 1: Store** toostore this certificate in Azure Key Vault.</span></span>

<span data-ttu-id="a8e02-122">Från **Key Vault Status** bladet, klickar du på **Key Vault databasen** toochoose en befintlig Key Vault-toostore certifikatet **eller skapa nya Key Vault** toocreate ny nyckel valvet inom samma prenumeration och resurs.</span><span class="sxs-lookup"><span data-stu-id="a8e02-122">From **Key Vault Status** Blade, click **Key Vault Repository** toochoose an existing Key Vault toostore this certificate **OR Create New Key Vault** toocreate new Key Vault inside same subscription and resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e02-123">Azure Key Vault har minimal kostnader för lagring av det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a8e02-123">Azure Key Vault has minimal charges for storing this certificate.</span></span>
> <span data-ttu-id="a8e02-124">Mer information finns i  **[prisinformation för Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.</span><span class="sxs-lookup"><span data-stu-id="a8e02-124">For more information, see **[Azure Key Vault Pricing Details](https://azure.microsoft.com/pricing/details/key-vault/)**.</span></span>
>

<span data-ttu-id="a8e02-125">När du har valt det här certifikatet i hello Key Vault databasen toostore, hello **Store** alternativet ska visa lyckades.</span><span class="sxs-lookup"><span data-stu-id="a8e02-125">Once you have selected hello Key Vault Repository toostore this certificate in, hello **Store** option should show success.</span></span>

![infoga bilden av store KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-hello-domain-ownership"></a><span data-ttu-id="a8e02-127">Steg 4 – Kontrollera hello ägarskap för domänen</span><span class="sxs-lookup"><span data-stu-id="a8e02-127">Step 4 - Verify hello Domain Ownership</span></span>

> [!NOTE]
> <span data-ttu-id="a8e02-128">Det finns tre typer av domänverifiering som stöds av Apptjänstcertifikat: verifiering av domän, e-post manuellt.</span><span class="sxs-lookup"><span data-stu-id="a8e02-128">There are three types of domain verification supported by App service Certificates: Domain, Mail, Manual Verification.</span></span> <span data-ttu-id="a8e02-129">Dessa beskrivs mer detaljerat i hello [avancerade avsnitt](#advanced).</span><span class="sxs-lookup"><span data-stu-id="a8e02-129">These are explained in more details in hello [Advanced section](#advanced).</span></span>

<span data-ttu-id="a8e02-130">Hej från samma **Certifikatkonfigureringen** bladet som du använde i steg3 klickar du på **steg 2: Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="a8e02-130">From hello same **Certificate Configuration** blade you used in Step 3, click **Step 2: Verify**.</span></span>

<span data-ttu-id="a8e02-131">**Verifiering av domän** det här är hello enklaste process **endast om** du har  **[köpt din anpassade domän från Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span><span class="sxs-lookup"><span data-stu-id="a8e02-131">**Domain Verification** This is hello most convenient process **ONLY IF** you have **[purchased your custom domain from Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span></span>
<span data-ttu-id="a8e02-132">Klicka på **Kontrollera** knappen toocomplete det här steget.</span><span class="sxs-lookup"><span data-stu-id="a8e02-132">Click on **Verify** button toocomplete this step.</span></span>

![infoga bilden för verifiering av domän](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

<span data-ttu-id="a8e02-134">När du klickar på **Kontrollera**, använda hello **uppdatera** knappen tills hello **Kontrollera** alternativet ska visa lyckades.</span><span class="sxs-lookup"><span data-stu-id="a8e02-134">After clicking **Verify**, use hello **Refresh** button until hello **Verify** option should show success.</span></span>

![Infoga bild av verifiera i KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-tooapp-service-app"></a><span data-ttu-id="a8e02-136">Steg 5 – tilldela certifikat tooApp Service-appen</span><span class="sxs-lookup"><span data-stu-id="a8e02-136">Step 5 - Assign Certificate tooApp Service App</span></span>

> [!NOTE]
> <span data-ttu-id="a8e02-137">Innan du utför hello stegen i det här avsnittet måste du har associerat ett anpassat domännamn med din app.</span><span class="sxs-lookup"><span data-stu-id="a8e02-137">Before performing hello steps in this section, you must have associated a custom domain name with your app.</span></span> <span data-ttu-id="a8e02-138">Mer information finns i  **[konfigurera ett anpassat domännamn för en webbapp.](app-service-web-tutorial-custom-domain.md)**</span><span class="sxs-lookup"><span data-stu-id="a8e02-138">For more information, see **[Configuring a custom domain name for a web app.](app-service-web-tutorial-custom-domain.md)**</span></span>
>

<span data-ttu-id="a8e02-139">I hello  **[Azure-portalen](https://portal.azure.com/)**, klicka på hello **Apptjänst** alternativet hello vänster hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="a8e02-139">In hello **[Azure portal](https://portal.azure.com/)**, click hello **App Service** option on hello left of hello page.</span></span>

<span data-ttu-id="a8e02-140">Hello klickar du på din app toowhich som du vill tooassign det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a8e02-140">Click hello name of your app toowhich you want tooassign this certificate.</span></span>

<span data-ttu-id="a8e02-141">I hello **inställningar**, klickar du på **SSL-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="a8e02-141">In hello **Settings**, click **SSL certificates**.</span></span>

<span data-ttu-id="a8e02-142">Klicka på **importera App Service certifikat** och välj hello-certifikat som du precis har köpt.</span><span class="sxs-lookup"><span data-stu-id="a8e02-142">Click **Import App Service Certificate** and select hello certificate that you just purchased.</span></span>

![Infoga bild av Importera certifikat](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

<span data-ttu-id="a8e02-144">I hello **ssl-bindningar** avsnittet Klicka på **lägga till bindningar**, och använda hello nedrullningsbara listorna tooselect hello domain name toosecure med SSL och hello certifikat toouse.</span><span class="sxs-lookup"><span data-stu-id="a8e02-144">In hello **ssl bindings** section Click on **Add bindings**, and use hello dropdowns tooselect hello domain name toosecure with SSL, and hello certificate toouse.</span></span> <span data-ttu-id="a8e02-145">Du kan också välja om toouse  **[Server Servernamnsindikation (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  eller IP-baserade SSL.</span><span class="sxs-lookup"><span data-stu-id="a8e02-145">You may also select whether toouse **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP based SSL.</span></span>

![infoga bilden av SSL-bindningar](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

<span data-ttu-id="a8e02-147">Klicka på **Lägg till bindning för** toosave hello ändringar och aktivera SSL.</span><span class="sxs-lookup"><span data-stu-id="a8e02-147">Click **Add Binding** toosave hello changes and enable SSL.</span></span>

> [!NOTE]
> <span data-ttu-id="a8e02-148">Om du har valt **IP-baserade SSL** och domänen är konfigurerad med en A-post, måste du utföra följande ytterligare hello.</span><span class="sxs-lookup"><span data-stu-id="a8e02-148">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform hello following additional steps.</span></span> <span data-ttu-id="a8e02-149">Dessa beskrivs mer detaljerat i hello [avancerade avsnitt](#Advanced).</span><span class="sxs-lookup"><span data-stu-id="a8e02-149">These are explained in more details in hello [Advanced section](#Advanced).</span></span>

<span data-ttu-id="a8e02-150">Nu är du ska kunna toovisit din app med hjälp av `HTTPS://` i stället för `HTTP://` tooverify som hello certifikat har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="a8e02-150">At this point, you should be able toovisit your app using `HTTPS://` instead of `HTTP://` tooverify that hello certificate has been configured correctly.</span></span>

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a><span data-ttu-id="a8e02-151">Steg 6 - administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="a8e02-151">Step 6 - Management tasks</span></span>

### <a name="azure-cli"></a><span data-ttu-id="a8e02-152">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a8e02-152">Azure CLI</span></span>

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a><span data-ttu-id="a8e02-153">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8e02-153">PowerShell</span></span>

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a><span data-ttu-id="a8e02-154">Advanced</span><span class="sxs-lookup"><span data-stu-id="a8e02-154">Advanced</span></span>

### <a name="verifying-domain-ownership"></a><span data-ttu-id="a8e02-155">Verifiera domänen ägarskap</span><span class="sxs-lookup"><span data-stu-id="a8e02-155">Verifying Domain Ownership</span></span>

<span data-ttu-id="a8e02-156">Det finns två flera typer av domänverifiering som stöds av Apptjänstcertifikat: e-post och manuell kontroll.</span><span class="sxs-lookup"><span data-stu-id="a8e02-156">There are two more types of domain verification supported by App service Certificates: Mail, and Manual Verification.</span></span>

#### <a name="mail-verification"></a><span data-ttu-id="a8e02-157">Verifiering av e-post</span><span class="sxs-lookup"><span data-stu-id="a8e02-157">Mail Verification</span></span>

<span data-ttu-id="a8e02-158">E-postmeddelandet har redan skickats toohello e-adresser som är associerade med den här domänen.</span><span class="sxs-lookup"><span data-stu-id="a8e02-158">Verification email has already been sent toohello Email Address(es) associated with this custom domain.</span></span>
<span data-ttu-id="a8e02-159">Verifieringssteg toocomplete hello e-post, öppna hello e-postmeddelande och klicka på hello verifieringslänken.</span><span class="sxs-lookup"><span data-stu-id="a8e02-159">toocomplete hello Email verification step, open hello email and click hello verification link.</span></span>

![Infoga bild av e-Postverifiering](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

<span data-ttu-id="a8e02-161">Om du behöver tooresend hello e-postmeddelandet klickar du på hello **skicka e-post** knappen.</span><span class="sxs-lookup"><span data-stu-id="a8e02-161">If you need tooresend hello verification email, click hello **Resend Email** button.</span></span>

#### <a name="manual-verification"></a><span data-ttu-id="a8e02-162">Manuell kontroll</span><span class="sxs-lookup"><span data-stu-id="a8e02-162">Manual Verification</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8e02-163">HTML-webbsida verifiering (fungerar bara med certifikat SKU: N)</span><span class="sxs-lookup"><span data-stu-id="a8e02-163">HTML Web Page Verification (only works with Standard Certificate SKU)</span></span>
>

1. <span data-ttu-id="a8e02-164">Skapa en HTML-fil med namnet **”starfield.html”**</span><span class="sxs-lookup"><span data-stu-id="a8e02-164">Create an HTML file named **"starfield.html"**</span></span>

1. <span data-ttu-id="a8e02-165">Innehållet för den här filen ska vara hello exakta namnet på domänen verifiering Token hello.</span><span class="sxs-lookup"><span data-stu-id="a8e02-165">Content of this file should be hello exact name of hello Domain Verification Token.</span></span> <span data-ttu-id="a8e02-166">(Du kan kopiera hello-token från hello domän verifiering Status bladet)</span><span class="sxs-lookup"><span data-stu-id="a8e02-166">(You can copy hello token from hello Domain Verification Status Blade)</span></span>

1. <span data-ttu-id="a8e02-167">Överföra filen hello roten i hello webbservern som värd för din domän`/.well-known/pki-validation/starfield.html`</span><span class="sxs-lookup"><span data-stu-id="a8e02-167">Upload this file at hello root of hello web server hosting your domain `/.well-known/pki-validation/starfield.html`</span></span>

1. <span data-ttu-id="a8e02-168">Klicka på **uppdatera** tooupdate hello certifikatstatus när verifieringen är klar.</span><span class="sxs-lookup"><span data-stu-id="a8e02-168">Click **Refresh** tooupdate hello certificate status after verification is completed.</span></span> <span data-ttu-id="a8e02-169">Det kan ta några minuter för verifiering toocomplete.</span><span class="sxs-lookup"><span data-stu-id="a8e02-169">It might take few minutes for verification toocomplete.</span></span>

> [!TIP]
> <span data-ttu-id="a8e02-170">Kontrollera i en terminal `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello svaret ska innehålla hello `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="a8e02-170">Verify in a terminal using `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello response should contain hello `<verification-token>`.</span></span>

#### <a name="dns-txt-record-verification"></a><span data-ttu-id="a8e02-171">Verifiering av DNS TXT-post</span><span class="sxs-lookup"><span data-stu-id="a8e02-171">DNS TXT Record Verification</span></span>

1. <span data-ttu-id="a8e02-172">Med hjälp av DNS-hanteraren, skapa en TXT-post på hello `@` underdomänen med värde lika toohello domän verifiering Token.</span><span class="sxs-lookup"><span data-stu-id="a8e02-172">Using your DNS manager, Create a TXT record on hello `@` subdomain with value equal toohello Domain Verification Token.</span></span>
1. <span data-ttu-id="a8e02-173">Klicka på **”uppdatera”** tooupdate hello certifikatstatus när verifieringen är klar.</span><span class="sxs-lookup"><span data-stu-id="a8e02-173">Click **“Refresh”** tooupdate hello Certificate status after verification is completed.</span></span>

> [!TIP]
> <span data-ttu-id="a8e02-174">Du behöver toocreate en TXT-post på `@.<domain>` med värdet `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="a8e02-174">You need toocreate a TXT record on `@.<domain>` with value `<verification-token>`.</span></span>

### <a name="assign-certificate-tooapp-service-app"></a><span data-ttu-id="a8e02-175">Tilldela certifikat tooApp Service-appen</span><span class="sxs-lookup"><span data-stu-id="a8e02-175">Assign Certificate tooApp Service App</span></span>

<span data-ttu-id="a8e02-176">Om du har valt **IP-baserade SSL** och domänen är konfigurerad med en A-post, måste du utföra följande ytterligare hello:</span><span class="sxs-lookup"><span data-stu-id="a8e02-176">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform hello following additional steps:</span></span>

<span data-ttu-id="a8e02-177">När du har konfigurerat en IP-baserade SSL-bindning, en dedicerad IP-adress tilldelas tooyour app.</span><span class="sxs-lookup"><span data-stu-id="a8e02-177">After you have configured an IP based SSL binding, a dedicated IP address is assigned tooyour app.</span></span> <span data-ttu-id="a8e02-178">Du hittar den här IP-adressen på hello **anpassad domän** sidan under inställningar för din app, precis ovanför hello **värdnamn** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a8e02-178">You can find this IP address on hello **Custom domain** page under settings of your app, right above hello **Hostnames** section.</span></span> <span data-ttu-id="a8e02-179">Den visas som **extern IP-adress**</span><span class="sxs-lookup"><span data-stu-id="a8e02-179">It is listed as **External IP Address**</span></span>

![infoga bilden av IP-SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

<span data-ttu-id="a8e02-181">Observera att denna IP-adress skiljer sig från hello virtuella IP-adressen används tidigare tooconfigure hello A-posten för din domän.</span><span class="sxs-lookup"><span data-stu-id="a8e02-181">Note that this IP address is different than hello virtual IP address used previously tooconfigure hello A record for your domain.</span></span> <span data-ttu-id="a8e02-182">Om du konfigurerat toouse SNI baserad SSL, eller är inte konfigurerad toouse SSL, ingen adress visas för den här posten.</span><span class="sxs-lookup"><span data-stu-id="a8e02-182">If you are configured toouse SNI based SSL, or are not configured toouse SSL, no address is listed for this entry.</span></span>

<span data-ttu-id="a8e02-183">Använda hello verktyg som tillhandahålls av din domännamnsregistrator kan ändra hello en post för din domän namn toopoint toohello IP-adress från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="a8e02-183">Using hello tools provided by your domain name registrar, modify hello A record for your custom domain name toopoint toohello IP address from hello previous step.</span></span>

## <a name="rekey-and-sync-hello-certificate"></a><span data-ttu-id="a8e02-184">Genererar nya nycklar och synkronisera hello certifikat</span><span class="sxs-lookup"><span data-stu-id="a8e02-184">Rekey and Sync hello Certificate</span></span>

<span data-ttu-id="a8e02-185">Om du någon gång behöver tooRekey ditt certifikat markerar **genererar nya nycklar och synkronisera** alternativet från **certifikategenskaper** bladet.</span><span class="sxs-lookup"><span data-stu-id="a8e02-185">If you ever need tooRekey your certificate, select **Rekey and Sync** option from **Certificate Properties** Blade.</span></span>

<span data-ttu-id="a8e02-186">Klicka på **genererar nya nycklar** knappen tooinitiate hello processen.</span><span class="sxs-lookup"><span data-stu-id="a8e02-186">Click **Rekey** Button tooinitiate hello process.</span></span> <span data-ttu-id="a8e02-187">Den här processen kan ta 1-10 minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="a8e02-187">This process can take 1-10 minutes toocomplete.</span></span>

![infoga bilden av nyckelförnyelse SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

<span data-ttu-id="a8e02-189">Omgenerering ditt certifikat samlar hello certifikat med ett nytt certifikat utfärdade av certifikatutfärdaren hello.</span><span class="sxs-lookup"><span data-stu-id="a8e02-189">Rekeying your certificate rolls hello certificate with a new certificate issued from hello certificate authority.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8e02-190">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8e02-190">Next Steps</span></span>

* [<span data-ttu-id="a8e02-191">Lägg till ett nätverk för Innehållsleverans</span><span class="sxs-lookup"><span data-stu-id="a8e02-191">Add a Content Delivery Network</span></span>](app-service-web-tutorial-content-delivery-network.md)