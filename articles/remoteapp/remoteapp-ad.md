---
title: "Azure AD + Active Directory-krav för Azure RemoteApp | Microsoft Docs"
description: "Lär dig mer om att konfigurera Active Directory för att fungera med Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 66366b25-6012-45fa-a4f6-da0ddfe0b486
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 78008a032faa93795cc02b720d68a0c6f5f16e9a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + Active Directory-krav för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Du måste göra följande för hybrid Azure RemoteApp-samlingen eller för en molnsamling som du vill federera med AD Connect.

### <a name="connect-azure-ad-and-active-directory"></a>Azure AD Connect och Active Directory
Om du vill ansluta din Azure AD-klient och din lokala Active Directory-miljöer, använder du AD Connect. Det tar du bara [4 klick](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) att ansluta två kataloger.

Obs! – katalogsynkronisering krävs för hybridsamlingar.

### <a name="make-sure-your-domaincom-match"></a>Kontrollera att din ”@domain.com” matchar
Innan du börjar måste du kontrollera att UPN för den lokala skogen matchar suffixet för Azure AD-domän. 

När du har skapat domänsuffixet UPN i Azure AD, alla användare som loggar in på Azure RemoteApp ska logga in som ”användare @<the suffix you set up>”. Kontrollera att användare kan också logga in med samma user@suffix i den lokala domänen. I vissa fall kan du konfigurera ett domännamn i Azure AD när du anger ett annat domänsuffix för en användare på-plats. I detta fall kan användarna inte att ansluta till alla domänanslutna datorer eller resurser via Azure RemoteApp.

Om du ställer in din domän UPN-suffix i AAD som contoso.com, men vissa användare på lokala/AD har konfigurerats för att logga in med till exempel @contoso.uk, och sedan dessa användare inte korrekt logga in i samlingen ARA. Användare UPN i AAD och AD måste vara samma för inloggningen till vara möjligt ”

### <a name="create-objects-for-azure-remoteapp"></a>Skapa objekt för Azure RemoteApp
Du måste också skapa följande lokal Active Directory-objekt:

* Ett tjänstkonto för att ge åtkomst till resurser i domänen för RemoteApp-program genom att anslutas RDSH-slutpunkter till den lokala domänen.
* En organisationsenhet (OU) som innehåller RemoteApp-datorobjekt. Användning av Organisationsenheten rekommenderas (men krävs inte) att isolera konton och principer som du vill använda med RemoteApp.

Du måste båda av dessa objekt när du skapar en RemoteApp-samlingen måste du utföra de här stegen först.

