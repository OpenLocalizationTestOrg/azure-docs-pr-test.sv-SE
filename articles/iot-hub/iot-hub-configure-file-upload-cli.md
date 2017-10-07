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
# <a name="configure-iot-hub-file-uploads-using-azure-cli"></a>Konfigurera IoT-hubb filöverföringar med hjälp av Azure CLI

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

toouse hello [filen överför funktioner i IoT-hubb][lnk-upload], måste du först associera ett Azure Storage-konto med din IoT-hubb. Du kan använda ett befintligt lagringskonto eller skapa en ny.

toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* [Azure CLI 2.0][lnk-CLI-install].
* Azure IoT-hubb. Om du inte har en IoT-hubb kan du använda hello `az iot hub create` [kommandot] [ lnk-cli-create-iothub] toocreate en eller Använd hello portal för [skapa en IoT-hubb] [lnk-portal-hubb].
* Ett Azure Storage-konto. Om du inte har ett Azure Storage-konto kan du använda hello [Azure CLI 2.0 - lagringskonton hantera] [ lnk-manage-storage] toocreate något eller Använd hello portal för[skapa ett lagringskonto] [lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Logga in och ange ditt Azure-konto

Logga in tooyour Azure-konto och välja din prenumeration.

1. Kör hello Kommandotolken hello [inloggningen kommandot][lnk-login-command]:

    ```azurecli
    az login
    ```

    Följ hello instruktioner tooauthenticate med hello kod och logga in tooyour Azure-konto via en webbläsare.

1. Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-konton som är associerade med dina autentiseringsuppgifter. Använder följande hello [kommandot toolist hello Azure konton] [ lnk-az-account-command] tillgängliga för du toouse:

    ```azurecli
    az account list
    ```

    Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toocreate din IoT-hubb hello. Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:

    ```azurecli
    az account set --subscription {your subscription name or id}
    ```

## <a name="retrieve-your-storage-account-details"></a>Hämta information om ditt lagringskonto

hello följande steg förutsätter att du har skapat ditt lagringskonto med hjälp av hello **Resource Manager** distributionsmodell och inte hello **klassiska** distributionsmodell.

tooconfigure filöverföringar från dina enheter måste du hello anslutningssträngen för Azure storage-konto. hello storage-konto måste vara i hello samma prenumeration som din IoT-hubb. Du måste också hello namnet på en blob-behållare i hello storage-konto. Använd följande kommando tooretrieve hello dina nycklar för lagringskonto:

```azurecli
az storage account show-connection-string --name {your storage account name} --resource-group {your storage account resource group}
```

Anteckna hello **connectionString** värde. Du behöver det i hello följande steg.

Du kan använda en befintlig blobbehållare för dina filöverföringar eller skapa nya:

* toolist hello befintlig blob-behållare i ditt lagringskonto, Använd hello följande kommando:

    ```azurecli
    az storage container list --connection-string "{your storage account connection string}"
    ```

* toocreate en blob-behållare i ditt lagringskonto, Använd hello följande kommando:

    ```azurecli
    az storage container create --name {container name} --connection-string "{your storage account connection string}"
    ```

## <a name="file-upload"></a>Ladda upp filen

Du kan nu konfigurera din IoT-hubb tooenable [filen överför funktioner] [ lnk-upload] med information om ditt lagringskonto.

hello konfigurationen kräver hello följande värden:

**Lagringsbehållaren**: en blob-behållare i ett Azure storage-konto i din aktuella Azure-prenumeration tooassociate med IoT-hubben. Du har hämtat hello information om nödvändiga lagringskonto i föregående avsnitt hello. IoT-hubb genererar automatiskt SAS URI: er med skriva behörigheter toothis blob-behållare för toouse enheter när de överför filer.

**Ta emot meddelanden för överförda filer**: aktivera eller inaktivera filen överför meddelanden.

**SAS TTL**: den här inställningen är hello time-to-live av hello SAS URI returneras toohello enhet av IoT-hubb. Ange tooone timme som standard.

**Filen notification inställningar Standardbegränsning**: hello time-to-live av en fil överför meddelande innan det förfaller. Ange tooone dagen som standard.

**Meddelande leverans av maximalt antal filer**: hello gånger hello IoT-hubb försök toodeliver ett meddelande om överföringen. Ange too10 som standard.

Använd följande Azure CLI-kommandon tooconfigure hello överför inställningar på din IoT-hubb hello:

En bash shell användning:

```azurecli
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.connectionString="{your storage account connection string}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.containerName="{your storage container name}"
az iot hub update --name {your iot hub name} --set properties.storageEndpoints.'$default'.sasTtlAsIso8601=PT1H0M0S

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

På en Windows kommandotolk användning:

```azurecli
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.connectionString="{your storage account connection string}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.containerName="{your storage container name}""
az iot hub update --name {your iot hub name} --set "properties.storageEndpoints.$default.sasTtlAsIso8601=PT1H0M0S"

az iot hub update --name {your iot hub name} --set properties.enableFileUploadNotifications=true
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.maxDeliveryCount=10
az iot hub update --name {your iot hub name} --set properties.messagingEndpoints.fileNotifications.ttlAsIso8601=PT1H0M0S
```

Du kan granska hello filen överför konfiguration på din IoT-hubb med hello följande kommando:

```azurecli
az iot hub show --name {your iot hub name}
```

## <a name="next-steps"></a>Nästa steg

Mer information om funktionerna i hello filen överför IoT-hubb finns [överföra filer från en enhet][lnk-upload].

Följ dessa länkar toolearn mer om hur du hanterar Azure IoT-hubb:

* [Massinläsning hantera IoT-enheter][lnk-bulk]
* [IoT-hubb mått][lnk-metrics]
* [Åtgärder som övervakning][lnk-monitor]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Utvecklarhandbok för IoT-hubb][lnk-devguide]
* [Simulera en enhet med IoT kant][lnk-iotedge]
* [Skydda din IoT-lösning från hello bakgrund][lnk-securing]

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