---
title: "Hur du använder service management API (Python) - Funktionsguide"
description: "Lär dig hur du programmässigt utföra vanliga hanteringsuppgifter för tjänsten från Python."
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: 13249ba9a4b317a3154776b411ce0bb1f316b3bb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-service-management-from-python"></a><span data-ttu-id="f5e22-103">Hur du använder Service Management från Python</span><span class="sxs-lookup"><span data-stu-id="f5e22-103">How to use Service Management from Python</span></span>
<span data-ttu-id="f5e22-104">Den här guiden visar hur du programmässigt utföra vanliga hanteringsuppgifter för tjänsten från Python.</span><span class="sxs-lookup"><span data-stu-id="f5e22-104">This guide shows you how to programmatically perform common service management tasks from Python.</span></span> <span data-ttu-id="f5e22-105">Den **ServiceManagementService** klassen i den [Azure SDK för Python](https://github.com/Azure/azure-sdk-for-python) stöder Programmeringsåtkomst till stor del av service management-relaterade funktioner som är tillgängliga i den [Azure klassiska portalen] [ management-portal] (exempelvis **skapa, uppdatera och ta bort molntjänster, distributioner, tjänster för data och virtuella datorer**).</span><span class="sxs-lookup"><span data-stu-id="f5e22-105">The **ServiceManagementService** class in the [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) supports programmatic access to much of the service management-related functionality that is available in the [Azure classic portal][management-portal] (such as **creating, updating, and deleting cloud services, deployments, data management services, and virtual machines**).</span></span> <span data-ttu-id="f5e22-106">Den här funktionen kan vara användbar vid utveckling av program som behöver programmatisk åtkomst till service management.</span><span class="sxs-lookup"><span data-stu-id="f5e22-106">This functionality can be useful in building applications that need programmatic access to service management.</span></span>

## <span data-ttu-id="f5e22-107"><a name="WhatIs"></a>Vad är Service Management</span><span class="sxs-lookup"><span data-stu-id="f5e22-107"><a name="WhatIs"> </a>What is Service Management</span></span>
<span data-ttu-id="f5e22-108">Service Management API som ger programmatisk åtkomst till stor del av hanteringsfunktioner för tjänsten via den [klassiska Azure-portalen][management-portal].</span><span class="sxs-lookup"><span data-stu-id="f5e22-108">The Service Management API provides programmatic access to much of the service management functionality available through the [Azure classic portal][management-portal].</span></span> <span data-ttu-id="f5e22-109">Azure SDK för Python kan du hantera dina molntjänster och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="f5e22-109">The Azure SDK for Python allows you to manage your cloud services and storage accounts.</span></span>

<span data-ttu-id="f5e22-110">Service Management API du vill använda, [skapa ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f5e22-110">To use the Service Management API, you need to [create an Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <span data-ttu-id="f5e22-111"><a name="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="f5e22-111"><a name="Concepts"> </a>Concepts</span></span>
<span data-ttu-id="f5e22-112">Azure SDK för Python radbryts i [Azure Service Management API][svc-mgmt-rest-api], vilket är en REST-API.</span><span class="sxs-lookup"><span data-stu-id="f5e22-112">The Azure SDK for Python wraps the [Azure Service Management API][svc-mgmt-rest-api], which is a REST API.</span></span> <span data-ttu-id="f5e22-113">Alla API-operationer utförs över SSL och autentiserar varandra med X.509 v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="f5e22-113">All API operations are performed over SSL and mutually authenticated using X.509 v3 certificates.</span></span> <span data-ttu-id="f5e22-114">Hanteringstjänsten kan nås inifrån en tjänst som körs i Azure eller direkt över Internet från alla program som kan skicka en HTTPS-begäran och ta emot HTTPS-svar.</span><span class="sxs-lookup"><span data-stu-id="f5e22-114">The management service may be accessed from within a service running in Azure, or directly over the Internet from any application that can send an HTTPS request and receive an HTTPS response.</span></span>

## <span data-ttu-id="f5e22-115"><a name="Installation"></a>Installation</span><span class="sxs-lookup"><span data-stu-id="f5e22-115"><a name="Installation"> </a>Installation</span></span>
<span data-ttu-id="f5e22-116">De funktioner som beskrivs i den här artikeln är tillgängliga i den `azure-servicemanagement-legacy` paket som du kan installera med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="f5e22-116">All the features described in this article are available in the `azure-servicemanagement-legacy` package, which you can install using pip.</span></span> <span data-ttu-id="f5e22-117">Mer information om installation (till exempel om du har använt Python) finns i den här artikeln: [installerar Python och Azure SDK](../python-how-to-install.md)</span><span class="sxs-lookup"><span data-stu-id="f5e22-117">For more information about installation (for example, if you are new to Python), see this article: [Installing Python and the Azure SDK](../python-how-to-install.md)</span></span>

## <span data-ttu-id="f5e22-118"><a name="Connect"></a>Så här: ansluta till service-hantering</span><span class="sxs-lookup"><span data-stu-id="f5e22-118"><a name="Connect"> </a>How to: Connect to service management</span></span>
<span data-ttu-id="f5e22-119">Om du vill ansluta till Service Management-slutpunkten måste Azure prenumerations-ID och ett giltigt certifikat.</span><span class="sxs-lookup"><span data-stu-id="f5e22-119">To connect to the Service Management endpoint, you need your Azure subscription ID and a valid management certificate.</span></span> <span data-ttu-id="f5e22-120">Du kan hämta ditt prenumerations-ID via den [klassiska Azure-portalen][management-portal].</span><span class="sxs-lookup"><span data-stu-id="f5e22-120">You can obtain your subscription ID through the [Azure classic portal][management-portal].</span></span>

> [!NOTE]
> <span data-ttu-id="f5e22-121">Nu är det möjligt att använda certifikat som skapas med OpenSSL när de körs på Windows.</span><span class="sxs-lookup"><span data-stu-id="f5e22-121">It is now possible to use certificates created with OpenSSL when running on Windows.</span></span>  <span data-ttu-id="f5e22-122">Det krävs Python 2.7.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f5e22-122">It requires Python 2.7.4 or later.</span></span> <span data-ttu-id="f5e22-123">Vi rekommenderar att användarna kan använda OpenSSL i stället för PFX, eftersom stöd för PFX-certifikat kommer troligen att tas bort i framtiden.</span><span class="sxs-lookup"><span data-stu-id="f5e22-123">We recommend users to use OpenSSL instead of .pfx, since support for .pfx certificates will likely be removed in the future.</span></span>
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a><span data-ttu-id="f5e22-124">Management-certifikat på Windows-/ Mac/Linux (OpenSSL)</span><span class="sxs-lookup"><span data-stu-id="f5e22-124">Management certificates on Windows/Mac/Linux (OpenSSL)</span></span>
<span data-ttu-id="f5e22-125">Du kan använda [OpenSSL](http://www.openssl.org/) att skapa management-certifikat.</span><span class="sxs-lookup"><span data-stu-id="f5e22-125">You can use [OpenSSL](http://www.openssl.org/) to create your management certificate.</span></span>  <span data-ttu-id="f5e22-126">Faktiskt måste du skapa två certifikat, en för servern (en `.cer` filen) och en för klienten (en `.pem` fil).</span><span class="sxs-lookup"><span data-stu-id="f5e22-126">You actually need to create two certificates, one for the server (a `.cer` file) and one for the client (a `.pem` file).</span></span> <span data-ttu-id="f5e22-127">Att skapa den `.pem` fil, kör:</span><span class="sxs-lookup"><span data-stu-id="f5e22-127">To create the `.pem` file, execute:</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="f5e22-128">Att skapa den `.cer` certifikat, kör:</span><span class="sxs-lookup"><span data-stu-id="f5e22-128">To create the `.cer` certificate, execute:</span></span>

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

<span data-ttu-id="f5e22-129">Läs mer om Azure certifikat [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="f5e22-129">For more information about Azure certificates, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="f5e22-130">En fullständig beskrivning av OpenSSL parametrar finns i dokumentationen på [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span><span class="sxs-lookup"><span data-stu-id="f5e22-130">For a complete description of OpenSSL parameters, see the documentation at [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span></span>

<span data-ttu-id="f5e22-131">När du har skapat de här filerna måste du ladda upp den `.cer` filen till Azure via ”överför”-åtgärd på fliken ”Inställningar” i den [klassiska Azure-portalen][management-portal], och du behöver anteckna var du Spara den `.pem` filen.</span><span class="sxs-lookup"><span data-stu-id="f5e22-131">After you have created these files, you need to upload the `.cer` file to Azure via the "Upload" action of the "Settings" tab of the [Azure classic portal][management-portal], and you need to make note of where you saved the `.pem` file.</span></span>

<span data-ttu-id="f5e22-132">När du har fått ditt prenumerations-ID, skapas ett certifikat och överföra den `.cer` filen till Azure, som du kan ansluta till Azure management-slutpunkten genom att skicka prenumerations-id och sökvägen till den `.pem` filen till  **ServiceManagementService**:</span><span class="sxs-lookup"><span data-stu-id="f5e22-132">After you have obtained your subscription ID, created a certificate, and uploaded the `.cer` file to Azure, you can connect to the Azure management endpoint by passing the subscription id and the path to the `.pem` file to **ServiceManagementService**:</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="f5e22-133">I föregående exempel `sms` är en **ServiceManagementService** objekt.</span><span class="sxs-lookup"><span data-stu-id="f5e22-133">In the preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="f5e22-134">Den **ServiceManagementService** klassen är den primära klassen som används för att hantera Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f5e22-134">The **ServiceManagementService** class is the primary class used to manage Azure services.</span></span>

### <a name="management-certificates-on-windows-makecert"></a><span data-ttu-id="f5e22-135">Hanteringscertifikat i Windows (MakeCert)</span><span class="sxs-lookup"><span data-stu-id="f5e22-135">Management certificates on Windows (MakeCert)</span></span>
<span data-ttu-id="f5e22-136">Du kan skapa ett självsignerat certifikat på datorn med hjälp av `makecert.exe`.</span><span class="sxs-lookup"><span data-stu-id="f5e22-136">You can create a self-signed management certificate on your machine using `makecert.exe`.</span></span>  <span data-ttu-id="f5e22-137">Öppna en **Kommandotolken Visual Studio** som en **administratör** och använder du följande kommando ersätter *AzureCertificate* med certifikatets namn som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="f5e22-137">Open a **Visual Studio command prompt** as an **administrator** and use the following command, replacing *AzureCertificate* with the certificate name you would like to use.</span></span>

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

<span data-ttu-id="f5e22-138">Kommandot skapar den `.cer` filen och installerar den i den **personliga** certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="f5e22-138">The command creates the `.cer` file, and installs it in the **Personal** certificate store.</span></span> <span data-ttu-id="f5e22-139">Mer information finns i [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="f5e22-139">For more information, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span>

<span data-ttu-id="f5e22-140">När du har skapat certifikatet, måste du ladda upp den `.cer` filen till Azure via ”överför”-åtgärd på fliken ”Inställningar” i den [klassiska Azure-portalen][management-portal].</span><span class="sxs-lookup"><span data-stu-id="f5e22-140">After you have created the certificate, you need to upload the `.cer` file to Azure via the "Upload" action of the "Settings" tab of the [Azure classic portal][management-portal].</span></span>

<span data-ttu-id="f5e22-141">När du har fått ditt prenumerations-ID, skapas ett certifikat och överföra den `.cer` filen till Azure, som du kan ansluta till Azure management-slutpunkten genom att skicka prenumerations-id och platsen för certifikatet i din **personliga**  certifikatarkiv **ServiceManagementService** (igen och Ersätt *AzureCertificate* med namnet på ditt certifikat):</span><span class="sxs-lookup"><span data-stu-id="f5e22-141">After you have obtained your subscription ID, created a certificate, and uploaded the `.cer` file to Azure, you can connect to the Azure management endpoint by passing the subscription id and the location of the certificate in your **Personal** certificate store to **ServiceManagementService** (again, replace *AzureCertificate* with the name of your certificate):</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="f5e22-142">I föregående exempel `sms` är en **ServiceManagementService** objekt.</span><span class="sxs-lookup"><span data-stu-id="f5e22-142">In the preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="f5e22-143">Den **ServiceManagementService** klassen är den primära klassen som används för att hantera Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="f5e22-143">The **ServiceManagementService** class is the primary class used to manage Azure services.</span></span>

## <span data-ttu-id="f5e22-144"><a name="ListAvailableLocations"></a>Så här: visa en lista över tillgängliga platser</span><span class="sxs-lookup"><span data-stu-id="f5e22-144"><a name="ListAvailableLocations"> </a>How to: List available locations</span></span>
<span data-ttu-id="f5e22-145">Om du vill visa de platser som är tillgängliga för värdtjänster använder den **lista\_platser** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-145">To list the locations that are available for hosting services, use the **list\_locations** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

<span data-ttu-id="f5e22-146">När du skapar en tjänst i molnet eller storage-tjänst som du måste ange en giltig plats.</span><span class="sxs-lookup"><span data-stu-id="f5e22-146">When you create a cloud service or storage service you need to provide a valid location.</span></span> <span data-ttu-id="f5e22-147">Den **lista\_platser** -metoden returnerar alltid en aktuell lista över tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="f5e22-147">The **list\_locations** method always returns an up-to-date list of the currently available locations.</span></span> <span data-ttu-id="f5e22-148">När detta skrivs är tillgängliga platser:</span><span class="sxs-lookup"><span data-stu-id="f5e22-148">As of this writing, the available locations are:</span></span>

* <span data-ttu-id="f5e22-149">Västra Europa</span><span class="sxs-lookup"><span data-stu-id="f5e22-149">West Europe</span></span>
* <span data-ttu-id="f5e22-150">Norra Europa</span><span class="sxs-lookup"><span data-stu-id="f5e22-150">North Europe</span></span>
* <span data-ttu-id="f5e22-151">Sydostasien</span><span class="sxs-lookup"><span data-stu-id="f5e22-151">Southeast Asia</span></span>
* <span data-ttu-id="f5e22-152">Östasien</span><span class="sxs-lookup"><span data-stu-id="f5e22-152">East Asia</span></span>
* <span data-ttu-id="f5e22-153">Centrala USA</span><span class="sxs-lookup"><span data-stu-id="f5e22-153">Central US</span></span>
* <span data-ttu-id="f5e22-154">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="f5e22-154">North Central US</span></span>
* <span data-ttu-id="f5e22-155">Södra centrala USA</span><span class="sxs-lookup"><span data-stu-id="f5e22-155">South Central US</span></span>
* <span data-ttu-id="f5e22-156">Västra USA</span><span class="sxs-lookup"><span data-stu-id="f5e22-156">West US</span></span>
* <span data-ttu-id="f5e22-157">Östra USA</span><span class="sxs-lookup"><span data-stu-id="f5e22-157">East US</span></span>
* <span data-ttu-id="f5e22-158">Östra Japan</span><span class="sxs-lookup"><span data-stu-id="f5e22-158">Japan East</span></span>
* <span data-ttu-id="f5e22-159">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="f5e22-159">Japan West</span></span>
* <span data-ttu-id="f5e22-160">Södra Brasilien</span><span class="sxs-lookup"><span data-stu-id="f5e22-160">Brazil South</span></span>
* <span data-ttu-id="f5e22-161">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="f5e22-161">Australia East</span></span>
* <span data-ttu-id="f5e22-162">Sydöstra Australien</span><span class="sxs-lookup"><span data-stu-id="f5e22-162">Australia Southeast</span></span>

## <span data-ttu-id="f5e22-163"><a name="CreateCloudService"></a>Så här: skapa en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="f5e22-163"><a name="CreateCloudService"> </a>How to: Create a cloud service</span></span>
<span data-ttu-id="f5e22-164">När du skapar ett program och kör det i Azure, koden och konfigurationen tillsammans kallas för en Azure [Molntjänsten] [ cloud service] (kallas även en *värdtjänsten* i tidigare Azure versioner).</span><span class="sxs-lookup"><span data-stu-id="f5e22-164">When you create an application and run it in Azure, the code and configuration together are called an Azure [cloud service][cloud service] (known as a *hosted service* in earlier Azure releases).</span></span> <span data-ttu-id="f5e22-165">Den **skapa\_finns\_service** metod kan du skapa en ny värdbaserad tjänst genom att tillhandahålla en värdtjänst namnet (måste vara unikt i Azure), en etikett (kodad automatiskt till base64), en beskrivning och en plats.</span><span class="sxs-lookup"><span data-stu-id="f5e22-165">The **create\_hosted\_service** method allows you to create a new hosted service by providing a hosted service name (which must be unique in Azure), a label (automatically encoded to base64), a description, and a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

<span data-ttu-id="f5e22-166">Du kan visa en lista med alla värdbaserade tjänster för din prenumeration med den **lista\_finns\_services** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-166">You can list all the hosted services for your subscription with the **list\_hosted\_services** method:</span></span>

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

<span data-ttu-id="f5e22-167">Om du vill få information om en viss värdbaserad tjänst du göra detta genom att skicka värdbaserade tjänstens namn till den **hämta\_finns\_service\_egenskaper** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-167">If you want to get information about a particular hosted service, you can do so by passing the hosted service name to the **get\_hosted\_service\_properties** method:</span></span>

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

<span data-ttu-id="f5e22-168">När du har skapat en tjänst i molnet, kan du distribuera din kod till tjänsten med den **skapa\_distribution** metod.</span><span class="sxs-lookup"><span data-stu-id="f5e22-168">After you have created a cloud service, you can deploy your code to the service with the **create\_deployment** method.</span></span>

## <span data-ttu-id="f5e22-169"><a name="DeleteCloudService"></a>Så här: ta bort en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="f5e22-169"><a name="DeleteCloudService"> </a>How to: Delete a cloud service</span></span>
<span data-ttu-id="f5e22-170">Du kan ta bort en tjänst i molnet genom att skicka tjänstnamnet som ska den **ta bort\_finns\_service** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-170">You can delete a cloud service by passing the service name to the **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service('myhostedservice')

<span data-ttu-id="f5e22-171">Innan du kan ta bort en tjänst måste du först bort alla distributioner för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f5e22-171">Before you can delete a service, all deployments for the service must first be deleted.</span></span> <span data-ttu-id="f5e22-172">(Se [så här: ta bort en distribution](#DeleteDeployment) mer information.)</span><span class="sxs-lookup"><span data-stu-id="f5e22-172">(See [How to: Delete a deployment](#DeleteDeployment) for details.)</span></span>

## <span data-ttu-id="f5e22-173"><a name="DeleteDeployment"></a>Så här: ta bort en distribution</span><span class="sxs-lookup"><span data-stu-id="f5e22-173"><a name="DeleteDeployment"> </a>How to: Delete a deployment</span></span>
<span data-ttu-id="f5e22-174">Ta bort en distribution genom att använda den **ta bort\_distribution** metod.</span><span class="sxs-lookup"><span data-stu-id="f5e22-174">To delete a deployment, use the **delete\_deployment** method.</span></span> <span data-ttu-id="f5e22-175">I följande exempel visas hur du tar bort en distribution med namnet `v1`.</span><span class="sxs-lookup"><span data-stu-id="f5e22-175">The following example shows how to delete a deployment named `v1`.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <span data-ttu-id="f5e22-176"><a name="CreateStorageService"></a>Så här: skapa en storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="f5e22-176"><a name="CreateStorageService"> </a>How to: Create a storage service</span></span>
<span data-ttu-id="f5e22-177">En [lagringstjänsten](../storage/common/storage-create-storage-account.md) ger dig åtkomst till Azure [Blobbar](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabeller](../cosmos-db/table-storage-how-to-use-python.md), och [köer](../storage/queues/storage-python-how-to-use-queue-storage.md).</span><span class="sxs-lookup"><span data-stu-id="f5e22-177">A [storage service](../storage/common/storage-create-storage-account.md) gives you access to Azure [Blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [Tables](../cosmos-db/table-storage-how-to-use-python.md), and [Queues](../storage/queues/storage-python-how-to-use-queue-storage.md).</span></span> <span data-ttu-id="f5e22-178">Om du vill skapa en lagringstjänsten, behöver du ett namn för tjänsten (mellan 3 och 24 gemener och unikt i Azure), en beskrivning, en etikett (upp till 100 tecken, kodade automatiskt till base64) och en plats.</span><span class="sxs-lookup"><span data-stu-id="f5e22-178">To create a storage service, you need a name for the service (between 3 and 24 lowercase characters and unique within Azure), a description, a label (up to 100 characters, automatically encoded to base64), and a location.</span></span> <span data-ttu-id="f5e22-179">I följande exempel visas hur du skapar en storage-tjänst genom att ange en plats.</span><span class="sxs-lookup"><span data-stu-id="f5e22-179">The following example shows how to create a storage service by specifying a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

<span data-ttu-id="f5e22-180">Observera i föregående exempel som status för den **skapa\_lagring\_konto** åtgärden kan hämtas genom att skicka resultatet som returneras av **skapa\_lagring\_konto** till den **hämta\_åtgärden\_status** metod.</span><span class="sxs-lookup"><span data-stu-id="f5e22-180">Note in the preceding example that the status of the **create\_storage\_account** operation can be retrieved by passing the result returned by **create\_storage\_account** to the **get\_operation\_status** method.</span></span>  

<span data-ttu-id="f5e22-181">Du kan visa dina lagringskonton och deras egenskaper med den **lista\_lagring\_konton** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-181">You can list your storage accounts and their properties with the **list\_storage\_accounts** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <span data-ttu-id="f5e22-182"><a name="DeleteStorageService"></a>Så här: ta bort storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="f5e22-182"><a name="DeleteStorageService"> </a>How to: Delete a storage service</span></span>
<span data-ttu-id="f5e22-183">Du kan ta bort storage-tjänst genom att skicka namnet på lagring till det **ta bort\_lagring\_konto** metoden.</span><span class="sxs-lookup"><span data-stu-id="f5e22-183">You can delete a storage service by passing the storage service name to the **delete\_storage\_account** method.</span></span> <span data-ttu-id="f5e22-184">Tar bort en lagringstjänsten alla data som lagras i tjänsten (blobbar, tabeller och köer).</span><span class="sxs-lookup"><span data-stu-id="f5e22-184">Deleting a storage service deletes all data stored in the service (blobs, tables, and queues).</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <span data-ttu-id="f5e22-185"><a name="ListOperatingSystems"></a>Så här: visa en lista över tillgängliga operativsystem</span><span class="sxs-lookup"><span data-stu-id="f5e22-185"><a name="ListOperatingSystems"> </a>How to: List available operating systems</span></span>
<span data-ttu-id="f5e22-186">Om du vill visa en lista över de operativsystem som är tillgängliga för värdtjänster, Använd den **lista\_operativsystem\_system** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-186">To list the operating systems that are available for hosting services, use the **list\_operating\_systems** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

<span data-ttu-id="f5e22-187">Du kan också använda den **lista\_operativsystem\_system\_familjer** metoden vilka grupper som operativsystem som familj:</span><span class="sxs-lookup"><span data-stu-id="f5e22-187">Alternatively, you can use the **list\_operating\_system\_families** method, which groups the operating systems by family:</span></span>

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <span data-ttu-id="f5e22-188"><a name="CreateVMImage"></a>Så här: skapa en operativsystemavbildning</span><span class="sxs-lookup"><span data-stu-id="f5e22-188"><a name="CreateVMImage"> </a>How to: Create an operating system image</span></span>
<span data-ttu-id="f5e22-189">Lägg till en operativsystemavbildning avbildningslagringsplatsen genom att använda den **lägga till\_os\_bild** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-189">To add an operating system image to the image repository, use the **add\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

<span data-ttu-id="f5e22-190">Om du vill visa de operativsystemsavbildningar som är tillgängliga att använda den **lista\_os\_bilder** metod.</span><span class="sxs-lookup"><span data-stu-id="f5e22-190">To list the operating system images that are available, use the **list\_os\_images** method.</span></span> <span data-ttu-id="f5e22-191">Det innehåller alla plattform bilder och användaren bilder:</span><span class="sxs-lookup"><span data-stu-id="f5e22-191">It includes all platform images and user images:</span></span>

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <span data-ttu-id="f5e22-192"><a name="DeleteVMImage"></a>Så här: ta bort en operativsystemavbildning</span><span class="sxs-lookup"><span data-stu-id="f5e22-192"><a name="DeleteVMImage"> </a>How to: Delete an operating system image</span></span>
<span data-ttu-id="f5e22-193">Ta bort en användaravbildning genom att använda den **ta bort\_os\_bild** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-193">To delete a user image, use the **delete\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <span data-ttu-id="f5e22-194"><a name="CreateVM"></a>Så här: skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f5e22-194"><a name="CreateVM"> </a>How to: Create a virtual machine</span></span>
<span data-ttu-id="f5e22-195">Om du vill skapa en virtuell dator, måste du först skapa en [Molntjänsten](#CreateCloudService).</span><span class="sxs-lookup"><span data-stu-id="f5e22-195">To create a virtual machine, you first need to create a [cloud service](#CreateCloudService).</span></span>  <span data-ttu-id="f5e22-196">Skapa sedan den virtuella datorn med den **skapa\_virtuella\_datorn\_distribution** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-196">Then create the virtual machine deployment using the **create\_virtual\_machine\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <span data-ttu-id="f5e22-197"><a name="DeleteVM"></a>Så här: ta bort en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f5e22-197"><a name="DeleteVM"> </a>How to: Delete a virtual machine</span></span>
<span data-ttu-id="f5e22-198">Om du vill ta bort en virtuell dator måste du först ta bort en distribution med hjälp av den **ta bort\_distribution** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-198">To delete a virtual machine, you first delete the deployment using the **delete\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

<span data-ttu-id="f5e22-199">Molntjänsten kan sedan tas bort med den **ta bort\_finns\_service** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-199">The cloud service can then be deleted using the **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a><span data-ttu-id="f5e22-200">Så här: Skapa en virtuell dator från en avbildning av den fångade virtuell dator</span><span class="sxs-lookup"><span data-stu-id="f5e22-200">How To: Create a Virtual Machine from a Captured Virtual Machine Image</span></span>
<span data-ttu-id="f5e22-201">Om du vill göra en VM-avbildning måste du först anropa den **avbilda\_vm\_bild** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5e22-201">To capture a VM image, you first call the **capture\_vm\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

<span data-ttu-id="f5e22-202">Kontrollera att du har sparat avbildningen, Använd den **listan\_vm\_bilder** api, och kontrollera att bilden visas i resultaten:</span><span class="sxs-lookup"><span data-stu-id="f5e22-202">Next, to make sure that you have successfully captured the image, use the **list\_vm\_images** api, and make sure your image is displayed in the results:</span></span>

    images = sms.list_vm_images()

<span data-ttu-id="f5e22-203">Använd för att skapa den virtuella datorn med avbildningen slutligen den **skapa\_virtuella\_datorn\_distribution** metod som tidigare, men nu skicka in vm_image_name i stället</span><span class="sxs-lookup"><span data-stu-id="f5e22-203">To finally create the virtual machine using the captured image, use the **create\_virtual\_machine\_deployment** method as before, but this time pass in the vm_image_name instead</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

<span data-ttu-id="f5e22-204">Mer information om hur du skapar en Linux-dator finns [så här skapar du en virtuell Linux-dator.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f5e22-204">To learn more about how to capture a Linux Virtual Machine, see [How to Capture a Linux Virtual Machine.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="f5e22-205">Mer information om hur du skapar en Windows-dator finns [så här skapar du en virtuell Windows-dator.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="f5e22-205">To learn more about how to capture a Windows Virtual Machine, see [How to Capture a Windows Virtual Machine.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="f5e22-206"><a name="What's Next"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5e22-206"><a name="What's Next"> </a>Next Steps</span></span>
<span data-ttu-id="f5e22-207">Nu när du har lärt dig grunderna om service-hantering, kan du komma åt den [fullständig API-referensdokumentationen för Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) och utföra komplicerade uppgifter enkelt om du vill hantera python-programmet.</span><span class="sxs-lookup"><span data-stu-id="f5e22-207">Now that you've learned the basics of service management, you can access the [Complete API reference documentation for the Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) and perform complex tasks easily to manage your python application.</span></span>

<span data-ttu-id="f5e22-208">Mer information finns i [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="f5e22-208">For more information, see the [Python Developer Center](/develop/python/).</span></span>

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://manage.windowsazure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
