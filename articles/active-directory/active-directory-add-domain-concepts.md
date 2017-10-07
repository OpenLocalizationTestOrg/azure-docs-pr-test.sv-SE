---
title: "aaaConceptual översikt över anpassade domännamn i Azure Active Directory | Microsoft Docs"
description: "Beskriver hello konceptuella framework för att använda anpassade domännamn i Azure Active directory, inklusive federation för enkel inloggning"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fd0c5def-0da2-43af-81bc-76f4cfe86afd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 0a3454ae6b733a8a13a71925df3cc664063f388e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Översikt över anpassade domännamn i Azure Active Directory
Ett domännamn kan vara en viktig identifierare för många directory-resurser som en del av:

* Ett användarnamn eller e-postadress för en användare
* hello-adressen för en grupp
* hello app-ID URI för ett program

En resurs i Azure Active Directory (Azure AD) kan innehålla ett domännamn som redan har verifierats toobe ägs av hello katalog som innehåller hello-resurs. Endast en global administratör kan utföra hanteringsuppgifter för domänen i Azure AD.

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. Hur toomanage ditt domännamn i administrationscentret för hello Azure AD, finns i [hantera anpassade domännamn i Azure Active Directory](active-directory-domains-manage-azure-portal.md).

Domännamn i Azure AD är globalt unika. Ett anpassat domännamn kan användas av endast en Azure AD-klient i taget. Om en Azure AD-katalog har verifierat att ett domännamn, kan sedan inga andra Azure AD-katalog verifiera eller använder samma domännamnet.

## <a name="initial-and-custom-domain-names"></a>Inledande och anpassade domännamn
Varje domännamn i Azure AD är antingen ett första domännamn eller ett anpassat domännamn.

Varje Azure AD innehåller ett första domännamn i hello formuläret contoso.onmicrosoft.com. Det här tredje nivån domännamnet i det här exemplet ”contoso.onmicrosoft.com” har upprättats när hello katalogen skapades, vanligtvis Hej administratör som skapade hello directory. hello ursprungliga domännamnet för en katalog kan inte ändras eller tas bort. hello första domännamn riktar medan fullständigt funktionella sig främst toobe används som en bootstrapping mekanism tills ett anpassat domännamn har verifierats.

En katalog har minst en verifierad domän, till exempel ”contoso.com” i de flesta produktionsmiljöer, och det är den anpassade domänen som är synliga tooend användare. Ett anpassat domännamn är ett domännamn som ägs och används av den organisationen, till exempel ”contoso.com” för använder som värd för webbplatsen. Det här domännamnet är bekant tooemployees eftersom den är en del av hello användarnamnet som de använder toosign i företagsnätverket för toohello eller toosend och hämta e-post.

Innan den kan användas av Azure AD, hello domännamn måste vara tillagda tooyour katalog och verifierats.

## <a name="verified-and-unverified-domain-names"></a>Verifierade och overifierade domännamn
hello ursprungliga domännamnet för en katalog utvärderas implicit som verifierats av Azure AD. När en administratör lägger till en anpassad domän namn tooan Azure AD, är det ursprungligen overifierade tillståndet. Azure AD kan inte alla directory resurser toouse ett overifierade domännamn. Detta säkerställer att endast en katalog kan använda ett visst domännamn och att hello organisation använder hello domännamn äger domännamnet.

Azure AD verifierar ägarskap för ett domännamn genom att söka efter en särskild post i hello domain name service (DNS)-zonfilen för hello domännamn. tooverify ägarskap av ett domännamn, administratör hämtar hello DNS-post från Azure AD att Azure AD att söka efter och lägger till den post toohello DNS-zonfilen för domännamnet hello. hello DNS-zonfilen underhålls av hello domännamnsregistratorn för domänen. hello steg tooverify en domän som visas i hello-artikel [att lägga till en anpassad domän tooyour Azure AD-katalog](active-directory-add-domain.md).

Lägga till en DNS-post toohello zonfilen för domännamnet hello påverkar inte andra domäntjänster, till exempel e-post eller webbhotell.

## <a name="federated-and-managed-domain-names"></a>Federerade och hanterade domännamn
Ett anpassat domännamn i Azure AD kan vara konfigurerade toogive användare en federerad inloggning upplevelse mellan din lokala Active Directory och Azure AD. Konfigurera en domän för federationen kräver uppdaterar tooprivileged resurser i Azure AD och även tooyour Windows Server Active Directory. Konfigurera en federerad domän måste utföras från Azure AD Connect eller med hjälp av PowerShell. Federering en anpassad domän kan inte initieras från hello klassiska Azure-portalen. [Titta på den här videon toolearn om hur du konfigurerar AD FS för användaren loggar in med Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domäner som inte är federerat kallas ibland hanterade domäner. hello initiala domänen för en Azure AD-katalog är implicit utvärderas som en hanterad domän.

## <a name="primary-domain-names"></a>Primära domännamn
hello primära domännamnet för en katalog är hello domännamn som är förvald som hello standardvärde för hello domän delen av hello användarnamn, när en administratör skapar en ny användare i hello [Azure-portalen](https://portal.azure.com/), eller en annan portal till exempel hello Office 365-administrationsportalen eller hello Microsoft Intune-portalen. En katalog kan ha endast en primär domännamn. En administratör kan ändra hello primär domän namn toobe några verifierade domän som inte är federerat eller toohello initiala domänen.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Domännamn i Azure AD och andra Microsoft Online Services
Ett domännamn måste verifieras i Azure AD innan den kan användas av en annan Microsoft Online Service som Exchange Online, SharePoint Online och Intune. Följande tjänster kräver normalt en administratör tooadd en eller flera DNS-poster som är särskilt toohello service.

En Azure-webbapp använder sin egen mekanism tooverify ägarskap för en domän. En domän måste verifieras för användning med Azure AD, även om det tidigare har verifierats för användning av en Azure webbapp i en prenumeration som förlitar sig på att Azure AD. En Azure-app kan använda ett domännamn som har verifierats i en annan katalog från hello-katalog som skyddar hello webbprogrammet.

## <a name="managing-domain-names"></a>Hantera domännamn
Hanteringsuppgifter för domänen kan utföras från hello klassiska Azure-portalen och PowerShell. Många aktiviteter kan utföras med hjälp av hello Azure AD Graph API.

* [Lägga till och verifiera ett eget domännamn](active-directory-add-domain.md)
* [Hantera domäner i hello klassiska Azure-portalen](active-directory-add-manage-domain-names.md)
* [Med hjälp av PowerShell toomanage domännamn i Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Använder hello Azure AD Graph API toomanage domännamn i Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

