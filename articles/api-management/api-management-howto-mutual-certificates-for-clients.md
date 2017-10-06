---
title: "aaaSecure API: er med hjälp av förautentisering av klientcertifikat i API Management - Azure API Management | Microsoft Docs"
description: "Lär dig hur toosecure åt tooAPIs som använder klientcertifikat"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: apimpm
ms.openlocfilehash: 6ff78bda3d429829da79d0dc4d652f19669cc919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-apis-using-client-certificate-authentication-in-api-management"></a><span data-ttu-id="aae06-103">Hur toosecure API: er med klienten certifikatautentisering i API Management</span><span class="sxs-lookup"><span data-stu-id="aae06-103">How toosecure APIs using client certificate authentication in API Management</span></span>

<span data-ttu-id="aae06-104">API Management ger hello kapaciteten toosecure åtkomst tooAPIs (d.v.s. klienten tooAPI Management) med hjälp av klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="aae06-104">API Management provides hello capability toosecure access tooAPIs (i.e., client tooAPI Management) using client certificates.</span></span> <span data-ttu-id="aae06-105">För närvarande kan du kontrollera hello tumavtrycket för ett klientcertifikat mot ett önskat värde.</span><span class="sxs-lookup"><span data-stu-id="aae06-105">Currently, you can check hello thumbprint of a client certificate against a desired value.</span></span> <span data-ttu-id="aae06-106">Du kan också kontrollera hello tumavtrycket mot befintliga certifikat har överförts tooAPI Management.</span><span class="sxs-lookup"><span data-stu-id="aae06-106">You can also check hello thumbprint against existing certificates uploaded tooAPI Management.</span></span>  

<span data-ttu-id="aae06-107">Information om hur du skyddar åtkomst toohello backend-tjänst för en API som använder klientcertifikat (d.v.s. API Management tooback-end) finns [hur toosecure backend-tjänster som använder klienten certifikatautentisering](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span><span class="sxs-lookup"><span data-stu-id="aae06-107">For information about securing access toohello back-end service of an API using client certificates (i.e., API Management tooback-end), see [How toosecure back-end services using client certificate authentication](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span></span>

## <a name="checking-hello-expiration-date"></a><span data-ttu-id="aae06-108">Kontrollera hello upphör att gälla</span><span class="sxs-lookup"><span data-stu-id="aae06-108">Checking hello expiration date</span></span>

<span data-ttu-id="aae06-109">Nedan principer kan vara konfigurerade toocheck om hello certifikatet har upphört att gälla:</span><span class="sxs-lookup"><span data-stu-id="aae06-109">Below policies can be configured toocheck if hello certificate is expired:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-issuer-and-subject"></a><span data-ttu-id="aae06-110">Kontrollerar hello utfärdare och ämne</span><span class="sxs-lookup"><span data-stu-id="aae06-110">Checking hello issuer and subject</span></span>

<span data-ttu-id="aae06-111">Nedan principer kan vara konfigurerade toocheck hello utfärdare och ämnet för ett klientcertifikat:</span><span class="sxs-lookup"><span data-stu-id="aae06-111">Below policies can be configured toocheck hello issuer and subject of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-thumbprint"></a><span data-ttu-id="aae06-112">Kontrollera hello tumavtrycket</span><span class="sxs-lookup"><span data-stu-id="aae06-112">Checking hello thumbprint</span></span>

<span data-ttu-id="aae06-113">Nedan principer kan vara konfigurerade toocheck hello tumavtrycket för ett klientcertifikat:</span><span class="sxs-lookup"><span data-stu-id="aae06-113">Below policies can be configured toocheck hello thumbprint of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-tooapi-management"></a><span data-ttu-id="aae06-114">Kontrollerar ett tumavtryck mot certifikat har överförts tooAPI Management</span><span class="sxs-lookup"><span data-stu-id="aae06-114">Checking a thumbprint against certificates uploaded tooAPI Management</span></span>

<span data-ttu-id="aae06-115">hello följande exempel visas hur toocheck hello tumavtrycket för ett klientcertifikat mot certifikat har överförts tooAPI hantering:</span><span class="sxs-lookup"><span data-stu-id="aae06-115">hello following example shows how toocheck hello thumbprint of a client certificate against certificates uploaded tooAPI Management:</span></span> 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a><span data-ttu-id="aae06-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aae06-116">Next step</span></span>

*  [<span data-ttu-id="aae06-117">Hur toosecure backend-tjänster som använder klienten certifikatautentisering</span><span class="sxs-lookup"><span data-stu-id="aae06-117">How toosecure back-end services using client certificate authentication</span></span>](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [<span data-ttu-id="aae06-118">Hur tooupload certifikat</span><span class="sxs-lookup"><span data-stu-id="aae06-118">How tooupload certificates</span></span>](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

