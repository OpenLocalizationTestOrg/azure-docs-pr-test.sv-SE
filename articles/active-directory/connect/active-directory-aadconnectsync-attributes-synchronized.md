---
title: Attribut som synkroniseras med Azure AD Connect | Microsoft Docs
description: Visar hello attribut som synkroniseras tooAzure Active Directory.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c2bb36e0-5205-454c-b9b6-f4990bcedf51
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: 2fe5b944a7fc832f245631416c265fb82eedeb15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-attributes-synchronized-tooazure-active-directory"></a>Azure AD Connect-synkronisering: attribut synkroniserade tooAzure Active Directory
Det här avsnittet visar hello-attribut som synkroniseras med Azure AD Connect-synkronisering.  
hello attribut är grupperade efter relaterade hello Azure AD-appar.

## <a name="attributes-toosynchronize"></a>Attribut toosynchronize
En vanlig fråga är *vad är hello lista över minsta attribut toosynchronize*. hello standardwebbplatsen och den rekommenderade metoden är tookeep hello standardattribut så att en fullständig GAL (globala adresslistan) kan konstrueras i hello moln och tooget alla funktioner i Office 365-arbetsbelastningar. I vissa fall kan det finns vissa attribut att organisationen inte vill synkroniserade toohello molnet eftersom dessa attribut innehåller känslig eller personligt identifierbar information (personligt identifierbar information) data, precis som i det här exemplet:  
![felaktigt attribut](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

I så fall måste börja med hello listan med attribut i det här avsnittet och identifiera de attribut som innehåller känslig eller personligt identifierbar information data och kan inte synkroniseras. Sedan avmarkera dessa attribut under installation med hjälp av [Azure AD-app och attributfiltrering](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering).

> [!WARNING]
> När avmarkera attribut, bör du vara försiktig och endast avmarkera dessa attribut absolut inte möjligt toosynchronize. Unselecting andra attribut kan ha negativ inverkan på funktioner.
>
>

## <a name="office-365-proplus"></a>Office 365 ProPlus
| Attributets namn | Användare | Kommentar |
| --- |:---:| --- |
| accountEnabled |X |Anger om ett konto har aktiverats. |
| CN |X | |
| Visningsnamn |X | |
| objectSID |X |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| pwdLastSet |X |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. Används av både Lösenordssynkronisering och federation. |
| sourceAnchor |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| usageLocation |X |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| UserPrincipalName |X |UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |

## <a name="exchange-online"></a>exchange online
| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Anger om ett konto har aktiverats. |
| Installationsassistenten |X |X | | |
| altRecipient |X | | |Kräver Azure AD Connect build 1.1.552.0 eller efter. |
| authOrig |X |X |X | |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Företag |X |X | | |
| CountryCode |X |X | | |
| Avdelning |X |X | | |
| description |X |X |X | |
| Visningsnamn |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| HomePhone |X |X | | |
| Info |X |X |X |Det här attributet används för närvarande inte för grupper. |
| initialer |X |X | | |
| L |X |X | | |
| LegacyExchangeDN |X |X |X | |
| mailNickname |X |X |X | |
| mangedBy | | |X | |
| Manager |X |X | | |
| Medlem | | |X | |
| mobila |X |X | | |
| msDS-HABSeniorityIndex |X |X |X | |
| msDS-PhoneticDisplayName |X |X |X | |
| msExchArchiveGUID |X | | | |
| msExchArchiveName |X | | | |
| msExchAssistantName |X |X | | |
| msExchAuditAdmin |X | | | |
| msExchAuditDelegate |X | | | |
| msExchAuditDelegateAdmin |X | | | |
| msExchAuditOwner |X | | | |
| msExchBlockedSendersHash |X |X | | |
| msExchBypassAudit |X | | | |
| msExchBypassModerationLink | | |X |Tillgängligt i Azure AD Connect version 1.1.524.0 |
| msExchCoManagedByLink | | |X | |
| msExchDelegateListLink |X | | | |
| msExchELCExpirySuspensionEnd |X | | | |
| msExchELCExpirySuspensionStart |X | | | |
| msExchELCMailboxFlags |X | | | |
| msExchEnableModeration |X | |X | |
| msExchExtensionCustomAttribute1 |X |X |X |Det här attributet används för närvarande inte av Exchange Online. |
| msExchExtensionCustomAttribute2 |X |X |X |Det här attributet används för närvarande inte av Exchange Online. |
| msExchExtensionCustomAttribute3 |X |X |X |Det här attributet används för närvarande inte av Exchange Online. |
| msExchExtensionCustomAttribute4 |X |X |X |Det här attributet används för närvarande inte av Exchange Online. |
| msExchExtensionCustomAttribute5 |X |X |X |Det här attributet används för närvarande inte av Exchange Online. |
| msExchHideFromAddressLists |X |X |X | |
| msExchImmutableID |X | | | |
| msExchLitigationHoldDate |X |X |X | |
| msExchLitigationHoldOwner |X |X |X | |
| msExchMailboxAuditEnable |X | | | |
| msExchMailboxAuditLogAgeLimit |X | | | |
| msExchMailboxGuid |X | | | |
| msExchModeratedByLink |X |X |X | |
| msExchModerationFlags |X |X |X | |
| msExchRecipientDisplayType |X |X |X | |
| msExchRecipientTypeDetails |X |X |X | |
| msExchRemoteRecipientType |X | | | |
| msExchRequireAuthToSendTo |X |X |X | |
| msExchResourceCapacity |X | | | |
| msExchResourceDisplay |X | | | |
| msExchResourceMetaData |X | | | |
| msExchResourceSearchProperties |X | | | |
| msExchRetentionComment |X |X |X | |
| msExchRetentionURL |X |X |X | |
| msExchSafeRecipientsHash |X |X | | |
| msExchSafeSendersHash |X |X | | |
| msExchSenderHintTranslations |X |X |X | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| msExchUserHoldPolicies |X | | | |
| msOrg IsOrganizational | | |X | |
| objectSID |X | |X |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherTelephone |X |X | | |
| Personsökare |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postnummer |X |X | | |
| proxyAddresses |X |X |X | |
| publicDelegates |X |X |X | |
| pwdLastSet |X | | |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. Används av både Lösenordssynkronisering och federation. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityEnabled | | |X |Härleds från groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Rubrik |X |X | | |
| unauthOrig |X |X |X | |
| usageLocation |X | | |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| userCertificate |X |X | | |
| UserPrincipalName |X | | |UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |
| userSMIMECertificates |X |X | | |
| wWWHomePage |X |X | | |

## <a name="sharepoint-online"></a>sharepoint online
| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Anger om ett konto har aktiverats. |
| authOrig |X |X |X | |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Företag |X |X | | |
| CountryCode |X |X | | |
| Avdelning |X |X | | |
| description |X |X |X | |
| Visningsnamn |X |X |X | |
| dLMemRejectPerms |X |X |X | |
| dLMemSubmitPerms |X |X |X | |
| extensionAttribute1 |X |X |X | |
| extensionAttribute10 |X |X |X | |
| extensionAttribute11 |X |X |X | |
| extensionAttribute12 |X |X |X | |
| extensionAttribute13 |X |X |X | |
| extensionAttribute14 |X |X |X | |
| extensionAttribute15 |X |X |X | |
| extensionAttribute2 |X |X |X | |
| extensionAttribute3 |X |X |X | |
| extensionAttribute4 |X |X |X | |
| extensionAttribute5 |X |X |X | |
| extensionAttribute6 |X |X |X | |
| extensionAttribute7 |X |X |X | |
| extensionAttribute8 |X |X |X | |
| extensionAttribute9 |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| hideDLMembership | | |X | |
| homephone |X |X | | |
| Info |X |X |X | |
| initialer |X |X | | |
| ipPhone |X |X | | |
| L |X |X | | |
| E-post |X |X |X | |
| mailNickname |X |X |X | |
| Av | | |X | |
| Manager |X |X | | |
| Medlem | | |X | |
| middleName |X |X | | |
| mobila |X |X | | |
| msExchTeamMailboxExpiration |X | | | |
| msExchTeamMailboxOwners |X | | | |
| msExchTeamMailboxSharePointLinkedBy |X | | | |
| msExchTeamMailboxSharePointUrl |X | | | |
| objectSID |X | |X |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| oOFReplyToOriginator | | |X | |
| otherFacsimileTelephone |X |X | | |
| otherHomePhone |X |X | | |
| otherIpPhone |X |X | | |
| otherMobile |X |X | | |
| otherPager |X |X | | |
| otherTelephone |X |X | | |
| Personsökare |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postnummer |X |X | | |
| postOfficeBox |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. Används av både Lösenordssynkronisering och federation. |
| reportToOriginator | | |X | |
| reportToOwner | | |X | |
| securityEnabled | | |X |Härleds från groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| targetAddress |X |X | | |
| telephoneAssistant |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Rubrik |X |X | | |
| unauthOrig |X |X |X | |
| URL: en |X |X | | |
| usageLocation |X | | |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| UserPrincipalName |X | | |UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |
| wWWHomePage |X |X | | |

## <a name="lync-online"></a>Lync Online
| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Anger om ett konto har aktiverats. |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Företag |X |X | | |
| Avdelning |X |X | | |
| description |X |X |X | |
| Visningsnamn |X |X |X | |
| facsimiletelephonenumber |X |X |X | |
| givenName |X |X | | |
| homephone |X |X | | |
| ipPhone |X |X | | |
| L |X |X | | |
| E-post |X |X |X | |
| mailNickname |X |X |X | |
| Av | | |X | |
| Manager |X |X | | |
| Medlem | | |X | |
| mobila |X |X | | |
| msExchHideFromAddressLists |X |X |X | |
| msRTCSIP-ApplicationOptions |X | | | |
| msRTCSIP-DeploymentLocator |X |X | | |
| msRTCSIP-Line |X |X | | |
| msRTCSIP-OptionFlags |X |X | | |
| msRTCSIP-OwnerUrn |X | | | |
| msRTCSIP-PrimaryUserAddress |X |X | | |
| msRTCSIP-UserEnabled |X |X | | |
| objectSID |X | |X |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| otherTelephone |X |X | | |
| physicalDeliveryOfficeName |X |X | | |
| Postnummer |X |X | | |
| preferredLanguage |X | | | |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. Används av både Lösenordssynkronisering och federation. |
| securityEnabled | | |X |Härleds från groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| telephoneNumber |X |X | | |
| thumbnailphoto |X |X | | |
| Rubrik |X |X | | |
| usageLocation |X | | |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| UserPrincipalName |X | | |UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |
| wWWHomePage |X |X | | |

## <a name="azure-rms"></a>Azure RMS
| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Anger om ett konto har aktiverats. |
| CN |X | |X |Namn eller alias. Oftast hello prefix för [e] värde. |
| Visningsnamn |X |X |X |En sträng som representerar namnet på hello ofta visas som hello eget namn (Förnamn Efternamn). |
| E-post |X |X |X |fullständig e-postadress. |
| Medlem | | |X | |
| objectSID |X | |X |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| proxyAddresses |X |X |X |mekanisk egenskap. Används av Azure AD. Innehåller alla sekundära e-postadresser för hello användare. |
| pwdLastSet |X | | |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. |
| securityEnabled | | |X |Härleds från groupType. |
| sourceAnchor |X |X |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| usageLocation |X | | |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| UserPrincipalName |X | | |Den här UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |

## <a name="intune"></a>Intune
| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Anger om ett konto har aktiverats. |
| C |X |X | | |
| CN |X | |X | |
| description |X |X |X | |
| Visningsnamn |X |X |X | |
| E-post |X |X |X | |
| mailNickname |X |X |X | |
| Medlem | | |X | |
| objectSID |X | |X |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. Används av både Lösenordssynkronisering och federation. |
| securityEnabled | | |X |Härleds från groupType |
| sourceAnchor |X |X |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| usageLocation |X | | |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| UserPrincipalName |X | | |UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |

## <a name="dynamics-crm"></a>Dynamics CRM
| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Anger om ett konto har aktiverats. |
| C |X |X | | |
| CN |X | |X | |
| CO |X |X | | |
| Företag |X |X | | |
| CountryCode |X |X | | |
| description |X |X |X | |
| Visningsnamn |X |X |X | |
| facsimiletelephonenumber |X |X | | |
| givenName |X |X | | |
| L |X |X | | |
| Av | | |X | |
| Manager |X |X | | |
| Medlem | | |X | |
| mobila |X |X | | |
| objectSID |X | |X |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| physicalDeliveryOfficeName |X |X | | |
| Postnummer |X |X | | |
| preferredLanguage |X | | | |
| pwdLastSet |X | | |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. Används av både Lösenordssynkronisering och federation. |
| securityEnabled | | |X |Härleds från groupType |
| SN |X |X | | |
| sourceAnchor |X |X |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| St |X |X | | |
| StreetAddress |X |X | | |
| telephoneNumber |X |X | | |
| Rubrik |X |X | | |
| usageLocation |X | | |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| UserPrincipalName |X | | |UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |

## <a name="3rd-party-applications"></a>3 partsprogram
Den här gruppen är en uppsättning attribut som används som hello minimal attribut som behövs för en allmän arbetsbelastning eller ett program. Det kan användas för en arbetsbelastning som inte finns med i ett annat avsnitt eller för en icke-Microsoft-app. Den används explicit för hello följande:

* Yammer (endast användare används)
* [Hybrid Business-to-Business (B2B) mellan org samarbete erbjuds av resurser som SharePoint](http://go.microsoft.com/fwlink/?LinkId=747036)

Den här gruppen är en uppsättning attribut som kan användas om hello Azure AD-katalog inte används toosupport Office 365, Dynamics eller Intune. Den har en liten uppsättning core attribut.

| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| accountEnabled |X | | |Anger om ett konto har aktiverats. |
| CN |X | |X | |
| Visningsnamn |X |X |X | |
| givenName |X |X | | |
| E-post |X | |X | |
| Av | | |X | |
| mailNickName |X |X |X | |
| Medlem | | |X | |
| objectSID |X | | |mekanisk egenskap. Identifierare för AD-användare används toomaintain synkronisering mellan Azure AD och AD. |
| proxyAddresses |X |X |X | |
| pwdLastSet |X | | |mekanisk egenskap. Använda tooknow när tooinvalidate redan utfärdade token. Används av både Lösenordssynkronisering och federation. |
| SN |X |X | | |
| sourceAnchor |X |X |X |mekanisk egenskap. Oåterkalleliga identifierare toomaintain relationen mellan ADDS och Azure AD. |
| usageLocation |X | | |mekanisk egenskap. hello användarens land. Används för licenstilldelning. |
| UserPrincipalName |X | | |UPN är hello inloggnings-ID för hello användare. Hej oftast samma som [e] värde. |

## <a name="windows-10"></a>Windows 10
En Windows 10-domänanslutna computer(device) synkroniserar vissa attribut tooAzure AD. Mer information om scenarier för hello finns [ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser](../active-directory-azureadjoin-devices-group-policy.md). Dessa attribut synkronisera alltid och Windows 10 visas inte som en app som du kan avmarkera. En domänansluten dator i Windows 10 identifieras genom att låta hello attributet userCertificate fylls i.

| Attributets namn | Enhet | Kommentar |
| --- |:---:| --- |
| accountEnabled |X | |
| deviceTrustType |X |Hårdkodad värde för domänanslutna datorer. |
| Visningsnamn |X | |
| MS-DS-CreatorSID |X |Kallas även registeredOwnerReference. |
| objectGUID |X |Kallas även deviceID. |
| objectSID |X |Kallas även onPremisesSecurityIdentifier. |
| Operativsystemet |X |Kallas även deviceOSType. |
| operatingSystemVersion |X |Kallas även deviceOSVersion. |
| userCertificate |X | |

Dessa attribut för **användaren** är dessutom toohello andra appar som du har valt.  

| Attributets namn | Användare | Kommentar |
| --- |:---:| --- |
| domainFQDN |X |Kallas även DNS-domännamn. Till exempel contoso.com. |
| domainNetBios |X |Kallas även NetBIOS-namn. Till exempel CONTOSO. |

## <a name="exchange-hybrid-writeback"></a>Tillbakaskrivning av Exchange-hybrid
Dessa attribut skrivs tillbaka från Azure AD tooon lokala Active Directory när du väljer tooenable **Exchange hybrid**. Beroende på din Exchange-version kan färre attribut synkroniseras.

| Attributets namn | Användare | Kontakt | Grupp | Kommentar |
| --- |:---:|:---:|:---:| --- |
| msDS-ExternalDirectoryObjectID |X | | |Härleds från cloudAnchor i Azure AD. Det här attributet är ny i Exchange 2016 och Windows Server 2016 AD. |
| msExchArchiveStatus |X | | |Online arkivera: Låter kunder tooarchive e-post. |
| msExchBlockedSendersHash |X | | |Filtrering: Skrivs tillbaka lokalt filtrering och online säkert och blockerade sändaren data från klienter. |
| msExchSafeRecipientsHash |X | | |Filtrering: Skrivs tillbaka lokalt filtrering och online säkert och blockerade sändaren data från klienter. |
| msExchSafeSendersHash |X | | |Filtrering: Skrivs tillbaka lokalt filtrering och online säkert och blockerade sändaren data från klienter. |
| msExchUCVoiceMailSettings |X | | |Aktivera Unified Messaging (UM) – Online röstmeddelanden: används av Microsoft Lync Server integrering tooindicate tooLync Server lokalt hello användaren har röstmeddelanden i online-tjänster. |
| msExchUserHoldPolicies |X | | |Tvister Hold: Gör det möjligt för cloud services toodetermine som användare under tvister håller. |
| proxyAddresses |X |X |X |Endast hello x500 adressen från Exchange Online infogas. |
| publicDelegates |X | | |Gör en Exchange Online-postlåda toobe beviljas SendOnBehalfTo rättigheter toousers med lokala Exchange-postlåda. Kräver Azure AD Connect build 1.1.552.0 eller efter. |

## <a name="exchange-mail-public-folder"></a>Offentlig mapp för Exchange-e-post
Dessa attribut synkroniseras från lokala Active Directory tooAzure AD när du väljer tooenable **offentlig mapp för Exchange-e-post**.

| Attributets namn | PublicFolder | Kommentar |
| --- | :---:| --- |
| Visningsnamn | X |  |
| E-post | X |  |
| msExchRecipientTypeDetails | X |  |
| objectGUID | X |  |
| proxyAddresses | X |  |
| targetAddress | X |  |

## <a name="device-writeback"></a>Tillbakaskrivning av enheter
Enhetsobjekt skapas i Active Directory. De här objekten kan vara enheter anslutna tooAzure AD eller domänanslutna Windows 10-datorer.

| Attributets namn | Enhet | Kommentar |
| --- |:---:| --- |
| altSecurityIdentities |X | |
| Visningsnamn |X | |
| DN |X | |
| msDS-CloudAnchor |X | |
| msDS-DeviceID |X | |
| msDS-DeviceObjectVersion |X | |
| msDS-DeviceOSType |X | |
| msDS-DeviceOSVersion |X | |
| msDS-DevicePhysicalIDs |X | |
| msDS-KeyCredentialLink |X |Endast med Windows Server 2016 AD-schema |
| msDS-IsCompliant |X | |
| msDS-IsEnabled |X | |
| msDS-IsManaged |X | |
| msDS-RegisteredOwner |X | |

## <a name="notes"></a>Anteckningar
* När du använder ett alternativt ID hello lokalt är attributet userPrincipalName synkroniserad med hello Azure AD-attributet onPremisesUserPrincipalName. Hej Alternativt ID-attribut, till exempel e-post, synkroniseras med hello Azure AD-attributet userPrincipalName.
* Hej objekttypen i hello listor ovan, **användare** gäller även toohello objekttyp **iNetOrgPerson**.

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md) konfiguration.

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
