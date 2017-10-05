---
title: "Så här konfigurerar du ett program med Application Proxy | Microsoft Docs"
description: "Lär dig hur du skapar en konfigurera ett program med APplication Proxy i några enkla steg"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c8f98536048a85ebb3f061d840457130579196d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-an-application-proxy-application"></a><span data-ttu-id="d6025-103">Så här konfigurerar du ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="d6025-103">How to configure an Application Proxy application</span></span>

<span data-ttu-id="d6025-104">Den här artikeln hjälper dig att förstå hur du konfigurerar ett program med Application Proxy i Azure AD att exponera dina lokala program till molnet.</span><span class="sxs-lookup"><span data-stu-id="d6025-104">This article help you to understand how to configure an Application Proxy application within Azure AD to expose your on-premises applications to the cloud.</span></span>

## <a name="recommended-documents"></a><span data-ttu-id="d6025-105">Rekommenderade dokument</span><span class="sxs-lookup"><span data-stu-id="d6025-105">Recommended documents</span></span> 

<span data-ttu-id="d6025-106">Mer information om inledande konfiguration och skapande av ett program för Application Proxy via Admin Portal, följer du de [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="d6025-106">To learn about the initial configurations and creation of an Application Proxy application through the Admin Portal, follow the [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).</span></span>

<span data-ttu-id="d6025-107">Mer information om hur du konfigurerar kopplingar finns [aktivera Application Proxy på Azure-portalen](active-directory-application-proxy-enable.md).</span><span class="sxs-lookup"><span data-stu-id="d6025-107">For details on configuring Connectors, see [Enable Application Proxy in the Azure portal](active-directory-application-proxy-enable.md).</span></span>

<span data-ttu-id="d6025-108">Information om överföring av certifikat och använder anpassade domäner finns i [arbeta med anpassade domäner i Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="d6025-108">For information on uploading certificates and using custom domains, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span>

## <a name="create-the-applicationsetting-the-urls"></a><span data-ttu-id="d6025-109">Skapa Programinställningen URL: er</span><span class="sxs-lookup"><span data-stu-id="d6025-109">Create the Application/Setting the URLs</span></span>

<span data-ttu-id="d6025-110">Om du följer stegen i den [publicera program med Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) dokumentation och kan få ett fel när programmet skulle se felinformationen information och förslag om hur du löser programmet.</span><span class="sxs-lookup"><span data-stu-id="d6025-110">If you are following the steps in the [Publish applications using Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) documentation and are getting an error creating the application, see the error details for information and suggestions for how to fix the application.</span></span> <span data-ttu-id="d6025-111">De vanligaste felmeddelanden som innehåller en föreslagen åtgärd.</span><span class="sxs-lookup"><span data-stu-id="d6025-111">Most error messages include a suggested fix.</span></span> <span data-ttu-id="d6025-112">Kontrollera följande för att undvika vanliga fel:</span><span class="sxs-lookup"><span data-stu-id="d6025-112">To avoid common errors, verify:</span></span>

-   <span data-ttu-id="d6025-113">Du är administratör med behörighet att skapa ett program med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="d6025-113">You are an administrator with permission to create an Application Proxy application</span></span>

-   <span data-ttu-id="d6025-114">Intern URL är unikt</span><span class="sxs-lookup"><span data-stu-id="d6025-114">The internal URL is unique</span></span>

-   <span data-ttu-id="d6025-115">Den externa URL: en är unikt</span><span class="sxs-lookup"><span data-stu-id="d6025-115">The external URL is unique</span></span>

-   <span data-ttu-id="d6025-116">URL: er börja med http eller https och sluta med en ”/”</span><span class="sxs-lookup"><span data-stu-id="d6025-116">The URLs start with http or https, and end with a “/”</span></span>

-   <span data-ttu-id="d6025-117">URL-Adressen ska vara ett domännamn, inte en IP-adress</span><span class="sxs-lookup"><span data-stu-id="d6025-117">The URL should be a domain name, not an IP address</span></span>

<span data-ttu-id="d6025-118">Felmeddelandet ska visas i det övre högra hörnet när du skapar i program.</span><span class="sxs-lookup"><span data-stu-id="d6025-118">The error message should display in the top right corner when you create the application.</span></span> <span data-ttu-id="d6025-119">Du kan också välja meddelandeikonen att se felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="d6025-119">You can also select the notification icon to see the error messages.</span></span>

   ![Meddelande-fråga](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a><span data-ttu-id="d6025-121">Konfigurera grupper kopplingar-koppling</span><span class="sxs-lookup"><span data-stu-id="d6025-121">Configure connectors/connector groups</span></span>

<span data-ttu-id="d6025-122">Om du har problem med hur du konfigurerar programmet på grund av varning om kopplingar och connector grupper finns instruktioner om hur du aktiverar programproxy för information om hur du hämtar kopplingar.</span><span class="sxs-lookup"><span data-stu-id="d6025-122">If you are having difficulty configuring your application because of warning about the connectors and connector groups, see instructions on enabling Application Proxy for details on how to download connectors.</span></span> <span data-ttu-id="d6025-123">Om du vill veta mer om kopplingar finns i [kopplingar dokumentationen](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span><span class="sxs-lookup"><span data-stu-id="d6025-123">If you want to learn more about connectors, see the [connectors documentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).</span></span>

<span data-ttu-id="d6025-124">Om din kopplingar är inaktiva, betyder det att de inte går att nå tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d6025-124">If your connectors are inactive, this means that they are unable to reach the service.</span></span> <span data-ttu-id="d6025-125">Detta är ofta eftersom portarna som krävs inte är öppna.</span><span class="sxs-lookup"><span data-stu-id="d6025-125">This is often because all the required ports are not open.</span></span> <span data-ttu-id="d6025-126">Om du vill se en lista över portar som krävs finns i avsnittet förutsättningar för att aktivera Application Proxy-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="d6025-126">To see a list of required ports, see the pre-requisites section of the enabling Application Proxy documentation.</span></span>

## <a name="upload-certificates-for-custom-domains"></a><span data-ttu-id="d6025-127">Överför certifikat för anpassade domäner</span><span class="sxs-lookup"><span data-stu-id="d6025-127">Upload certificates for custom domains</span></span>

<span data-ttu-id="d6025-128">Anpassade domäner kan du ange domänen för de externa URL: er.</span><span class="sxs-lookup"><span data-stu-id="d6025-128">Custom Domains allow you to specify the domain of your external URLs.</span></span> <span data-ttu-id="d6025-129">Om du vill använda anpassade domäner, måste du ladda upp certifikatet för domänen.</span><span class="sxs-lookup"><span data-stu-id="d6025-129">To use custom domains, you need to upload the certificate for that domain.</span></span> <span data-ttu-id="d6025-130">Information om hur du använder anpassade domäner och certifikat finns i [arbeta med anpassade domäner i Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span><span class="sxs-lookup"><span data-stu-id="d6025-130">For information on using custom domains and certificates, see [Working with custom domains in Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).</span></span> 

<span data-ttu-id="d6025-131">Om du upptäcker problem överföra ditt certifikat, leta efter felmeddelanden för ytterligare information om problem med certifikatet i portalen.</span><span class="sxs-lookup"><span data-stu-id="d6025-131">If you are encountering issues uploading your certificate, look for the error messages in the portal for additional information on the problem with the certificate.</span></span> <span data-ttu-id="d6025-132">Vanliga problem med säkerhetscertifikat inkluderar:</span><span class="sxs-lookup"><span data-stu-id="d6025-132">Common certificate problems include:</span></span>

-   <span data-ttu-id="d6025-133">Certifikatet har upphört att gälla</span><span class="sxs-lookup"><span data-stu-id="d6025-133">Expired certificate</span></span>

-   <span data-ttu-id="d6025-134">Certifikatet är självsignerat</span><span class="sxs-lookup"><span data-stu-id="d6025-134">Certificate is self-signed</span></span>

-   <span data-ttu-id="d6025-135">Certifikatet saknar privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d6025-135">Certificate is missing the private key</span></span>

<span data-ttu-id="d6025-136">Ett felmeddelande visas i det övre högra hörnet som du försöker ladda upp certifikatet.</span><span class="sxs-lookup"><span data-stu-id="d6025-136">The error message display in the top right corner as you try to upload the certificate.</span></span> <span data-ttu-id="d6025-137">Du kan också välja meddelandeikonen att se felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="d6025-137">You can also select the notification icon to see the error messages.</span></span>

   ![Meddelande-fråga](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a><span data-ttu-id="d6025-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6025-139">Next steps</span></span>
[<span data-ttu-id="d6025-140">Publicera program med Azure AD Application Proxy</span><span class="sxs-lookup"><span data-stu-id="d6025-140">Publish applications using Azure AD Application Proxy</span></span>](application-proxy-publish-azure-portal.md)
