---
title: "Anpassade inställningar för Apptjänstmiljöer"
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
ms.openlocfilehash: 687475fae0c90713c15e8abbb92b71059eae81c0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="custom-configuration-settings-for-app-service-environments"></a><span data-ttu-id="1ea7a-103">Anpassade konfigurationsinställningar för Apptjänstmiljöer</span><span class="sxs-lookup"><span data-stu-id="1ea7a-103">Custom configuration settings for App Service Environments</span></span>
## <a name="overview"></a><span data-ttu-id="1ea7a-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1ea7a-104">Overview</span></span>
<span data-ttu-id="1ea7a-105">Eftersom Apptjänstmiljöer är isolerad för att en kund är vissa konfigurationsinställningar som kan tillämpas enbart på Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-105">Because App Service Environments are isolated to a single customer, there are certain configuration settings that can be applied exclusively to App Service Environments.</span></span> <span data-ttu-id="1ea7a-106">Den här artikeln beskrivs de olika anpassningar som är tillgängliga för Apptjänstmiljöer.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-106">This article documents the various specific customizations that are available for App Service Environments.</span></span>

<span data-ttu-id="1ea7a-107">Om du inte har en Apptjänst-miljö, se [så här skapar du en Apptjänst-miljö](app-service-web-how-to-create-an-app-service-environment.md).</span><span class="sxs-lookup"><span data-stu-id="1ea7a-107">If you do not have an App Service Environment, see [How to Create an App Service Environment](app-service-web-how-to-create-an-app-service-environment.md).</span></span>

<span data-ttu-id="1ea7a-108">Du kan lagra Apptjänstmiljö anpassningar med hjälp av en matris i den nya **clusterSettings** attribut.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-108">You can store App Service Environment customizations by using an array in the new **clusterSettings** attribute.</span></span> <span data-ttu-id="1ea7a-109">Det här attributet finns i ”egenskaper” ordlistan av den *hostingEnvironments* Azure Resource Manager-entiteten.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-109">This attribute is found in the "Properties" dictionary of the *hostingEnvironments* Azure Resource Manager entity.</span></span>

<span data-ttu-id="1ea7a-110">Följande förkortas Resource Manager mallen fragment visas den **clusterSettings** attribut:</span><span class="sxs-lookup"><span data-stu-id="1ea7a-110">The following abbreviated Resource Manager template snippet shows the **clusterSettings** attribute:</span></span>

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

<span data-ttu-id="1ea7a-111">Den **clusterSettings** attribut kan ingå i en Resource Manager-mall för att uppdatera Apptjänst-miljön.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-111">The **clusterSettings** attribute can be included in a Resource Manager template to update the App Service Environment.</span></span>

## <a name="use-azure-resource-explorer-to-update-an-app-service-environment"></a><span data-ttu-id="1ea7a-112">Använda Azure Resource Explorer för att uppdatera en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="1ea7a-112">Use Azure Resource Explorer to update an App Service Environment</span></span>
<span data-ttu-id="1ea7a-113">Du kan också uppdatera Apptjänst-miljön med hjälp av [resursutforskaren Azure](https://resources.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1ea7a-113">Alternatively, you can update the App Service Environment by using [Azure Resource Explorer](https://resources.azure.com).</span></span>  

1. <span data-ttu-id="1ea7a-114">Gå till noden i resursutforskaren, för Apptjänst-miljön (**prenumerationer** > **resursgrupper** > **providers** > **Microsoft.Web** > **hostingEnvironments**).</span><span class="sxs-lookup"><span data-stu-id="1ea7a-114">In Resource Explorer, go to the node for the App Service Environment (**subscriptions** > **resourceGroups** > **providers** > **Microsoft.Web** > **hostingEnvironments**).</span></span> <span data-ttu-id="1ea7a-115">Klicka på den specifika Apptjänstmiljö som du vill uppdatera.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-115">Then click the specific App Service Environment that you want to update.</span></span>
2. <span data-ttu-id="1ea7a-116">I den högra rutan, klickar du på **läsning och skrivning** i det övre verktygsfältet för att tillåta interaktiva redigering i Resursläsaren.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-116">In the right pane, click **Read/Write** in the upper toolbar to allow interactive editing in Resource Explorer.</span></span>  
3. <span data-ttu-id="1ea7a-117">Klicka på blå **redigera** knappen så att Resource Manager-mall kan redigeras.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-117">Click the blue **Edit** button to make the Resource Manager template editable.</span></span>
4. <span data-ttu-id="1ea7a-118">Bläddra längst ned i den högra rutan.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-118">Scroll to the bottom of the right pane.</span></span> <span data-ttu-id="1ea7a-119">Den **clusterSettings** attributet är på längst ned, där du kan ange eller uppdatera dess värde.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-119">The **clusterSettings** attribute is at the very bottom, where you can enter or update its value.</span></span>
5. <span data-ttu-id="1ea7a-120">Skriv (eller kopiera och klistra in) matrisen konfigurationsvärden som du vill ha i den **clusterSettings** attribut.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-120">Type (or copy and paste) the array of configuration values you want in the **clusterSettings** attribute.</span></span>  
6. <span data-ttu-id="1ea7a-121">Klicka på gröna **PLACERA** knappen som finns längst upp i den högra rutan för att utföra ändringen i Apptjänst-miljön.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-121">Click the green **PUT** button that's located at the top of the right pane to commit the change to the App Service Environment.</span></span>

<span data-ttu-id="1ea7a-122">Men du skickar ändringen tar ungefär 30 minuter multiplicerat med antalet synliga komponenter i Apptjänst-miljön för att ändringarna ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-122">However you submit the change, it takes roughly 30 minutes multiplied by the number of front ends in the App Service Environment for the change to take effect.</span></span>
<span data-ttu-id="1ea7a-123">Till exempel om en Apptjänst-miljö har fyra frontwebbservrarna, tar ungefär två timmar för av konfigurationsuppdateringen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-123">For example, if an App Service Environment has four front ends, it will take roughly two hours for the configuration update to finish.</span></span> <span data-ttu-id="1ea7a-124">Medan konfigurationsändringen lyfts, kan någon annan skalning operations eller ändra konfigurationsåtgärder ske i Apptjänst-miljön.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-124">While the configuration change is being rolled out, no other scaling operations or configuration change operations can take place in the App Service Environment.</span></span>

## <a name="disable-tls-10"></a><span data-ttu-id="1ea7a-125">Inaktivera TLS 1.0</span><span class="sxs-lookup"><span data-stu-id="1ea7a-125">Disable TLS 1.0</span></span>
<span data-ttu-id="1ea7a-126">En återkommande fråga från kunder, särskilt kunder som arbetar med PCI-överensstämmelse granskningar, så att uttryckligen inaktivera TLS 1.0 för sina appar.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-126">A recurring question from customers, especially customers who are dealing with PCI compliance audits, is how to explicitly disable TLS 1.0 for their apps.</span></span>

<span data-ttu-id="1ea7a-127">TLS 1.0 kan inaktiveras via följande **clusterSettings** post:</span><span class="sxs-lookup"><span data-stu-id="1ea7a-127">TLS 1.0 can be disabled through the following **clusterSettings** entry:</span></span>

        "clusterSettings": [
            {
                "name": "DisableTls1.0",
                "value": "1"
            }
        ],

## <a name="change-tls-cipher-suite-order"></a><span data-ttu-id="1ea7a-128">Ändra TLS cipher suite order</span><span class="sxs-lookup"><span data-stu-id="1ea7a-128">Change TLS cipher suite order</span></span>
<span data-ttu-id="1ea7a-129">En annan fråga från kunder är om de kan ändra listan över chiffer som förhandlas fram av servern, och detta kan uppnås genom att ändra den **clusterSettings** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-129">Another question from customers is if they can modify the list of ciphers negotiated by their server and this can be achieved by modifying the **clusterSettings** as shown below.</span></span> <span data-ttu-id="1ea7a-130">Listan över tillgängliga krypteringssviter kan hämtas från [MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span><span class="sxs-lookup"><span data-stu-id="1ea7a-130">The list of cipher suites available can be retrieved from [this MSDN article](https://msdn.microsoft.com/library/windows/desktop/aa374757\(v=vs.85\).aspx).</span></span>

        "clusterSettings": [
            {
                "name": "FrontEndSSLCipherSuiteOrder",
                "value": "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384_P256,TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256_P256,TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA_P256,TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA_P256"
            }
        ],

> [!WARNING]
> <span data-ttu-id="1ea7a-131">Om felaktiga värden har angetts för chiffersviten SChannel inte förstår kan alla TLS-kommunikation till servern sluta fungera.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-131">If incorrect values are set for the cipher suite that SChannel cannot understand, all TLS communication to your server might stop functioning.</span></span> <span data-ttu-id="1ea7a-132">I så fall behöver du ta bort den *FrontEndSSLCipherSuiteOrder* post från **clusterSettings** och skicka den uppdaterade mallen Resource Manager om du vill återgå till standardinställningarna cipher suite.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-132">In such a case, you will need to remove the *FrontEndSSLCipherSuiteOrder* entry from **clusterSettings** and submit the updated Resource Manager template to revert back to the default cipher suite settings.</span></span>  <span data-ttu-id="1ea7a-133">Använd den här funktionen med försiktighet.</span><span class="sxs-lookup"><span data-stu-id="1ea7a-133">Please use this functionality with caution.</span></span>
> 
> 

## <a name="get-started"></a><span data-ttu-id="1ea7a-134">Kom igång</span><span class="sxs-lookup"><span data-stu-id="1ea7a-134">Get started</span></span>
<span data-ttu-id="1ea7a-135">Mallwebbplatsen för Azure Quickstart Resource Manager-innehåller en mall med basdefinitionen för [att skapa en Apptjänst-miljö](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span><span class="sxs-lookup"><span data-stu-id="1ea7a-135">The Azure Quickstart Resource Manager template site includes a template with the base definition for [creating an App Service Environment](https://azure.microsoft.com/documentation/templates/201-web-app-ase-create/).</span></span>

<!-- LINKS -->

<!-- IMAGES -->
