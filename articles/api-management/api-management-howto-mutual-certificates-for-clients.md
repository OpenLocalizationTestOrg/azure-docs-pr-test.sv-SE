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
# <a name="how-toosecure-apis-using-client-certificate-authentication-in-api-management"></a>Hur toosecure API: er med klienten certifikatautentisering i API Management

API Management ger hello kapaciteten toosecure åtkomst tooAPIs (d.v.s. klienten tooAPI Management) med hjälp av klientcertifikat. För närvarande kan du kontrollera hello tumavtrycket för ett klientcertifikat mot ett önskat värde. Du kan också kontrollera hello tumavtrycket mot befintliga certifikat har överförts tooAPI Management.  

Information om hur du skyddar åtkomst toohello backend-tjänst för en API som använder klientcertifikat (d.v.s. API Management tooback-end) finns [hur toosecure backend-tjänster som använder klienten certifikatautentisering](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)

## <a name="checking-hello-expiration-date"></a>Kontrollera hello upphör att gälla

Nedan principer kan vara konfigurerade toocheck om hello certifikatet har upphört att gälla:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-issuer-and-subject"></a>Kontrollerar hello utfärdare och ämne

Nedan principer kan vara konfigurerade toocheck hello utfärdare och ämnet för ett klientcertifikat:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-thumbprint"></a>Kontrollera hello tumavtrycket

Nedan principer kan vara konfigurerade toocheck hello tumavtrycket för ett klientcertifikat:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-tooapi-management"></a>Kontrollerar ett tumavtryck mot certifikat har överförts tooAPI Management

hello följande exempel visas hur toocheck hello tumavtrycket för ett klientcertifikat mot certifikat har överförts tooAPI hantering: 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a>Nästa steg

*  [Hur toosecure backend-tjänster som använder klienten certifikatautentisering](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [Hur tooupload certifikat](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

