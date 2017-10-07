---
title: aaaHow toouse hello service management API (Python) - Funktionsguide
description: "Lär dig hur tooprogrammatically utföra vanliga hanteringsåtgärder för tjänsten från Python."
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
ms.openlocfilehash: b59622203470e1586484cec4033515edb39ca4d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-service-management-from-python"></a><span data-ttu-id="d5822-103">Hur toouse Service Management från Python</span><span class="sxs-lookup"><span data-stu-id="d5822-103">How toouse Service Management from Python</span></span>
<span data-ttu-id="d5822-104">Den här guiden visar hur tooprogrammatically utföra vanliga hanteringsåtgärder för tjänsten från Python.</span><span class="sxs-lookup"><span data-stu-id="d5822-104">This guide shows you how tooprogrammatically perform common service management tasks from Python.</span></span> <span data-ttu-id="d5822-105">Hej **ServiceManagementService** klassen i hello [Azure SDK för Python](https://github.com/Azure/azure-sdk-for-python) stöder Programmeringsåtkomst toomuch hello service management-relaterade funktioner som är tillgängliga i hello [Klassiska azure-portalen] [ management-portal] (exempelvis **skapa, uppdatera och ta bort molntjänster, distributioner, tjänster för data och virtuella datorer**).</span><span class="sxs-lookup"><span data-stu-id="d5822-105">hello **ServiceManagementService** class in hello [Azure SDK for Python](https://github.com/Azure/azure-sdk-for-python) supports programmatic access toomuch of hello service management-related functionality that is available in hello [Azure classic portal][management-portal] (such as **creating, updating, and deleting cloud services, deployments, data management services, and virtual machines**).</span></span> <span data-ttu-id="d5822-106">Den här funktionen kan vara användbar vid utveckling av program som behöver Programmeringsåtkomst tooservice management.</span><span class="sxs-lookup"><span data-stu-id="d5822-106">This functionality can be useful in building applications that need programmatic access tooservice management.</span></span>

## <span data-ttu-id="d5822-107"><a name="WhatIs"></a>Vad är Service Management</span><span class="sxs-lookup"><span data-stu-id="d5822-107"><a name="WhatIs"> </a>What is Service Management</span></span>
<span data-ttu-id="d5822-108">hello Service Management API ger Programmeringsåtkomst toomuch hello service management-funktioner tillgängliga via hello [klassiska Azure-portalen][management-portal].</span><span class="sxs-lookup"><span data-stu-id="d5822-108">hello Service Management API provides programmatic access toomuch of hello service management functionality available through hello [Azure classic portal][management-portal].</span></span> <span data-ttu-id="d5822-109">hello Azure SDK för Python kan du toomanage dina molntjänster och storage-konton.</span><span class="sxs-lookup"><span data-stu-id="d5822-109">hello Azure SDK for Python allows you toomanage your cloud services and storage accounts.</span></span>

<span data-ttu-id="d5822-110">toouse hello Service Management API du behöver för[skapa ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d5822-110">toouse hello Service Management API, you need too[create an Azure account](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <span data-ttu-id="d5822-111"><a name="Concepts"></a>Begrepp</span><span class="sxs-lookup"><span data-stu-id="d5822-111"><a name="Concepts"> </a>Concepts</span></span>
<span data-ttu-id="d5822-112">hello Azure SDK för Python radbryts hello [Azure Service Management API][svc-mgmt-rest-api], vilket är en REST-API.</span><span class="sxs-lookup"><span data-stu-id="d5822-112">hello Azure SDK for Python wraps hello [Azure Service Management API][svc-mgmt-rest-api], which is a REST API.</span></span> <span data-ttu-id="d5822-113">Alla API-operationer utförs över SSL och autentiserar varandra med X.509 v3-certifikat.</span><span class="sxs-lookup"><span data-stu-id="d5822-113">All API operations are performed over SSL and mutually authenticated using X.509 v3 certificates.</span></span> <span data-ttu-id="d5822-114">hello management-tjänsten kan nås inifrån en tjänst som körs i Azure, eller direkt via hello Internet från alla program som kan skicka en HTTPS-begäran och ett svar för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="d5822-114">hello management service may be accessed from within a service running in Azure, or directly over hello Internet from any application that can send an HTTPS request and receive an HTTPS response.</span></span>

## <span data-ttu-id="d5822-115"><a name="Installation"></a>Installation</span><span class="sxs-lookup"><span data-stu-id="d5822-115"><a name="Installation"> </a>Installation</span></span>
<span data-ttu-id="d5822-116">Alla hello-funktioner som beskrivs i den här artikeln är tillgängliga i hello `azure-servicemanagement-legacy` paket som du kan installera med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="d5822-116">All hello features described in this article are available in hello `azure-servicemanagement-legacy` package, which you can install using pip.</span></span> <span data-ttu-id="d5822-117">Mer information om installation (till exempel om du är ny tooPython) finns i den här artikeln: [installerar Python och hello Azure SDK](../python-how-to-install.md)</span><span class="sxs-lookup"><span data-stu-id="d5822-117">For more information about installation (for example, if you are new tooPython), see this article: [Installing Python and hello Azure SDK](../python-how-to-install.md)</span></span>

## <span data-ttu-id="d5822-118"><a name="Connect"></a>Så här: ansluta tooservice management</span><span class="sxs-lookup"><span data-stu-id="d5822-118"><a name="Connect"> </a>How to: Connect tooservice management</span></span>
<span data-ttu-id="d5822-119">tooconnect toohello Service Management slutpunkt måste Azure prenumerations-ID och ett giltigt certifikat.</span><span class="sxs-lookup"><span data-stu-id="d5822-119">tooconnect toohello Service Management endpoint, you need your Azure subscription ID and a valid management certificate.</span></span> <span data-ttu-id="d5822-120">Du kan hämta ditt prenumerations-ID via hello [klassiska Azure-portalen][management-portal].</span><span class="sxs-lookup"><span data-stu-id="d5822-120">You can obtain your subscription ID through hello [Azure classic portal][management-portal].</span></span>

> [!NOTE]
> <span data-ttu-id="d5822-121">Det är nu möjligt toouse certifikat som skapas med OpenSSL när de körs på Windows.</span><span class="sxs-lookup"><span data-stu-id="d5822-121">It is now possible toouse certificates created with OpenSSL when running on Windows.</span></span>  <span data-ttu-id="d5822-122">Det krävs Python 2.7.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="d5822-122">It requires Python 2.7.4 or later.</span></span> <span data-ttu-id="d5822-123">Vi rekommenderar användare toouse OpenSSL i stället för PFX, eftersom stöd för PFX-certifikat kommer troligen att tas bort i framtiden hello.</span><span class="sxs-lookup"><span data-stu-id="d5822-123">We recommend users toouse OpenSSL instead of .pfx, since support for .pfx certificates will likely be removed in hello future.</span></span>
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a><span data-ttu-id="d5822-124">Management-certifikat på Windows-/ Mac/Linux (OpenSSL)</span><span class="sxs-lookup"><span data-stu-id="d5822-124">Management certificates on Windows/Mac/Linux (OpenSSL)</span></span>
<span data-ttu-id="d5822-125">Du kan använda [OpenSSL](http://www.openssl.org/) toocreate hanteringscertifikatet.</span><span class="sxs-lookup"><span data-stu-id="d5822-125">You can use [OpenSSL](http://www.openssl.org/) toocreate your management certificate.</span></span>  <span data-ttu-id="d5822-126">Behöver du faktiskt toocreate två certifikat, en för hello server (en `.cer` filen) och en för hello klienten (en `.pem` fil).</span><span class="sxs-lookup"><span data-stu-id="d5822-126">You actually need toocreate two certificates, one for hello server (a `.cer` file) and one for hello client (a `.pem` file).</span></span> <span data-ttu-id="d5822-127">toocreate hello `.pem` fil, kör:</span><span class="sxs-lookup"><span data-stu-id="d5822-127">toocreate hello `.pem` file, execute:</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="d5822-128">toocreate hello `.cer` certifikat, kör:</span><span class="sxs-lookup"><span data-stu-id="d5822-128">toocreate hello `.cer` certificate, execute:</span></span>

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

<span data-ttu-id="d5822-129">Läs mer om Azure certifikat [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="d5822-129">For more information about Azure certificates, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span> <span data-ttu-id="d5822-130">En fullständig beskrivning av OpenSSL parametrar finns hello dokumentationen på [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span><span class="sxs-lookup"><span data-stu-id="d5822-130">For a complete description of OpenSSL parameters, see hello documentation at [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).</span></span>

<span data-ttu-id="d5822-131">När du har skapat de här filerna måste tooupload hello `.cer` filen tooAzure med hello ”överför” åtgärd av hello ”inställningar” fliken hello [klassiska Azure-portalen][management-portal], och du behöver toomake anteckna där du sparade hello `.pem` fil.</span><span class="sxs-lookup"><span data-stu-id="d5822-131">After you have created these files, you need tooupload hello `.cer` file tooAzure via hello "Upload" action of hello "Settings" tab of hello [Azure classic portal][management-portal], and you need toomake note of where you saved hello `.pem` file.</span></span>

<span data-ttu-id="d5822-132">När du har fått ditt prenumerations-ID, skapas ett certifikat och överföra hello `.cer` filen tooAzure du kan ansluta toohello Azure hanteringsslutpunkten genom att skicka hello prenumerations-id och hello sökvägen toohello `.pem` filen för**ServiceManagementService**:</span><span class="sxs-lookup"><span data-stu-id="d5822-132">After you have obtained your subscription ID, created a certificate, and uploaded hello `.cer` file tooAzure, you can connect toohello Azure management endpoint by passing hello subscription id and hello path toohello `.pem` file too**ServiceManagementService**:</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="d5822-133">I föregående exempel hello `sms` är en **ServiceManagementService** objekt.</span><span class="sxs-lookup"><span data-stu-id="d5822-133">In hello preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="d5822-134">Hej **ServiceManagementService** klass är hello primära klassen används toomanage Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d5822-134">hello **ServiceManagementService** class is hello primary class used toomanage Azure services.</span></span>

### <a name="management-certificates-on-windows-makecert"></a><span data-ttu-id="d5822-135">Hanteringscertifikat i Windows (MakeCert)</span><span class="sxs-lookup"><span data-stu-id="d5822-135">Management certificates on Windows (MakeCert)</span></span>
<span data-ttu-id="d5822-136">Du kan skapa ett självsignerat certifikat på datorn med hjälp av `makecert.exe`.</span><span class="sxs-lookup"><span data-stu-id="d5822-136">You can create a self-signed management certificate on your machine using `makecert.exe`.</span></span>  <span data-ttu-id="d5822-137">Öppna en **Kommandotolken Visual Studio** som en **administratör** och använda hello följande kommando, där du ersätter *AzureCertificate* med hello certifikatnamnet som toouse.</span><span class="sxs-lookup"><span data-stu-id="d5822-137">Open a **Visual Studio command prompt** as an **administrator** and use hello following command, replacing *AzureCertificate* with hello certificate name you would like toouse.</span></span>

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

<span data-ttu-id="d5822-138">hello-kommando skapar hello `.cer` filen och installerar den i hello **personliga** certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="d5822-138">hello command creates hello `.cer` file, and installs it in hello **Personal** certificate store.</span></span> <span data-ttu-id="d5822-139">Mer information finns i [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="d5822-139">For more information, see [Certificates Overview for Azure Cloud Services](cloud-services-certs-create.md).</span></span>

<span data-ttu-id="d5822-140">När du har skapat hello certifikat måste tooupload hello `.cer` filen tooAzure med hello ”överför” åtgärd av hello ”inställningar” fliken hello [klassiska Azure-portalen][management-portal].</span><span class="sxs-lookup"><span data-stu-id="d5822-140">After you have created hello certificate, you need tooupload hello `.cer` file tooAzure via hello "Upload" action of hello "Settings" tab of hello [Azure classic portal][management-portal].</span></span>

<span data-ttu-id="d5822-141">När du har fått ditt prenumerations-ID, skapas ett certifikat och överföra hello `.cer` filen tooAzure du kan ansluta toohello Azure hanteringsslutpunkten genom att skicka hello prenumerations-id och hello platsen för hello certifikatet i din **Personliga** certifikatarkivet för**ServiceManagementService** (igen och Ersätt *AzureCertificate* med hello namnet på ditt certifikat):</span><span class="sxs-lookup"><span data-stu-id="d5822-141">After you have obtained your subscription ID, created a certificate, and uploaded hello `.cer` file tooAzure, you can connect toohello Azure management endpoint by passing hello subscription id and hello location of hello certificate in your **Personal** certificate store too**ServiceManagementService** (again, replace *AzureCertificate* with hello name of your certificate):</span></span>

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

<span data-ttu-id="d5822-142">I föregående exempel hello `sms` är en **ServiceManagementService** objekt.</span><span class="sxs-lookup"><span data-stu-id="d5822-142">In hello preceding example, `sms` is a **ServiceManagementService** object.</span></span> <span data-ttu-id="d5822-143">Hej **ServiceManagementService** klass är hello primära klassen används toomanage Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d5822-143">hello **ServiceManagementService** class is hello primary class used toomanage Azure services.</span></span>

## <span data-ttu-id="d5822-144"><a name="ListAvailableLocations"></a>Så här: visa en lista över tillgängliga platser</span><span class="sxs-lookup"><span data-stu-id="d5822-144"><a name="ListAvailableLocations"> </a>How to: List available locations</span></span>
<span data-ttu-id="d5822-145">toolist hello platser som är tillgängliga för värdtjänster, använda hello **lista\_platser** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-145">toolist hello locations that are available for hosting services, use hello **list\_locations** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

<span data-ttu-id="d5822-146">När du skapar en tjänst i molnet eller lagringstjänsten måste tooprovide en giltig plats.</span><span class="sxs-lookup"><span data-stu-id="d5822-146">When you create a cloud service or storage service you need tooprovide a valid location.</span></span> <span data-ttu-id="d5822-147">Hej **lista\_platser** -metoden returnerar alltid en aktuell lista över hello tillgängliga platser.</span><span class="sxs-lookup"><span data-stu-id="d5822-147">hello **list\_locations** method always returns an up-to-date list of hello currently available locations.</span></span> <span data-ttu-id="d5822-148">När detta skrivs är hello tillgängliga platser:</span><span class="sxs-lookup"><span data-stu-id="d5822-148">As of this writing, hello available locations are:</span></span>

* <span data-ttu-id="d5822-149">Västra Europa</span><span class="sxs-lookup"><span data-stu-id="d5822-149">West Europe</span></span>
* <span data-ttu-id="d5822-150">Norra Europa</span><span class="sxs-lookup"><span data-stu-id="d5822-150">North Europe</span></span>
* <span data-ttu-id="d5822-151">Sydostasien</span><span class="sxs-lookup"><span data-stu-id="d5822-151">Southeast Asia</span></span>
* <span data-ttu-id="d5822-152">Östasien</span><span class="sxs-lookup"><span data-stu-id="d5822-152">East Asia</span></span>
* <span data-ttu-id="d5822-153">Centrala USA</span><span class="sxs-lookup"><span data-stu-id="d5822-153">Central US</span></span>
* <span data-ttu-id="d5822-154">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="d5822-154">North Central US</span></span>
* <span data-ttu-id="d5822-155">Södra centrala USA</span><span class="sxs-lookup"><span data-stu-id="d5822-155">South Central US</span></span>
* <span data-ttu-id="d5822-156">Västra USA</span><span class="sxs-lookup"><span data-stu-id="d5822-156">West US</span></span>
* <span data-ttu-id="d5822-157">Östra USA</span><span class="sxs-lookup"><span data-stu-id="d5822-157">East US</span></span>
* <span data-ttu-id="d5822-158">Östra Japan</span><span class="sxs-lookup"><span data-stu-id="d5822-158">Japan East</span></span>
* <span data-ttu-id="d5822-159">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="d5822-159">Japan West</span></span>
* <span data-ttu-id="d5822-160">Södra Brasilien</span><span class="sxs-lookup"><span data-stu-id="d5822-160">Brazil South</span></span>
* <span data-ttu-id="d5822-161">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="d5822-161">Australia East</span></span>
* <span data-ttu-id="d5822-162">Sydöstra Australien</span><span class="sxs-lookup"><span data-stu-id="d5822-162">Australia Southeast</span></span>

## <span data-ttu-id="d5822-163"><a name="CreateCloudService"></a>Så här: skapa en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="d5822-163"><a name="CreateCloudService"> </a>How to: Create a cloud service</span></span>
<span data-ttu-id="d5822-164">När du skapar ett program och kör det i Azure, hello koden och konfigurationen tillsammans kallas en Azure [Molntjänsten] [ cloud service] (kallas även en *värdtjänsten* i tidigare Azure versioner).</span><span class="sxs-lookup"><span data-stu-id="d5822-164">When you create an application and run it in Azure, hello code and configuration together are called an Azure [cloud service][cloud service] (known as a *hosted service* in earlier Azure releases).</span></span> <span data-ttu-id="d5822-165">Hej **skapa\_finns\_service** metod kan du toocreate en ny värdbaserad tjänst genom att tillhandahålla värdbaserad tjänstnamn (som måste vara unikt i Azure), en etikett (automatiskt kodade toobase64), en Beskrivning och en plats.</span><span class="sxs-lookup"><span data-stu-id="d5822-165">hello **create\_hosted\_service** method allows you toocreate a new hosted service by providing a hosted service name (which must be unique in Azure), a label (automatically encoded toobase64), a description, and a location.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

<span data-ttu-id="d5822-166">Du kan visa en lista med alla hello värdbaserade tjänster för din prenumeration med hello **lista\_finns\_services** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-166">You can list all hello hosted services for your subscription with hello **list\_hosted\_services** method:</span></span>

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

<span data-ttu-id="d5822-167">Om du vill tooget information om en viss värdbaserad tjänst kan du göra det genom att skicka hello värdbaserade tjänsten namnet toohello **hämta\_finns\_service\_egenskaper** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-167">If you want tooget information about a particular hosted service, you can do so by passing hello hosted service name toohello **get\_hosted\_service\_properties** method:</span></span>

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

<span data-ttu-id="d5822-168">När du har skapat en tjänst i molnet, kan du distribuera din kod toohello tjänst med hello **skapa\_distribution** metod.</span><span class="sxs-lookup"><span data-stu-id="d5822-168">After you have created a cloud service, you can deploy your code toohello service with hello **create\_deployment** method.</span></span>

## <span data-ttu-id="d5822-169"><a name="DeleteCloudService"></a>Så här: ta bort en tjänst i molnet</span><span class="sxs-lookup"><span data-stu-id="d5822-169"><a name="DeleteCloudService"> </a>How to: Delete a cloud service</span></span>
<span data-ttu-id="d5822-170">Du kan ta bort en tjänst i molnet genom att skicka hello service name toohello **ta bort\_finns\_service** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-170">You can delete a cloud service by passing hello service name toohello **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service('myhostedservice')

<span data-ttu-id="d5822-171">Innan du kan ta bort en tjänst, måste alla distributioner för hello tjänsten först tas bort.</span><span class="sxs-lookup"><span data-stu-id="d5822-171">Before you can delete a service, all deployments for hello service must first be deleted.</span></span> <span data-ttu-id="d5822-172">(Se [så här: ta bort en distribution](#DeleteDeployment) mer information.)</span><span class="sxs-lookup"><span data-stu-id="d5822-172">(See [How to: Delete a deployment](#DeleteDeployment) for details.)</span></span>

## <span data-ttu-id="d5822-173"><a name="DeleteDeployment"></a>Så här: ta bort en distribution</span><span class="sxs-lookup"><span data-stu-id="d5822-173"><a name="DeleteDeployment"> </a>How to: Delete a deployment</span></span>
<span data-ttu-id="d5822-174">toodelete en distribution kan använda hello **ta bort\_distribution** metod.</span><span class="sxs-lookup"><span data-stu-id="d5822-174">toodelete a deployment, use hello **delete\_deployment** method.</span></span> <span data-ttu-id="d5822-175">hello följande exempel visas hur toodelete en distribution med namnet `v1`.</span><span class="sxs-lookup"><span data-stu-id="d5822-175">hello following example shows how toodelete a deployment named `v1`.</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <span data-ttu-id="d5822-176"><a name="CreateStorageService"></a>Så här: skapa en storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="d5822-176"><a name="CreateStorageService"> </a>How to: Create a storage service</span></span>
<span data-ttu-id="d5822-177">En [lagringstjänsten](../storage/common/storage-create-storage-account.md) ger dig åtkomst tooAzure [Blobbar](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabeller](../cosmos-db/table-storage-how-to-use-python.md), och [köer](../storage/queues/storage-python-how-to-use-queue-storage.md).</span><span class="sxs-lookup"><span data-stu-id="d5822-177">A [storage service](../storage/common/storage-create-storage-account.md) gives you access tooAzure [Blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [Tables](../cosmos-db/table-storage-how-to-use-python.md), and [Queues](../storage/queues/storage-python-how-to-use-queue-storage.md).</span></span> <span data-ttu-id="d5822-178">toocreate storage-tjänst som du behöver ett namn för hello service (mellan 3 och 24 gemener och unikt i Azure), en beskrivning, en etikett (upp too100 tecken, automatiskt kodade toobase64) och en plats.</span><span class="sxs-lookup"><span data-stu-id="d5822-178">toocreate a storage service, you need a name for hello service (between 3 and 24 lowercase characters and unique within Azure), a description, a label (up too100 characters, automatically encoded toobase64), and a location.</span></span> <span data-ttu-id="d5822-179">hello som följande exempel visar hur toocreate en tjänst genom att ange en plats.</span><span class="sxs-lookup"><span data-stu-id="d5822-179">hello following example shows how toocreate a storage service by specifying a location.</span></span>

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

<span data-ttu-id="d5822-180">Observera i föregående exempel hello hello statusen på hello **skapa\_lagring\_konto** åtgärden kan hämtas genom att skicka hello resultatet som returneras av **skapa\_lagring \_konto** toohello **hämta\_åtgärden\_status** metod.</span><span class="sxs-lookup"><span data-stu-id="d5822-180">Note in hello preceding example that hello status of hello **create\_storage\_account** operation can be retrieved by passing hello result returned by **create\_storage\_account** toohello **get\_operation\_status** method.</span></span>  

<span data-ttu-id="d5822-181">Du kan visa dina lagringskonton och deras egenskaper med hello **lista\_lagring\_konton** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-181">You can list your storage accounts and their properties with hello **list\_storage\_accounts** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <span data-ttu-id="d5822-182"><a name="DeleteStorageService"></a>Så här: ta bort storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="d5822-182"><a name="DeleteStorageService"> </a>How to: Delete a storage service</span></span>
<span data-ttu-id="d5822-183">Du kan ta bort storage-tjänst genom att skicka hello storage service name toohello **ta bort\_lagring\_konto** metod.</span><span class="sxs-lookup"><span data-stu-id="d5822-183">You can delete a storage service by passing hello storage service name toohello **delete\_storage\_account** method.</span></span> <span data-ttu-id="d5822-184">Tar bort en lagringstjänsten alla data som lagras i hello-tjänsten (blobbar, tabeller och köer).</span><span class="sxs-lookup"><span data-stu-id="d5822-184">Deleting a storage service deletes all data stored in hello service (blobs, tables, and queues).</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <span data-ttu-id="d5822-185"><a name="ListOperatingSystems"></a>Så här: visa en lista över tillgängliga operativsystem</span><span class="sxs-lookup"><span data-stu-id="d5822-185"><a name="ListOperatingSystems"> </a>How to: List available operating systems</span></span>
<span data-ttu-id="d5822-186">toolist hello-operativsystem som är tillgängliga för värdtjänster, använder hello **lista\_operativsystem\_system** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-186">toolist hello operating systems that are available for hosting services, use hello **list\_operating\_systems** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

<span data-ttu-id="d5822-187">Du kan också använda hello **lista\_operativsystem\_system\_familjer** metoden vilka grupper hello-system med familj:</span><span class="sxs-lookup"><span data-stu-id="d5822-187">Alternatively, you can use hello **list\_operating\_system\_families** method, which groups hello operating systems by family:</span></span>

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <span data-ttu-id="d5822-188"><a name="CreateVMImage"></a>Så här: skapa en operativsystemavbildning</span><span class="sxs-lookup"><span data-stu-id="d5822-188"><a name="CreateVMImage"> </a>How to: Create an operating system image</span></span>
<span data-ttu-id="d5822-189">tooadd ett operativsystem avbildningen toohello avbildningslagringsplatsen, använda hello **lägga till\_os\_bild** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-189">tooadd an operating system image toohello image repository, use hello **add\_os\_image** method:</span></span>

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

<span data-ttu-id="d5822-190">toolist hello avbildningar av operativsystem som är tillgängliga, använda hello **lista\_os\_bilder** metod.</span><span class="sxs-lookup"><span data-stu-id="d5822-190">toolist hello operating system images that are available, use hello **list\_os\_images** method.</span></span> <span data-ttu-id="d5822-191">Det innehåller alla plattform bilder och användaren bilder:</span><span class="sxs-lookup"><span data-stu-id="d5822-191">It includes all platform images and user images:</span></span>

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

## <span data-ttu-id="d5822-192"><a name="DeleteVMImage"></a>Så här: ta bort en operativsystemavbildning</span><span class="sxs-lookup"><span data-stu-id="d5822-192"><a name="DeleteVMImage"> </a>How to: Delete an operating system image</span></span>
<span data-ttu-id="d5822-193">toodelete en användaravbildning använda hello **ta bort\_os\_bild** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-193">toodelete a user image, use hello **delete\_os\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <span data-ttu-id="d5822-194"><a name="CreateVM"></a>Så här: skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d5822-194"><a name="CreateVM"> </a>How to: Create a virtual machine</span></span>
<span data-ttu-id="d5822-195">toocreate en virtuell dator, måste du först toocreate en [Molntjänsten](#CreateCloudService).</span><span class="sxs-lookup"><span data-stu-id="d5822-195">toocreate a virtual machine, you first need toocreate a [cloud service](#CreateCloudService).</span></span>  <span data-ttu-id="d5822-196">Skapa hello distribution av virtuella datorer med hjälp av hello **skapa\_virtuella\_datorn\_distribution** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-196">Then create hello virtual machine deployment using hello **create\_virtual\_machine\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where hello VM disk
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

## <span data-ttu-id="d5822-197"><a name="DeleteVM"></a>Så här: ta bort en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d5822-197"><a name="DeleteVM"> </a>How to: Delete a virtual machine</span></span>
<span data-ttu-id="d5822-198">toodelete en virtuell dator du först ta bort hello-distribution med hello **ta bort\_distribution** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-198">toodelete a virtual machine, you first delete hello deployment using hello **delete\_deployment** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

<span data-ttu-id="d5822-199">Hej Molntjänsten kan sedan tas bort med hello **ta bort\_finns\_service** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-199">hello cloud service can then be deleted using hello **delete\_hosted\_service** method:</span></span>

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a><span data-ttu-id="d5822-200">Så här: Skapa en virtuell dator från en avbildning av den fångade virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d5822-200">How To: Create a Virtual Machine from a Captured Virtual Machine Image</span></span>
<span data-ttu-id="d5822-201">toocapture en VM-avbildning du först anropa hello **avbilda\_vm\_bild** metoden:</span><span class="sxs-lookup"><span data-stu-id="d5822-201">toocapture a VM image, you first call hello **capture\_vm\_image** method:</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace hello below three parameters with actual values
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

<span data-ttu-id="d5822-202">Därefter går du till att du har har avbildats hello avbildning, använder hello toomake **lista\_vm\_bilder** api, och kontrollera att bilden visas i resultaten för hello:</span><span class="sxs-lookup"><span data-stu-id="d5822-202">Next, toomake sure that you have successfully captured hello image, use hello **list\_vm\_images** api, and make sure your image is displayed in hello results:</span></span>

    images = sms.list_vm_images()

<span data-ttu-id="d5822-203">toofinally skapar hello virtuell dator med hello avbildningen använder hello **skapa\_virtuella\_datorn\_distribution** metod som tidigare, men nu skicka in hello vm_image_name i stället</span><span class="sxs-lookup"><span data-stu-id="d5822-203">toofinally create hello virtual machine using hello captured image, use hello **create\_virtual\_machine\_deployment** method as before, but this time pass in hello vm_image_name instead</span></span>

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set hello location
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

<span data-ttu-id="d5822-204">toolearn mer om hur toocapture en virtuell Linux-dator, se [hur tooCapture en Linux-dator.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d5822-204">toolearn more about how toocapture a Linux Virtual Machine, see [How tooCapture a Linux Virtual Machine.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)</span></span>

<span data-ttu-id="d5822-205">toolearn mer om hur toocapture Windows-dator, se [hur tooCapture Windows-dator.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="d5822-205">toolearn more about how toocapture a Windows Virtual Machine, see [How tooCapture a Windows Virtual Machine.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

## <span data-ttu-id="d5822-206"><a name="What's Next"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5822-206"><a name="What's Next"> </a>Next Steps</span></span>
<span data-ttu-id="d5822-207">Nu när du har lärt dig hello grunderna i service management kan du komma åt hello [fullständig API-referensdokumentationen för hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) och utföra komplexa uppgifter enkelt toomanage python-programmet.</span><span class="sxs-lookup"><span data-stu-id="d5822-207">Now that you've learned hello basics of service management, you can access hello [Complete API reference documentation for hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) and perform complex tasks easily toomanage your python application.</span></span>

<span data-ttu-id="d5822-208">Mer information finns i hello [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="d5822-208">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect tooservice management]: #Connect
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
