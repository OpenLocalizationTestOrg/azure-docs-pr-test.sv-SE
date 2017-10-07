---
title: "aaaAzure referens för AD-SAML-protokollet | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över hello enkel inloggning och enkel Sign-Out SAML-profiler i Azure Active Directory."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.assetid: 88125cfc-45c1-448b-9903-a629d8f31b01
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: priyamo
ms.custom: aaddev
ms.reviewer: dastrock
ms.openlocfilehash: d712289b16dc40a6b43a96fadef729c55cdaac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# Hur Azure Active Directory använder hello SAML-protokoll
Azure Active Directory (AD Azure) använder hello SAML 2.0-protokollet tooenable program tooprovide en enkel inloggning upplevelse tootheir användare. Hej [enkel inloggning](active-directory-single-sign-on-protocol-reference.md) och [enkel utloggning](active-directory-single-sign-out-protocol-reference.md) SAML-profiler i Azure AD förklarar hur SAML intyg, protokoll och bindningar används i hello identity provider-tjänsten.

SAML-protokoll måste hello identitetsprovider (Azure AD) och hello service provider (hello-program) tooexchange information om sig själva.

När ett program registreras med Azure AD, registrerar hello apputvecklaren federation-relaterad information med Azure AD. Detta inkluderar hello **omdirigerings-URI** och **Metadata URI** av programmet hello.

Azure AD använder hello **Metadata URI** av hello cloud service tooretrieve hello signering nyckel och hello logga ut URI för hello-Molntjänsten. Om hello programmet inte har stöd för metadata URI kontakta hello utvecklare Microsoft support tooprovide hello logga ut URI och nyckel för signeringscertifikatet.

Azure Active Directory visar specifika klient- och vanliga (oberoende av klient) enkel inloggning och enkel utloggning slutpunkter. Dessa URL: er representerar adresserbara platser – de är inte bara en identifierare--så att du kan gå toohello endpoint tooread hello metadata.

* hello klient-specifika slutpunkt finns på `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Hej <TenantDomainName> är ett registrerat domännamn eller TenantID GUID för en Azure AD-klient. Exempelvis hello federationsmetadata hello contoso.com klient finns på: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* hello oberoende av klient-slutpunkt finns på `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. I den här slutpunktsadress **vanliga** visas i stället för en klient domän- eller -ID.

Information om hello Federationsmetadata dokument som Azure AD publicerar finns [Federationsmetadata](active-directory-federation-metadata.md).
