---
title: "aaaConfigure SSL för en tjänst i molnet (klassisk) | Microsoft Docs"
description: "Lär dig hur toospecify en HTTPS-slutpunkt för en webbroll och hur tooupload en SSL-certifikatet toosecure ditt program."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 4cbb7f38-7994-454d-b4f0-7259b558c766
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: a1ca031b98af49d371977a208ed24f6dc8ea2ac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-ssl-for-an-application-in-azure"></a><span data-ttu-id="4b481-103">Konfigurera SSL för ett program i Azure</span><span class="sxs-lookup"><span data-stu-id="4b481-103">Configuring SSL for an application in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4b481-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4b481-104">Azure portal</span></span>](cloud-services-configure-ssl-certificate-portal.md)
> * [<span data-ttu-id="4b481-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="4b481-105">Azure classic portal</span></span>](cloud-services-configure-ssl-certificate.md)
> 
> 

<span data-ttu-id="4b481-106">Secure Socket Layer (SSL)-kryptering är hello vanligaste metoden för att skydda data som skickas över hello internet.</span><span class="sxs-lookup"><span data-stu-id="4b481-106">Secure Socket Layer (SSL) encryption is hello most commonly used method of securing data sent across hello internet.</span></span> <span data-ttu-id="4b481-107">Arbetet beskrivs hur toospecify en HTTPS-slutpunkt för en webbroll och hur tooupload en SSL-certifikatet toosecure ditt program.</span><span class="sxs-lookup"><span data-stu-id="4b481-107">This common task discusses how toospecify an HTTPS endpoint for a web role and how tooupload an SSL certificate toosecure your application.</span></span>

> [!NOTE]
> <span data-ttu-id="4b481-108">hello procedurer i det här steget gäller tooAzure molntjänster; App-tjänster, se [detta](../app-service-web/web-sites-configure-ssl-certificate.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="4b481-108">hello procedures in this task apply tooAzure Cloud Services; for App Services, see [this](../app-service-web/web-sites-configure-ssl-certificate.md) article.</span></span>
> 
> 

<span data-ttu-id="4b481-109">Den här aktiviteten använder en Produktionsdistribution.</span><span class="sxs-lookup"><span data-stu-id="4b481-109">This task uses a production deployment.</span></span> <span data-ttu-id="4b481-110">Information om hur du använder en fristående distribution har angetts hello slutet av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4b481-110">Information on using a staging deployment is provided at hello end of this topic.</span></span>

<span data-ttu-id="4b481-111">Läs [detta](cloud-services-how-to-create-deploy.md) artikeln först om du inte har skapat en tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="4b481-111">Read [this](cloud-services-how-to-create-deploy.md) article first if you have not yet created a cloud service.</span></span>

[!INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

## <a name="step-1-get-an-ssl-certificate"></a><span data-ttu-id="4b481-112">Steg 1: Hämta ett SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="4b481-112">Step 1: Get an SSL certificate</span></span>
<span data-ttu-id="4b481-113">tooconfigure SSL för ett program måste du först tooget ett SSL-certifikat som har signerats av en certifikatet (CA), en betrodd tredje part som utfärdar certifikat för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="4b481-113">tooconfigure SSL for an application, you first need tooget an SSL certificate that has been signed by a Certificate Authority (CA), a trusted third party who issues certificates for this purpose.</span></span> <span data-ttu-id="4b481-114">Om du inte redan har en, måste tooobtain något på ett företag som säljer SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="4b481-114">If you do not already have one, you need tooobtain one from a company that sells SSL certificates.</span></span>

<span data-ttu-id="4b481-115">hello certifikat måste uppfylla följande krav för SSL-certifikat i Azure hello:</span><span class="sxs-lookup"><span data-stu-id="4b481-115">hello certificate must meet hello following requirements for SSL certificates in Azure:</span></span>

* <span data-ttu-id="4b481-116">hello certifikatet måste innehålla en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="4b481-116">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="4b481-117">hello certifikat måste skapas för nyckelutbyte, exportera tooa Personal Information Exchange (.pfx)-fil.</span><span class="sxs-lookup"><span data-stu-id="4b481-117">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="4b481-118">hello måste certifikatets ämnesnamn matcha hello domän används tooaccess hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="4b481-118">hello certificate's subject name must match hello domain used tooaccess hello cloud service.</span></span> <span data-ttu-id="4b481-119">Du kan skaffa ett SSL-certifikat från en certifikatutfärdare (CA) för hello cloudapp.net domän.</span><span class="sxs-lookup"><span data-stu-id="4b481-119">You cannot obtain an SSL certificate from a certificate authority (CA) for hello cloudapp.net domain.</span></span> <span data-ttu-id="4b481-120">Du måste skaffa en anpassad domän namnet toouse när komma åt tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4b481-120">You must acquire a custom domain name toouse when access your service.</span></span> <span data-ttu-id="4b481-121">När du begär ett certifikat från en Certifikatutfärdare måste hello certifikatets ämnesnamn matcha hello domänen namnet som används för tooaccess ditt program.</span><span class="sxs-lookup"><span data-stu-id="4b481-121">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="4b481-122">Om ditt domännamn är till exempel **contoso.com** du vill begära ett certifikat från Certifikatutfärdaren för ***. contoso.com** eller **www.contoso.com**.</span><span class="sxs-lookup"><span data-stu-id="4b481-122">For example, if your custom domain name is **contoso.com** you would request a certificate from your CA for ***.contoso.com** or **www.contoso.com**.</span></span>
* <span data-ttu-id="4b481-123">hello certifikat måste använda minst 2048-bitars kryptering.</span><span class="sxs-lookup"><span data-stu-id="4b481-123">hello certificate must use a minimum of 2048-bit encryption.</span></span>

<span data-ttu-id="4b481-124">För testning, kan du [skapa](cloud-services-certs-create.md) och använda ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="4b481-124">For test purposes, you can [create](cloud-services-certs-create.md) and use a self-signed certificate.</span></span> <span data-ttu-id="4b481-125">Ett självsignerat certifikat har autentiserats inte via en Certifikatutfärdare och kan använda hello cloudapp.net domän som hello Webbadress.</span><span class="sxs-lookup"><span data-stu-id="4b481-125">A self-signed certificate is not authenticated through a CA and can use hello cloudapp.net domain as hello website URL.</span></span> <span data-ttu-id="4b481-126">Till exempel hello följande uppgift använder ett självsignerat certifikat i vilka hello nätverksnamn (CN) används i hello certifikat är **sslexample.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="4b481-126">For example, hello following task uses a self-signed certificate in which hello common name (CN) used in hello certificate is **sslexample.cloudapp.net**.</span></span>

<span data-ttu-id="4b481-127">Därefter måste du ha information om hello certifikat i tjänstdefinitionen och konfigurationsfiler för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4b481-127">Next, you must include information about hello certificate in your service definition and service configuration files.</span></span>

## <a name="step-2-modify-hello-service-definition-and-configuration-files"></a><span data-ttu-id="4b481-128">Steg 2: Ändra hello service definition och configuration-filer</span><span class="sxs-lookup"><span data-stu-id="4b481-128">Step 2: Modify hello service definition and configuration files</span></span>
<span data-ttu-id="4b481-129">Programmet måste vara konfigurerade toouse hello certifikat och en HTTPS-slutpunkt måste läggas till.</span><span class="sxs-lookup"><span data-stu-id="4b481-129">Your application must be configured toouse hello certificate, and an HTTPS endpoint must be added.</span></span> <span data-ttu-id="4b481-130">Därför hello tjänstdefinitionen och service configuration-filer måste toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="4b481-130">As a result, hello service definition and service configuration files need toobe updated.</span></span>

1. <span data-ttu-id="4b481-131">Öppna i din utvecklingsmiljö hello tjänstdefinitionsfilen (CSDEF) och lägga till en **certifikat** avsnitt i hello **WebRole** avsnittet och inkludera hello följande information den certifikatet (och mellanliggande certifikat):</span><span class="sxs-lookup"><span data-stu-id="4b481-131">In your development environment, open hello service definition file (CSDEF), add a **Certificates** section within hello **WebRole** section, and include hello following information about the certificate (and intermediate certificates):</span></span>
   
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
   
   <span data-ttu-id="4b481-132">Hej **certifikat** avsnittet definierar hello namnet på vår certifikat, dess plats och hello namnet på hello store där den finns.</span><span class="sxs-lookup"><span data-stu-id="4b481-132">hello **Certificates** section defines hello name of our certificate, its location, and hello name of hello store where it is located.</span></span>
   
   <span data-ttu-id="4b481-133">Behörigheter (`permisionLevel` attributet) kan vara set tooone av hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="4b481-133">Permissions (`permisionLevel` attribute) can be set tooone of hello following values:</span></span>
   
   | <span data-ttu-id="4b481-134">Behörighetsvärdet</span><span class="sxs-lookup"><span data-stu-id="4b481-134">Permission Value</span></span> | <span data-ttu-id="4b481-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="4b481-135">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="4b481-136">limitedOrElevated</span><span class="sxs-lookup"><span data-stu-id="4b481-136">limitedOrElevated</span></span> |<span data-ttu-id="4b481-137">**(Standard)**  Alla processer som rollen har åtkomst till hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="4b481-137">**(Default)** All role processes can access hello private key.</span></span> |
   | <span data-ttu-id="4b481-138">upphöjd</span><span class="sxs-lookup"><span data-stu-id="4b481-138">elevated</span></span> |<span data-ttu-id="4b481-139">Endast utökade processer kan komma åt hello privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="4b481-139">Only elevated processes can access hello private key.</span></span> |
2. <span data-ttu-id="4b481-140">Lägg till i din tjänstdefinitionsfilen en **InputEndpoint** element i hello **slutpunkter** avsnittet tooenable HTTPS:</span><span class="sxs-lookup"><span data-stu-id="4b481-140">In your service definition file, add an **InputEndpoint** element within hello **Endpoints** section tooenable HTTPS:</span></span>
   
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

3. <span data-ttu-id="4b481-141">Lägg till i din tjänstdefinitionsfilen en **bindning** element i hello **platser** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4b481-141">In your service definition file, add a **Binding** element within hello **Sites** section.</span></span> <span data-ttu-id="4b481-142">Det här avsnittet lägger till en HTTPS-bindningen toomap endpoint tooyour plats:</span><span class="sxs-lookup"><span data-stu-id="4b481-142">This section adds an HTTPS binding toomap the endpoint tooyour site:</span></span>
   
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
   
   <span data-ttu-id="4b481-143">Alla hello nödvändiga ändringar toohello tjänstdefinitionsfilen har slutförts, men du behöver fortfarande tooadd hello certifikatinformationen till hello tjänstkonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="4b481-143">All hello required changes toohello service definition file have been completed, but you still need tooadd hello certificate information to hello service configuration file.</span></span>
4. <span data-ttu-id="4b481-144">Lägg till i din tjänstekonfigurationsfil (CSCFG) ServiceConfiguration.Cloud.cscfg, en **certifikat** avsnitt i hello **rollen** avsnittet, ersätter hello tumavtrycket exempelvärde nedan med som ditt certifikat:</span><span class="sxs-lookup"><span data-stu-id="4b481-144">In your service configuration file (CSCFG), ServiceConfiguration.Cloud.cscfg, add a **Certificates** section within hello **Role** section, replacing hello sample thumbprint value shown below with that of your certificate:</span></span>
   
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

<span data-ttu-id="4b481-145">(hello föregående exempel använder **sha1** för hello tumavtrycket algoritmen.</span><span class="sxs-lookup"><span data-stu-id="4b481-145">(hello preceding example uses **sha1** for hello thumbprint algorithm.</span></span> <span data-ttu-id="4b481-146">Ange hello lämpligt värde för din certifikatets tumavtryck algoritm.)</span><span class="sxs-lookup"><span data-stu-id="4b481-146">Specify hello appropriate value for your certificate's thumbprint algorithm.)</span></span>

<span data-ttu-id="4b481-147">Nu när hello service definitions- och konfigurationsfiler har uppdaterats, paketera din distribution för uppladdning av tooAzure.</span><span class="sxs-lookup"><span data-stu-id="4b481-147">Now that hello service definition and service configuration files have been updated, package your deployment for uploading tooAzure.</span></span> <span data-ttu-id="4b481-148">Om du använder **cspack**, Använd inte den **/generateConfigurationFile** flagga som som skriver över certifikatinformationen du infogas.</span><span class="sxs-lookup"><span data-stu-id="4b481-148">If you are using **cspack**, don't use the **/generateConfigurationFile** flag, as that overwrites the certificate information you inserted.</span></span>

## <a name="step-3-upload-a-certificate"></a><span data-ttu-id="4b481-149">Steg 3: Överför certifikat</span><span class="sxs-lookup"><span data-stu-id="4b481-149">Step 3: Upload a certificate</span></span>
<span data-ttu-id="4b481-150">Distributionspaketet har uppdaterats toouse hello certifikat och en HTTPS-slutpunkt har lagts till.</span><span class="sxs-lookup"><span data-stu-id="4b481-150">Your deployment package has been updated toouse hello certificate, and an HTTPS endpoint has been added.</span></span> <span data-ttu-id="4b481-151">Nu kan du överföra hello och certifikat tooAzure med hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b481-151">Now you can upload hello package and certificate tooAzure with hello Azure classic portal.</span></span>

1. <span data-ttu-id="4b481-152">Logga in toohello [klassiska Azure-portalen][Azure classic portal].</span><span class="sxs-lookup"><span data-stu-id="4b481-152">Log in toohello [Azure classic portal][Azure classic portal].</span></span> 
2. <span data-ttu-id="4b481-153">Klicka på **molntjänster** hello vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="4b481-153">Click **Cloud Services** on hello left-side navigation pane.</span></span>
3. <span data-ttu-id="4b481-154">Klicka på hello önskad tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="4b481-154">Click hello desired cloud service.</span></span>
4. <span data-ttu-id="4b481-155">Klicka på hello **certifikat** fliken.</span><span class="sxs-lookup"><span data-stu-id="4b481-155">Click hello **Certificates** tab.</span></span>
   
    ![Klicka på fliken för hello-certifikat](./media/cloud-services-configure-ssl-certificate/click-cert.png)

5. <span data-ttu-id="4b481-157">Klicka på hello **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="4b481-157">Click hello **Upload** button.</span></span>
   
    ![Ladda upp](./media/cloud-services-configure-ssl-certificate/upload-button.png)
    
6. <span data-ttu-id="4b481-159">Ange hello **filen**, **lösenord**, klicka på **Slutför** (hello bock).</span><span class="sxs-lookup"><span data-stu-id="4b481-159">Provide hello **File**, **Password**, then click **Complete** (hello checkmark).</span></span>

## <a name="step-4-connect-toohello-role-instance-by-using-https"></a><span data-ttu-id="4b481-160">Steg 4: Anslut toohello rollinstansen med hjälp av HTTPS</span><span class="sxs-lookup"><span data-stu-id="4b481-160">Step 4: Connect toohello role instance by using HTTPS</span></span>
<span data-ttu-id="4b481-161">Nu när distributionen är igång och körs i Azure, kan du ansluta tooit med hjälp av HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4b481-161">Now that your deployment is up and running in Azure, you can connect tooit using HTTPS.</span></span>

1. <span data-ttu-id="4b481-162">I hello klassiska Azure-portalen, Välj distributionen och sedan på hello länken under **Webbadress**.</span><span class="sxs-lookup"><span data-stu-id="4b481-162">In hello Azure classic portal, select your deployment, then click hello link under **Site URL**.</span></span>
   
   ![Fastställa Webbadress][2]
2. <span data-ttu-id="4b481-164">Ändra hello länken toouse i webbläsaren, **https** i stället för **http**, och besök hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="4b481-164">In your web browser, modify hello link toouse **https** instead of **http**, and then visit hello page.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="4b481-165">Om du använder ett självsignerat certifikat när du bläddrar tooan HTTPS-slutpunkt som är kopplad till hello självsignerat certifikat kan du se ett certifikatfel i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="4b481-165">If you are using a self-signed certificate, when you browse tooan HTTPS endpoint that's associated with hello self-signed certificate you may see a certificate error in hello browser.</span></span> <span data-ttu-id="4b481-166">Använder ett certifikat som signerats av en betrodd certifikatutfärdare åtgärdar problemet. i hello tiden kan du ignorera hello felet.</span><span class="sxs-lookup"><span data-stu-id="4b481-166">Using a certificate signed by a trusted certification authority eliminates this problem; in hello meantime, you can ignore hello error.</span></span> <span data-ttu-id="4b481-167">(Ett annat alternativ är tooadd hello självsignerat certifikat toohello användarens certifikat från betrodd utfärdare certifikatarkiv.)</span><span class="sxs-lookup"><span data-stu-id="4b481-167">(Another option is tooadd hello self-signed certificate toohello user's trusted certificate authority certificate store.)</span></span>
   > 
   > 
   
   ![Webbplats för SSL-exempel][3]

<span data-ttu-id="4b481-169">Om du vill toouse SSL för en fristående distribution i stället för en Produktionsdistribution, måste du först toodetermine hello URL som används för hello mellanlagring av distribution.</span><span class="sxs-lookup"><span data-stu-id="4b481-169">If you want toouse SSL for a staging deployment instead of a production deployment, you first need toodetermine hello URL used for hello staging deployment.</span></span> <span data-ttu-id="4b481-170">Distribuera service toohello fristående molnmiljön utan ett certifikat eller information om certifikat.</span><span class="sxs-lookup"><span data-stu-id="4b481-170">Deploy your cloud service toohello staging environment without including a certificate or any certificate information.</span></span> <span data-ttu-id="4b481-171">Distribution kan du bestämma hello GUID-baserad URL, vilket visas i hello klassiska Azure-portalen **Webbadress** fältet.</span><span class="sxs-lookup"><span data-stu-id="4b481-171">Once deployed, you can determine hello GUID-based URL, which is listed in hello Azure classic portal's **Site URL** field.</span></span> <span data-ttu-id="4b481-172">Skapa ett certifikat med hello vanliga namn (CN) lika toohello GUID-baserad URL (till exempel **32818777 6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span><span class="sxs-lookup"><span data-stu-id="4b481-172">Create a certificate with hello common name (CN) equal toohello GUID-based URL (for example, **32818777-6e77-4ced-a8fc-57609d404462.cloudapp.net**).</span></span> <span data-ttu-id="4b481-173">Använd hello Azure klassiska portal tooadd hello certifikat tooyour mellanlagras tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="4b481-173">Use hello Azure classic portal tooadd hello certificate tooyour staged cloud service.</span></span> <span data-ttu-id="4b481-174">Lägg sedan till hello information tooyour CSDEF- och CSCFG certifikatfiler, paketera programmet och uppdatera mellanlagrade toouse hello nya distributionspaketet.</span><span class="sxs-lookup"><span data-stu-id="4b481-174">Then, add hello certificate information tooyour CSDEF and CSCFG files, repackage your application, and update your staged deployment toouse hello new package.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4b481-175">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4b481-175">Next steps</span></span>
* <span data-ttu-id="4b481-176">[Allmän konfiguration av Molntjänsten](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="4b481-176">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="4b481-177">Lär dig hur för[distribuera en tjänst i molnet](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="4b481-177">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="4b481-178">Konfigurera en [domännamn](cloud-services-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="4b481-178">Configure a [custom domain name](cloud-services-custom-domain-name.md).</span></span>
* <span data-ttu-id="4b481-179">[Hantera din molntjänst](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="4b481-179">[Manage your cloud service](cloud-services-how-to-manage.md).</span></span>

[Azure classic portal]: http://manage.windowsazure.com
[0]: ./media/cloud-services-configure-ssl-certificate/CreateCloudService.png
[1]: ./media/cloud-services-configure-ssl-certificate/AddCertificate.png
[2]: ./media/cloud-services-configure-ssl-certificate/CopyURL.png
[3]: ./media/cloud-services-configure-ssl-certificate/SSLCloudService.png
[4]: ./media/cloud-services-configure-ssl-certificate/AddCertificateComplete.png  
