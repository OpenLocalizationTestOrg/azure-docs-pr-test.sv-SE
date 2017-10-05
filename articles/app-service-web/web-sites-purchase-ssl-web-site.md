---
title: "Lägg till ett SSL-certifikat i appen Azure App Service | Microsoft Docs"
description: "Lär dig hur du lägger till ett SSL-certifikat till din Apptjänst-app."
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
ms.openlocfilehash: 191dd7240ad15b4936a72bc27a2d0162350f3afb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a><span data-ttu-id="99b88-103">Köp och konfigurera ett SSL-certifikat för din Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="99b88-103">Buy and Configure an SSL Certificate for your Azure App Service</span></span>

<span data-ttu-id="99b88-104">I den här självstudiekursen kommer du skyddar ditt webbprogram genom att köpa ett SSL-certifikat för din  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, på ett säkert sätt lagra det i [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), och associera den med en anpassad domän.</span><span class="sxs-lookup"><span data-stu-id="99b88-104">In this tutorial, you will secure your web app by purchasing an SSL certificate for your **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, securely storing it in [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis), and associating it with a custom domain.</span></span>

## <a name="step-1---log-in-to-azure"></a><span data-ttu-id="99b88-105">Steg 1 – Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="99b88-105">Step 1 - Log in to Azure</span></span>

<span data-ttu-id="99b88-106">Logga in på Azure-portalen på http://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="99b88-106">Log in to the Azure portal at http://portal.azure.com</span></span>

## <a name="step-2---place-an-ssl-certificate-order"></a><span data-ttu-id="99b88-107">Steg 2 – beställning SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="99b88-107">Step 2 - Place an SSL Certificate order</span></span>

<span data-ttu-id="99b88-108">Du kan placera en order för SSL-certifikat genom att skapa en ny [App tjänstcertifikat](https://portal.azure.com/#create/Microsoft.SSL) i den **Azure-portalen**.</span><span class="sxs-lookup"><span data-stu-id="99b88-108">You can place an SSL Certificate order by creating a new [App Service Certificate](https://portal.azure.com/#create/Microsoft.SSL) In the **Azure portal**.</span></span>

![Skapande av certifikat](./media/app-service-web-purchase-ssl-web-site/createssl.png)

<span data-ttu-id="99b88-110">Ange ett eget **namn** för SSL-certifikatet och ange den **domännamn**</span><span class="sxs-lookup"><span data-stu-id="99b88-110">Enter a friendly **Name** for your SSL certificate and enter the **Domain Name**</span></span>

> [!NOTE]
> <span data-ttu-id="99b88-111">Detta är en av de viktigaste delarna i köpet.</span><span class="sxs-lookup"><span data-stu-id="99b88-111">This is one of the most critical parts of the purchase process.</span></span> <span data-ttu-id="99b88-112">Kontrollera att du anger rätt värdnamn (anpassad domän) som du vill skydda med det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="99b88-112">Make sure to enter correct host name (custom domain) that you want to protect with this certificate.</span></span> <span data-ttu-id="99b88-113">**INTE** lägga till värdnamnet med WWW.</span><span class="sxs-lookup"><span data-stu-id="99b88-113">**DO NOT** append the Host name with WWW.</span></span> 
>

<span data-ttu-id="99b88-114">Välj din **prenumeration**, **resursgruppen**, och **certifikat SKU**</span><span class="sxs-lookup"><span data-stu-id="99b88-114">Select your **Subscription**, **Resource Group**, and **Certificate SKU**</span></span>

> [!WARNING]
> <span data-ttu-id="99b88-115">Apptjänstcertifikat kan endast användas av andra App-tjänster inom samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="99b88-115">App Service Certificates can only be used by other App Services within the same subscription.</span></span>  
>

## <a name="step-3---store-the-certificate-in-azure-key-vault"></a><span data-ttu-id="99b88-116">Steg 3 – lagra certifikatet i Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="99b88-116">Step 3 - Store the certificate in Azure Key Vault</span></span>

> [!NOTE]
> <span data-ttu-id="99b88-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) är en Azure-tjänst som hjälper dig skydda krypteringsnycklar och hemligheter som används av molnprogram och tjänster.</span><span class="sxs-lookup"><span data-stu-id="99b88-117">[Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) is an Azure service that helps safeguard cryptographic keys and secrets used by cloud applications and services.</span></span>
>

<span data-ttu-id="99b88-118">När köpet SSL-certifikat är klar kan du behöva öppna [Apptjänstcertifikat](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) resursbladet.</span><span class="sxs-lookup"><span data-stu-id="99b88-118">Once the SSL Certificate purchase is complete, you need to open [App Service Certificates](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) Resource blade.</span></span>

![Infoga bild av redo att lagra i KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

<span data-ttu-id="99b88-120">Du kommer att märka att certifikatstatus är **”väntande utfärdande”** eftersom det inte finns några fler steg som du måste utföra innan du kan börja använda det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="99b88-120">You will notice that Certificate status is **“Pending Issuance”** as there are few more steps you need to complete before you can start using this certificate.</span></span>

<span data-ttu-id="99b88-121">Klicka på **Certifikatkonfigureringen** inuti certifikategenskaper bladet och klicka på **steg 1: lagra** att lagra det här certifikatet i Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="99b88-121">Click **Certificate Configuration** inside Certificate Properties blade and Click on **Step 1: Store** to store this certificate in Azure Key Vault.</span></span>

<span data-ttu-id="99b88-122">Från **Key Vault Status** bladet, klickar du på **Key Vault databasen** att välja en befintlig Key Vault för lagring av det här certifikatet **eller skapa nya Key Vault** att skapa nytt Nyckelvalv inom samma prenumeration och resurs.</span><span class="sxs-lookup"><span data-stu-id="99b88-122">From **Key Vault Status** Blade, click **Key Vault Repository** to choose an existing Key Vault to store this certificate **OR Create New Key Vault** to create new Key Vault inside same subscription and resource group.</span></span>

> [!NOTE]
> <span data-ttu-id="99b88-123">Azure Key Vault har minimal kostnader för lagring av det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="99b88-123">Azure Key Vault has minimal charges for storing this certificate.</span></span>
> <span data-ttu-id="99b88-124">Mer information finns i  **[prisinformation för Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/)**.</span><span class="sxs-lookup"><span data-stu-id="99b88-124">For more information, see **[Azure Key Vault Pricing Details](https://azure.microsoft.com/pricing/details/key-vault/)**.</span></span>
>

<span data-ttu-id="99b88-125">När du har valt nyckeln valvet databasen att lagra certifikatet, den **lagra** alternativet ska visa lyckades.</span><span class="sxs-lookup"><span data-stu-id="99b88-125">Once you have selected the Key Vault Repository to store this certificate in, the **Store** option should show success.</span></span>

![infoga bilden av store KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-the-domain-ownership"></a><span data-ttu-id="99b88-127">Steg 4 – verifiera domänen ägarskap</span><span class="sxs-lookup"><span data-stu-id="99b88-127">Step 4 - Verify the Domain Ownership</span></span>

> [!NOTE]
> <span data-ttu-id="99b88-128">Det finns tre typer av domänverifiering som stöds av Apptjänstcertifikat: verifiering av domän, e-post manuellt.</span><span class="sxs-lookup"><span data-stu-id="99b88-128">There are three types of domain verification supported by App service Certificates: Domain, Mail, Manual Verification.</span></span> <span data-ttu-id="99b88-129">Dessa beskrivs mer detaljerat i den [avancerade avsnitt](#advanced).</span><span class="sxs-lookup"><span data-stu-id="99b88-129">These are explained in more details in the [Advanced section](#advanced).</span></span>

<span data-ttu-id="99b88-130">Från samma **Certifikatkonfigureringen** bladet som du använde i steg3 klickar du på **steg 2: Kontrollera**.</span><span class="sxs-lookup"><span data-stu-id="99b88-130">From the same **Certificate Configuration** blade you used in Step 3, click **Step 2: Verify**.</span></span>

<span data-ttu-id="99b88-131">**Verifiering av domän** det här är den enklaste processen **endast om** du har  **[köpt din anpassade domän från Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span><span class="sxs-lookup"><span data-stu-id="99b88-131">**Domain Verification** This is the most convenient process **ONLY IF** you have **[purchased your custom domain from Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**</span></span>
<span data-ttu-id="99b88-132">Klicka på **Kontrollera** för att slutföra det här steget.</span><span class="sxs-lookup"><span data-stu-id="99b88-132">Click on **Verify** button to complete this step.</span></span>

![infoga bilden för verifiering av domän](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

<span data-ttu-id="99b88-134">När du klickar på **Kontrollera**, använda den **uppdatera** knappen förrän den **Kontrollera** alternativet ska visa lyckades.</span><span class="sxs-lookup"><span data-stu-id="99b88-134">After clicking **Verify**, use the **Refresh** button until the **Verify** option should show success.</span></span>

![Infoga bild av verifiera i KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-to-app-service-app"></a><span data-ttu-id="99b88-136">Steg 5 – distribuera certifikat till App Service-appen</span><span class="sxs-lookup"><span data-stu-id="99b88-136">Step 5 - Assign Certificate to App Service App</span></span>

> [!NOTE]
> <span data-ttu-id="99b88-137">Innan du utför stegen i det här avsnittet måste du har associerat ett anpassat domännamn med din app.</span><span class="sxs-lookup"><span data-stu-id="99b88-137">Before performing the steps in this section, you must have associated a custom domain name with your app.</span></span> <span data-ttu-id="99b88-138">Mer information finns i  **[konfigurera ett anpassat domännamn för en webbapp.](app-service-web-tutorial-custom-domain.md)**</span><span class="sxs-lookup"><span data-stu-id="99b88-138">For more information, see **[Configuring a custom domain name for a web app.](app-service-web-tutorial-custom-domain.md)**</span></span>
>

<span data-ttu-id="99b88-139">I den  **[Azure-portalen](https://portal.azure.com/)**, klicka på den **Apptjänst** alternativet till vänster på sidan.</span><span class="sxs-lookup"><span data-stu-id="99b88-139">In the **[Azure portal](https://portal.azure.com/)**, click the **App Service** option on the left of the page.</span></span>

<span data-ttu-id="99b88-140">Klicka på namnet på den app du vill koppla certifikatet till.</span><span class="sxs-lookup"><span data-stu-id="99b88-140">Click the name of your app to which you want to assign this certificate.</span></span>

<span data-ttu-id="99b88-141">I den **inställningar**, klickar du på **SSL-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="99b88-141">In the **Settings**, click **SSL certificates**.</span></span>

<span data-ttu-id="99b88-142">Klicka på **importera App Service certifikat** och välj det certifikat som du precis har köpt.</span><span class="sxs-lookup"><span data-stu-id="99b88-142">Click **Import App Service Certificate** and select the certificate that you just purchased.</span></span>

![Infoga bild av Importera certifikat](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

<span data-ttu-id="99b88-144">I den **ssl-bindningar** avsnittet Klicka på **lägga till bindningar**, och de nedrullningsbara listorna väljer domännamn för att skydda med SSL och certifikatet för att använda.</span><span class="sxs-lookup"><span data-stu-id="99b88-144">In the **ssl bindings** section Click on **Add bindings**, and use the dropdowns to select the domain name to secure with SSL, and the certificate to use.</span></span> <span data-ttu-id="99b88-145">Du kan också välja om du vill använda  **[Server Servernamnsindikation (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  eller IP-baserade SSL.</span><span class="sxs-lookup"><span data-stu-id="99b88-145">You may also select whether to use **[Server Name Indication (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)** or IP based SSL.</span></span>

![infoga bilden av SSL-bindningar](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

<span data-ttu-id="99b88-147">Klicka på **Lägg till bindning för** att spara ändringarna och aktivera SSL.</span><span class="sxs-lookup"><span data-stu-id="99b88-147">Click **Add Binding** to save the changes and enable SSL.</span></span>

> [!NOTE]
> <span data-ttu-id="99b88-148">Om du har valt **IP-baserade SSL** och domänen är konfigurerad med en A-post måste du utföra följande åtgärder.</span><span class="sxs-lookup"><span data-stu-id="99b88-148">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform the following additional steps.</span></span> <span data-ttu-id="99b88-149">Dessa beskrivs mer detaljerat i den [avancerade avsnitt](#Advanced).</span><span class="sxs-lookup"><span data-stu-id="99b88-149">These are explained in more details in the [Advanced section](#Advanced).</span></span>

<span data-ttu-id="99b88-150">Du ska nu kunna besöka app med hjälp av `HTTPS://` i stället för `HTTP://` att verifiera att certifikatet har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="99b88-150">At this point, you should be able to visit your app using `HTTPS://` instead of `HTTP://` to verify that the certificate has been configured correctly.</span></span>

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a><span data-ttu-id="99b88-151">Steg 6 - administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="99b88-151">Step 6 - Management tasks</span></span>

### <a name="azure-cli"></a><span data-ttu-id="99b88-152">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="99b88-152">Azure CLI</span></span>

<span data-ttu-id="99b88-153">[!code-azurecli[huvudsakliga](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "binda ett anpassat SSL-certifikat till en webbapp")]</span><span class="sxs-lookup"><span data-stu-id="99b88-153">[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")]</span></span> 

### <a name="powershell"></a><span data-ttu-id="99b88-154">PowerShell</span><span class="sxs-lookup"><span data-stu-id="99b88-154">PowerShell</span></span>

<span data-ttu-id="99b88-155">[!code-powershell[huvudsakliga](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "binda ett anpassat SSL-certifikat till en webbapp")]</span><span class="sxs-lookup"><span data-stu-id="99b88-155">[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]</span></span>

## <a name="advanced"></a><span data-ttu-id="99b88-156">Advanced</span><span class="sxs-lookup"><span data-stu-id="99b88-156">Advanced</span></span>

### <a name="verifying-domain-ownership"></a><span data-ttu-id="99b88-157">Verifiera domänen ägarskap</span><span class="sxs-lookup"><span data-stu-id="99b88-157">Verifying Domain Ownership</span></span>

<span data-ttu-id="99b88-158">Det finns två flera typer av domänverifiering som stöds av Apptjänstcertifikat: e-post och manuell kontroll.</span><span class="sxs-lookup"><span data-stu-id="99b88-158">There are two more types of domain verification supported by App service Certificates: Mail, and Manual Verification.</span></span>

#### <a name="mail-verification"></a><span data-ttu-id="99b88-159">Verifiering av e-post</span><span class="sxs-lookup"><span data-stu-id="99b88-159">Mail Verification</span></span>

<span data-ttu-id="99b88-160">E-postmeddelandet har redan skickats till den e-adress som är associerade med den här domänen.</span><span class="sxs-lookup"><span data-stu-id="99b88-160">Verification email has already been sent to the Email Address(es) associated with this custom domain.</span></span>
<span data-ttu-id="99b88-161">Öppna e-postmeddelandet för att slutföra verifieringssteg e-post, och klicka på verifieringslänken.</span><span class="sxs-lookup"><span data-stu-id="99b88-161">To complete the Email verification step, open the email and click the verification link.</span></span>

![Infoga bild av e-Postverifiering](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

<span data-ttu-id="99b88-163">Om du vill skicka e-postmeddelandet klickar du på den **skicka e-post** knappen.</span><span class="sxs-lookup"><span data-stu-id="99b88-163">If you need to resend the verification email, click the **Resend Email** button.</span></span>

#### <a name="manual-verification"></a><span data-ttu-id="99b88-164">Manuell kontroll</span><span class="sxs-lookup"><span data-stu-id="99b88-164">Manual Verification</span></span>

> [!IMPORTANT]
> <span data-ttu-id="99b88-165">HTML-webbsida verifiering (fungerar bara med certifikat SKU: N)</span><span class="sxs-lookup"><span data-stu-id="99b88-165">HTML Web Page Verification (only works with Standard Certificate SKU)</span></span>
>

1. <span data-ttu-id="99b88-166">Skapa en HTML-fil med namnet **”starfield.html”**</span><span class="sxs-lookup"><span data-stu-id="99b88-166">Create an HTML file named **"starfield.html"**</span></span>

1. <span data-ttu-id="99b88-167">Innehållet i den här filen bör vara det exakta namnet på domänen verifiering Token.</span><span class="sxs-lookup"><span data-stu-id="99b88-167">Content of this file should be the exact name of the Domain Verification Token.</span></span> <span data-ttu-id="99b88-168">(Du kan kopiera token från bladet domän verifiering Status)</span><span class="sxs-lookup"><span data-stu-id="99b88-168">(You can copy the token from the Domain Verification Status Blade)</span></span>

1. <span data-ttu-id="99b88-169">Överför den här filen i roten på webbservern som värd för din domän`/.well-known/pki-validation/starfield.html`</span><span class="sxs-lookup"><span data-stu-id="99b88-169">Upload this file at the root of the web server hosting your domain `/.well-known/pki-validation/starfield.html`</span></span>

1. <span data-ttu-id="99b88-170">Klicka på **uppdatera** att uppdatera certifikatstatus när verifieringen är klar.</span><span class="sxs-lookup"><span data-stu-id="99b88-170">Click **Refresh** to update the certificate status after verification is completed.</span></span> <span data-ttu-id="99b88-171">Det kan ta några minuter för verifiering att slutföra.</span><span class="sxs-lookup"><span data-stu-id="99b88-171">It might take few minutes for verification to complete.</span></span>

> [!TIP]
> <span data-ttu-id="99b88-172">Kontrollera i en terminal `curl -G http://<domain>/.well-known/pki-validation/starfield.html` svaret ska innehålla den `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="99b88-172">Verify in a terminal using `curl -G http://<domain>/.well-known/pki-validation/starfield.html` the response should contain the `<verification-token>`.</span></span>

#### <a name="dns-txt-record-verification"></a><span data-ttu-id="99b88-173">Verifiering av DNS TXT-post</span><span class="sxs-lookup"><span data-stu-id="99b88-173">DNS TXT Record Verification</span></span>

1. <span data-ttu-id="99b88-174">Med hjälp av DNS-hanteraren, skapa en TXT-post på den `@` underdomänen med värdet som motsvarar domänen verifiering Token.</span><span class="sxs-lookup"><span data-stu-id="99b88-174">Using your DNS manager, Create a TXT record on the `@` subdomain with value equal to the Domain Verification Token.</span></span>
1. <span data-ttu-id="99b88-175">Klicka på **”uppdatera”** att uppdatera certifikat-status när verifieringen är klar.</span><span class="sxs-lookup"><span data-stu-id="99b88-175">Click **“Refresh”** to update the Certificate status after verification is completed.</span></span>

> [!TIP]
> <span data-ttu-id="99b88-176">Du måste skapa en TXT-post på `@.<domain>` med värdet `<verification-token>`.</span><span class="sxs-lookup"><span data-stu-id="99b88-176">You need to create a TXT record on `@.<domain>` with value `<verification-token>`.</span></span>

### <a name="assign-certificate-to-app-service-app"></a><span data-ttu-id="99b88-177">Tilldela certifikatet till App Service-appen</span><span class="sxs-lookup"><span data-stu-id="99b88-177">Assign Certificate to App Service App</span></span>

<span data-ttu-id="99b88-178">Om du har valt **IP-baserade SSL** och domänen är konfigurerad med en A-post måste du utföra följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="99b88-178">If you selected **IP based SSL** and your custom domain is configured using an A record, you must perform the following additional steps:</span></span>

<span data-ttu-id="99b88-179">När du har konfigurerat en IP-baserade SSL-bindning, en dedicerad IP-adress har tilldelats din app.</span><span class="sxs-lookup"><span data-stu-id="99b88-179">After you have configured an IP based SSL binding, a dedicated IP address is assigned to your app.</span></span> <span data-ttu-id="99b88-180">Du hittar den här IP-adressen på den **anpassad domän** sidan under inställningar för din app, precis ovanför den **värdnamn** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="99b88-180">You can find this IP address on the **Custom domain** page under settings of your app, right above the **Hostnames** section.</span></span> <span data-ttu-id="99b88-181">Den visas som **extern IP-adress**</span><span class="sxs-lookup"><span data-stu-id="99b88-181">It is listed as **External IP Address**</span></span>

![infoga bilden av IP-SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

<span data-ttu-id="99b88-183">Observera att denna IP-adress är densamma som den virtuella IP-adressen som användes för att konfigurera en A-post för din domän.</span><span class="sxs-lookup"><span data-stu-id="99b88-183">Note that this IP address is different than the virtual IP address used previously to configure the A record for your domain.</span></span> <span data-ttu-id="99b88-184">Om du har konfigurerats för att använda SNI baserad SSL, eller inte har konfigurerats för att använda SSL, ingen adress visas för den här posten.</span><span class="sxs-lookup"><span data-stu-id="99b88-184">If you are configured to use SNI based SSL, or are not configured to use SSL, no address is listed for this entry.</span></span>

<span data-ttu-id="99b88-185">Använda de verktyg som tillhandahålls av din domännamnsregistrator kan ändra A-post för ditt anpassade domännamn peka till IP-adressen från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="99b88-185">Using the tools provided by your domain name registrar, modify the A record for your custom domain name to point to the IP address from the previous step.</span></span>

## <a name="rekey-and-sync-the-certificate"></a><span data-ttu-id="99b88-186">Genererar nya nycklar och synkronisera certifikatet</span><span class="sxs-lookup"><span data-stu-id="99b88-186">Rekey and Sync the Certificate</span></span>

<span data-ttu-id="99b88-187">Om du någon gång behöver nyckelförnyelse ditt certifikat väljer **genererar nya nycklar och synkronisera** alternativet från **certifikategenskaper** bladet.</span><span class="sxs-lookup"><span data-stu-id="99b88-187">If you ever need to Rekey your certificate, select **Rekey and Sync** option from **Certificate Properties** Blade.</span></span>

<span data-ttu-id="99b88-188">Klicka på **genererar nya nycklar** för att starta processen.</span><span class="sxs-lookup"><span data-stu-id="99b88-188">Click **Rekey** Button to initiate the process.</span></span> <span data-ttu-id="99b88-189">Den här processen kan ta 1-10 minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="99b88-189">This process can take 1-10 minutes to complete.</span></span>

![infoga bilden av nyckelförnyelse SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

<span data-ttu-id="99b88-191">Omgenerering ditt certifikat samlar certifikatet med ett nytt certifikat som utfärdats av certifikatutfärdaren.</span><span class="sxs-lookup"><span data-stu-id="99b88-191">Rekeying your certificate rolls the certificate with a new certificate issued from the certificate authority.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99b88-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99b88-192">Next Steps</span></span>

* [<span data-ttu-id="99b88-193">Lägg till ett nätverk för Innehållsleverans</span><span class="sxs-lookup"><span data-stu-id="99b88-193">Add a Content Delivery Network</span></span>](app-service-web-tutorial-content-delivery-network.md)