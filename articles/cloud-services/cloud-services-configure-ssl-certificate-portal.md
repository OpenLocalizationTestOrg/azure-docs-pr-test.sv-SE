---
title: "aaaConfigure SSL för en molnbaserad tjänst | Microsoft Docs"
description: "Lär dig hur toospecify en HTTPS-slutpunkt för en webbroll och hur tooupload en SSL-certifikatet toosecure ditt program. Dessa exempel används hello Azure-portalen."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 371ba204-48b6-41af-ab9f-ed1d64efe704
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: adegeo
ms.openlocfilehash: b19283bb7b0e95374f2ae9c3532eb1effc7d6a9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="eda90-104">Konfigurera SSL för ett program i Azure</span><span class="sxs-lookup"><span data-stu-id="eda90-104">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="eda90-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="eda90-105">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="eda90-106">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="eda90-106">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
>

<span data-ttu-id="eda90-107">Secure Socket Layer (SSL)-kryptering är hello vanligaste metoden för att skydda data som skickas över hello internet.</span><span class="sxs-lookup"><span data-stu-id="eda90-107">Secure Socket Layer (SSL) encryption is hello most commonly used method of securing data sent across hello internet.</span></span> <span data-ttu-id="eda90-108">Arbetet beskrivs hur toospecify en HTTPS-slutpunkt för en webbroll och hur tooupload en SSL-certifikatet toosecure ditt program.</span><span class="sxs-lookup"><span data-stu-id="eda90-108">This common task discusses how toospecify an HTTPS endpoint for a web role and how tooupload an SSL certificate toosecure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="eda90-109">hello procedurer i det här steget gäller tooAzure molntjänster; App-tjänster, se [detta](../app-service-web/web-sites-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="eda90-109">hello procedures in this task apply tooAzure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md).</span></span>
>

<span data-ttu-id="eda90-110">Den här aktiviteten använder en Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="eda90-110">This task uses a production deployment.</span></span> <span data-ttu-id="eda90-111">Information om hur du använder en fristående distribution har angetts hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="eda90-111">Information on using a staging deployment is provided at hello end of this topic.</span></span>

<span data-ttu-id="eda90-112">Läs [detta](cloud-services-how-to-create-deploy-portal.md) första om du inte har skapat en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="eda90-112">Read [this](cloud-services-how-to-create-deploy-portal.md) first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="eda90-113">Steg 1: Hämta ett SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="eda90-113">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="eda90-114">tooconfigure SSL för ett program måste du först tooget ett SSL-certifikat som har signerats av en certifikatet (CA), en betrodd tredje part som utfärdar certifikat för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="eda90-114">tooconfigure SSL for an application, you first need tooget an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="eda90-115">Om du inte redan har en, måste tooobtain något på ett företag som säljer SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="eda90-115">If you do not already have one, you need tooobtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="eda90-116">hello certifikat måste uppfylla följande krav för SSL-certifikat i Azure hello:</span><span class="sxs-lookup"><span data-stu-id="eda90-116">hello certificate must meet hello following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="eda90-117">hello certifikatet måste innehålla en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="eda90-117">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="eda90-118">hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="eda90-118">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="eda90-119">hello måste certifikatets ämnesnamn matcha hello domän används tooaccess hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="eda90-119">hello certificate's subject name must match hello domain used tooaccess hello cloud service.</span></span> <span data-ttu-id="eda90-120">Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello cloudapp.net domän.</span><span class="sxs-lookup"><span data-stu-id="eda90-120">You cannot obtain an SSL certificate from a certificate authority (CA) for hello cloudapp.net domain.</span></span> <span data-ttu-id="eda90-121">Du måste skaffa en anpassad domän namnet toouse när komma åt tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eda90-121">You must acquire a custom domain name toouse when access your service.</span></span> <span data-ttu-id="eda90-122">När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domänen namnet som används för tooaccess ditt program.</span><span class="sxs-lookup"><span data-stu-id="eda90-122">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="eda90-123">Om ditt domännamn är till exempel **contoso.com** du vill begära ett certifikat från Certifikatutfärdaren för ***. contoso.com** eller **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="eda90-123">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="eda90-124">hello certifikat måste använda minst 2048-bitars kryptering.</span><span class="sxs-lookup"><span data-stu-id="eda90-124">hello certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="eda90-125">För testning, kan du [skapa](cloud-services-certs-create.md) och använda ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="eda90-125">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="eda90-126">Ett självsignerat certifikat har autentiserats inte via en Certifikatutfärdare och kan använda hello cloudapp.net domän som hello Webbadress.</span><span class="sxs-lookup"><span data-stu-id="eda90-126">A self-signed certificate is not authenticated through a CA and can use hello cloudapp.net domain as hello website URL.</span></span> <span data-ttu-id="eda90-127">Till exempel hello följande uppgift använder ett självsignerat certifikat i vilka hello nätverksnamn (CN) används i hello certifikat är **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="eda90-127">For example, hello following task uses a self-signed certificate in which hello common name (CN) used in hello certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="eda90-128">Därefter måste du ha information om hello certifikat i tjänstdefinitionen och konfigurationsfiler för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="eda90-128">Next, you must include information about hello certificate in your service definition and service configuration files.</span></span>

<span data-ttu-id="eda90-129"><a name="modify"> </a></span><span class="sxs-lookup"><span data-stu-id="eda90-129"><a name="modify"> </a></span></span>

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a><span data-ttu-id="eda90-130">Steg 2: Ändra hello service definition och configuration-filer</span><span class="sxs-lookup"><span data-stu-id="eda90-130">Step 2: Modify hello service definition and configuration files</span></span>
<span data-ttu-id="eda90-131">Programmet måste vara konfigurerade toouse hello certifikat och en HTTPS-slutpunkt måste läggas till.</span><span class="sxs-lookup"><span data-stu-id="eda90-131">Your application must be configured toouse hello certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="eda90-132">Därför hello tjänstdefinitionen och service configuration-filer måste toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="eda90-132">As a result, hello service definition and service configuration files need toobe updated.</span></span>

1. <span data-ttu-id="eda90-133">Öppna i din utvecklingsmiljö hello tjänstdefinitionsfilen (CSDEF) och lägga till en **certifikat** avsnitt i hello **WebRole** avsnittet och inkludera hello följande information den certifikatet (och mellanliggande certifikat):</span><span class="sxs-lookup"><span data-stu-id="eda90-133">In your development environment, open hello service definition file (CSDEF), add a **Certificates** section within hello **WebRole** section, and include hello following information about the certificate (and intermediate certificates):</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="My"
                        permissionLevel="limitedOrElevated" />
            <!-- IMPORTANT! Unless your certificate is either
            self-signed or signed directly by hello CA root, you
            must include all hello intermediate certificates
            here. You must list them here, even if they are
            not bound tooany endpoints. Failing toolist any of
            hello intermediate certificates may cause hard-to-reproduce
            interoperability problems on some clients.-->
            <Certificate name="CAForSampleCertificate"
                        storeLocation="LocalMachine"
                        storeName="CA"
                        permissionLevel="limitedOrElevated" />
        </Certificates>
    ...
    </WebRole>
    ```

   <span data-ttu-id="eda90-134">Hej **certifikat** avsnittet definierar hello namnet på vår certifikat, dess plats och hello namnet på hello store där den finns.</span><span class="sxs-lookup"><span data-stu-id="eda90-134">hello **Certificates** section defines hello name of our certificate, its location, and hello name of hello store where it is located.</span></span>

   <span data-ttu-id="eda90-135">Behörigheter (`permisionLevel` attributet) kan vara set tooone av hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="eda90-135">Permissions (`permisionLevel` attribute) can be set tooone of hello following values:</span></span>

   | <span data-ttu-id="eda90-136">Behörighetsvärdet</span><span class="sxs-lookup"><span data-stu-id="eda90-136">Permission Value</span></span> | <span data-ttu-id="eda90-137">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="eda90-137">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="eda90-138">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="eda90-138">limitedOrElevated</span></span> |<span data-ttu-id="eda90-139">**(Standard)**  Alla processer som rollen har åtkomst till hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="eda90-139">**(Default)** All role processes can access hello private key.</span></span> |
   | <span data-ttu-id="eda90-140">upphöjd</span><span class="sxs-lookup"><span data-stu-id="eda90-140">elevated</span></span> |<span data-ttu-id="eda90-141">Endast utökade processer kan komma åt hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="eda90-141">Only elevated processes can access hello private key.</span></span> |

2. <span data-ttu-id="eda90-142">Lägg till i din tjänstdefinitionsfilen en **InputEndpoint** element i hello **slutpunkter** avsnittet tooenable HTTPS:</span><span class="sxs-lookup"><span data-stu-id="eda90-142">In your service definition file, add an **InputEndpoint** element within hello **Endpoints** section tooenable HTTPS:</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Endpoints>
            <InputEndpoint name="HttpsIn" protocol="https" port="443"
                certificate="SampleCertificate" />
        </Endpoints>
    ...
    </WebRole>
    ```

3. <span data-ttu-id="eda90-143">Lägg till i din tjänstdefinitionsfilen en **bindning** element i hello **platser** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="eda90-143">In your service definition file, add a **Binding** element within hello **Sites** section.</span></span> <span data-ttu-id="eda90-144">Det här elementet lägger till en HTTPS-bindningen toomap endpoint tooyour plats:</span><span class="sxs-lookup"><span data-stu-id="eda90-144">This element adds an HTTPS binding toomap the endpoint tooyour site:</span></span>

   ```xml
    <WebRole name="CertificateTesting" vmsize="Small">
    ...
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="HttpsIn" endpointName="HttpsIn" />
                </Bindings>
            </Site>
        </Sites>
    ...
    </WebRole>
    ```

   <span data-ttu-id="eda90-145">Alla hello nödvändiga ändringar toohello tjänstdefinitionsfilen har avslutats. men du behöver fortfarande tooadd hello certifikatinformationen till hello tjänstkonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="eda90-145">All hello required changes toohello service definition file have been completed; but, you still need tooadd hello certificate information to hello service configuration file.</span></span>
4. <span data-ttu-id="eda90-146">Lägg till i din tjänstekonfigurationsfil (CSCFG) ServiceConfiguration.Cloud.cscfg, en **certifikat** värdet med ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="eda90-146">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** value with that of your certificate.</span></span> <span data-ttu-id="eda90-147">hello följande kodexempel innehåller information om hello **certifikat** avsnittet förutom hello tumavtrycksvärde.</span><span class="sxs-lookup"><span data-stu-id="eda90-147">hello following code sample provides details of hello **Certificates** section, except for hello thumbprint value.</span></span>

   ```xml
    <Role name="Deployment">
    ...
        <Certificates>
            <Certificate name="SampleCertificate"
                thumbprint="9427befa18ec6865a9ebdc79d4c38de50e6316ff"
                thumbprintAlgorithm="sha1" />
            <Certificate name="CAForSampleCertificate"
                thumbprint="79d4c38de50e6316ff9427befa18ec6865a9ebdc"
                thumbprintAlgorithm="sha1" />
        </Certificates>
    ...
    </Role>
    ```

<span data-ttu-id="eda90-148">(Det här exemplet används **sha1** för hello tumavtrycket algoritmen.</span><span class="sxs-lookup"><span data-stu-id="eda90-148">(This example uses **sha1** for hello thumbprint algorithm.</span></span> <span data-ttu-id="eda90-149">Ange hello lämpligt värde för din certifikatets tumavtryck algoritm.)</span><span class="sxs-lookup"><span data-stu-id="eda90-149">Specify hello appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="eda90-150">Nu när hello service definitions- och konfigurationsfiler har uppdaterats, paketera din distribution för uppladdning av tooAzure.</span><span class="sxs-lookup"><span data-stu-id="eda90-150">Now that hello service definition and service configuration files have been updated, package your deployment for uploading tooAzure.</span></span> <span data-ttu-id="eda90-151">Om du använder **cspack**, Använd inte den **/generateConfigurationFile** flagga som skrivs som du har infogat certifikatinformationen.</span><span class="sxs-lookup"><span data-stu-id="eda90-151">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that will overwrite the certificate information you just inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="eda90-152">Steg 3: Överför certifikat</span><span class="sxs-lookup"><span data-stu-id="eda90-152">Step 3: Upload a certificate</span></span>
<span data-ttu-id="eda90-153">Ansluta toohello Azure-portalen och...</span><span class="sxs-lookup"><span data-stu-id="eda90-153">Connect toohello Azure portal and...</span></span>

1. <span data-ttu-id="eda90-154">I hello **alla resurser** avsnitt i hello Portal, Välj din molntjänst.</span><span class="sxs-lookup"><span data-stu-id="eda90-154">In hello **All resources** section of hello Portal, select your cloud service.</span></span>

    ![Publicera din molntjänst](media/cloud-services-configure-ssl-certificate-portal/browse.png)

2. <span data-ttu-id="eda90-156">Klicka på **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="eda90-156">Click **Certificates**.</span></span>

    ![Klicka på ikonen för hello certifikat](media/cloud-services-configure-ssl-certificate-portal/certificate-item.png)

3. <span data-ttu-id="eda90-158">Klicka på **överför** hello överst i hello certifikat område.</span><span class="sxs-lookup"><span data-stu-id="eda90-158">Click **Upload** at hello top of hello certificates area.</span></span>

    ![Klicka på menyalternativet för hello överför](media/cloud-services-configure-ssl-certificate-portal/Upload_menu.png)

4. <span data-ttu-id="eda90-160">Ange hello **filen**, **lösenord**, klicka på **överför** längst hello hello dataområdet transaktionen.</span><span class="sxs-lookup"><span data-stu-id="eda90-160">Provide hello **File**, **Password**, then click **Upload** at hello bottom of hello data entry area.</span></span>

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a><span data-ttu-id="eda90-161">Steg 4: Anslut toohello rollinstansen med hjälp av HTTPS</span><span class="sxs-lookup"><span data-stu-id="eda90-161">Step 4: Connect toohello role instance by using HTTPS</span></span>
<span data-ttu-id="eda90-162">Nu när distributionen är igång och körs i Azure, kan du ansluta tooit med hjälp av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="eda90-162">Now that your deployment is up and running in Azure, you can connect tooit using HTTPS.</span></span>

1. <span data-ttu-id="eda90-163">Klicka på hello **Webbadress** tooopen in hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="eda90-163">Click hello **Site URL** tooopen up hello web browser.</span></span>

   ![Klicka på hello Webbadress](media/cloud-services-configure-ssl-certificate-portal/navigate.png)

2. <span data-ttu-id="eda90-165">Ändra hello länken toouse i webbläsaren, **https** i stället för **http**, och besök hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="eda90-165">In your web browser, modify hello link toouse **https** instead of **http**, and then visit hello page.</span></span>

   > [!NOTE]
   > <span data-ttu-id="eda90-166">Om du använder ett självsignerat certifikat när du bläddrar tooan HTTPS-slutpunkt som är kopplad till hello självsignerat certifikat kan du se ett certifikatfel i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="eda90-166">If you are using a self-signed certificate, when you browse tooan HTTPS endpoint that's associated with hello self-signed certificate you may see a certificate error in hello browser.</span></span> <span data-ttu-id="eda90-167">Använder ett certifikat som signerats av en betrodd certifikatutfärdare åtgärdar problemet. i hello tiden kan du ignorera hello felet.</span><span class="sxs-lookup"><span data-stu-id="eda90-167">Using a certificate signed by a trusted certification authority eliminates this problem; in hello meantime, you can ignore hello error.</span></span> <span data-ttu-id="eda90-168">(Ett annat alternativ är tooadd hello självsignerat certifikat toohello användarens certifikat från betrodd utfärdare certifikatarkiv.)</span><span class="sxs-lookup"><span data-stu-id="eda90-168">(Another option is tooadd hello self-signed certificate toohello user's trusted certificate authority certificate store.)</span></span>
   >
   >

   ![Platsen preview](media/cloud-services-configure-ssl-certificate-portal/show-site.png)

   > [!TIP]
   > <span data-ttu-id="eda90-170">Om du vill toouse SSL för en fristående distribution i stället för en Produktionsdistribution, behöver du toodetermine hello URL som används för hello mellanlagring av distribution.</span><span class="sxs-lookup"><span data-stu-id="eda90-170">If you want toouse SSL for a staging deployment instead of a production deployment, you'll first need toodetermine hello URL used for hello staging deployment.</span></span> <span data-ttu-id="eda90-171">När Molntjänsten har distribuerats, hello URL toohello mellanlagring miljö bestäms av hello **distributions-ID** GUID i det här formatet:`https://deployment-id.cloudapp.net/`</span><span class="sxs-lookup"><span data-stu-id="eda90-171">Once your cloud service has been deployed, hello URL toohello staging environment is determined by hello **Deployment ID** GUID in this format: `https://deployment-id.cloudapp.net/`</span></span>  
   >
   > <span data-ttu-id="eda90-172">Skapa ett certifikat med hello vanliga namn (CN) lika toohello GUID-baserad URL (till exempel **328187776e774ceda8fc57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="eda90-172">Create a certificate with hello common name (CN) equal toohello GUID-based URL (for example, **328187776e774ceda8fc57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="eda90-173">Använd hello portal tooadd hello certifikat tooyour mellanlagras tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="eda90-173">Use hello portal tooadd hello certificate tooyour staged cloud service.</span></span> <span data-ttu-id="eda90-174">Lägg sedan till hello information tooyour CSDEF- och CSCFG certifikatfiler, paketera programmet och uppdatera mellanlagrade toouse hello nya distributionspaketet.</span><span class="sxs-lookup"><span data-stu-id="eda90-174">Then, add hello certificate information tooyour CSDEF and CSCFG files, repackage your application, and update your staged deployment toouse hello new package.</span></span>
   >

## <a name="next-steps"></a><span data-ttu-id="eda90-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eda90-175">Next steps</span></span>
* <span data-ttu-id="eda90-176">[Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="eda90-176">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="eda90-177">Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="eda90-177">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="eda90-178">Konfigurera en [domännamn](cloud-services-custom-domain-name-portal.md).</span><span class="sxs-lookup"><span data-stu-id="eda90-178">Configure a [custom domain name](cloud-services-custom-domain-name-portal.md).</span></span>
* <span data-ttu-id="eda90-179">[Hantera din molntjänst](cloud-services-how-to-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="eda90-179">[Manage your cloud service](cloud-services-how-to-manage-portal.md).</span></span>
