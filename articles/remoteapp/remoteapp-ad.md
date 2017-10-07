---
title: "aaaAzure AD + Active Directory-krav för Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur tooset in Active Directory toowork med Azure RemoteApp."
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
ms.openlocfilehash: c1c4a7ad6fb96ec4d479fdc231f03d81b58b2b71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad--active-directory-requirements-for-azure-remoteapp"></a>Azure AD + Active Directory-krav för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

För hybrid Azure RemoteApp-samlingen eller för en molnsamling som du vill toofederate med AD Connect måste toodo hello följande.

### <a name="connect-azure-ad-and-active-directory"></a>Azure AD Connect och Active Directory
Om du vill tooconnect Azure AD-klienten och din lokala Active Directory-miljöer, använder du AD Connect. Det tar du bara [4 klick](https://blogs.technet.microsoft.com/enterprisemobility/2014/08/04/connecting-ad-and-azure-ad-only-4-clicks-with-azure-ad-connect/) tooconnect hello två kataloger.

Obs! – katalogsynkronisering krävs för hybridsamlingar.

### <a name="make-sure-your-domaincom-match"></a>Kontrollera att din ”@domain.com” matchar
Innan du börjar måste du kontrollera om den hello UPN för din lokala skog matchar hello suffix för Azure AD-domän. 

När du har skapat hello UPN domänsuffix i Azure AD, alla användare som loggar in på Azure RemoteApp ska logga in som ”användare @<hello suffix you set up>”. Se till att användarna kan också logga in med hello samma user@suffix i hello lokal domän. I vissa fall kan du konfigurera ett domännamn i Azure AD när du anger ett annat domänsuffix för hello användaren på-plats. I det här fallet kan användarna inte kan tooconnect tooany domänanslutna datorer eller resurser via Azure RemoteApp.

Om du ställer in din domän UPN-suffix i AAD som contoso.com, men vissa användare på lokala/AD finns konfigurerade toolog med till exempel @contoso.uk, och sedan dessa användare inte kommer att kunna toocorrectly logga in på hello ARA samling. Användare måste vara i UPN i AAD och AD hello samma för hello inloggningen toobe möjliga ”

### <a name="create-objects-for-azure-remoteapp"></a>Skapa objekt för Azure RemoteApp
Du måste också toocreate hello följande lokala Active Directory-objekt:

* En service-kontot tooprovide åtkomst toodomain resurser för RemoteApp-program genom att anslutas RDSH slutpunkterna toohello lokal domän.
* En organisationsenhet (OU) toocontain RemoteApp-datorobjekt. Hello OU används rekommenderade (men krävs inte) tooisolate hello konton och principer som du vill använda med RemoteApp.

Du måste båda av dessa objekt när du skapar en RemoteApp-samlingen så att du toodo här först.

