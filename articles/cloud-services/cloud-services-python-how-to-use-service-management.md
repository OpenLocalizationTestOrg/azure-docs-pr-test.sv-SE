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
# <a name="how-toouse-service-management-from-python"></a>Hur toouse Service Management från Python
Den här guiden visar hur tooprogrammatically utföra vanliga hanteringsåtgärder för tjänsten från Python. Hej **ServiceManagementService** klassen i hello [Azure SDK för Python](https://github.com/Azure/azure-sdk-for-python) stöder Programmeringsåtkomst toomuch hello service management-relaterade funktioner som är tillgängliga i hello [Klassiska azure-portalen] [ management-portal] (exempelvis **skapa, uppdatera och ta bort molntjänster, distributioner, tjänster för data och virtuella datorer**). Den här funktionen kan vara användbar vid utveckling av program som behöver Programmeringsåtkomst tooservice management.

## <a name="WhatIs"></a>Vad är Service Management
hello Service Management API ger Programmeringsåtkomst toomuch hello service management-funktioner tillgängliga via hello [klassiska Azure-portalen][management-portal]. hello Azure SDK för Python kan du toomanage dina molntjänster och storage-konton.

toouse hello Service Management API du behöver för[skapa ett Azure-konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Begrepp
hello Azure SDK för Python radbryts hello [Azure Service Management API][svc-mgmt-rest-api], vilket är en REST-API. Alla API-operationer utförs över SSL och autentiserar varandra med X.509 v3-certifikat. hello management-tjänsten kan nås inifrån en tjänst som körs i Azure, eller direkt via hello Internet från alla program som kan skicka en HTTPS-begäran och ett svar för HTTPS.

## <a name="Installation"></a>Installation
Alla hello-funktioner som beskrivs i den här artikeln är tillgängliga i hello `azure-servicemanagement-legacy` paket som du kan installera med hjälp av pip. Mer information om installation (till exempel om du är ny tooPython) finns i den här artikeln: [installerar Python och hello Azure SDK](../python-how-to-install.md)

## <a name="Connect"></a>Så här: ansluta tooservice management
tooconnect toohello Service Management slutpunkt måste Azure prenumerations-ID och ett giltigt certifikat. Du kan hämta ditt prenumerations-ID via hello [klassiska Azure-portalen][management-portal].

> [!NOTE]
> Det är nu möjligt toouse certifikat som skapas med OpenSSL när de körs på Windows.  Det krävs Python 2.7.4 eller senare. Vi rekommenderar användare toouse OpenSSL i stället för PFX, eftersom stöd för PFX-certifikat kommer troligen att tas bort i framtiden hello.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Management-certifikat på Windows-/ Mac/Linux (OpenSSL)
Du kan använda [OpenSSL](http://www.openssl.org/) toocreate hanteringscertifikatet.  Behöver du faktiskt toocreate två certifikat, en för hello server (en `.cer` filen) och en för hello klienten (en `.pem` fil). toocreate hello `.pem` fil, kör:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

toocreate hello `.cer` certifikat, kör:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Läs mer om Azure certifikat [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md). En fullständig beskrivning av OpenSSL parametrar finns hello dokumentationen på [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

När du har skapat de här filerna måste tooupload hello `.cer` filen tooAzure med hello ”överför” åtgärd av hello ”inställningar” fliken hello [klassiska Azure-portalen][management-portal], och du behöver toomake anteckna där du sparade hello `.pem` fil.

När du har fått ditt prenumerations-ID, skapas ett certifikat och överföra hello `.cer` filen tooAzure du kan ansluta toohello Azure hanteringsslutpunkten genom att skicka hello prenumerations-id och hello sökvägen toohello `.pem` filen för**ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

I föregående exempel hello `sms` är en **ServiceManagementService** objekt. Hej **ServiceManagementService** klass är hello primära klassen används toomanage Azure-tjänster.

### <a name="management-certificates-on-windows-makecert"></a>Hanteringscertifikat i Windows (MakeCert)
Du kan skapa ett självsignerat certifikat på datorn med hjälp av `makecert.exe`.  Öppna en **Kommandotolken Visual Studio** som en **administratör** och använda hello följande kommando, där du ersätter *AzureCertificate* med hello certifikatnamnet som toouse.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

hello-kommando skapar hello `.cer` filen och installerar den i hello **personliga** certifikatarkiv. Mer information finns i [certifikat översikt för Azure Cloud Services](cloud-services-certs-create.md).

När du har skapat hello certifikat måste tooupload hello `.cer` filen tooAzure med hello ”överför” åtgärd av hello ”inställningar” fliken hello [klassiska Azure-portalen][management-portal].

När du har fått ditt prenumerations-ID, skapas ett certifikat och överföra hello `.cer` filen tooAzure du kan ansluta toohello Azure hanteringsslutpunkten genom att skicka hello prenumerations-id och hello platsen för hello certifikatet i din **Personliga** certifikatarkivet för**ServiceManagementService** (igen och Ersätt *AzureCertificate* med hello namnet på ditt certifikat):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

I föregående exempel hello `sms` är en **ServiceManagementService** objekt. Hej **ServiceManagementService** klass är hello primära klassen används toomanage Azure-tjänster.

## <a name="ListAvailableLocations"></a>Så här: visa en lista över tillgängliga platser
toolist hello platser som är tillgängliga för värdtjänster, använda hello **lista\_platser** metoden:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

När du skapar en tjänst i molnet eller lagringstjänsten måste tooprovide en giltig plats. Hej **lista\_platser** -metoden returnerar alltid en aktuell lista över hello tillgängliga platser. När detta skrivs är hello tillgängliga platser:

* Västra Europa
* Norra Europa
* Sydostasien
* Östasien
* Centrala USA
* Norra centrala USA
* Södra centrala USA
* Västra USA
* Östra USA
* Östra Japan
* Västra Japan
* Södra Brasilien
* Östra Australien
* Sydöstra Australien

## <a name="CreateCloudService"></a>Så här: skapa en tjänst i molnet
När du skapar ett program och kör det i Azure, hello koden och konfigurationen tillsammans kallas en Azure [Molntjänsten] [ cloud service] (kallas även en *värdtjänsten* i tidigare Azure versioner). Hej **skapa\_finns\_service** metod kan du toocreate en ny värdbaserad tjänst genom att tillhandahålla värdbaserad tjänstnamn (som måste vara unikt i Azure), en etikett (automatiskt kodade toobase64), en Beskrivning och en plats.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

Du kan visa en lista med alla hello värdbaserade tjänster för din prenumeration med hello **lista\_finns\_services** metoden:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Om du vill tooget information om en viss värdbaserad tjänst kan du göra det genom att skicka hello värdbaserade tjänsten namnet toohello **hämta\_finns\_service\_egenskaper** metoden:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

När du har skapat en tjänst i molnet, kan du distribuera din kod toohello tjänst med hello **skapa\_distribution** metod.

## <a name="DeleteCloudService"></a>Så här: ta bort en tjänst i molnet
Du kan ta bort en tjänst i molnet genom att skicka hello service name toohello **ta bort\_finns\_service** metoden:

    sms.delete_hosted_service('myhostedservice')

Innan du kan ta bort en tjänst, måste alla distributioner för hello tjänsten först tas bort. (Se [så här: ta bort en distribution](#DeleteDeployment) mer information.)

## <a name="DeleteDeployment"></a>Så här: ta bort en distribution
toodelete en distribution kan använda hello **ta bort\_distribution** metod. hello följande exempel visas hur toodelete en distribution med namnet `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Så här: skapa en storage-tjänst
En [lagringstjänsten](../storage/common/storage-create-storage-account.md) ger dig åtkomst tooAzure [Blobbar](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabeller](../cosmos-db/table-storage-how-to-use-python.md), och [köer](../storage/queues/storage-python-how-to-use-queue-storage.md). toocreate storage-tjänst som du behöver ett namn för hello service (mellan 3 och 24 gemener och unikt i Azure), en beskrivning, en etikett (upp too100 tecken, automatiskt kodade toobase64) och en plats. hello som följande exempel visar hur toocreate en tjänst genom att ange en plats.

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

Observera i föregående exempel hello hello statusen på hello **skapa\_lagring\_konto** åtgärden kan hämtas genom att skicka hello resultatet som returneras av **skapa\_lagring \_konto** toohello **hämta\_åtgärden\_status** metod.  

Du kan visa dina lagringskonton och deras egenskaper med hello **lista\_lagring\_konton** metoden:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Så här: ta bort storage-tjänst
Du kan ta bort storage-tjänst genom att skicka hello storage service name toohello **ta bort\_lagring\_konto** metod. Tar bort en lagringstjänsten alla data som lagras i hello-tjänsten (blobbar, tabeller och köer).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Så här: visa en lista över tillgängliga operativsystem
toolist hello-operativsystem som är tillgängliga för värdtjänster, använder hello **lista\_operativsystem\_system** metoden:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

Du kan också använda hello **lista\_operativsystem\_system\_familjer** metoden vilka grupper hello-system med familj:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Så här: skapa en operativsystemavbildning
tooadd ett operativsystem avbildningen toohello avbildningslagringsplatsen, använda hello **lägga till\_os\_bild** metoden:

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

toolist hello avbildningar av operativsystem som är tillgängliga, använda hello **lista\_os\_bilder** metod. Det innehåller alla plattform bilder och användaren bilder:

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

## <a name="DeleteVMImage"></a>Så här: ta bort en operativsystemavbildning
toodelete en användaravbildning använda hello **ta bort\_os\_bild** metoden:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Så här: skapa en virtuell dator
toocreate en virtuell dator, måste du först toocreate en [Molntjänsten](#CreateCloudService).  Skapa hello distribution av virtuella datorer med hjälp av hello **skapa\_virtuella\_datorn\_distribution** metoden:

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

## <a name="DeleteVM"></a>Så här: ta bort en virtuell dator
toodelete en virtuell dator du först ta bort hello-distribution med hello **ta bort\_distribution** metoden:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

Hej Molntjänsten kan sedan tas bort med hello **ta bort\_finns\_service** metoden:

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Så här: Skapa en virtuell dator från en avbildning av den fångade virtuell dator
toocapture en VM-avbildning du först anropa hello **avbilda\_vm\_bild** metoden:

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

Därefter går du till att du har har avbildats hello avbildning, använder hello toomake **lista\_vm\_bilder** api, och kontrollera att bilden visas i resultaten för hello:

    images = sms.list_vm_images()

toofinally skapar hello virtuell dator med hello avbildningen använder hello **skapa\_virtuella\_datorn\_distribution** metod som tidigare, men nu skicka in hello vm_image_name i stället

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

toolearn mer om hur toocapture en virtuell Linux-dator, se [hur tooCapture en Linux-dator.](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

toolearn mer om hur toocapture Windows-dator, se [hur tooCapture Windows-dator.](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

## <a name="What's Next"> </a>Nästa steg
Nu när du har lärt dig hello grunderna i service management kan du komma åt hello [fullständig API-referensdokumentationen för hello Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) och utföra komplexa uppgifter enkelt toomanage python-programmet.

Mer information finns i hello [Python Developer Center](/develop/python/).

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
