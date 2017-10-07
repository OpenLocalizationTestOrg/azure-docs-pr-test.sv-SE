---
title: "attribut för aaaAzure AD Connect sync shadow | Microsoft Docs"
description: "Beskriver hur shadow attribut fungerar i Azure AD Connect-synkroniseringstjänsten."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 1b8665e7488c6078b655f8a3e35519145bacd898
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-service-shadow-attributes"></a>Azure AD Connect sync shadow attribut
De flesta attribut representeras hello samma sätt i Azure AD som i din lokala Active Directory. Men vissa attribut har en särskild hantering och hello attributvärdet i Azure AD kan vara annorlunda än Azure AD Connect synkroniserar.

## <a name="introducing-shadow-attributes"></a>Introduktion till shadow attribut
Vissa attribut har två representationer i Azure AD. Både hello lokalt värde och ett beräknat värde som lagras. Dessa extra attribut kallas shadow attribut. hello två vanligaste attributen där du ser det här beteendet är **userPrincipalName** och **proxyAddress**. hello förändring attributvärden händer när det finns värden i attributen som motsvarar icke verifierade domäner. Men hello Synkroniseringsmotorn i Connect läser hello värde i hello shadow attributet så från dess perspektiv, hello-attributet har bekräftats av Azure AD.

Du kan inte se hello shadow attribut med hello Azure portal eller PowerShell. Men förstå hello konceptet kan du tootroubleshoot vissa scenarier där hello-attributet har olika värden på lokalt och i hello molnet.

toobetter förstå hello beteende, titta på det här exemplet från Fabrikam:  
![Domäner](./media/active-directory-aadconnectsyncservice-shadow-attributes/domains.png)  
De har flera UPN-suffix i sina lokala Active Directory, men de bara har verifierat en.

### <a name="userprincipalname"></a>UserPrincipalName
En användare har hello följande attributvärden i en icke-verifierad domän:

| Attribut | Värde |
| --- | --- |
| lokala userPrincipalName | lee.sperry@fabrikam.com |
| Azure AD-shadowUserPrincipalName | lee.sperry@fabrikam.com |
| Azure AD-userPrincipalName | lee.sperry@fabrikam.onmicrosoft.com |

attributet för hello userPrincipalName är hello värdet när du använder PowerShell.

Eftersom hello verkliga lokalt attributvärdet lagras i Azure AD kan du verifiera när hello fabrikam.com domän, uppdaterar Azure AD hello userPrincipalName attributet med hello-värde från hello shadowUserPrincipalName. Du har inte toosynchronize ändringar från Azure AD Connect för dessa värden toobe uppdateras.

### <a name="proxyaddresses"></a>proxyAddresses
hello samma process för att bara inkludera verifierade domäner inträffar också för proxyAddresses men med vissa ytterligare logik. hello Sök efter verifierade domäner sker endast för postlådan användare. En e-postaktiverade användare eller kontakta representerar en användare i en annan Exchange-organisation och du kan lägga till alla värden i proxyAddresses toothese objekt.

För en postlåda användare, antingen lokalt eller i Exchange Online visas bara värden för verifierade domäner. Det kan se ut så här:

| Attribut | Värde |
| --- | --- |
| lokala proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie.spencer@fabrikam.com</br>smtp:abbie@fabrikamonline.com |
| Exchange Online proxyAddresses | SMTP:abbie.spencer@fabrikamonline.com</br>smtp:abbie@fabrikamonline.com</br>SIP:abbie.spencer@fabrikamonline.com |

I det här fallet  **smtp:abbie.spencer@fabrikam.com**  har tagits bort eftersom den domänen inte har verifierats. Men har även lagt till Exchange  **SIP:abbie.spencer@fabrikamonline.com** . Fabrikam inte har använt Lync/Skype lokal, men Azure AD och Exchange Online förbereda för den.

Den här logiken för proxyAddresses är refererad tooas **ProxyCalc**. ProxyCalc anropas med varje ändring för en användare när:

- hello användaren har tilldelats en serviceplan som innehåller Exchange Online, även om hello användaren inte har licensierats för Exchange. Till exempel om hello användaren har tilldelats hello Office E3 SKU, men endast har tilldelats SharePoint Online. Detta gäller även om postlådan är fortfarande lokalt.
- hello attributet msExchRecipientTypeDetails har ett värde.
- Du gör en ändring tooproxyAddresses eller userPrincipalName.

ProxyCalc kan ta viss tid tooprocess ändras för en användare och är inte synkroniserad med hello Azure AD Connect exporten.

> [!NOTE]
> Hej ProxyCalc logik har vissa ytterligare beteenden för avancerade scenarier som inte beskrivs i det här avsnittet. Det här avsnittet är du toounderstand hello beteendet inte dokumentera alla internt.

### <a name="quarantined-attribute-values"></a>I karantän attributvärden
Shadow-attribut används också när det finns dubblerade attributvärden. Mer information finns i [dubblettattribut återhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="see-also"></a>Se även
* [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
