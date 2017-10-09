---
title: aaaAzure API Management autentiseringsprinciper | Microsoft Docs
description: "Läs mer om hello autentiseringsprinciper kan användas i Azure API Management."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 061702a7-3a78-472b-a54a-f3b1e332490d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: ce93cced66cb67520e97c7c15f3685bffb08e1f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="api-management-authentication-policies"></a><span data-ttu-id="a8e8c-103">Principer för autentisering av API-hantering</span><span class="sxs-lookup"><span data-stu-id="a8e8c-103">API Management authentication policies</span></span>
<span data-ttu-id="a8e8c-104">Det här avsnittet innehåller en referens för hello följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-104">This topic provides a reference for hello following API Management policies.</span></span> <span data-ttu-id="a8e8c-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="a8e8c-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="a8e8c-106"><a name="AuthenticationPolicies"></a>Principer för autentisering</span><span class="sxs-lookup"><span data-stu-id="a8e8c-106"><a name="AuthenticationPolicies"></a> Authentication policies</span></span>  
  
-   <span data-ttu-id="a8e8c-107">[Autentisera med grundläggande](api-management-authentication-policies.md#Basic) -autentisering med en serverdelstjänst som använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-107">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
-   <span data-ttu-id="a8e8c-108">[Autentisera med klientcertifikatet](api-management-authentication-policies.md#ClientCertificate) -autentisering med en serverdelstjänst som använder klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-108">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
##  <span data-ttu-id="a8e8c-109"><a name="Basic"></a>Autentisera med Basic</span><span class="sxs-lookup"><span data-stu-id="a8e8c-109"><a name="Basic"></a> Authenticate with Basic</span></span>  
 <span data-ttu-id="a8e8c-110">Använd hello `authentication-basic` princip tooauthenticate med en serverdelstjänst som använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-110">Use hello `authentication-basic` policy tooauthenticate with a backend service using Basic authentication.</span></span> <span data-ttu-id="a8e8c-111">Den här principen anger effektivt hello auktorisering för HTTP-huvudet toohello värde motsvarande toohello autentiseringsuppgifterna i hello princip.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-111">This policy effectively sets hello HTTP Authorization header toohello value corresponding toohello credentials provided in hello policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="a8e8c-112">Principframställning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-112">Policy statement</span></span>  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a><span data-ttu-id="a8e8c-113">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8e8c-113">Example</span></span>  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a><span data-ttu-id="a8e8c-114">Element</span><span class="sxs-lookup"><span data-stu-id="a8e8c-114">Elements</span></span>  
  
|<span data-ttu-id="a8e8c-115">Namn</span><span class="sxs-lookup"><span data-stu-id="a8e8c-115">Name</span></span>|<span data-ttu-id="a8e8c-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-116">Description</span></span>|<span data-ttu-id="a8e8c-117">Krävs</span><span class="sxs-lookup"><span data-stu-id="a8e8c-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="a8e8c-118">grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="a8e8c-118">authentication-basic</span></span>|<span data-ttu-id="a8e8c-119">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-119">Root element.</span></span>|<span data-ttu-id="a8e8c-120">Ja</span><span class="sxs-lookup"><span data-stu-id="a8e8c-120">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="a8e8c-121">Attribut</span><span class="sxs-lookup"><span data-stu-id="a8e8c-121">Attributes</span></span>  
  
|<span data-ttu-id="a8e8c-122">Namn</span><span class="sxs-lookup"><span data-stu-id="a8e8c-122">Name</span></span>|<span data-ttu-id="a8e8c-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-123">Description</span></span>|<span data-ttu-id="a8e8c-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="a8e8c-124">Required</span></span>|<span data-ttu-id="a8e8c-125">Standard</span><span class="sxs-lookup"><span data-stu-id="a8e8c-125">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="a8e8c-126">användarnamn</span><span class="sxs-lookup"><span data-stu-id="a8e8c-126">username</span></span>|<span data-ttu-id="a8e8c-127">Anger hello användarnamnet för grundläggande hello-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-127">Specifies hello username of hello Basic credential.</span></span>|<span data-ttu-id="a8e8c-128">Ja</span><span class="sxs-lookup"><span data-stu-id="a8e8c-128">Yes</span></span>|<span data-ttu-id="a8e8c-129">Saknas</span><span class="sxs-lookup"><span data-stu-id="a8e8c-129">N/A</span></span>|  
|<span data-ttu-id="a8e8c-130">lösenord</span><span class="sxs-lookup"><span data-stu-id="a8e8c-130">password</span></span>|<span data-ttu-id="a8e8c-131">Anger hello lösenord grundläggande hello-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-131">Specifies hello password of hello Basic credential.</span></span>|<span data-ttu-id="a8e8c-132">Ja</span><span class="sxs-lookup"><span data-stu-id="a8e8c-132">Yes</span></span>|<span data-ttu-id="a8e8c-133">Saknas</span><span class="sxs-lookup"><span data-stu-id="a8e8c-133">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="a8e8c-134">Användning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-134">Usage</span></span>  
 <span data-ttu-id="a8e8c-135">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="a8e8c-135">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="a8e8c-136">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="a8e8c-136">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="a8e8c-137">**Princip för scope:** API</span><span class="sxs-lookup"><span data-stu-id="a8e8c-137">**Policy scopes:** API</span></span>  
  
##  <span data-ttu-id="a8e8c-138"><a name="ClientCertificate"></a>Autentisera med klientcertifikatet</span><span class="sxs-lookup"><span data-stu-id="a8e8c-138"><a name="ClientCertificate"></a> Authenticate with client certificate</span></span>  
 <span data-ttu-id="a8e8c-139">Använd hello `authentication-certificate` princip tooauthenticate med en serverdelstjänst som använder klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-139">Use hello `authentication-certificate` policy tooauthenticate with a backend service using client certificate.</span></span> <span data-ttu-id="a8e8c-140">hello certifikat måste toobe [installerats i API Management](http://go.microsoft.com/fwlink/?LinkID=511599) första och identifieras av dess tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-140">hello certificate needs toobe [installed into API Management](http://go.microsoft.com/fwlink/?LinkID=511599) first and is identified by its thumbprint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="a8e8c-141">Principframställning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-141">Policy statement</span></span>  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a><span data-ttu-id="a8e8c-142">Exempel</span><span class="sxs-lookup"><span data-stu-id="a8e8c-142">Example</span></span>  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a><span data-ttu-id="a8e8c-143">Element</span><span class="sxs-lookup"><span data-stu-id="a8e8c-143">Elements</span></span>  
  
|<span data-ttu-id="a8e8c-144">Namn</span><span class="sxs-lookup"><span data-stu-id="a8e8c-144">Name</span></span>|<span data-ttu-id="a8e8c-145">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-145">Description</span></span>|<span data-ttu-id="a8e8c-146">Krävs</span><span class="sxs-lookup"><span data-stu-id="a8e8c-146">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="a8e8c-147">certifikat för serverautentisering</span><span class="sxs-lookup"><span data-stu-id="a8e8c-147">authentication-certificate</span></span>|<span data-ttu-id="a8e8c-148">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-148">Root element.</span></span>|<span data-ttu-id="a8e8c-149">Ja</span><span class="sxs-lookup"><span data-stu-id="a8e8c-149">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="a8e8c-150">Attribut</span><span class="sxs-lookup"><span data-stu-id="a8e8c-150">Attributes</span></span>  
  
|<span data-ttu-id="a8e8c-151">Namn</span><span class="sxs-lookup"><span data-stu-id="a8e8c-151">Name</span></span>|<span data-ttu-id="a8e8c-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-152">Description</span></span>|<span data-ttu-id="a8e8c-153">Krävs</span><span class="sxs-lookup"><span data-stu-id="a8e8c-153">Required</span></span>|<span data-ttu-id="a8e8c-154">Standard</span><span class="sxs-lookup"><span data-stu-id="a8e8c-154">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="a8e8c-155">tumavtrycket</span><span class="sxs-lookup"><span data-stu-id="a8e8c-155">thumbprint</span></span>|<span data-ttu-id="a8e8c-156">hello tumavtryck för hello klientcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="a8e8c-156">hello thumbprint for hello client certificate.</span></span>|<span data-ttu-id="a8e8c-157">Ja</span><span class="sxs-lookup"><span data-stu-id="a8e8c-157">Yes</span></span>|<span data-ttu-id="a8e8c-158">Saknas</span><span class="sxs-lookup"><span data-stu-id="a8e8c-158">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="a8e8c-159">Användning</span><span class="sxs-lookup"><span data-stu-id="a8e8c-159">Usage</span></span>  
 <span data-ttu-id="a8e8c-160">Den här principen kan användas i hello följa principen [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="a8e8c-160">This policy can be used in hello following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="a8e8c-161">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="a8e8c-161">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="a8e8c-162">**Princip för scope:** API</span><span class="sxs-lookup"><span data-stu-id="a8e8c-162">**Policy scopes:** API</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="a8e8c-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a8e8c-163">Next steps</span></span>
<span data-ttu-id="a8e8c-164">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="a8e8c-164">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  