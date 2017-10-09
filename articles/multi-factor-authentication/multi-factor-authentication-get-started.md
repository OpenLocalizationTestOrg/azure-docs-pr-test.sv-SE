---
title: aaaChoose mellan Azure MFA-molnet eller server | Microsoft Docs
description: "Välj hello multifaktorautentisering lösning som passar dig genom att fråga, vilka am jag försökte toosecure och där är Mina användare finns.  Välj sedan molnet, MFA Server eller AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: ec2270ea-13d7-4ebc-8a00-fa75ce6c746d
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: bd9639e5f744f586d9143c6e90b105ed645eecb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="choose-hello-azure-multi-factor-authentication-solution-for-you"></a>Välj hello Azure Multi-Factor Authentication-lösningen för dig
Eftersom det finns flera varianter av Azure Multi-Factor Authentication (MFA), måste vi besvara några frågor toofigure reda på vilken version är hello rätt en toouse.  Dessa frågor är:

* [Vad kan jag försöker toosecure](#what-am-i-trying-to-secure)
* [Var finns hello användare](#where-are-the-users-located)
* [Vilka funktioner behöver jag?](#what-featured-do-i-need)

hello följande avsnitt innehåller anvisningar om hur du bestämmer var och en av dessa svar.

## <a name="what-am-i-trying-toosecure"></a>Vad kan jag försöker toosecure?
toodetermine hello rätt tvåstegsverifiering verifiering lösningen först måste vi svara hello frågan om vilka som du försöker toosecure med en annan metod för autentisering.  Är det ett program som finns i Azure?  Eller ett fjärråtkomstsystem?  Genom att bestämma vad vi försöker toosecure, vi svarar hello fråga där Multi-Factor Authentication måste toobe aktiverat.  

| Vad är försök toosecure | MFA i molnet hello | MFA-server |
| --- |:---:|:---:|
| Första parts Microsoft-appar |● |● |
| SaaS-appar i hello app-galleriet |● |  |
| Webbprogram publicerade via Azure AD App Proxy |● |  |
| IIS-program som inte är publicerade via Azure AD App Proxy | |● |
| Fjärråtkomst som VPN eller Fjärrskrivbordsgateway (RDG) | ● | ● |

## <a name="where-are-hello-users-located"></a>Var finns hello användare
Därefter granskar där våra användare finns kan toodetermine hello rätt lösning toouse om i hello molnet eller lokalt med hello MFA-servern.

| Användarplats | MFA i molnet hello | MFA-server |
| --- |:---:|:---:|
| Azure Active Directory |● | |
| Azure AD och lokalt AD med federation med AD FS |● |● |
| Azure AD och lokalt AD med DirSync, Azure AD Sync, Azure AD Connect – utan lösenordssynkronisering |● |● |
| Azure AD och lokalt AD med DirSync, Azure AD Sync, Azure AD Connect – med lösenordssynkronisering |● | |
| Lokalt Active Directory | |● |

## <a name="what-features-do-i-need"></a>Vilka funktioner behöver jag?
hello jämförs följande tabell hello-funktioner som är tillgängliga med Multi-Factor Authentication i molnet hello och hello Multi-Factor Authentication-servern.

| Funktion | MFA i molnet hello | MFA-server |
| --- |:---:|:---:|
| Meddelanden via mobilapp som en andra faktor | ● | ● |
| Verifieringskod via mobilapp som en andra faktor | ● | ● |
| Telefonsamtal som en andra faktor | ● | ● |
| Enkelriktad SMS som en andra faktor | ● | ● |
| Dubbelriktad SMS som en andra faktor | | ● |
| Maskinvarutoken som en andra faktor | | ● |
| Applösenord för Office 365-klienter som inte stöder MFA | ● | |
| Administratörskontroll över autentiseringsmetoder | ● | ● |
| PIN-läge | | ● |
| Bedrägerivarning |● | ● |
| MFA-rapporter |● | ● |
| Engångsförbikoppling | | ● |
| Anpassade hälsningar för telefonsamtal | ● | ● |
| Anpassningsbar nummerpresentation för telefonsamtal | ● | ● |
| Tillförlitliga IP-adresser | ● | ● |
| MFA sparas för betrodda enheter | ● | |
| Villkorlig åtkomst | ● | ● |
| Cache |  | ● |

## <a name="next-steps"></a>Nästa steg

Nu när vi har fastställt om toouse molnet multifaktorautentisering eller hello MFA-servern lokalt, kan vi börjar ställa in och använda Azure Multi-Factor Authentication. **Välj hello-ikon som representerar ditt scenario**

<center>




[![Molnet](./media/multi-factor-authentication-get-started/cloud2.png)](multi-factor-authentication-get-started-cloud.md)  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[![Server](./media/multi-factor-authentication-get-started/server2.png)](multi-factor-authentication-get-started-server.md) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </center>
