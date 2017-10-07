---
title: aaaTwo steg verifiering och AD FS - Azure MFA | Microsoft Docs
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver hur tooget igång med Azure MFA och AD FS."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 44fbba68-6cf9-46c1-a9df-736580b68ae3
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2017
ms.author: kgremban
ms.openlocfilehash: 7c1c925039d3cb753ba60e286168e5869faeae4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Komma igång med Azure Multi-Factor Authentication och Active Directory Federation Services
<center>![Moln](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Om din organisation har federerat det lokala Active Directory med Azure Active Directory med hjälp av AD FS finns det två Azure Multi-Factor Authentication-alternativ tillgängliga.

* Skydda molnresurser med Azure Multi-Factor Authentication eller Active Directory Federation Services
* Skydda molnet och lokala resurser med Azure Multi-Factor Authentication Server

hello följande tabell sammanfattas hello verifiering upplevelse mellan att skydda resurser med Azure Multi-Factor Authentication och AD FS

| Verifieringsupplevelse – webbläsarbaserade appar | Verifieringsupplevelse – appar som inte är webbläsarbaserade |
|:--- |:--- |:--- |
| Skydda Azure AD-resurser med hjälp av Azure Multi-Factor Authentication |<li>hello första verifieringssteget utförs lokalt med hjälp av AD FS.</li> <li>hello andra steget är en telefonbaserad metod som utförs genom att använda autentisering i molnet.</li> |
| Skydda Azure AD-resurser med hjälp av Active Directory Federation Services |<li>hello första verifieringssteget utförs lokalt med hjälp av AD FS.</li><li>hello andra steget är utförs lokalt genom att respektera hello anspråk.</li> |

Varningar med applösenord för federerade användare:

* Applösenord verifieras med molnautentisering och kringgår därför federation. Federation används endast aktivt när applösenorden konfigureras.
* Inställningar för lokal klientåtkomstkontroll respekteras inte av applösenord.
* Du kan inte använda lokal autentiseringsloggning med applösenord.
* Inaktivering/borttagning av konto kan ta toothree timmar för directory-synkronisering, vilket fördröjer inaktivering/borttagning av applösenord i hello molnidentitet.

## <a name="next-steps"></a>Nästa steg
Information om hur du konfigurerar Azure Multi-Factor Authentication eller hello Azure Multi-Factor Authentication-Server med AD FS finns i hello följande artiklar:

* [Skydda molnresurser med hjälp av Azure Multi-Factor Authentication och AD FS](multi-factor-authentication-get-started-adfs-cloud.md)
* [Skydda molnresurser och lokala resurser med Azure Multi-Factor Authentication Server med Windows Server 2012 R2 AD FS](multi-factor-authentication-get-started-adfs-w2k12.md)
* [Skydda molnet och lokala resurser med Azure Multi-Factor Authentication Server med AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md)
