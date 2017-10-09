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
# <a name="custom-configuration-settings-for-app-service-environments"></a><span data-ttu-id="961e0-103">Anpassade konfigurationsinställningar för Apptjänstmiljöer</span><span class="sxs-lookup"><span data-stu-id="961e0-103">Custom configuration settings for App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="961e0-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="961e0-104">Overview</span></span>
<span data-ttu-id="961e0-105">Eftersom Apptjänstmiljöer är isolerad tooa kund, är vissa konfigurationsinställningar som kan användas exklusivt tooApp miljöer.</span><span class="sxs-lookup"><span data-stu-id="961e0-105">Because App Service Environments are isolated tooa single customer, there are certain configuration settings that can be applied exclusively tooApp Service Environments.</span></span> <span data-ttu-id="961e0-106">Den här artikeln dokument hello olika anpassningar som är tillgängliga för Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="961e0-106">This article documents hello various specific customizations that are available for App Service Environments.</span></span>

<span data-ttu-id="961e0-107">Om du inte har en Apptjänst-miljö, se [hur tooCreate en Apptjänstmiljö](app-service-web-how-to-create-an-app-service-environment.md).</span><span class="sxs-lookup"><span data-stu-id="961e0-107">If you do not have an App Service Environment, see [How tooCreate an App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).</span></span>

<span data-ttu-id="961e0-108">Du kan lagra Apptjänstmiljö anpassningar med hjälp av en matris i hello nya **clusterSettings** attribut.</span><span class="sxs-lookup"><span data-stu-id="961e0-108">You can store App Service Environment customizations by using an array in hello new **clusterSettings** attribute.</span></span> <span data-ttu-id="961e0-109">Det här attributet kan hittas i hello ”egenskaper” uppslagslista av hello *hostingEnvironments* Azure Resource Manager-entiteten.</span><span class="sxs-lookup"><span data-stu-id="961e0-109">This attribute is found in hello "Properties" dictionary of hello *hostingEnvironments* Azure Resource Manager entity.</span></span>

<span data-ttu-id="961e0-110">hello följande förkortade Resource Manager mallen utdrag visar hello **clusterSettings** attribut:</span><span class="sxs-lookup"><span data-stu-id="961e0-110">hello following abbreviated Resource Manager template snippet shows hello **clusterSettings** attribute:</span></span>

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

<span data-ttu-id="961e0-111">Hej **clusterSettings** attribut kan ingå i en Resource Manager mallen tooupdate hello Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="961e0-111">hello **clusterSettings** attribute can be included in a Resource Manager template tooupdate hello App Service Environment.</span></span>

## <a name="use-azure-resource-explorer-tooupdate-an-app-service-environment"></a><span data-ttu-id="961e0-112">Använd resursutforskaren Azure tooupdate en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="961e0-112">Use Azure Resource Explorer tooupdate an App Service Environment</span></span>
<span data-ttu-id="961e0-113">Du kan också uppdatera hello Apptjänst-miljön med hjälp av [resursutforskaren Azure](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="961e0-113">Alternatively, you can update hello App Service Environment by using [Azure Resource Explorer](https://resources.azure.com).</span></span>  

1. <span data-ttu-id="961e0-114">Resursutforskaren, gå toohello nod för hello Apptjänst-miljö (**prenumerationer** > **resursgrupper** > **providers**  >  **Microsoft.Web** > **hostingEnvironments**).</span><span class="sxs-lookup"><span data-stu-id="961e0-114">In Resource Explorer, go toohello node for hello App Service Environment (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**).</span></span> <span data-ttu-id="961e0-115">Klicka sedan på hello specifika Apptjänst-miljö som du vill tooupdate.</span><span class="sxs-lookup"><span data-stu-id="961e0-115">Then click hello specific App Service Environment that you want tooupdate.</span></span>
2. <span data-ttu-id="961e0-116">I hello högra rutan, klickar du på **läsning och skrivning** i hello övre verktygsfältet tooallow interaktiva redigering i Resursläsaren.</span><span class="sxs-lookup"><span data-stu-id="961e0-116">In hello right pane, click **Read/Write** in hello upper toolbar tooallow interactive editing in Resource Explorer.</span></span>  
3. <span data-ttu-id="961e0-117">Klicka på hello blå **redigera** knappen toomake hello Resource Manager-mall kan redigeras.</span><span class="sxs-lookup"><span data-stu-id="961e0-117">Click hello blue **Edit** button toomake hello Resource Manager template editable.</span></span>
4. <span data-ttu-id="961e0-118">Rulla toohello längst ned på hello högra rutan.</span><span class="sxs-lookup"><span data-stu-id="961e0-118">Scroll toohello bottom of hello right pane.</span></span> <span data-ttu-id="961e0-119">Hej **clusterSettings** attributet är hello mycket längst ned i där du kan ange eller uppdatera dess värde.</span><span class="sxs-lookup"><span data-stu-id="961e0-119">hello **clusterSettings** attribute is at hello very bottom, where you can enter or update its value.</span></span>
5. <span data-ttu-id="961e0-120">Typen (eller kopiera och klistra in) hello-matris med konfigurationsvärden som du vill använda i hello **clusterSettings** attribut.</span><span class="sxs-lookup"><span data-stu-id="961e0-120">Type (or copy and paste) hello array of configuration values you want in hello **clusterSettings** attribute.</span></span>  
6. <span data-ttu-id="961e0-121">Klicka på hello grön **PLACERA** knappen som finns hello överst i hello högra fönstret toocommit hello ändra toohello Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="961e0-121">Click hello green **PUT** button that's located at hello top of hello right pane toocommit hello change toohello App Service Environment.</span></span>

<span data-ttu-id="961e0-122">Men du skickar hello ändra tar ungefär 30 minuter multiplicerat med hello antal frontwebbservrarna i hello Apptjänstmiljö för hello ändra tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="961e0-122">However you submit hello change, it takes roughly 30 minutes multiplied by hello number of front ends in hello App Service Environment for hello change tootake effect.</span></span>
<span data-ttu-id="961e0-123">Till exempel om en Apptjänst-miljö har fyra frontwebbservrarna, tar ungefär två timmar för hello configuration uppdatering toofinish.</span><span class="sxs-lookup"><span data-stu-id="961e0-123">For example, if an App Service Environment has four front ends, it will take roughly two hours for hello configuration update toofinish.</span></span> <span data-ttu-id="961e0-124">Medan hello konfigurationsändring lyfts, kan någon annan skalning operations eller ändra konfigurationsåtgärder ske på hello Apptjänst-miljö.</span><span class="sxs-lookup"><span data-stu-id="961e0-124">While hello configuration change is being rolled out, no other scaling operations or configuration change operations can take place in hello App Service Environment.</span></span>

## <a name="disable-tls-10"></a><span data-ttu-id="961e0-125">Inaktivera TLS 1.0</span><span class="sxs-lookup"><span data-stu-id="961e0-125">Disable TLS 1.0</span></span>
<span data-ttu-id="961e0-126">En återkommande fråga från kunder, särskilt de kunder som arbetar med PCI-överensstämmelse granskningar, är hur tooexplicitly inaktivera TLS 1.0 för sina appar.</span><span class="sxs-lookup"><span data-stu-id="961e0-126">A recurring question from customers, especially customers who are dealing with PCI compliance audits, is how tooexplicitly disable TLS 1.0 for their apps.</span></span>

<span data-ttu-id="961e0-127">TLS 1.0 kan inaktiveras via hello följande **clusterSettings** post:</span><span class="sxs-lookup"><span data-stu-id="961e0-127">TLS 1.0 can be disabled through hello following **clusterSettings** entry:</span></span>

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a><span data-ttu-id="961e0-128">Ändra TLS cipher suite order</span><span class="sxs-lookup"><span data-stu-id="961e0-128">Change TLS cipher suite order</span></span>
<span data-ttu-id="961e0-129">En annan fråga från kunder är om de kan ändra hello lista över chiffer som förhandlas fram av servern, och detta kan uppnås genom att ändra hello **clusterSettings** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="961e0-129">Another question from customers is if they can modify hello list of ciphers negotiated by their server and this can be achieved by modifying hello **clusterSettings** as shown below.</span></span> <span data-ttu-id="961e0-130">hello lista över tillgängliga krypteringssviter kan hämtas från [MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span><span class="sxs-lookup"><span data-stu-id="961e0-130">hello list of cipher suites available can be retrieved from [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span></span>

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> <span data-ttu-id="961e0-131">Om felaktiga värden har angetts för hello cipher suite som SChannel inte förstår kan alla TLS tooyour kommunikationsservern sluta fungera.</span><span class="sxs-lookup"><span data-stu-id="961e0-131">If incorrect values are set for hello cipher suite that SChannel cannot understand, all TLS communication tooyour server might stop functioning.</span></span> <span data-ttu-id="961e0-132">I så fall behöver du tooremove hello *FrontEndSSLCipherSuiteOrder* post från **clusterSettings** och skicka hello uppdateras Resource Manager mallen toorevert tillbaka toohello standard cipher Suite-inställningar.</span><span class="sxs-lookup"><span data-stu-id="961e0-132">In such a case, you will need tooremove hello *FrontEndSSLCipherSuiteOrder* entry from **clusterSettings** and submit hello updated Resource Manager template toorevert back toohello default cipher suite settings.</span></span>  <span data-ttu-id="961e0-133">Använd den här funktionen med försiktighet.</span><span class="sxs-lookup"><span data-stu-id="961e0-133">Please use this functionality with caution.</span></span>
> 
> 

## <a name="get-started"></a><span data-ttu-id="961e0-134">Kom igång</span><span class="sxs-lookup"><span data-stu-id="961e0-134">Get started</span></span>
<span data-ttu-id="961e0-135">plats för hello Azure Quickstart Resource Manager-mallen innehåller en mall med hello basdefinitionen för [att skapa en Apptjänst-miljö](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span><span class="sxs-lookup"><span data-stu-id="961e0-135">hello Azure Quickstart Resource Manager template site includes a template with hello base definition for [creating an App Service Environment](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span></span>

<!-- LINKS -->

<!-- IMAGES -->
