---
title: "aaaCustom inställningar för Apptjänstmiljöer"
description: "Anpassade konfigurationsinställningar för Apptjänstmiljöer"
services: app-service
documentationcenter: 
author: stefsch
manager: nirma
editor: 
ms.assetid: 1d1d85f3-6cc6-4d57-ae1a-5b37c642d812
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: stefsch
ms.openlocfilehash: 3d140688c88b389e71bfdd465c418339cccab3a6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a>Anpassade konfigurationsinställningar för Apptjänstmiljöer
## <a name="overview"></a>Översikt
Eftersom Apptjänstmiljöer är isolerad tooa kund, är vissa konfigurationsinställningar som kan användas exklusivt tooApp miljöer. Den här artikeln dokument hello olika anpassningar som är tillgängliga för Apptjänstmiljöer.

Om du inte har en Apptjänst-miljö, se [hur tooCreate en Apptjänstmiljö](app-service-web-how-to-create-an-app-service-environment.md).

Du kan lagra Apptjänstmiljö anpassningar med hjälp av en matris i hello nya **clusterSettings** attribut. Det här attributet kan hittas i hello ”egenskaper” uppslagslista av hello *hostingEnvironments* Azure Resource Manager-entiteten.

hello följande förkortade Resource Manager mallen utdrag visar hello **clusterSettings** attribut:

    "resources": [
    {
       "apiVersion": "2015-08-01",
       "type": "Microsoft.Web/hostingEnvironments",
       "name": ...,
       "location": ...,
       "properties": {
          "clusterSettings": [
             {
                 "name": "nameOfCustomSetting",
                 "value": "valueOfCustomSetting"
             }
          ],
          "workerPools": [ ...],
          etc...
       }
    }

Hej **clusterSettings** attribut kan ingå i en Resource Manager mallen tooupdate hello Apptjänst-miljö.

## <a name="use-azure-resource-explorer-tooupdate-an-app-service-environment"></a>Använd resursutforskaren Azure tooupdate en Apptjänst-miljö
Du kan också uppdatera hello Apptjänst-miljön med hjälp av [resursutforskaren Azure](https://resources.azure.com).  

1. Resursutforskaren, gå toohello nod för hello Apptjänst-miljö (**prenumerationer** > **resursgrupper** > **providers**  >  **Microsoft.Web** > **hostingEnvironments**). Klicka sedan på hello specifika Apptjänst-miljö som du vill tooupdate.
2. I hello högra rutan, klickar du på **läsning och skrivning** i hello övre verktygsfältet tooallow interaktiva redigering i Resursläsaren.  
3. Klicka på hello blå **redigera** knappen toomake hello Resource Manager-mall kan redigeras.
4. Rulla toohello längst ned på hello högra rutan. Hej **clusterSettings** attributet är hello mycket längst ned i där du kan ange eller uppdatera dess värde.
5. Typen (eller kopiera och klistra in) hello-matris med konfigurationsvärden som du vill använda i hello **clusterSettings** attribut.  
6. Klicka på hello grön **PLACERA** knappen som finns hello överst i hello högra fönstret toocommit hello ändra toohello Apptjänst-miljö.

Men du skickar hello ändra tar ungefär 30 minuter multiplicerat med hello antal frontwebbservrarna i hello Apptjänstmiljö för hello ändra tootake effekt.
Till exempel om en Apptjänst-miljö har fyra frontwebbservrarna, tar ungefär två timmar för hello configuration uppdatering toofinish. Medan hello konfigurationsändring lyfts, kan någon annan skalning operations eller ändra konfigurationsåtgärder ske på hello Apptjänst-miljö.

## <a name="disable-tls-10"></a>Inaktivera TLS 1.0
En återkommande fråga från kunder, särskilt de kunder som arbetar med PCI-överensstämmelse granskningar, är hur tooexplicitly inaktivera TLS 1.0 för sina appar.

TLS 1.0 kan inaktiveras via hello följande **clusterSettings** post:

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a>Ändra TLS cipher suite order
En annan fråga från kunder är om de kan ändra hello lista över chiffer som förhandlas fram av servern, och detta kan uppnås genom att ändra hello **clusterSettings** enligt nedan. hello lista över tillgängliga krypteringssviter kan hämtas från [MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> Om felaktiga värden har angetts för hello cipher suite som SChannel inte förstår kan alla TLS tooyour kommunikationsservern sluta fungera. I så fall behöver du tooremove hello *FrontEndSSLCipherSuiteOrder* post från **clusterSettings** och skicka hello uppdateras Resource Manager mallen toorevert tillbaka toohello standard cipher Suite-inställningar.  Använd den här funktionen med försiktighet.
> 
> 

## <a name="get-started"></a>Kom igång
plats för hello Azure Quickstart Resource Manager-mallen innehåller en mall med hello basdefinitionen för [att skapa en Apptjänst-miljö](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).

<!-- LINKS -->

<!-- IMAGES -->
