---
title: "aaaCloud tjänster och hantering av certifikat | Microsoft Docs"
description: "Lär dig hur toocreate och använda certifikat med Microsoft Azure"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 69cb5467ece58a91dae06b4120954aeb2826bde1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a><span data-ttu-id="7a1e8-103">Översikt över certifikat för Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="7a1e8-103">Certificates overview for Azure Cloud Services</span></span>
<span data-ttu-id="7a1e8-104">Certifikat används i Azure för molntjänster ([tjänsten certifikat](#what-are-service-certificates)) och för att autentisera med hello management API ([hanteringscertifikat](#what-are-management-certificates) när med hello klassiska Azure-portalen och inte hello-klassisk Azure-portalen).</span><span class="sxs-lookup"><span data-stu-id="7a1e8-104">Certificates are used in Azure for cloud services ([service certificates](#what-are-service-certificates)) and for authenticating with hello management API ([management certificates](#what-are-management-certificates) when using hello Azure classic portal and not hello non-classic Azure portal).</span></span> <span data-ttu-id="7a1e8-105">Det här avsnittet ger en allmän översikt över båda typer av certifikat, hur för[skapa](#create) och [distribuera](#deploy) dem tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-105">This topic gives a general overview of both certificate types, how too[create](#create) and [deploy](#deploy) them tooAzure.</span></span>

<span data-ttu-id="7a1e8-106">Certifikat som används i Azure är x.509 v3-certifikat och kan vara signerat av en annan betrodda certifikat eller så kan de vara självsignerade.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-106">Certificates used in Azure are x.509 v3 certificates and can be signed by another trusted certificate or they can be self-signed.</span></span> <span data-ttu-id="7a1e8-107">Ett självsignerat certifikat som är signerat av en egen skapare, därför inte är betrodd som standard.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-107">A self-signed certificate is signed by its own creator, therefore it is not trusted by default.</span></span> <span data-ttu-id="7a1e8-108">De flesta webbläsare kan ignorera det här problemet.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-108">Most browsers can ignore this problem.</span></span> <span data-ttu-id="7a1e8-109">Du bör endast använda självsignerade certifikat när du utvecklar och testar dina molntjänster.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-109">You should only use self-signed certificates when developing and testing your cloud services.</span></span> 

<span data-ttu-id="7a1e8-110">Certifikat som används av Azure kan innehålla en privat eller offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-110">Certificates used by Azure can contain a private or a public key.</span></span> <span data-ttu-id="7a1e8-111">Certifikatet har ett tumavtryck som ger en innebär tooidentify dem på ett entydigt sätt.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-111">Certificates have a thumbprint that provides a means tooidentify them in an unambiguous way.</span></span> <span data-ttu-id="7a1e8-112">Det här tumavtrycket används i hello Azure [konfigurationsfilen](cloud-services-configure-ssl-certificate.md) tooidentify som en tjänst i molnet av certifikat ska använda.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-112">This thumbprint is used in hello Azure [configuration file](cloud-services-configure-ssl-certificate.md) tooidentify which certificate a cloud service should use.</span></span> 

## <a name="what-are-service-certificates"></a><span data-ttu-id="7a1e8-113">Vad är service-certifikat?</span><span class="sxs-lookup"><span data-stu-id="7a1e8-113">What are service certificates?</span></span>
<span data-ttu-id="7a1e8-114">Tjänstcertifikat som är bifogade toocloud tjänster och aktivera säker kommunikation tooand från hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-114">Service certificates are attached toocloud services and enable secure communication tooand from hello service.</span></span> <span data-ttu-id="7a1e8-115">Till exempel om du har distribuerat en webbroll kan toosupply ett certifikat som kan autentisera en exponerade HTTPS-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-115">For example, if you deployed a web role, you would want toosupply a certificate that can authenticate an exposed HTTPS endpoint.</span></span> <span data-ttu-id="7a1e8-116">Tjänstcertifikat som definierats i din tjänstdefinitionen är automatiskt distribuerade toohello virtuell dator som kör en instans av din roll.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-116">Service certificates, defined in your service definition, are automatically deployed toohello virtual machine that is running an instance of your role.</span></span> 

<span data-ttu-id="7a1e8-117">Du kan ladda upp tjänsten certifikat tooAzure klassisk portal antingen med hjälp av hello Azure klassiska portal eller genom att använda hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-117">You can upload service certificates tooAzure classic portal either using hello Azure classic portal or by using hello classic deployment model.</span></span> <span data-ttu-id="7a1e8-118">Tjänstcertifikat som är associerade med en specifik molnbaserad tjänst.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-118">Service certificates are associated with a specific cloud service.</span></span> <span data-ttu-id="7a1e8-119">De är tilldelade tooa distribution i hello tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-119">They are assigned tooa deployment in hello service definition file.</span></span>

<span data-ttu-id="7a1e8-120">Tjänstcertifikat kan hanteras separat från dina tjänster och kan hanteras av olika personer.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-120">Service certificates can be managed separately from your services, and may be managed by different individuals.</span></span> <span data-ttu-id="7a1e8-121">En utvecklare kan till exempel överföra ett servicepaket som refererar tooa certifikat att en IT-chef tidigare har överförts tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-121">For example, a developer may upload a service package that refers tooa certificate that an IT manager has previously uploaded tooAzure.</span></span> <span data-ttu-id="7a1e8-122">En IT-chef kan hantera och förnya certifikatet (ändra hello konfigurering av hello-tjänst) utan att behöva tooupload ett nytt tjänstepaket.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-122">An IT manager can manage and renew that certificate (changing hello configuration of hello service) without needing tooupload a new service package.</span></span> <span data-ttu-id="7a1e8-123">Det är möjligt att uppdatera utan ett nytt tjänstepaket eftersom hello logiskt namn och store-namn och placering för hello certifikatet tjänstdefinitionsfilen hello och när hello certifikatets tumavtryck har angetts i konfigurationsfilen för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-123">Updating without a new service package is possible because hello logical name, store name, and location of hello certificate is in hello service definition file and while hello certificate thumbprint is specified in hello service configuration file.</span></span> <span data-ttu-id="7a1e8-124">tooupdate hello certifikat, är det endast nödvändiga tooupload ett nytt certifikat och ändra hello tumavtrycket värdet i konfigurationsfilen för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-124">tooupdate hello certificate, it's only necessary tooupload a new certificate and change hello thumbprint value in hello service configuration file.</span></span>

>[!Note]
><span data-ttu-id="7a1e8-125">Hej [Cloud Services vanliga frågor och svar](cloud-services-faq.md) artikeln innehåller användbar information om certifikat.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-125">hello [Cloud Services FAQ](cloud-services-faq.md) article has some helpful information about certificates.</span></span>

## <a name="what-are-management-certificates"></a><span data-ttu-id="7a1e8-126">Vad är certifikat?</span><span class="sxs-lookup"><span data-stu-id="7a1e8-126">What are management certificates?</span></span>
<span data-ttu-id="7a1e8-127">Certifikat kan du tooauthenticate med hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-127">Management certificates allow you tooauthenticate with hello classic deployment model.</span></span> <span data-ttu-id="7a1e8-128">Många program och verktyg (till exempel Visual Studio eller hello Azure SDK) använda dessa certifikat tooautomate konfiguration och distribution av olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-128">Many programs and tools (such as Visual Studio or hello Azure SDK) use these certificates tooautomate configuration and deployment of various Azure services.</span></span> <span data-ttu-id="7a1e8-129">Dessa är inte verkligen relaterade toocloud tjänster.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-129">These are not really related toocloud services.</span></span> 

> [!WARNING]
> <span data-ttu-id="7a1e8-130">Var försiktig!</span><span class="sxs-lookup"><span data-stu-id="7a1e8-130">Be careful!</span></span> <span data-ttu-id="7a1e8-131">Dessa typer av certifikat att alla som autentiserar med dem toomanage hello prenumeration de hör till.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-131">These types of certificates allow anyone who authenticates with them toomanage hello subscription they are associated with.</span></span> 
> 
> 

### <a name="limitations"></a><span data-ttu-id="7a1e8-132">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="7a1e8-132">Limitations</span></span>
<span data-ttu-id="7a1e8-133">Det finns en gräns på 100 hanteringscertifikat per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-133">There is a limit of 100 management certificates per subscription.</span></span> <span data-ttu-id="7a1e8-134">Det finns en gräns på 100 hanteringscertifikat för alla prenumerationer under en viss tjänstadministratör användar-ID.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-134">There is also a limit of 100 management certificates for all subscriptions under a specific service administrator’s user ID.</span></span> <span data-ttu-id="7a1e8-135">Om hello användar-ID för hello kontoadministratör redan har använt tooadd 100 hanteringscertifikat och det finns flera certifikat måste du lägga till en medadministratör tooadd hello ytterligare certifikat.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-135">If hello user ID for hello account administrator has already been used tooadd 100 management certificates and there is a need for more certificates, you can add a co-administrator tooadd hello additional certificates.</span></span> 

<span data-ttu-id="7a1e8-136">Innan du lägger till fler än 100 certifikat finns i om du kan återanvända ett befintligt certifikat.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-136">Before adding more than 100 certificates, see if you can reuse an existing certificate.</span></span> <span data-ttu-id="7a1e8-137">Med hjälp av medadministratörer lägger till potentiellt onödig komplexitet tooyour process för hantering av certifikat.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-137">Using co-administrators adds potentially unneeded complexity tooyour certificate management process.</span></span>

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="7a1e8-138">Skapa ett nytt självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="7a1e8-138">Create a new self-signed certificate</span></span>
<span data-ttu-id="7a1e8-139">Du kan använda alla tillgängliga toocreate för verktyget ett självsignerat certifikat, så länge de följer toothese inställningar:</span><span class="sxs-lookup"><span data-stu-id="7a1e8-139">You can use any tool available toocreate a self-signed certificate as long as they adhere toothese settings:</span></span>

* <span data-ttu-id="7a1e8-140">Ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-140">An X.509 certificate.</span></span>
* <span data-ttu-id="7a1e8-141">Innehåller en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-141">Contains a private key.</span></span>
* <span data-ttu-id="7a1e8-142">Skapa för utbyte av nycklar (.pfx-fil).</span><span class="sxs-lookup"><span data-stu-id="7a1e8-142">Created for key exchange (.pfx file).</span></span>
* <span data-ttu-id="7a1e8-143">Ämnesnamn måste matcha hello domän används tooaccess hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-143">Subject name must match hello domain used tooaccess hello cloud service.</span></span>

    > <span data-ttu-id="7a1e8-144">Du kan inte skaffa ett SSL-certifikat för hello cloudapp.net (eller något Azure-relaterade) domän. hello certifikatets ämnesnamn måste matcha hello domänen namnet som används för tooaccess ditt program.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-144">You cannot acquire an SSL certificate for hello cloudapp.net (or for any Azure-related) domain; hello certificate's subject name must match hello custom domain name used tooaccess your application.</span></span> <span data-ttu-id="7a1e8-145">Till exempel **contoso.net**, inte **contoso.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-145">For example, **contoso.net**, not **contoso.cloudapp.net**.</span></span>

* <span data-ttu-id="7a1e8-146">Minst 2048-bitars kryptering.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-146">Minimum of 2048-bit encryption.</span></span>
* <span data-ttu-id="7a1e8-147">**Tjänsten certifikat endast**: klientsidan certifikatet måste finnas i hello *personliga* certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-147">**Service Certificate Only**: Client-side certificate must reside in hello *Personal* certificate store.</span></span>

<span data-ttu-id="7a1e8-148">Det finns två sätt toocreate ett certifikat i Windows, med hello `makecert.exe` verktyg eller IIS.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-148">There are two easy ways toocreate a certificate on Windows, with hello `makecert.exe` utility, or IIS.</span></span>

### <a name="makecertexe"></a><span data-ttu-id="7a1e8-149">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="7a1e8-149">Makecert.exe</span></span>
<span data-ttu-id="7a1e8-150">Det här verktyget är inaktuell och inte längre dokumenteras här.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-150">This utility has been deprecated and is no longer documented here.</span></span> <span data-ttu-id="7a1e8-151">Mer information finns i [MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span><span class="sxs-lookup"><span data-stu-id="7a1e8-151">For more information, see [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span></span>

### <a name="powershell"></a><span data-ttu-id="7a1e8-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7a1e8-152">PowerShell</span></span>
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> <span data-ttu-id="7a1e8-153">Om du vill toouse hello certifikat med en IP-adress i stället för en domän, kan du använda hello IP-adress i hello - DnsName-parametern.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-153">If you want toouse hello certificate with an IP address instead of a domain, use hello IP address in hello -DnsName parameter.</span></span>


<span data-ttu-id="7a1e8-154">Om du vill toouse [certifikat med hanteringsportalen hello](../azure-api-management-certs.md), exportera den tooa **.cer** fil:</span><span class="sxs-lookup"><span data-stu-id="7a1e8-154">If you want toouse this [certificate with hello management portal](../azure-api-management-certs.md), export it tooa **.cer** file:</span></span>

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a><span data-ttu-id="7a1e8-155">Internet Information Services (IIS)</span><span class="sxs-lookup"><span data-stu-id="7a1e8-155">Internet Information Services (IIS)</span></span>
<span data-ttu-id="7a1e8-156">Det finns många sidor på hello internet som beskriver hur toodo detta med IIS.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-156">There are many pages on hello internet that cover how toodo this with IIS.</span></span> <span data-ttu-id="7a1e8-157">[Här](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) är en bra jag hittade som tror förklarar det bra.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-157">[Here](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is a great one I found that I think explains it well.</span></span> 

### <a name="java"></a><span data-ttu-id="7a1e8-158">Java</span><span class="sxs-lookup"><span data-stu-id="7a1e8-158">Java</span></span>
<span data-ttu-id="7a1e8-159">Du kan använda Java för[skapa ett certifikat](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span><span class="sxs-lookup"><span data-stu-id="7a1e8-159">You can use Java too[create a certificate](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span></span>

### <a name="linux"></a><span data-ttu-id="7a1e8-160">Linux</span><span class="sxs-lookup"><span data-stu-id="7a1e8-160">Linux</span></span>
<span data-ttu-id="7a1e8-161">[Detta](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikeln beskriver hur toocreate certifikat med SSH.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-161">[This](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article describes how toocreate certificates with SSH.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a1e8-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7a1e8-162">Next steps</span></span>
<span data-ttu-id="7a1e8-163">[Ladda upp din tjänst certifikat toohello klassiska Azure-portalen](cloud-services-configure-ssl-certificate.md) (eller hello [Azure-portalen](cloud-services-configure-ssl-certificate-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="7a1e8-163">[Upload your service certificate toohello Azure classic portal](cloud-services-configure-ssl-certificate.md) (or hello [Azure portal](cloud-services-configure-ssl-certificate-portal.md)).</span></span>

<span data-ttu-id="7a1e8-164">Överför en [hanteringscertifikat API](../azure-api-management-certs.md) toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-164">Upload a [management API certificate](../azure-api-management-certs.md) toohello Azure classic portal.</span></span> <span data-ttu-id="7a1e8-165">hello Azure-portalen använder inte certifikat för autentisering.</span><span class="sxs-lookup"><span data-stu-id="7a1e8-165">hello Azure portal does not use management certificates for authentication.</span></span>

