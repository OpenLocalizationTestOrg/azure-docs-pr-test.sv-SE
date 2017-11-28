---
title: "aaaConfigure filen överför tooIoT Händelsehubben med hjälp av Azure CLI (az.py) | Microsoft Docs"
description: "Hur tooconfigure filöverföringar tooAzure IoT-hubb med hello plattformsoberoende Azure CLI 2.0 (az.py)."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 390113df2d96df9833b6aa383ed66805528614a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a><span data-ttu-id="de6d0-103">Konfigurera IoT-hubb filöverföringar med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="de6d0-103">Configure IoT Hub file uploads using Azure CLI</span></span>

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

<span data-ttu-id="de6d0-104">toouse hello [filen överför funktioner i IoT-hubb][lnk-upload], måste du först associera ett Azure Storage-konto med din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de6d0-104">toouse hello [file upload functionality in IoT Hub][lnk-upload], you must first associate an Azure Storage account with your IoT hub.</span></span> <span data-ttu-id="de6d0-105">Du kan använda ett befintligt lagringskonto eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="de6d0-105">You can use an existing storage account or create a new one.</span></span>

<span data-ttu-id="de6d0-106">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="de6d0-106">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="de6d0-107">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="de6d0-107">An active Azure account.</span></span> <span data-ttu-id="de6d0-108">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="de6d0-108">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>
* <span data-ttu-id="de6d0-109">[Azure CLI 2.0][lnk-CLI-install].</span><span class="sxs-lookup"><span data-stu-id="de6d0-109">[Azure CLI 2.0][lnk-CLI-install].</span></span>
* <span data-ttu-id="de6d0-110">Azure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de6d0-110">An Azure IoT hub.</span></span> <span data-ttu-id="de6d0-111">Om du inte har en IoT-hubb kan du använda hello `az iot hub create` [kommandot] [ lnk-cli-create-iothub] toocreate en eller Använd hello portal för [skapa en IoT-hubb] [lnk-portal-hubb].</span><span class="sxs-lookup"><span data-stu-id="de6d0-111">If you don't have an IoT hub, you can use hello `az iot hub create` [command][lnk-cli-create-iothub] toocreate one or use hello portal too[Create an IoT hub][lnk-portal-hub].</span></span>
* <span data-ttu-id="de6d0-112">Ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="de6d0-112">An Azure Storage account.</span></span> <span data-ttu-id="de6d0-113">Om du inte har ett Azure Storage-konto kan du använda hello [Azure CLI 2.0 - lagringskonton hantera] [ lnk-manage-storage] toocreate något eller Använd hello portal för[skapa ett lagringskonto] [lnk-portal-storage].</span><span class="sxs-lookup"><span data-stu-id="de6d0-113">If you don't have an Azure Storage account, you can use hello [Azure CLI 2.0 - Manage storage accounts][lnk-manage-storage] toocreate one or use hello portal too[Create a storage account][lnk-portal-storage].</span></span>

## <a name="sign-in-and-set-your-azure-account"></a><span data-ttu-id="de6d0-114">Logga in och ange ditt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="de6d0-114">Sign in and set your Azure account</span></span>

<span data-ttu-id="de6d0-115">Logga in tooyour Azure-konto och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="de6d0-115">Sign in tooyour Azure account and select your subscription.</span></span>

1. <span data-ttu-id="de6d0-116">Kör hello Kommandotolken hello [inloggningen kommandot][lnk-login-command]:</span><span class="sxs-lookup"><span data-stu-id="de6d0-116">At hello command prompt, run hello [login command][lnk-login-command]:</span></span>

    ```azurecli
    az login
    ```

    <span data-ttu-id="de6d0-117">Följ hello instruktioner tooauthenticate med hello kod och logga in tooyour Azure-konto via en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="de6d0-117">Follow hello instructions tooauthenticate using hello code and sign in tooyour Azure account through a web browser.</span></span>

1. <span data-ttu-id="de6d0-118">Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-konton som är associerade med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="de6d0-118">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure accounts associated with your credentials.</span></span> <span data-ttu-id="de6d0-119">Använder följande hello [kommandot toolist hello Azure konton] [ lnk-az-account-command] tillgängliga för du toouse:</span><span class="sxs-lookup"><span data-stu-id="de6d0-119">Use hello following [command toolist hello Azure accounts][lnk-az-account-command] available for you toouse:</span></span>

    ```azurecli
    az account list
    ```

    <span data-ttu-id="de6d0-120">Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toocreate din IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="de6d0-120">Use hello following command tooselect subscription that you want toouse toorun hello commands toocreate your IoT hub.</span></span> <span data-ttu-id="de6d0-121">Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="de6d0-121">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a><span data-ttu-id="de6d0-122">Hämta information om ditt lagringskonto</span><span class="sxs-lookup"><span data-stu-id="de6d0-122">Retrieve your storage account details</span></span>

<span data-ttu-id="de6d0-123">hello följande steg förutsätter att du har skapat ditt lagringskonto med hjälp av hello **Resource Manager** distributionsmodell och inte hello **klassiska** distributionsmodell.</span><span class="sxs-lookup"><span data-stu-id="de6d0-123">hello following steps assume that you created your storage account using hello **Resource Manager** deployment model, and not hello **Classic** deployment model.</span></span>

<span data-ttu-id="de6d0-124">tooconfigure filöverföringar från dina enheter måste du hello anslutningssträngen för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="de6d0-124">tooconfigure file uploads from your devices, you need hello connection string for an Azure storage account.</span></span> <span data-ttu-id="de6d0-125">hello storage-konto måste vara i hello samma prenumeration som din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de6d0-125">hello storage account must be in hello same subscription as your IoT hub.</span></span> <span data-ttu-id="de6d0-126">Du måste också hello namnet på en blob-behållare i hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="de6d0-126">You also need hello name of a blob container in hello storage account.</span></span> <span data-ttu-id="de6d0-127">Använd följande kommando tooretrieve hello dina nycklar för lagringskonto:</span><span class="sxs-lookup"><span data-stu-id="de6d0-127">Use hello following command tooretrieve your storage account keys:</span></span>

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

<span data-ttu-id="de6d0-128">Anteckna hello **connectionString** värde.</span><span class="sxs-lookup"><span data-stu-id="de6d0-128">Make a note of hello **connectionString** value.</span></span> <span data-ttu-id="de6d0-129">Du behöver det i hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="de6d0-129">You need it in hello following steps.</span></span>

<span data-ttu-id="de6d0-130">Du kan använda en befintlig blobbehållare för dina filöverföringar eller skapa nya:</span><span class="sxs-lookup"><span data-stu-id="de6d0-130">You can either use an existing blob container for your file uploads or create new one:</span></span>

* <span data-ttu-id="de6d0-131">toolist hello befintlig blob-behållare i ditt lagringskonto, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="de6d0-131">toolist hello existing blob containers in your storage account, use hello following command:</span></span>

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* <span data-ttu-id="de6d0-132">toocreate en blob-behållare i ditt lagringskonto, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="de6d0-132">toocreate a blob container in your storage account, use hello following command:</span></span>

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a><span data-ttu-id="de6d0-133">Ladda upp filen</span><span class="sxs-lookup"><span data-stu-id="de6d0-133">File upload</span></span>

<span data-ttu-id="de6d0-134">Du kan nu konfigurera din IoT-hubb tooenable [filen överför funktioner] [ lnk-upload] med information om ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="de6d0-134">You can now configure your IoT hub tooenable [file upload functionality][lnk-upload] using your storage account details.</span></span>

<span data-ttu-id="de6d0-135">hello konfigurationen kräver hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="de6d0-135">hello configuration requires hello following values:</span></span>

<span data-ttu-id="de6d0-136">**Lagringsbehållaren**: en blob-behållare i ett Azure storage-konto i din aktuella Azure-prenumeration tooassociate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="de6d0-136">**Storage container**: A blob container in an Azure storage account in your current Azure subscription tooassociate with your IoT hub.</span></span> <span data-ttu-id="de6d0-137">Du har hämtat hello information om nödvändiga lagringskonto i föregående avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="de6d0-137">You retrieved hello necessary storage account information in hello preceding section.</span></span> <span data-ttu-id="de6d0-138">IoT-hubb genererar automatiskt SAS URI: er med skriva behörigheter toothis blob-behållare för toouse enheter när de överför filer.</span><span class="sxs-lookup"><span data-stu-id="de6d0-138">IoT Hub automatically generates SAS URIs with write permissions toothis blob container for devices toouse when they upload files.</span></span>

<span data-ttu-id="de6d0-139">**Ta emot meddelanden för överförda filer**: aktivera eller inaktivera filen överför meddelanden.</span><span class="sxs-lookup"><span data-stu-id="de6d0-139">**Receive notifications for uploaded files**: Enable or disable file upload notifications.</span></span>

<span data-ttu-id="de6d0-140">**SAS TTL**: den här inställningen är hello time-to-live av hello SAS URI returneras toohello enhet av IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="de6d0-140">**SAS TTL**: This setting is hello time-to-live of hello SAS URIs returned toohello device by IoT Hub.</span></span> <span data-ttu-id="de6d0-141">Ange tooone timme som standard.</span><span class="sxs-lookup"><span data-stu-id="de6d0-141">Set tooone hour by default.</span></span>

<span data-ttu-id="de6d0-142">**Filen notification inställningar Standardbegränsning**: hello time-to-live av en fil överför meddelande innan det förfaller.</span><span class="sxs-lookup"><span data-stu-id="de6d0-142">**File notification settings default TTL**: hello time-to-live of a file upload notification before it is expired.</span></span> <span data-ttu-id="de6d0-143">Ange tooone dagen som standard.</span><span class="sxs-lookup"><span data-stu-id="de6d0-143">Set tooone day by default.</span></span>

<span data-ttu-id="de6d0-144">**Meddelande leverans av maximalt antal filer**: hello gånger hello IoT-hubb försök toodeliver ett meddelande om överföringen.</span><span class="sxs-lookup"><span data-stu-id="de6d0-144">**File notification maximum delivery count**: hello number of times hello IoT Hub attempts toodeliver a file upload notification.</span></span> <span data-ttu-id="de6d0-145">Ange too10 som standard.</span><span class="sxs-lookup"><span data-stu-id="de6d0-145">Set too10 by default.</span></span>

<span data-ttu-id="de6d0-146">Använd följande Azure CLI-kommandon tooconfigure hello överför inställningar på din IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="de6d0-146">Use hello following Azure CLI commands tooconfigure hello file upload settings on your IoT hub:</span></span>

<span data-ttu-id="de6d0-147">En bash shell användning:</span><span class="sxs-lookup"><span data-stu-id="de6d0-147">In a bash shell use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="de6d0-148">På en Windows kommandotolk användning:</span><span class="sxs-lookup"><span data-stu-id="de6d0-148">At a Windows command prompt use:</span></span>

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

<span data-ttu-id="de6d0-149">Du kan granska hello filen överför konfiguration på din IoT-hubb med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="de6d0-149">You can review hello file upload configuration on your IoT hub using hello following command:</span></span>

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a><span data-ttu-id="de6d0-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="de6d0-150">Next steps</span></span>

<span data-ttu-id="de6d0-151">Mer information om funktionerna i hello filen överför IoT-hubb finns [överföra filer från en enhet][lnk-upload].</span><span class="sxs-lookup"><span data-stu-id="de6d0-151">For more information about hello file upload capabilities of IoT Hub, see [Upload files from a device][lnk-upload].</span></span>

<span data-ttu-id="de6d0-152">Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="de6d0-152">Follow these links toolearn more about managing Azure IoT Hub:</span></span>

* <span data-ttu-id="de6d0-153">[Massinläsning hantera IoT-enheter][lnk-bulk]</span><span class="sxs-lookup"><span data-stu-id="de6d0-153">[Bulk manage IoT devices][lnk-bulk]</span></span>
* <span data-ttu-id="de6d0-154">[IoT-hubb mått][lnk-metrics]</span><span class="sxs-lookup"><span data-stu-id="de6d0-154">[IoT Hub metrics][lnk-metrics]</span></span>
* <span data-ttu-id="de6d0-155">[Åtgärder som övervakning][lnk-monitor]</span><span class="sxs-lookup"><span data-stu-id="de6d0-155">[Operations monitoring][lnk-monitor]</span></span>

<span data-ttu-id="de6d0-156">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="de6d0-156">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="de6d0-157">[Utvecklarhandbok för IoT-hubb][lnk-devguide]</span><span class="sxs-lookup"><span data-stu-id="de6d0-157">[IoT Hub developer guide][lnk-devguide]</span></span>
* <span data-ttu-id="de6d0-158">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="de6d0-158">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>
* <span data-ttu-id="de6d0-159">[Skydda din IoT-lösning från hello bakgrund][lnk-securing]</span><span class="sxs-lookup"><span data-stu-id="de6d0-159">[Secure your IoT solution from hello ground up][lnk-securing]</span></span>

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md


[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-CLI-install]: https://docs.microsoft.com/cli/azure/install-az-cli2
[lnk-login-command]: https://docs.microsoft.com/cli/azure/get-started-with-az-cli2
[lnk-az-account-command]: https://docs.microsoft.com/cli/azure/account
[lnk-az-register-command]: https://docs.microsoft.com/cli/azure/provider
[lnk-az-addcomponent-command]: https://docs.microsoft.com/cli/azure/component
[lnk-az-resource-command]: https://docs.microsoft.com/cli/azure/resource
[lnk-az-iot-command]: https://docs.microsoft.com/cli/azure/iot
[lnk-iot-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-manage-storage]:../storage/common/storage-azure-cli.md#manage-storage-accounts
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md
[lnk-cli-create-iothub]: https://docs.microsoft.com/cli/azure/iot/hub#create