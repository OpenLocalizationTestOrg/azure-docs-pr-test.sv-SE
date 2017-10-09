---
title: "aaaEnabling end tooend SSL på Azure Programgateway | Microsoft Docs"
description: "Den här sidan innehåller en översikt över hello Programgateway slutet tooend stöd för SSL."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: 3976399b-25ad-45eb-8eb3-fdb736a598c5
ms.service: application-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: amsriva
ms.openlocfilehash: c5cb398a1e7d9a08662a3120baad98edb5575917
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-end-tooend-ssl-with-application-gateway"></a><span data-ttu-id="b00ee-103">Översikt över slutet tooend SSL med Programgateway</span><span class="sxs-lookup"><span data-stu-id="b00ee-103">Overview of end tooend SSL with Application Gateway</span></span>

<span data-ttu-id="b00ee-104">Programgateway stöder SSL-avslutning vid hello gateway, efter vilken trafiken flödar vanligtvis okrypterade toohello backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="b00ee-104">Application gateway supports SSL termination at hello gateway, after which traffic typically flows unencrypted toohello backend servers.</span></span> <span data-ttu-id="b00ee-105">Den här funktionen tillåter web servrar toobe unburdened från kostsamma kryptering och dekryptering kostnader.</span><span class="sxs-lookup"><span data-stu-id="b00ee-105">This feature allows web servers toobe unburdened from costly encryption and decryption overhead.</span></span> <span data-ttu-id="b00ee-106">Men för vissa kunder okrypterad kommunikation toohello backend-servrarna är inte en godkänd alternativ.</span><span class="sxs-lookup"><span data-stu-id="b00ee-106">However for some customers unencrypted communication toohello backend servers is not an acceptable option.</span></span> <span data-ttu-id="b00ee-107">Den här okrypterade kommunikationen kan bero på att toosecurity krav, krav på efterlevnad eller hello program kan endast ha en säker anslutning.</span><span class="sxs-lookup"><span data-stu-id="b00ee-107">This unencrypted communication could be due toosecurity requirements, compliance requirements, or hello application may only accept a secure connection.</span></span> <span data-ttu-id="b00ee-108">För sådana program stöder Programgateway slutet tooend SSL kryptering.</span><span class="sxs-lookup"><span data-stu-id="b00ee-108">For such applications, application gateway supports end tooend SSL encryption.</span></span>

## <a name="overview"></a><span data-ttu-id="b00ee-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="b00ee-109">Overview</span></span>

<span data-ttu-id="b00ee-110">End tooend SSL kan du toosecurely överföra känsliga data toohello backend krypterade medan fortfarande dra nytta av hello fördelarna med funktioner för belastningsutjämning Layer 7 vilka Programgateway innehåller.</span><span class="sxs-lookup"><span data-stu-id="b00ee-110">End tooend SSL allows you toosecurely transmit sensitive data toohello backend encrypted while still taking advantage of hello benefits of Layer 7 load balancing features which application gateway provides.</span></span> <span data-ttu-id="b00ee-111">Vissa av dessa funktioner är cookie-baserad session tillhörighet, URL-baserade routning, stöd för routning baserat på webbplatser eller möjlighet tooinject X - vidarebefordrad-* huvuden.</span><span class="sxs-lookup"><span data-stu-id="b00ee-111">Some of these features are cookie-based session affinity, URL-based routing, support for routing based on sites, or ability tooinject X-Forwarded-* headers.</span></span>

<span data-ttu-id="b00ee-112">När konfigurerad med end tooend SSL-kommunikation läge, avslutas hello SSL-sessioner på hello gateway Programgateway och dekrypterar användaraktiviteten.</span><span class="sxs-lookup"><span data-stu-id="b00ee-112">When configured with end tooend SSL communication mode, application gateway terminates hello SSL sessions at hello gateway and decrypts user traffic.</span></span> <span data-ttu-id="b00ee-113">Den gäller sedan hello konfigurerade regler tooselect en lämplig backend poolen instans tooroute trafik till.</span><span class="sxs-lookup"><span data-stu-id="b00ee-113">It then applies hello configured rules tooselect an appropriate backend pool instance tooroute traffic to.</span></span> <span data-ttu-id="b00ee-114">Programgateway sedan initierar en ny SSL-anslutning toohello backend-server och krypterar data med hjälp av hello backend-serverns offentliga nyckelcertifikat innan de överförs hello begäran toohello backend.</span><span class="sxs-lookup"><span data-stu-id="b00ee-114">Application gateway then initiates a new SSL connection toohello backend server and re-encrypts data using hello backend server's public key certificate before transmitting hello request toohello backend.</span></span> <span data-ttu-id="b00ee-115">End tooend SSL är aktiverat genom att ange protokollinställningen i BackendHTTPSetting tooHTTPS, som sedan tillämpas tooa serverdelspool.</span><span class="sxs-lookup"><span data-stu-id="b00ee-115">End tooend SSL is enabled by setting protocol setting in BackendHTTPSetting tooHTTPS, which is then applied tooa backend pool.</span></span> <span data-ttu-id="b00ee-116">Varje backend-servern i hello serverdelspool med end tooend SSL aktiverat måste konfigureras med en certifikat tooallow säker kommunikation.</span><span class="sxs-lookup"><span data-stu-id="b00ee-116">Each backend server in hello backend pool with end tooend SSL enabled must be configured with a certificate tooallow secure communication.</span></span>

![tooend ssl scenariot][1]

<span data-ttu-id="b00ee-118">I det här exemplet är begäranden som använder TLS1.2 routade toobackend servrar i Pool1 med end tooend SSL.</span><span class="sxs-lookup"><span data-stu-id="b00ee-118">In this example, requests using TLS1.2 are routed toobackend servers in Pool1 using end tooend SSL.</span></span>

## <a name="end-tooend-ssl-and-whitelisting-of-certificates"></a><span data-ttu-id="b00ee-119">Avsluta tooend SSL och vitlistning av certifikat</span><span class="sxs-lookup"><span data-stu-id="b00ee-119">End tooend SSL and whitelisting of certificates</span></span>

<span data-ttu-id="b00ee-120">Programgateway kommunicerar endast med kända backend-instanser som har godkända sina certifikat med hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="b00ee-120">Application gateway only communicates with known backend instances that have whitelisted their certificate with hello application gateway.</span></span> <span data-ttu-id="b00ee-121">tooenable vitlistning av certifikat, måste du överföra hello offentliga nyckeln för backend-servern certifikat toohello Programgateway (inte hello rotcertifikat).</span><span class="sxs-lookup"><span data-stu-id="b00ee-121">tooenable whitelisting of certificates, you must upload hello public key of backend server certificates toohello application gateway (not hello root certificate).</span></span> <span data-ttu-id="b00ee-122">Endast anslutningar tooknown och listan över godkända serverdelar tillåts sedan.</span><span class="sxs-lookup"><span data-stu-id="b00ee-122">Only connections tooknown and whitelisted backends are then allowed.</span></span> <span data-ttu-id="b00ee-123">hello återstående serverdelar resulterar i en gateway-fel.</span><span class="sxs-lookup"><span data-stu-id="b00ee-123">hello remaining backends results in a gateway error.</span></span> <span data-ttu-id="b00ee-124">Självsignerade certifikat är enbart för testningsändamål och rekommenderas inte för produktions-arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="b00ee-124">Self-signed certificates are for test purposes only and not recommended for production workloads.</span></span> <span data-ttu-id="b00ee-125">Dessa certifikat har toobe godkända med hello Programgateway enligt beskrivningen i föregående steg innan de kan användas hello.</span><span class="sxs-lookup"><span data-stu-id="b00ee-125">Such certificates have toobe whitelisted with hello application gateway as described in hello preceding steps before they can be used.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b00ee-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b00ee-126">Next steps</span></span>

<span data-ttu-id="b00ee-127">När du lära dig mer om slutet tooend SSL, gå för[aktivera slutet tooend SSL på Programgateway](application-gateway-end-to-end-ssl-powershell.md) toocreate som en gateway som använder avslutas tooend SSL.</span><span class="sxs-lookup"><span data-stu-id="b00ee-127">After learning about end tooend SSL, go too[enable end tooend SSL on application gateway](application-gateway-end-to-end-ssl-powershell.md) toocreate an application gateway using end tooend SSL.</span></span>

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png
