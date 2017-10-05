---
title: "Molntjänster och hanteringscertifikat | Microsoft Docs"
description: "Lär dig hur du skapar och använder certifikat med Microsoft Azure"
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
ms.openlocfilehash: f760bfd93b19c43d12889b5dd38015c5eba0a8ac
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a><span data-ttu-id="15584-103">Översikt över certifikat för Azure-molntjänster</span><span class="sxs-lookup"><span data-stu-id="15584-103">Certificates overview for Azure Cloud Services</span></span>
<span data-ttu-id="15584-104">Certifikat används i Azure för molntjänster ([tjänsten certifikat](#what-are-service-certificates)) och för att autentisera med management API ([hanteringscertifikat](#what-are-management-certificates) när du använder den klassiska Azure-portalen och inte den icke-klassisk Azure-portalen).</span><span class="sxs-lookup"><span data-stu-id="15584-104">Certificates are used in Azure for cloud services ([service certificates](#what-are-service-certificates)) and for authenticating with the management API ([management certificates](#what-are-management-certificates) when using the Azure classic portal and not the non-classic Azure portal).</span></span> <span data-ttu-id="15584-105">Det här avsnittet ger en allmän översikt över båda typer av certifikat, hur till [skapa](#create) och [distribuera](#deploy) dem till Azure.</span><span class="sxs-lookup"><span data-stu-id="15584-105">This topic gives a general overview of both certificate types, how to [create](#create) and [deploy](#deploy) them to Azure.</span></span>

<span data-ttu-id="15584-106">Certifikat som används i Azure är x.509 v3-certifikat och kan vara signerat av en annan betrodda certifikat eller så kan de vara självsignerade.</span><span class="sxs-lookup"><span data-stu-id="15584-106">Certificates used in Azure are x.509 v3 certificates and can be signed by another trusted certificate or they can be self-signed.</span></span> <span data-ttu-id="15584-107">Ett självsignerat certifikat som är signerat av en egen skapare, därför inte är betrodd som standard.</span><span class="sxs-lookup"><span data-stu-id="15584-107">A self-signed certificate is signed by its own creator, therefore it is not trusted by default.</span></span> <span data-ttu-id="15584-108">De flesta webbläsare kan ignorera det här problemet.</span><span class="sxs-lookup"><span data-stu-id="15584-108">Most browsers can ignore this problem.</span></span> <span data-ttu-id="15584-109">Du bör endast använda självsignerade certifikat när du utvecklar och testar dina molntjänster.</span><span class="sxs-lookup"><span data-stu-id="15584-109">You should only use self-signed certificates when developing and testing your cloud services.</span></span> 

<span data-ttu-id="15584-110">Certifikat som används av Azure kan innehålla en privat eller offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="15584-110">Certificates used by Azure can contain a private or a public key.</span></span> <span data-ttu-id="15584-111">Certifikatet har ett tumavtryck som ett sätt att identifiera dem på ett entydigt sätt.</span><span class="sxs-lookup"><span data-stu-id="15584-111">Certificates have a thumbprint that provides a means to identify them in an unambiguous way.</span></span> <span data-ttu-id="15584-112">Det här tumavtrycket används i Azure [konfigurationsfilen](cloud-services-configure-ssl-certificate.md) för att identifiera vilket certifikat som en tjänst i molnet ska använda.</span><span class="sxs-lookup"><span data-stu-id="15584-112">This thumbprint is used in the Azure [configuration file](cloud-services-configure-ssl-certificate.md) to identify which certificate a cloud service should use.</span></span> 

## <a name="what-are-service-certificates"></a><span data-ttu-id="15584-113">Vad är service-certifikat?</span><span class="sxs-lookup"><span data-stu-id="15584-113">What are service certificates?</span></span>
<span data-ttu-id="15584-114">Tjänstcertifikat som är kopplade till molntjänster och aktivera säker kommunikation till och från tjänsten.</span><span class="sxs-lookup"><span data-stu-id="15584-114">Service certificates are attached to cloud services and enable secure communication to and from the service.</span></span> <span data-ttu-id="15584-115">Om du har distribuerat en webbroll skulle du till exempel vill ange ett certifikat som kan autentisera en exponerade HTTPS-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="15584-115">For example, if you deployed a web role, you would want to supply a certificate that can authenticate an exposed HTTPS endpoint.</span></span> <span data-ttu-id="15584-116">Tjänstcertifikat som definierats i din tjänstdefinitionen distribueras automatiskt till den virtuella datorn som kör en instans av din roll.</span><span class="sxs-lookup"><span data-stu-id="15584-116">Service certificates, defined in your service definition, are automatically deployed to the virtual machine that is running an instance of your role.</span></span> 

<span data-ttu-id="15584-117">Du kan överföra tjänstcertifikat till klassiska Azure-portalen antingen med hjälp av den klassiska Azure-portalen eller genom att använda den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="15584-117">You can upload service certificates to Azure classic portal either using the Azure classic portal or by using the classic deployment model.</span></span> <span data-ttu-id="15584-118">Tjänstcertifikat som är associerade med en specifik molnbaserad tjänst.</span><span class="sxs-lookup"><span data-stu-id="15584-118">Service certificates are associated with a specific cloud service.</span></span> <span data-ttu-id="15584-119">De är tilldelade till en distribution i tjänstdefinitionsfilen.</span><span class="sxs-lookup"><span data-stu-id="15584-119">They are assigned to a deployment in the service definition file.</span></span>

<span data-ttu-id="15584-120">Tjänstcertifikat kan hanteras separat från dina tjänster och kan hanteras av olika personer.</span><span class="sxs-lookup"><span data-stu-id="15584-120">Service certificates can be managed separately from your services, and may be managed by different individuals.</span></span> <span data-ttu-id="15584-121">En utvecklare kan till exempel överföra en tjänstpaket som refererar till ett certifikat som en IT-chef tidigare har överförts till Azure.</span><span class="sxs-lookup"><span data-stu-id="15584-121">For example, a developer may upload a service package that refers to a certificate that an IT manager has previously uploaded to Azure.</span></span> <span data-ttu-id="15584-122">En IT-chef kan hantera och förnya certifikatet (ändra konfigurationen av tjänsten) utan att behöva ladda upp ett nytt tjänstepaket.</span><span class="sxs-lookup"><span data-stu-id="15584-122">An IT manager can manage and renew that certificate (changing the configuration of the service) without needing to upload a new service package.</span></span> <span data-ttu-id="15584-123">Det är möjligt att uppdatera utan ett nytt tjänstepaket eftersom logiska namn, store-namn och plats för certifikatet tjänstdefinitionsfilen och när certifikatets tumavtryck har angetts i tjänstkonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="15584-123">Updating without a new service package is possible because the logical name, store name, and location of the certificate is in the service definition file and while the certificate thumbprint is specified in the service configuration file.</span></span> <span data-ttu-id="15584-124">Om du vill uppdatera certifikatet krävs endast att överföra ett nytt certifikat och ändra tumavtrycksvärde i tjänstekonfigurationsfilen.</span><span class="sxs-lookup"><span data-stu-id="15584-124">To update the certificate, it's only necessary to upload a new certificate and change the thumbprint value in the service configuration file.</span></span>

>[!Note]
><span data-ttu-id="15584-125">Den [Cloud Services vanliga frågor och svar](cloud-services-faq.md) artikeln innehåller användbar information om certifikat.</span><span class="sxs-lookup"><span data-stu-id="15584-125">The [Cloud Services FAQ](cloud-services-faq.md) article has some helpful information about certificates.</span></span>

## <a name="what-are-management-certificates"></a><span data-ttu-id="15584-126">Vad är certifikat?</span><span class="sxs-lookup"><span data-stu-id="15584-126">What are management certificates?</span></span>
<span data-ttu-id="15584-127">Certifikat kan du autentisera med den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="15584-127">Management certificates allow you to authenticate with the classic deployment model.</span></span> <span data-ttu-id="15584-128">Många program och verktyg (till exempel Visual Studio eller Azure SDK) kan du använda dessa certifikat för att automatisera konfigurationen och distributionen av olika Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="15584-128">Many programs and tools (such as Visual Studio or the Azure SDK) use these certificates to automate configuration and deployment of various Azure services.</span></span> <span data-ttu-id="15584-129">Dessa är inte relaterade till molntjänster verkligen.</span><span class="sxs-lookup"><span data-stu-id="15584-129">These are not really related to cloud services.</span></span> 

> [!WARNING]
> <span data-ttu-id="15584-130">Var försiktig!</span><span class="sxs-lookup"><span data-stu-id="15584-130">Be careful!</span></span> <span data-ttu-id="15584-131">Dessa typer av certifikat kan alla som autentiserar med dem för att hantera de är associerade med prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="15584-131">These types of certificates allow anyone who authenticates with them to manage the subscription they are associated with.</span></span> 
> 
> 

### <a name="limitations"></a><span data-ttu-id="15584-132">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="15584-132">Limitations</span></span>
<span data-ttu-id="15584-133">Det finns en gräns på 100 hanteringscertifikat per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="15584-133">There is a limit of 100 management certificates per subscription.</span></span> <span data-ttu-id="15584-134">Det finns en gräns på 100 hanteringscertifikat för alla prenumerationer under en viss tjänstadministratör användar-ID.</span><span class="sxs-lookup"><span data-stu-id="15584-134">There is also a limit of 100 management certificates for all subscriptions under a specific service administrator’s user ID.</span></span> <span data-ttu-id="15584-135">Om användar-ID för kontoadministratören har redan använts för att lägga till 100 hanteringscertifikat och det finns flera certifikat måste du lägga till en medadministratör för att lägga till ytterligare certifikat.</span><span class="sxs-lookup"><span data-stu-id="15584-135">If the user ID for the account administrator has already been used to add 100 management certificates and there is a need for more certificates, you can add a co-administrator to add the additional certificates.</span></span> 

<span data-ttu-id="15584-136">Innan du lägger till fler än 100 certifikat finns i om du kan återanvända ett befintligt certifikat.</span><span class="sxs-lookup"><span data-stu-id="15584-136">Before adding more than 100 certificates, see if you can reuse an existing certificate.</span></span> <span data-ttu-id="15584-137">Med hjälp av medadministratörer lägger till potentiellt onödig komplexitet process för hantering av certifikat.</span><span class="sxs-lookup"><span data-stu-id="15584-137">Using co-administrators adds potentially unneeded complexity to your certificate management process.</span></span>

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="15584-138">Skapa ett nytt självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="15584-138">Create a new self-signed certificate</span></span>
<span data-ttu-id="15584-139">Du kan använda ett verktyg som är tillgängliga för att skapa ett självsignerat certifikat, så länge de följer dessa inställningar:</span><span class="sxs-lookup"><span data-stu-id="15584-139">You can use any tool available to create a self-signed certificate as long as they adhere to these settings:</span></span>

* <span data-ttu-id="15584-140">Ett X.509-certifikat.</span><span class="sxs-lookup"><span data-stu-id="15584-140">An X.509 certificate.</span></span>
* <span data-ttu-id="15584-141">Innehåller en privat nyckel.</span><span class="sxs-lookup"><span data-stu-id="15584-141">Contains a private key.</span></span>
* <span data-ttu-id="15584-142">Skapa för utbyte av nycklar (.pfx-fil).</span><span class="sxs-lookup"><span data-stu-id="15584-142">Created for key exchange (.pfx file).</span></span>
* <span data-ttu-id="15584-143">Ämnesnamn måste matcha den domän som används för åtkomst till Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="15584-143">Subject name must match the domain used to access the cloud service.</span></span>

    > <span data-ttu-id="15584-144">Du kan inte skaffa ett SSL-certifikat för cloudapp.net (eller något Azure-relaterade) domän. certifikatets ämnesnamn måste matcha det anpassade domännamnet som används för åtkomst till ditt program.</span><span class="sxs-lookup"><span data-stu-id="15584-144">You cannot acquire an SSL certificate for the cloudapp.net (or for any Azure-related) domain; the certificate's subject name must match the custom domain name used to access your application.</span></span> <span data-ttu-id="15584-145">Till exempel **contoso.net**, inte **contoso.cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="15584-145">For example, **contoso.net**, not **contoso.cloudapp.net**.</span></span>

* <span data-ttu-id="15584-146">Minst 2048-bitars kryptering.</span><span class="sxs-lookup"><span data-stu-id="15584-146">Minimum of 2048-bit encryption.</span></span>
* <span data-ttu-id="15584-147">**Tjänsten certifikat endast**: klientsidan certifikatet måste finnas i den *personliga* certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="15584-147">**Service Certificate Only**: Client-side certificate must reside in the *Personal* certificate store.</span></span>

<span data-ttu-id="15584-148">Det finns två enkelt sätt att skapa ett certifikat i Windows, med den `makecert.exe` verktyg eller IIS.</span><span class="sxs-lookup"><span data-stu-id="15584-148">There are two easy ways to create a certificate on Windows, with the `makecert.exe` utility, or IIS.</span></span>

### <a name="makecertexe"></a><span data-ttu-id="15584-149">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="15584-149">Makecert.exe</span></span>
<span data-ttu-id="15584-150">Det här verktyget är inaktuell och inte längre dokumenteras här.</span><span class="sxs-lookup"><span data-stu-id="15584-150">This utility has been deprecated and is no longer documented here.</span></span> <span data-ttu-id="15584-151">Mer information finns i [MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span><span class="sxs-lookup"><span data-stu-id="15584-151">For more information, see [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa386968).</span></span>

### <a name="powershell"></a><span data-ttu-id="15584-152">PowerShell</span><span class="sxs-lookup"><span data-stu-id="15584-152">PowerShell</span></span>
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> <span data-ttu-id="15584-153">Om du vill använda ett certifikat med en IP-adress i stället för en domän kan du använda IP-adressen i parametern - DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="15584-153">If you want to use the certificate with an IP address instead of a domain, use the IP address in the -DnsName parameter.</span></span>


<span data-ttu-id="15584-154">Om du vill använda den här [certifikat med hanteringsportalen](../azure-api-management-certs.md), exportera den till en **.cer** fil:</span><span class="sxs-lookup"><span data-stu-id="15584-154">If you want to use this [certificate with the management portal](../azure-api-management-certs.md), export it to a **.cer** file:</span></span>

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a><span data-ttu-id="15584-155">Internet Information Services (IIS)</span><span class="sxs-lookup"><span data-stu-id="15584-155">Internet Information Services (IIS)</span></span>
<span data-ttu-id="15584-156">Det finns många sidor på internet som beskriver hur du gör detta med IIS.</span><span class="sxs-lookup"><span data-stu-id="15584-156">There are many pages on the internet that cover how to do this with IIS.</span></span> <span data-ttu-id="15584-157">[Här](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) är en bra jag hittade som tror förklarar det bra.</span><span class="sxs-lookup"><span data-stu-id="15584-157">[Here](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) is a great one I found that I think explains it well.</span></span> 

### <a name="java"></a><span data-ttu-id="15584-158">Java</span><span class="sxs-lookup"><span data-stu-id="15584-158">Java</span></span>
<span data-ttu-id="15584-159">Du kan använda Java till [skapa ett certifikat](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span><span class="sxs-lookup"><span data-stu-id="15584-159">You can use Java to [create a certificate](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).</span></span>

### <a name="linux"></a><span data-ttu-id="15584-160">Linux</span><span class="sxs-lookup"><span data-stu-id="15584-160">Linux</span></span>
<span data-ttu-id="15584-161">[Detta](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikeln beskriver hur du skapar certifikat med SSH.</span><span class="sxs-lookup"><span data-stu-id="15584-161">[This](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) article describes how to create certificates with SSH.</span></span>

## <a name="next-steps"></a><span data-ttu-id="15584-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="15584-162">Next steps</span></span>
<span data-ttu-id="15584-163">[Överför din tjänstcertifikat till den klassiska Azure-portalen](cloud-services-configure-ssl-certificate.md) (eller [Azure-portalen](cloud-services-configure-ssl-certificate-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="15584-163">[Upload your service certificate to the Azure classic portal](cloud-services-configure-ssl-certificate.md) (or the [Azure portal](cloud-services-configure-ssl-certificate-portal.md)).</span></span>

<span data-ttu-id="15584-164">Överför en [hanteringscertifikat API](../azure-api-management-certs.md) till den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="15584-164">Upload a [management API certificate](../azure-api-management-certs.md) to the Azure classic portal.</span></span> <span data-ttu-id="15584-165">Azure-portalen använder inte certifikat för autentisering.</span><span class="sxs-lookup"><span data-stu-id="15584-165">The Azure portal does not use management certificates for authentication.</span></span>

