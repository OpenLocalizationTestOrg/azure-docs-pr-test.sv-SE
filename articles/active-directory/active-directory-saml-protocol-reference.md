---
title: "Referens för Azure AD-SAML-protokollet | Microsoft Docs"
description: "Den här artikeln innehåller en översikt över enkel inloggning och enkel Sign-Out SAML-profiler i Azure Active Directory."
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
ms.date: 07/19/2017
ms.author: priyamo
ms.openlocfilehash: 7361d05850cf3ae997c0c186bf9a674c139f1f9e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# Hur Azure Active Directory använder SAML-protokoll
Azure Active Directory (AD Azure) använder SAML 2.0-protokollet för att programmen ska tillhandahålla en enkel inloggning för sina användare. Den [enkel inloggning](active-directory-single-sign-on-protocol-reference.md) och [enkel utloggning](active-directory-single-sign-out-protocol-reference.md) SAML-profiler i Azure AD förklarar hur SAML intyg, protokoll och bindningar används i tjänsten identitet provider.

SAML-protokoll kräver identitetsprovider (Azure AD) och service provider (programmet) för att utbyta information om sig själva.

När ett program registreras med Azure AD, registrerar apputvecklaren federation-relaterad information med Azure AD. Detta inkluderar den **omdirigerings-URI** av programmet.

Azure Active Directory visar specifika klient- och vanliga (oberoende av klient) enkel inloggning och enkel utloggning slutpunkter. Dessa URL: er representerar adresserbara platser – de är inte bara en identifierare--så att du kan gå till slutpunkten att läsa metadata.

* Klient-specifika slutpunkten finns på `https://login.microsoftonline.com/<TenantDomainName>/FederationMetadata/2007-06/FederationMetadata.xml`.  Den <TenantDomainName> är ett registrerat domännamn eller TenantID GUID för en Azure AD-klient. Till exempel federationsmetadata för contoso.com-klienten är på: https://login.microsoftonline.com/contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

* Slutpunkten oberoende av klient finns på `https://login.microsoftonline.com/common/FederationMetadata/2007-06/FederationMetadata.xml`. I den här slutpunktsadress **vanliga** visas i stället för en klient domän- eller -ID.

Information om Federationsmetadata dokument som Azure AD publicerar finns [Federationsmetadata](active-directory-federation-metadata.md).
