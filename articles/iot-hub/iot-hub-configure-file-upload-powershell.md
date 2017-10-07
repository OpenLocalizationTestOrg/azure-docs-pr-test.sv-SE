---
title: aaaUse hello Azure PowerShell tooconfigure ladda upp filen | Microsoft Docs
description: "Hur toouse hello Azure PowerShell-cmdlets tooconfigure din IoT-hubb tooenable filöverföringar från anslutna enheter. Innehåller information om hur du konfigurerar hello mål Azure storage-konto."
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
ms.openlocfilehash: 9dcdc41693c09cece411921b30c91d7b3db47395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>Konfigurera IoT-hubb filöverföringar med hjälp av PowerShell

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

toouse hello [filen överför funktioner i IoT-hubb][lnk-upload], måste du först associera ett Azure storage-konto med din IoT-hubb. Du kan använda ett befintligt lagringskonto eller skapa en ny.

toocomplete den här kursen behöver du hello följande:

* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.
* [Azure PowerShell-cmdlets][lnk-powershell-install].
* Azure IoT-hubb. Om du inte har en IoT-hubb kan du använda hello [cmdlet New-AzureRmIoTHub] [ lnk-powershell-iothub] toocreate en eller Använd hello portal för[skapar en IoT-hubb] [ lnk-portal-hub].
* Ett Azure-lagringskonto. Om du inte har ett Azure storage-konto kan du använda hello [Azure Storage PowerShell-cmdlets] [ lnk-powershell-storage] toocreate en eller Använd hello portal för[skapa ett lagringskonto] [ lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Logga in och ange ditt Azure-konto

Logga in tooyour Azure-konto och välja din prenumeration.

1. Kör i PowerShell-Kommandotolken hello hello **Login-AzureRmAccount** cmdlet:

    ```powershell
    Login-AzureRmAccount
    ```

1. Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter. Använd följande kommando toolist hello Azure-prenumerationer tillgängliga för dig toouse hello:

    ```powershell
    Get-AzureRMSubscription
    ```

    Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toomanage din IoT-hubb hello. Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>Hämta information om ditt lagringskonto

hello följande steg förutsätter att du har skapat ditt lagringskonto med hjälp av hello **Resource Manager** distributionsmodell och inte hello **klassiska** distributionsmodell.

tooconfigure filöverföringar från dina enheter måste du hello anslutningssträngen för Azure storage-konto. hello storage-konto måste vara i hello samma prenumeration som din IoT-hubb. Du måste också hello namnet på en blob-behållare i hello storage-konto. Använd följande kommando tooretrieve hello dina nycklar för lagringskonto:

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Anteckna hello **key1** nyckelvärdet för storage-konto. Du behöver det i hello följande steg.

Du kan använda en befintlig blobbehållare för dina filöverföringar eller skapa nya:

* toolist hello befintlig blob-behållare i ditt lagringskonto, Använd hello följande kommandon:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* toocreate en blob-behållare i ditt lagringskonto, Använd hello följande kommandon:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>Konfigurera din IoT-hubb

Du kan nu konfigurera din IoT-hubb tooenable [filen överför funktioner] [ lnk-upload] med information om ditt lagringskonto.

hello konfigurationen kräver hello följande värden:

**Lagringsbehållaren**: en blob-behållare i ett Azure storage-konto i din aktuella Azure-prenumeration tooassociate med IoT-hubben. Du har hämtat hello information om nödvändiga lagringskonto i föregående avsnitt hello. IoT-hubb genererar automatiskt SAS URI: er med skriva behörigheter toothis blob-behållare för toouse enheter när de överför filer.

**Ta emot meddelanden för överförda filer**: aktivera eller inaktivera filen överför meddelanden.

**SAS TTL**: den här inställningen är hello time-to-live av hello SAS URI returneras toohello enhet av IoT-hubb. Ange tooone timme som standard.

**Filen notification inställningar Standardbegränsning**: hello time-to-live av en fil överför meddelande innan det förfaller. Ange tooone dagen som standard.

**Meddelande leverans av maximalt antal filer**: hello gånger hello IoT-hubb försök toodeliver ett meddelande om överföringen. Ange too10 som standard.

Använd följande PowerShell-cmdlet tooconfigure hello överför inställningar på din IoT-hubb hello:

```powershell
Set-AzureRmIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
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

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-powershell-storage]: https://docs.microsoft.com/powershell/module/azurerm.storage/
[lnk-powershell-iothub]: https://docs.microsoft.com/powershell/module/azurerm.iothub/new-azurermiothub
[lnk-portal-hub]: iot-hub-create-through-portal.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal-storage]:../storage/common/storage-create-storage-account.md