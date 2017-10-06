---
title: aaaFederating flera Azure AD med enda AD FS | Microsoft Docs
description: "I det här dokumentet får du lära dig hur toofederate flera Azure AD med en enda AD FS."
keywords: federate, ADFS, AD FS, multiple tenants, single AD FS, one ADFS, multi-tenant federation, multi-forest adfs, aad connect, federation, cross-tenant federation
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: anandy; billmath
ms.openlocfilehash: 442192896b3b13f7bf9388396cd3769e194329d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#<a name="federate-multiple-instances-of-azure-ad-with-single-instance-of-ad-fs"></a>Federera flera instanser av Azure AD med en enda instans av AD FS

En enda AD FS-servergrupp med hög tillgänglighet kan federera flera skogar om de har ett dubbelriktat förtroende. Dessa flera skogar kanske eller kanske inte stämmer överens toohello samma Azure Active Directory. Den här artikeln innehåller anvisningar för hur tooconfigure federation mellan en enskild AD FS-distribution och flera skogar som synkronisering toodifferent Azure AD.

![Federation med flera innehavare med en enda AD FS](media/active-directory-aadconnectfed-single-adfs-multitenant-federation/concept.png)
 
> [!NOTE]
> Tillbakaskrivning av enheter och automatisk enhetskoppling stöds inte i det här scenariot.

> [!NOTE]
> Azure AD Connect kan inte vara används tooconfigure federation i det här scenariot som Azure AD Connect kan konfigurera federation för domäner i en enda Azure AD.

##<a name="steps-for-federating-ad-fs-with-multiple-azure-ad"></a>Steg för att federera AD FS med flera Azure AD-instanser

Överväg att en domänen contoso.com i Azure Active Directory contoso.onmicrosoft.com redan är federerat med hello AD FS lokalt installerade på contoso.com lokala Active Directory-miljö. Fabrikam.com är en domän i Azure Active Directory fabrikam.onmicrosoft.com.

##<a name="step-1-establish-a-two-way-trust"></a>Steg 1: Upprätta ett dubbelriktat förtroende
 
För AD FS i contoso.com toobe kan tooauthenticate användare i fabrikam.com krävs ett dubbelriktat förtroende mellan contoso.com och fabrikam.com. Följ hello riktlinje i det här [artikel](https://technet.microsoft.com/library/cc816590.aspx) toocreate hello dubbelriktat förtroende.
 
##<a name="step-2-modify-contosocom-federation-settings"></a>Steg 2: Ändra federationsinställningarna för contoso.com 
 
hello standard utfärdaren som angetts för en enda domän federerad tooAD FS är ”http://ADFSServiceFQDN/adfs/services/trust”, till exempel ”http://fs.contoso.com/adfs/services/trust”. Azure Active Directory kräver en unik utfärdare för varje federerad domän. Eftersom hello samma AD FS kommer toofederate två domäner, måste värdet för hello utfärdaren toobe ändras så att den är unik för varje domän som AD FS federates med Azure Active Directory. 
 
Öppna Azure AD PowerShell på hello AD FS-servern och utföra hello följande steg:
 
Ansluta toohello Azure Active Directory som innehåller hello domänen contoso.com Anslut MsolService hello federation uppdateringsinställningar för contoso.com uppdatering MsolFederatedDomain - DomainName contoso.com – SupportMultipleDomain
 
Utfärdare i hello domänfederationsinställningen kommer att ändras för ”http://contoso.com/adfs/services/trust” och en utgivningsprinciper anspråk regeln ska läggas till för hello Azure AD förlitande part tooissue hello rätt issuerId värde baserat på hello UPN-suffix.
 
##<a name="step-3-federate-fabrikamcom-with-ad-fs"></a>Steg 3: Federera fabrikam.com med AD FS
 
I Azure AD powershell utföras sessionen hello följande: Anslut tooAzure Active Directory som innehåller hello domän fabrikam.com

    Connect-MsolService
Konvertera hello fabrikam.com hanterade domänen toofederated:

    Convert-MsolDomainToFederated -DomainName anandmsft.com -Verbose -SupportMultipleDomain
 
hello ovan åtgärden kommer federera hello domän fabrikam.com med hello samma AD FS. Du kan kontrollera hello Domäninställningar med hjälp av Get-MsolDomainFederationSettings för båda domänerna.

## <a name="next-steps"></a>Nästa steg
[Ansluta Active Directory med Azure Active Directory](active-directory-aadconnect.md)
