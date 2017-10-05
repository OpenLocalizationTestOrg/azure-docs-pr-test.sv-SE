---
title: Azure API Management autentiseringsprinciper | Microsoft Docs
description: "Läs mer om autentiseringsprinciper som är tillgängliga för användning i Azure API Management."
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
ms.openlocfilehash: 63ef20a56ab7721f9ecc7025d05963cc4b0c27a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="api-management-authentication-policies"></a><span data-ttu-id="2f935-103">Principer för autentisering av API-hantering</span><span class="sxs-lookup"><span data-stu-id="2f935-103">API Management authentication policies</span></span>
<span data-ttu-id="2f935-104">Det här avsnittet innehåller en referens för följande API Management-principer.</span><span class="sxs-lookup"><span data-stu-id="2f935-104">This topic provides a reference for the following API Management policies.</span></span> <span data-ttu-id="2f935-105">Mer information om att lägga till och konfigurera principer finns [principer i API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span><span class="sxs-lookup"><span data-stu-id="2f935-105">For information on adding and configuring policies, see [Policies in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).</span></span>  
  
##  <span data-ttu-id="2f935-106"><a name="AuthenticationPolicies"></a>Principer för autentisering</span><span class="sxs-lookup"><span data-stu-id="2f935-106"><a name="AuthenticationPolicies"></a> Authentication policies</span></span>  
  
-   <span data-ttu-id="2f935-107">[Autentisera med grundläggande](api-management-authentication-policies.md#Basic) -autentisering med en serverdelstjänst som använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="2f935-107">[Authenticate with Basic](api-management-authentication-policies.md#Basic) - Authenticate with a backend service using Basic authentication.</span></span>  
  
-   <span data-ttu-id="2f935-108">[Autentisera med klientcertifikatet](api-management-authentication-policies.md#ClientCertificate) -autentisering med en serverdelstjänst som använder klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="2f935-108">[Authenticate with client certificate](api-management-authentication-policies.md#ClientCertificate) - Authenticate with a backend service using client certificates.</span></span>  
  
##  <span data-ttu-id="2f935-109"><a name="Basic"></a>Autentisera med Basic</span><span class="sxs-lookup"><span data-stu-id="2f935-109"><a name="Basic"></a> Authenticate with Basic</span></span>  
 <span data-ttu-id="2f935-110">Använd den `authentication-basic` princip för att autentisera med en serverdelstjänst som använder grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="2f935-110">Use the `authentication-basic` policy to authenticate with a backend service using Basic authentication.</span></span> <span data-ttu-id="2f935-111">Den här principen anger effektivt HTTP Authorization-huvud till värdet som motsvarar de autentiseringsuppgifter som anges i principen.</span><span class="sxs-lookup"><span data-stu-id="2f935-111">This policy effectively sets the HTTP Authorization header to the value corresponding to the credentials provided in the policy.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2f935-112">Principframställning</span><span class="sxs-lookup"><span data-stu-id="2f935-112">Policy statement</span></span>  
  
```xml  
<authentication-basic username="username" password="password" />  
```  
  
### <a name="example"></a><span data-ttu-id="2f935-113">Exempel</span><span class="sxs-lookup"><span data-stu-id="2f935-113">Example</span></span>  
  
```xml  
<authentication-basic username="testuser" password="testpassword" />  
```  
  
### <a name="elements"></a><span data-ttu-id="2f935-114">Element</span><span class="sxs-lookup"><span data-stu-id="2f935-114">Elements</span></span>  
  
|<span data-ttu-id="2f935-115">Namn</span><span class="sxs-lookup"><span data-stu-id="2f935-115">Name</span></span>|<span data-ttu-id="2f935-116">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f935-116">Description</span></span>|<span data-ttu-id="2f935-117">Krävs</span><span class="sxs-lookup"><span data-stu-id="2f935-117">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2f935-118">grundläggande autentisering</span><span class="sxs-lookup"><span data-stu-id="2f935-118">authentication-basic</span></span>|<span data-ttu-id="2f935-119">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="2f935-119">Root element.</span></span>|<span data-ttu-id="2f935-120">Ja</span><span class="sxs-lookup"><span data-stu-id="2f935-120">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="2f935-121">Attribut</span><span class="sxs-lookup"><span data-stu-id="2f935-121">Attributes</span></span>  
  
|<span data-ttu-id="2f935-122">Namn</span><span class="sxs-lookup"><span data-stu-id="2f935-122">Name</span></span>|<span data-ttu-id="2f935-123">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f935-123">Description</span></span>|<span data-ttu-id="2f935-124">Krävs</span><span class="sxs-lookup"><span data-stu-id="2f935-124">Required</span></span>|<span data-ttu-id="2f935-125">Standard</span><span class="sxs-lookup"><span data-stu-id="2f935-125">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2f935-126">användarnamn</span><span class="sxs-lookup"><span data-stu-id="2f935-126">username</span></span>|<span data-ttu-id="2f935-127">Anger användarnamnet för grundläggande autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="2f935-127">Specifies the username of the Basic credential.</span></span>|<span data-ttu-id="2f935-128">Ja</span><span class="sxs-lookup"><span data-stu-id="2f935-128">Yes</span></span>|<span data-ttu-id="2f935-129">Saknas</span><span class="sxs-lookup"><span data-stu-id="2f935-129">N/A</span></span>|  
|<span data-ttu-id="2f935-130">lösenord</span><span class="sxs-lookup"><span data-stu-id="2f935-130">password</span></span>|<span data-ttu-id="2f935-131">Anger lösenordet för autentiseringsuppgifterna för grundläggande.</span><span class="sxs-lookup"><span data-stu-id="2f935-131">Specifies the password of the Basic credential.</span></span>|<span data-ttu-id="2f935-132">Ja</span><span class="sxs-lookup"><span data-stu-id="2f935-132">Yes</span></span>|<span data-ttu-id="2f935-133">Saknas</span><span class="sxs-lookup"><span data-stu-id="2f935-133">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2f935-134">Användning</span><span class="sxs-lookup"><span data-stu-id="2f935-134">Usage</span></span>  
 <span data-ttu-id="2f935-135">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2f935-135">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2f935-136">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="2f935-136">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="2f935-137">**Princip för scope:** API</span><span class="sxs-lookup"><span data-stu-id="2f935-137">**Policy scopes:** API</span></span>  
  
##  <span data-ttu-id="2f935-138"><a name="ClientCertificate"></a>Autentisera med klientcertifikatet</span><span class="sxs-lookup"><span data-stu-id="2f935-138"><a name="ClientCertificate"></a> Authenticate with client certificate</span></span>  
 <span data-ttu-id="2f935-139">Använd den `authentication-certificate` princip för att autentisera med en serverdelstjänst som använder klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="2f935-139">Use the `authentication-certificate` policy to authenticate with a backend service using client certificate.</span></span> <span data-ttu-id="2f935-140">Certifikatet måste vara [installerats i API Management](http://go.microsoft.com/fwlink/?LinkID=511599) första och identifieras av dess tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="2f935-140">The certificate needs to be [installed into API Management](http://go.microsoft.com/fwlink/?LinkID=511599) first and is identified by its thumbprint.</span></span>  
  
### <a name="policy-statement"></a><span data-ttu-id="2f935-141">Principframställning</span><span class="sxs-lookup"><span data-stu-id="2f935-141">Policy statement</span></span>  
  
```xml  
<authentication-certificate thumbprint="thumbprint" />  
```  
  
### <a name="example"></a><span data-ttu-id="2f935-142">Exempel</span><span class="sxs-lookup"><span data-stu-id="2f935-142">Example</span></span>  
  
```xml  
<authentication-certificate thumbprint="....." />  
```  
  
### <a name="elements"></a><span data-ttu-id="2f935-143">Element</span><span class="sxs-lookup"><span data-stu-id="2f935-143">Elements</span></span>  
  
|<span data-ttu-id="2f935-144">Namn</span><span class="sxs-lookup"><span data-stu-id="2f935-144">Name</span></span>|<span data-ttu-id="2f935-145">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f935-145">Description</span></span>|<span data-ttu-id="2f935-146">Krävs</span><span class="sxs-lookup"><span data-stu-id="2f935-146">Required</span></span>|  
|----------|-----------------|--------------|  
|<span data-ttu-id="2f935-147">certifikat för serverautentisering</span><span class="sxs-lookup"><span data-stu-id="2f935-147">authentication-certificate</span></span>|<span data-ttu-id="2f935-148">Rotelementet.</span><span class="sxs-lookup"><span data-stu-id="2f935-148">Root element.</span></span>|<span data-ttu-id="2f935-149">Ja</span><span class="sxs-lookup"><span data-stu-id="2f935-149">Yes</span></span>|  
  
### <a name="attributes"></a><span data-ttu-id="2f935-150">Attribut</span><span class="sxs-lookup"><span data-stu-id="2f935-150">Attributes</span></span>  
  
|<span data-ttu-id="2f935-151">Namn</span><span class="sxs-lookup"><span data-stu-id="2f935-151">Name</span></span>|<span data-ttu-id="2f935-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="2f935-152">Description</span></span>|<span data-ttu-id="2f935-153">Krävs</span><span class="sxs-lookup"><span data-stu-id="2f935-153">Required</span></span>|<span data-ttu-id="2f935-154">Standard</span><span class="sxs-lookup"><span data-stu-id="2f935-154">Default</span></span>|  
|----------|-----------------|--------------|-------------|  
|<span data-ttu-id="2f935-155">tumavtrycket</span><span class="sxs-lookup"><span data-stu-id="2f935-155">thumbprint</span></span>|<span data-ttu-id="2f935-156">Tumavtryck för klientcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="2f935-156">The thumbprint for the client certificate.</span></span>|<span data-ttu-id="2f935-157">Ja</span><span class="sxs-lookup"><span data-stu-id="2f935-157">Yes</span></span>|<span data-ttu-id="2f935-158">Saknas</span><span class="sxs-lookup"><span data-stu-id="2f935-158">N/A</span></span>|  
  
### <a name="usage"></a><span data-ttu-id="2f935-159">Användning</span><span class="sxs-lookup"><span data-stu-id="2f935-159">Usage</span></span>  
 <span data-ttu-id="2f935-160">Den här principen kan användas i följande princip [avsnitt](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) och [scope](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span><span class="sxs-lookup"><span data-stu-id="2f935-160">This policy can be used in the following policy [sections](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) and [scopes](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).</span></span>  
  
-   <span data-ttu-id="2f935-161">**Avsnitt i princip:** inkommande</span><span class="sxs-lookup"><span data-stu-id="2f935-161">**Policy sections:** inbound</span></span>  
  
-   <span data-ttu-id="2f935-162">**Princip för scope:** API</span><span class="sxs-lookup"><span data-stu-id="2f935-162">**Policy scopes:** API</span></span>  
  

## <a name="next-steps"></a><span data-ttu-id="2f935-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f935-163">Next steps</span></span>
<span data-ttu-id="2f935-164">Arbeta med principer för mer information finns i [principer i API Management](api-management-howto-policies.md).</span><span class="sxs-lookup"><span data-stu-id="2f935-164">For more information working with policies, see [Policies in API Management](api-management-howto-policies.md).</span></span>  