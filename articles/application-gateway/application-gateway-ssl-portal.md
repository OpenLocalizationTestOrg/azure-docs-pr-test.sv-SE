---
title: Konfigurera SSL-avlastning - Azure Application Gateway - Azure-portalen | Microsoft Docs
description: "Den här sidan innehåller instruktioner för att skapa en Programgateway med SSL-avlastning med hjälp av portalen"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
ms.openlocfilehash: f61be0cc4c9274c9914f7c468ce48a2a3d0a4f4a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a><span data-ttu-id="bc713-103">Konfigurera en Programgateway för SSL-avlastning med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="bc713-103">Configure an application gateway for SSL offload by using the portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="bc713-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bc713-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="bc713-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bc713-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="bc713-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="bc713-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="bc713-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="bc713-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="bc713-108">Azure Application Gateway kan konfigureras att avsluta SSL-sessionen (Secure Sockets Layer) på gatewayen så att du undviker kostsamma SSL-dekrypteringsaktiviteter i webbservergruppen.</span><span class="sxs-lookup"><span data-stu-id="bc713-108">Azure Application Gateway can be configured to terminate the Secure Sockets Layer (SSL) session at the gateway to avoid costly SSL decryption tasks to happen at the web farm.</span></span> <span data-ttu-id="bc713-109">SSL-avlastning förenklar också frontend-serverkonfigurationen och hanteringen av webbappen.</span><span class="sxs-lookup"><span data-stu-id="bc713-109">SSL offload also simplifies the front-end server setup and management of the web application.</span></span>

## <a name="scenario"></a><span data-ttu-id="bc713-110">Scenario</span><span class="sxs-lookup"><span data-stu-id="bc713-110">Scenario</span></span>

<span data-ttu-id="bc713-111">Följande scenario går igenom hur du konfigurerar SSL avlasta på en befintlig gateway för programmet.</span><span class="sxs-lookup"><span data-stu-id="bc713-111">The following scenario goes through configuring SSL offload on an existing application gateway.</span></span> <span data-ttu-id="bc713-112">Scenariot förutsätter att du redan har följt stegen för att [skapa en Programgateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bc713-112">The scenario assumes that you have already followed the steps to [Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="bc713-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bc713-113">Before you begin</span></span>

<span data-ttu-id="bc713-114">Ett certifikat krävs för att konfigurera SSL-avlastning med en Programgateway.</span><span class="sxs-lookup"><span data-stu-id="bc713-114">To configure SSL offload with an application gateway, a certificate is required.</span></span> <span data-ttu-id="bc713-115">Det här certifikatet har lästs in på programgatewayen och används för att kryptera och dekryptera den trafik som skickas via SSL.</span><span class="sxs-lookup"><span data-stu-id="bc713-115">This certificate is loaded on the application gateway and used to encrypt and decrypt the traffic sent via SSL.</span></span> <span data-ttu-id="bc713-116">Certifikatet måste vara i formatet Personal Information Exchange (pfx).</span><span class="sxs-lookup"><span data-stu-id="bc713-116">The certificate needs to be in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="bc713-117">Det här filformatet kan för den privata nyckeln exporteras som krävs av programgatewayen att genomföra kryptering och dekryptering av trafik.</span><span class="sxs-lookup"><span data-stu-id="bc713-117">This file format allows for the private key to be exported which is required by the application gateway to perform the encryption and decryption of traffic.</span></span>

## <a name="add-an-https-listener"></a><span data-ttu-id="bc713-118">Lägg till en HTTPS-lyssnare</span><span class="sxs-lookup"><span data-stu-id="bc713-118">Add an HTTPS listener</span></span>

<span data-ttu-id="bc713-119">HTTPS-lyssnaren söker efter trafik baserat på dess konfiguration och hjälper till att vidarebefordra trafiken till backend-pooler.</span><span class="sxs-lookup"><span data-stu-id="bc713-119">The HTTPS listener looks for traffic based on its configuration and helps route the traffic to the backend pools.</span></span>

### <a name="step-1"></a><span data-ttu-id="bc713-120">Steg 1</span><span class="sxs-lookup"><span data-stu-id="bc713-120">Step 1</span></span>

<span data-ttu-id="bc713-121">Gå till Azure-portalen och väljer en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="bc713-121">Navigate to the Azure portal and select an existing application gateway</span></span>

### <a name="step-2"></a><span data-ttu-id="bc713-122">Steg 2</span><span class="sxs-lookup"><span data-stu-id="bc713-122">Step 2</span></span>

<span data-ttu-id="bc713-123">Klicka på lyssnare och klicka på Lägg till för att lägga till en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="bc713-123">Click Listeners and click the Add button to add a listener.</span></span>

![Översikt över App nätverksgateway-bladet][1]

### <a name="step-3"></a><span data-ttu-id="bc713-125">Steg 3</span><span class="sxs-lookup"><span data-stu-id="bc713-125">Step 3</span></span>

<span data-ttu-id="bc713-126">Fyll i informationen som krävs för lyssnaren och ladda upp PFX-certifikat, när du är klar klickar du på OK.</span><span class="sxs-lookup"><span data-stu-id="bc713-126">Fill out the required information for the listener and upload the .pfx certificate, when complete click OK.</span></span>

<span data-ttu-id="bc713-127">**Namnet** -värdet är ett eget namn för lyssnare.</span><span class="sxs-lookup"><span data-stu-id="bc713-127">**Name** - This value is a friendly name of the listener.</span></span>

<span data-ttu-id="bc713-128">**Frontend-IP-konfiguration** -värdet är klientdelens IP-konfiguration som används för lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="bc713-128">**Frontend IP configuration** - This value is the frontend IP configuration that is used for the listener.</span></span>

<span data-ttu-id="bc713-129">**Klientdelsport (namn/Port)** – ett eget namn för porten som används på klientdelen för programgatewayen och den faktiska porten används.</span><span class="sxs-lookup"><span data-stu-id="bc713-129">**Frontend port (Name/Port)** - A friendly name for the port used on the front end of the application gateway and the actual port used.</span></span>

<span data-ttu-id="bc713-130">**Protokollet** -en växel för att avgöra om https eller http används för klientdelen.</span><span class="sxs-lookup"><span data-stu-id="bc713-130">**Protocol** - A switch to determine if https or http is used for the front end.</span></span>

<span data-ttu-id="bc713-131">**Certifikatet (och lösenord)** - avlastning om SSL används ett PFX-certifikat krävs för den här inställningen och ett eget namn och lösenord krävs.</span><span class="sxs-lookup"><span data-stu-id="bc713-131">**Certificate (Name/Password)** - If SSL offload is used, a .pfx certificate is required for this setting and a friendly name and password are required.</span></span>

![lyssnare bladet Lägg till][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a><span data-ttu-id="bc713-133">Skapa en regel och koppla den till lyssnaren</span><span class="sxs-lookup"><span data-stu-id="bc713-133">Create a rule and associate it to the listener</span></span>

<span data-ttu-id="bc713-134">Lyssnaren har skapats.</span><span class="sxs-lookup"><span data-stu-id="bc713-134">The listener has now been created.</span></span> <span data-ttu-id="bc713-135">Det är dags att skapa en regel för att hantera trafik från lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="bc713-135">It is time to create a rule to handle the traffic from the listener.</span></span> <span data-ttu-id="bc713-136">Regler som definierar hur trafiken dirigeras till serverdelspooler baserat på flera konfigurationsinställningar, inklusive om cookie-baserad session tillhörighet används, protokoll, port och hälsoavsökningar.</span><span class="sxs-lookup"><span data-stu-id="bc713-136">Rules define how traffic is routed to the backend pools based on multiple configuration settings, including whether cookie-based session affinity is used, protocol, port, and health probes.</span></span>

### <a name="step-1"></a><span data-ttu-id="bc713-137">Steg 1</span><span class="sxs-lookup"><span data-stu-id="bc713-137">Step 1</span></span>

<span data-ttu-id="bc713-138">Klicka på den **regler** för Programgateway, och klicka sedan på Lägg till.</span><span class="sxs-lookup"><span data-stu-id="bc713-138">Click the **Rules** of the application gateway, and then click Add.</span></span>

![appen gateway regler bladet][3]

### <a name="step-2"></a><span data-ttu-id="bc713-140">Steg 2</span><span class="sxs-lookup"><span data-stu-id="bc713-140">Step 2</span></span>

<span data-ttu-id="bc713-141">På den **Lägg till regel** bladet Skriv i det egna namnet för regeln och välj lyssnaren som skapades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="bc713-141">On the **Add basic rule** blade, type in the friendly name for the rule and choose the listener created in the previous step.</span></span> <span data-ttu-id="bc713-142">Välj lämplig serverdelspool och HTTP-inställningen och klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="bc713-142">Choose the appropriate backend pool and http setting and click **OK**</span></span>

![fönster för HTTPS-inställningar][4]

<span data-ttu-id="bc713-144">Inställningarna sparas nu i programgatewayen.</span><span class="sxs-lookup"><span data-stu-id="bc713-144">The settings are now saved to the application gateway.</span></span> <span data-ttu-id="bc713-145">Spara bearbeta för de här inställningarna kan ta en stund innan de kan visa via portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="bc713-145">The save process for these settings may take a while before they are available to view through the portal or through PowerShell.</span></span> <span data-ttu-id="bc713-146">När sparat programgatewayen hanterar kryptering och dekryptering av trafik.</span><span class="sxs-lookup"><span data-stu-id="bc713-146">Once saved the application gateway handles the encryption and decryption of traffic.</span></span> <span data-ttu-id="bc713-147">All trafik mellan programgatewayen och backend-webbservrar hanteras via http.</span><span class="sxs-lookup"><span data-stu-id="bc713-147">All traffic between the application gateway and the backend web servers will be handled over http.</span></span> <span data-ttu-id="bc713-148">All kommunikation till klienten om initieras via https returneras till klienten krypteras.</span><span class="sxs-lookup"><span data-stu-id="bc713-148">Any communication back to the client if initiated over https will be returned to the client encrypted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc713-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc713-149">Next steps</span></span>

<span data-ttu-id="bc713-150">Information om hur du konfigurerar en anpassad hälsoavsökningen med Azure Programgateway finns [skapa en anpassad hälsoavsökningen](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bc713-150">To learn how to configure a custom health probe with Azure Application Gateway, see [Create a custom health probe](application-gateway-create-gateway-portal.md).</span></span>

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
