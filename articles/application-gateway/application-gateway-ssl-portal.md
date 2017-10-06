---
title: aaaConfigure SSL-avlastning - Azure Application Gateway - Azure-portalen | Microsoft Docs
description: "Den här sidan finns instruktioner toocreate en Programgateway med SSL-avlastning med hello-portalen"
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
ms.openlocfilehash: e87ac0bbe10ac45e307c18802741c7bc31764a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-hello-portal"></a><span data-ttu-id="50132-103">Konfigurera en Programgateway för SSL-avlastning med hello-portalen</span><span class="sxs-lookup"><span data-stu-id="50132-103">Configure an application gateway for SSL offload by using hello portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="50132-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="50132-104">Azure portal</span></span>](application-gateway-ssl-portal.md)
> * [<span data-ttu-id="50132-105">PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="50132-105">Azure Resource Manager PowerShell</span></span>](application-gateway-ssl-arm.md)
> * [<span data-ttu-id="50132-106">PowerShell och den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="50132-106">Azure Classic PowerShell</span></span>](application-gateway-ssl.md)
> * [<span data-ttu-id="50132-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50132-107">Azure CLI 2.0</span></span>](application-gateway-ssl-cli.md)

<span data-ttu-id="50132-108">Azure Application Gateway kan vara konfigurerade tooterminate hello Secure Sockets Layer (SSL)-session på hello gateway tooavoid kostsamma SSL dekryptering uppgifter toohappen på hello webbservergrupp.</span><span class="sxs-lookup"><span data-stu-id="50132-108">Azure Application Gateway can be configured tooterminate hello Secure Sockets Layer (SSL) session at hello gateway tooavoid costly SSL decryption tasks toohappen at hello web farm.</span></span> <span data-ttu-id="50132-109">SSL-avlastning förenklar också hello front-end-serverinstallation och hantering av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="50132-109">SSL offload also simplifies hello front-end server setup and management of hello web application.</span></span>

## <a name="scenario"></a><span data-ttu-id="50132-110">Scenario</span><span class="sxs-lookup"><span data-stu-id="50132-110">Scenario</span></span>

<span data-ttu-id="50132-111">hello följande scenario går igenom hur du konfigurerar SSL avlasta på en befintlig Programgateway.</span><span class="sxs-lookup"><span data-stu-id="50132-111">hello following scenario goes through configuring SSL offload on an existing application gateway.</span></span> <span data-ttu-id="50132-112">hello scenariot förutsätter att du har redan följt hello stegen för[skapa en Programgateway](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="50132-112">hello scenario assumes that you have already followed hello steps too[Create an Application Gateway](application-gateway-create-gateway-portal.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="50132-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="50132-113">Before you begin</span></span>

<span data-ttu-id="50132-114">tooconfigure SSL-avlastning med en Programgateway, krävs ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="50132-114">tooconfigure SSL offload with an application gateway, a certificate is required.</span></span> <span data-ttu-id="50132-115">Det här certifikatet har lästs in på hello Programgateway används tooencrypt och dekryptera hello trafik som skickas via SSL.</span><span class="sxs-lookup"><span data-stu-id="50132-115">This certificate is loaded on hello application gateway and used tooencrypt and decrypt hello traffic sent via SSL.</span></span> <span data-ttu-id="50132-116">hello certifikat måste toobe Personal Information Exchange (pfx)-format.</span><span class="sxs-lookup"><span data-stu-id="50132-116">hello certificate needs toobe in Personal Information Exchange (pfx) format.</span></span> <span data-ttu-id="50132-117">Det här filformatet tillåter hello privata nyckel toobe exporteras som krävs för hello programmet gateway tooperform hello kryptering och dekryptering av trafik.</span><span class="sxs-lookup"><span data-stu-id="50132-117">This file format allows for hello private key toobe exported which is required by hello application gateway tooperform hello encryption and decryption of traffic.</span></span>

## <a name="add-an-https-listener"></a><span data-ttu-id="50132-118">Lägg till en HTTPS-lyssnare</span><span class="sxs-lookup"><span data-stu-id="50132-118">Add an HTTPS listener</span></span>

<span data-ttu-id="50132-119">HTTPS-lyssnare för hello söker efter trafik baserat på dess konfiguration och hjälper väg hello trafik toohello serverdelspooler.</span><span class="sxs-lookup"><span data-stu-id="50132-119">hello HTTPS listener looks for traffic based on its configuration and helps route hello traffic toohello backend pools.</span></span>

### <a name="step-1"></a><span data-ttu-id="50132-120">Steg 1</span><span class="sxs-lookup"><span data-stu-id="50132-120">Step 1</span></span>

<span data-ttu-id="50132-121">Navigera toohello Azure-portalen och väljer en befintlig Programgateway</span><span class="sxs-lookup"><span data-stu-id="50132-121">Navigate toohello Azure portal and select an existing application gateway</span></span>

### <a name="step-2"></a><span data-ttu-id="50132-122">Steg 2</span><span class="sxs-lookup"><span data-stu-id="50132-122">Step 2</span></span>

<span data-ttu-id="50132-123">Lyssnare och klicka på knappen tooadd för hello Lägg till en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="50132-123">Click Listeners and click hello Add button tooadd a listener.</span></span>

![Översikt över App nätverksgateway-bladet][1]

### <a name="step-3"></a><span data-ttu-id="50132-125">Steg 3</span><span class="sxs-lookup"><span data-stu-id="50132-125">Step 3</span></span>

<span data-ttu-id="50132-126">Fyll i hello information som krävs för hello-lyssnare och överför hello .pfx-certifikat, när du är klar klickar du på OK.</span><span class="sxs-lookup"><span data-stu-id="50132-126">Fill out hello required information for hello listener and upload hello .pfx certificate, when complete click OK.</span></span>

<span data-ttu-id="50132-127">**Namnet** -värdet är ett eget namn på hello lyssnare.</span><span class="sxs-lookup"><span data-stu-id="50132-127">**Name** - This value is a friendly name of hello listener.</span></span>

<span data-ttu-id="50132-128">**Frontend-IP-konfiguration** -värdet är hello klientdelens IP-konfiguration som används för hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="50132-128">**Frontend IP configuration** - This value is hello frontend IP configuration that is used for hello listener.</span></span>

<span data-ttu-id="50132-129">**Klientdelsport (namn/Port)** -ett eget namn för hello-port som används på hello klientdel hello Programgateway och hello faktiska porten som används.</span><span class="sxs-lookup"><span data-stu-id="50132-129">**Frontend port (Name/Port)** - A friendly name for hello port used on hello front end of hello application gateway and hello actual port used.</span></span>

<span data-ttu-id="50132-130">**Protokollet** -en växel toodetermine om https eller http används för hello klientdelen.</span><span class="sxs-lookup"><span data-stu-id="50132-130">**Protocol** - A switch toodetermine if https or http is used for hello front end.</span></span>

<span data-ttu-id="50132-131">**Certifikatet (och lösenord)** - avlastning om SSL används ett PFX-certifikat krävs för den här inställningen och ett eget namn och lösenord krävs.</span><span class="sxs-lookup"><span data-stu-id="50132-131">**Certificate (Name/Password)** - If SSL offload is used, a .pfx certificate is required for this setting and a friendly name and password are required.</span></span>

![lyssnare bladet Lägg till][2]

## <a name="create-a-rule-and-associate-it-toohello-listener"></a><span data-ttu-id="50132-133">Skapa en regel och associera den toohello lyssnare</span><span class="sxs-lookup"><span data-stu-id="50132-133">Create a rule and associate it toohello listener</span></span>

<span data-ttu-id="50132-134">hello-lyssnare har skapats.</span><span class="sxs-lookup"><span data-stu-id="50132-134">hello listener has now been created.</span></span> <span data-ttu-id="50132-135">Det är tid toocreate regeln toohandle hello trafik från hello-lyssnaren.</span><span class="sxs-lookup"><span data-stu-id="50132-135">It is time toocreate a rule toohandle hello traffic from hello listener.</span></span> <span data-ttu-id="50132-136">Regler som definierar hur trafik dirigeras toohello serverdelspooler baserat på flera konfigurationsinställningar, inklusive om cookie-baserad session tillhörighet används, protokoll, port och hälsoavsökningar.</span><span class="sxs-lookup"><span data-stu-id="50132-136">Rules define how traffic is routed toohello backend pools based on multiple configuration settings, including whether cookie-based session affinity is used, protocol, port, and health probes.</span></span>

### <a name="step-1"></a><span data-ttu-id="50132-137">Steg 1</span><span class="sxs-lookup"><span data-stu-id="50132-137">Step 1</span></span>

<span data-ttu-id="50132-138">Klicka på hello **regler** för hello Programgateway, och klicka sedan på Lägg till.</span><span class="sxs-lookup"><span data-stu-id="50132-138">Click hello **Rules** of hello application gateway, and then click Add.</span></span>

![appen gateway regler bladet][3]

### <a name="step-2"></a><span data-ttu-id="50132-140">Steg 2</span><span class="sxs-lookup"><span data-stu-id="50132-140">Step 2</span></span>

<span data-ttu-id="50132-141">På hello **Lägg till regel** bladet ange hello eget namn för hello regeln och välj hello lyssnare har skapats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="50132-141">On hello **Add basic rule** blade, type in hello friendly name for hello rule and choose hello listener created in hello previous step.</span></span> <span data-ttu-id="50132-142">Välj hello lämpliga serverdelspool och HTTP-inställningen och klicka på **OK**</span><span class="sxs-lookup"><span data-stu-id="50132-142">Choose hello appropriate backend pool and http setting and click **OK**</span></span>

![fönster för HTTPS-inställningar][4]

<span data-ttu-id="50132-144">hello inställningar sparas nu toohello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="50132-144">hello settings are now saved toohello application gateway.</span></span> <span data-ttu-id="50132-145">hello processen för de här inställningarna kan ta en stund innan de tillgängliga tooview via hello portal eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="50132-145">hello save process for these settings may take a while before they are available tooview through hello portal or through PowerShell.</span></span> <span data-ttu-id="50132-146">När du har sparat hello Programgateway hanterar hello kryptering och dekryptering av trafik.</span><span class="sxs-lookup"><span data-stu-id="50132-146">Once saved hello application gateway handles hello encryption and decryption of traffic.</span></span> <span data-ttu-id="50132-147">All trafik mellan hello Programgateway och hello backend-webbservrar hanteras via http.</span><span class="sxs-lookup"><span data-stu-id="50132-147">All traffic between hello application gateway and hello backend web servers will be handled over http.</span></span> <span data-ttu-id="50132-148">Kommunikation tillbaka toohello klienter om initieras via https returneras toohello klienten krypteras.</span><span class="sxs-lookup"><span data-stu-id="50132-148">Any communication back toohello client if initiated over https will be returned toohello client encrypted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="50132-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="50132-149">Next steps</span></span>

<span data-ttu-id="50132-150">toolearn hur tooconfigure anpassade hälsa avsökning med Azure Application Gateway finns i [skapa en anpassad hälsoavsökningen](application-gateway-create-gateway-portal.md).</span><span class="sxs-lookup"><span data-stu-id="50132-150">toolearn how tooconfigure a custom health probe with Azure Application Gateway, see [Create a custom health probe](application-gateway-create-gateway-portal.md).</span></span>

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png
