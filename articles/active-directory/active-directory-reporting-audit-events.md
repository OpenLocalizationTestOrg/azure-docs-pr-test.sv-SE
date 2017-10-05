---
title: "Azure Active Directory-granskningsrapporthändelser | Microsoft Docs"
description: "Granskade händelser som är tillgängliga för att visa och ladda ned från Azure Active Directory"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 307eedf7-05bc-448d-a84d-bead5a4c5770
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 1620d917acf5a2c36767b5b03750c405f3631ee2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory granska rapporthändelser
*Den här dokumentationen är en del av [Azure Active Directory Reporting-guiden](active-directory-reporting-guide.md).*

Azure Active Directory granska rapporten hjälper kunder att identifiera Privilegierade åtgärder som uppstått i sina Azure Active Directory. Privilegierade åtgärder omfattar höjning ändringar (till exempel skapa användarrollen eller återställning av lösenord), ändra principkonfigurationer (till exempel lösenordsprinciper) eller ändringar av katalogkonfiguration (till exempel ändringar i federation domäninställningar). Med rapporterna får granskningsposten för händelsenamnet aktören som utförde åtgärden målresursen påverkas av ändringen, samt datum och tid (UTC). Kunder kan hämta listan över granskningshändelser för sina Azure Active Directory via den [Azure Portal](https://portal.azure.com/), enligt beskrivningen i [visa granskningsloggarna](active-directory-reporting-azure-portal.md).

## <a name="list-of-audit-report-events"></a>Lista över Granskningsrapporthändelser
<!--- audit event descriptions should be in the past tense --->

| Händelser | Händelsebeskrivning |
| --- | --- |
| **Användarhändelser** | |
| Lägg till användare |Lägga till en användare i katalogen. |
| Ta bort användare |Ta bort en användare i katalogen. |
| Ange egenskaper för licens |Ange licens-egenskaperna för en användare i katalogen. |
| Återställa användarlösenord |Återställa lösenordet för en användare i katalogen. |
| Ändra användarens lösenord |Ändra lösenordet för en användare i katalogen. |
| Ändra användarlicens |Ändra licens till en användare i katalogen. För att se vilka licenser har uppdaterats, visar den [uppdatering användaren](#update-user-attributes) egenskaper nedan |
| Uppdatera användare |Uppdatera en användare i katalogen. [Se nedan](#update-user-attributes) för attribut som inte kan uppdateras. |
| Ange kraft ändra användarens lösenord |Egenskapen som gör att en användare kan ändra sina lösenord vid inloggningen. |
| Uppdatera autentiseringsuppgifterna |Användaren har ändrat lösenordet |
| **Gruppera händelser** | |
| Lägg till grupp |Skapa en grupp i katalogen. |
| Uppdateringsgrupp |Uppdatera en grupp i katalogen. Om du vill se vilka egenskaper har uppdaterats, referera till [grupp egenskaper granskas](#update-group-attributes) i avsnittet nedan |
| Ta bort grupp |Ta bort en grupp från katalogen. |
| CreateGroupSettings |Skapade gruppinställningar |
| UpdateGroupSettings |Uppdatera inställningarna. Om du vill se vilka inställningarna har uppdaterats, referera till [grupp egenskaper granskas](#update-group-attributes) i avsnittet nedan |
| DeleteGroupSettings |Borttagen gruppinställningar |
| SetGroupLicense |Ange gruppen licens. |
| SetGroupManagedBy |Ange grupp som ska hanteras av användare |
| AddGroupMember |Lade till medlem i gruppen |
| RemoveGroupMember |Ta bort medlemmen från gruppen |
| AddGroupOwner |Tillagda ägare till grupp |
| RemoveGroupOwner |Borttagna ägare av gruppen |
| **Programhändelser** | |
| Lägga till tjänstens huvudnamn |Lägga till ett huvudnamn för tjänsten till katalogen. |
| Ta bort tjänstens huvudnamn |Ta bort ett huvudnamn för tjänsten från katalogen. |
| Lägga till tjänstens huvudnamn autentiseringsuppgifter |Tillagd referenserna till ett huvudnamn för tjänsten. |
| Ta bort service principal autentiseringsuppgifter |Borttagna autentiseringsuppgifter från en tjänstens huvudnamn. |
| Lägg till delegeringspost |Skapa en [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) i katalogen. |
| Delegering transaktion |Uppdatera en [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) i katalogen. |
| Ta bort posten för delegering |Ta bort en [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) i katalogen. |
| **Rollhändelser** | |
| Lägg till Rollmedlem i rollen |Lägga till en användare till en roll i katalogen. |
| Ta bort Rollmedlem i rollen |Ta bort en användare från en roll i katalogen. |
| AddRoleDefinition |Tillagda rolldefinitionen. |
| UpdateRoleDefinition |Uppdaterade rolldefinitionen. Om du vill se vilka rollinställningar som har uppdaterats, referera till [rollen Definition egenskaper granskas](#update-role-definition-attributes) i avsnittet nedan |
| DeleteRoleDefinition |Ta bort rolldefinitionen. |
| AddRoleAssignmentToRoleDefinition |Tillagda rolltilldelning rolldefinitionen. |
| RemoveRoleAssignmentFromRoleDefinition |Borttagna rolltilldelningen från rolldefinitionen. |
| AddRoleFromTemplate |Tillagda roll från mallen. |
| UpdateRole |Uppdaterade roll. |
| AddRoleScopeMemberToRole |Tillagda begränsade medlem i rollen. |
| RemoveRoleScopedMemberFromRole |Ta bort begränsade medlem från rollen. |
| **Enhetshändelser (alla nya händelser)** | |
| AddDevice |Tillagd enhet. |
| UpdateDevice |Uppdaterade enhet. Om du vill se vilka egenskaper för enhet har uppdaterats, referera till [enhetsegenskaper Audited](#update-device-attributes) i avsnittet nedan |
| DeleteDevice |Borttagna enhet. |
| AddDeviceConfiguration |Tillagda enhetskonfigurationen. |
| UpdateDeviceConfiguration |Uppdaterade enhetskonfigurationen. Om du vill se vilka enhetsegenskaper konfiguration har uppdaterats, referera till [enhetsegenskaper configuration Audited](#update-device-configuration-attributes) i avsnittet nedan |
| DeleteDeviceConfiguration |Borttagna enhetskonfigurationen. |
| AddRegisteredOwner |Tillagda registrerad ägare till enheten. |
| AddRegisteredUsers |Lägga till registrerade användare till enheten. |
| RemoveRegisteredOwner |Ta bort registrerade ägaren från enheten. |
| RemoveRegisteredUsers |Ta bort registrerade användare från enheten. |
| RemoveDeviceCredentials |Ta bort enheten autentiseringsuppgifter. |
| **B2B-händelser** | |
| Inbjudan har skickats ut batch upp. |En administratör har överfört en fil som innehåller inbjudningar som ska skickas till partneranvändarna. |
| Batch inbjudan har skickats ut bearbetas. |En fil som innehåller inbjudningar till partneranvändarna har bearbetats. |
| Bjud in externa användare. |En extern användare har bjudits in till katalogen. |
| Lösa externa användare inbjudan. |En extern användare har lösts in sina inbjudan till katalogen. |
| Lägga till externa användare till gruppen. |En extern användare har tilldelats medlemskap i en grupp i katalogen. |
| Tilldela externa användare till programmet. |En extern användare har tilldelats direkt åtkomst till ett program. |
| Viral klient skapas. |En ny klient har skapats i Azure AD med inbjudan-åtgärd. |
| Skapa viral användare. |En användare har skapats i en befintlig klient i Azure AD med inbjudan-åtgärd. |
| **Administrativa enheter (alla nya händelser)** | |
| AddAdministrativeUnit |Lägg till administrativa enhet. |
| UpdateAdministrativeUnit |Uppdatera administrativa enhet. Om du vill se vilka administrativa enhet egenskaper har uppdaterats, referera till [enhet Administrationsegenskaper granskas](#update-administrative-unit-attributes) i avsnittet nedan |
| DeleteAdministrativeUnit |Ta bort administrativa enhet. |
| AddMemberToAdministrativeUnit |Lägga till medlem i administrativa enhet. |
| RemoveMemberFromAdministrativeUnit |Ta bort medlemmen från administrativa enhet. |
| **Directory händelser** | |
| Lägg till partner i företaget |Lägga till en partner till katalogen. |
| Ta bort Partner från företaget |Ta bort en partner från katalogen. |
| DemotePartner |Degradera partner. |
| Lägg till domän för företag |Lägga till en domän i katalogen. |
| Ta bort domänen från företaget |Ta bort en domän i katalogen. |
| Uppdatera domänen |Uppdatera en domän i katalogen. Om du vill se vilka egenskaper för domän har uppdaterats, referera till [egenskaper för domän granskas](#update-domain-attributes) i avsnittet nedan |
| Ange domänautentisering |Ändra standardinställningen för domän för företaget. |
| Ange företagets kontaktuppgifter |Ange företagsnivån kontaktinställningar. Detta inkluderar e-postadresser för marknadsföring, samt tekniska meddelanden om Microsoft Online Services. |
| Ange inställningar på domänen för federation |Uppdatera federation-inställningarna för en domän. |
| Verifiera domän |Verifiera en domän i katalogen. |
| Kontrollera e-verifierad domän |Verifiera en domän i katalogen med e-verifiering. |
| Flaggan DirSyncEnabled på företaget |Egenskapen som aktiverar en katalog för Azure AD Sync. |
| Ange lösenordsprincip |Ange villkor för längd och tecken för lösenord. |
| Ange företagsinformation |Uppdatera informationen om företagsnivån. Finns det [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) PowerShell-cmdleten för mer information. |
| SetCompanyAllowedDataLocation |Ange företagets tillåtna Dataplats. |
| SetCompanyDirSyncEnabled |Flaggan DirSyncEnabled. |
| SetCompanyDirSyncFeature |Ange DirSync-funktionen. |
| SetCompanyInformation |Ange företagsinformation. |
| SetCompanyMultiNationalEnabled |Ange företagets flerspråkig funktionen aktiverad. |
| SetDirectoryFeatureOnTenant |Ange directory funktionen innehavaren. |
| SetTenantLicenseProperties |Ange egenskaper för klient-licens. |
| CreateCompanySettings |Skapa Företagsinställningar |
| UpdateCompanySettings |Uppdatera Företagsinställningar. Om du vill se vilka egenskaper du företagets har uppdaterats, referera till [företagets egenskaper granskas](#update-company-attributes) i avsnittet nedan |
| DeleteCompanySettings |Ta bort Företagsinställningar |
| SetAccidentalDeletionThreshold |Ange tröskelvärdet för oavsiktlig borttagning. |
| SetRightsManagementProperties |Ange rights management-egenskaper. |
| PurgeRightsManagementProperties |Rensa egenskaper för rights management. |
| UpdateExternalSecrets |Uppdatera externa hemligheter |
| **Principhändelser (alla nya händelser)** | |
| AddPolicy |Lägg till princip. |
| UpdatePolicy |Uppdatera princip. |
| Ta bort princip |Ta bort principen. |
| AddDefaultPolicyApplication |Lägg till princip för program. |
| AddDefaultPolicyServicePrincipal |Lägg till princip till tjänstens huvudnamn. |
| RemoveDefaultPolicyApplication |Ta bort principen från programmet. |
| RemoveDefaultPolicyServicePrincipal |Ta bort principen från tjänstens huvudnamn. |
| RemovePolicyCredentials |Ta bort princip autentiseringsuppgifter. |

## <a name="audit-report-retention"></a>Granska rapporten kvarhållning

Den senaste informationen om kvarhållning finns [rapportkvarhållningsregler i Azure Active Directory](active-directory-reporting-retention.md).


För kunder som vill lagra sina granskningshändelser under längre bevarandeperioder, användas Reporting API för att hämta regelbundet granskningshändelser till ett separat datalager. Se [komma igång med Reporting-API](active-directory-reporting-api-getting-started.md) mer information.

## <a name="properties-included-with-each-audit-event"></a>Egenskaper som ingår i varje händelse
| Egenskap | Beskrivning |
| --- | --- |
| Datum och tid |Datum och tid då audit händelsen inträffade |
| aktören |Användaren eller tjänstens huvudnamn som utförde åtgärden |
| Åtgärd |Den åtgärd som utfördes |
| mål |Användaren eller tjänstens huvudnamn som åtgärden utfördes på |

## <a name="update-user-attributes"></a>”Uppdatera användare” attribut
Granskningshändelse ”uppdatering användare” innehåller ytterligare information om vilka användarattribut har uppdaterats. För varje attribut ingår både det tidigare värdet och det nya värdet.

| Attribut | Beskrivning |
| --- | --- |
| accountEnabled |Användaren kan logga in. |
| AssignedLicense |Alla licenser som har tilldelats användaren. |
| AssignedPlan |Tjänsten planer som uppstår till följd av licenser som tilldelats användaren. |
| LicenseAssignmentDetail |Information om licenser som tilldelats användaren. Till exempel skulle gruppbaserade licensiering var inblandad detta inkludera gruppen som beviljats licensen. |
| Mobil |Användarens mobiltelefon. |
| OtherMail |Användarens alternativa e-postadress. |
| otherMobile |Användarens alternativa mobiltelefon. |
| StrongAuthenticationMethod |En lista över verifieringsmetoderna som konfigurerats av användaren för Multifaktorautentisering, till exempel röst anropa, SMS eller verifiering kod från en mobil app. |
| StrongAuthenticationRequirement |Om Multifaktorautentisering har framtvingats aktiveras eller inaktiveras för den här användaren. |
| StrongAuthenticationUserDetails |Användarens telefonnummer alternativa telefonnummer och e-postadress används för verifiering för Multifaktorautentisering och lösenord för återställning. |
| StrongAuthenticationPhoneAppDetail |Information om forof phone-appar som registrerats för att utföra 2FA inloggning |
| telephoneNumber |Användarens telefonnummer. |
| AlternativeSecurityId |Ett alternativt säkerhets-ID för objektet. |
| CreationType |Metod för skapande av användaren (antingen av inbjudan eller viral). |
| InviteTicket |Lista över inbjudan biljetter för användaren. |
| InviteReplyUrl |Lista över URL: er att svara på inbjudan godkännande. |
| InviteResources |Lista över resurser som användaren har bjudits in. |
| LastDirSyncTime |Objektet senast uppdaterades på grund av synkroniserar från den auktoritativa (kund, lokalt) directory. |
| MSExchRemoteRecipientType |Mappas till MS mottagande typer. Referera till [MS mottagande typer] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx för mottagande typer |
| PreferredDataLocation |Önskad plats för användare, grupp, kontaktens, offentlig mapp eller enhetens data. |
| ProxyAddresses |Adressen som ett mottagande Exchange Server-objekt kan identifieras i en främmande e-postsystemet. |
| StsRefreshTokensValidFrom |Utför uppdatera token återkallningsinformation - några STS uppdaterings-tokens som utfärdats innan den här gången betraktas som upphört att gälla. |
| UserPrincipalName |UPN-namnet som är ett inloggningsnamn för Internet-format för en användare. |
| UserState |Status för användaren väntar på godkännande/PendingAcceptance/accepteras/PendingVerification. |
| UserStateChangedOn |Tidsstämpel för senaste ändring UserState. Används för att utlösa livscykel arbetsflöden. |
| UserType |Typ av användare. Medlem (0), Gäst (1), Viral(2). |

## <a name="update-group-attributes"></a>”Uppdateringsgrupp”-attribut
| Attribut | Beskrivning |
| --- | --- |
| Klassificering |Klassificeringen för en enhetlig grupp (HBI, MBI osv.). |
| Beskrivning |Läsbart beskrivande fraser om objektet. |
| Visningsnamn |Visningsnamnet för ett objekt |
| DirSyncEnabled |Anger om synkronisering sker från en auktoritativ (kund, lokalt) directory. |
| GroupLicenseAssignment |Licenstilldelning i en grupp |
| groupType |Typ av grupp, enhetlig (0) |
| IsMembershipRuleLocked |Anger att egenskapen MembershipRule anges av självbetjäning management-tjänsten och kan inte ändras av användare. Gäller endast för grupper där GroupType innehåller GroupType.DynamicMembership |
| IsPublic |Flagga som indikerar om gruppen är offentliga och privata. |
| LastDirSyncTime |Objektet senast uppdaterades till följd av synkroniserar från den auktoritativa (lokalt kund) directory. |
| Post |Primär e-postadress |
| MailEnabled |Anger om den här gruppen har e-funktionen. |
| mailNickname |Moniker för ett adress book objekt, vanligtvis en del av dess föregående e-postnamn i ”@” symbol. |
| MembershipRule |En sträng som anger de kriterier som används av självbetjäning management-tjänsten för att avgöra vilka medlemmar som ska tillhöra den här gruppen. Se även IsMembershipRuleLocked. Gäller endast för grupper där GroupType innehåller GroupType.DynamicMembership. |
| MembershipRuleProcessingState |Ett uppräkningsvärde som definieras av hanteringstjänsten självbetjäning definierar status för medlemskap för den här gruppen bearbetades. Gäller endast för grupper där GroupType innehåller GroupType.DynamicMembership. |
| ProxyAddresses |Adressen som ett mottagande Exchange Server-objekt kan identifieras i en främmande e-postsystemet. |
| RenewedDateTime |Tidsstämpel logg över när en grupp senast förnyas. |
| securityEnabled |Anger om medlemskap i gruppen kan påverka auktoriseringsbeslut. |
| WellKnownObject |Märker ett katalogobjekt ange som en av de fördefinierade. |

## <a name="update-device-attributes"></a>”Uppdatera enheten” attribut
| Attribut | Beskrivning |
| --- | --- |
| accountEnabled |Anger om ett säkerhetsobjekt kan autentisera. |
| CloudAccountEnabled |Anger om ett säkerhetsobjekt kan autentisera. Skrivs av InTune när enheten hanteras lokalt. |
| CloudDeviceOSType |Typ av enheten baserat på enhetens operativsystem t.ex. Windows RT, iOS. Ange när en tjänst i molnet (till exempel Intune) måste blir det här attributet auktoritära i katalogen för DeviceOSType. |
| CloudDeviceOSVersion |Versionen av Operativsystemet. Ange när en tjänst i molnet (till exempel Intune) måste blir det här attributet auktoritära i katalogen för DeviceOSVersion. |
| CloudDisplayName |Värdet för displayName LDAP-attribut. Ange när en tjänst i molnet (till exempel Intune) måste blir det här attributet auktoritära i katalogen för displayName. |
| CloudCreated |Anger om objektet har skapats av molntjänster. |
| CompliantUntil |Vilka tills enheten anses kompatibla. |
| DeviceMetadata |Anpassade metadata för enheten |
| DeviceObjectVersion |Det här attributet används för att identifiera schemaversionen för enheten. |
| DeviceOSType |Typ av enheten baserat på enhetens operativsystem t.ex. Windows RT, iOS. Skrivs av registreringstjänsten och ska uppdateras av MDM ljus management-tjänsten eller STS management-tjänsten. |
| DeviceOSVersion |Versionen av Operativsystemet. Skrivs av registreringstjänsten och ska uppdateras av MDM ljus management-tjänsten eller STS management-tjänsten. |
| DevicePhysicalIds |Flervärdesattribut ska lagra identifierare för den fysiska enheten. Detta kan inkludera BIOS-ID, TPM-tumavtryck, maskinvara specifikt ID: N, osv. |
| DirSyncEnabled |Anger om synkronisering sker från en auktoritativ (kund, lokalt) directory. |
| Visningsnamn |Visningsnamnet för ett objekt |
| IsCompliant |Det här attributet används för att hantera status för hantering av mobila enheter för enheten. |
| IsManaged |Det här attributet används för att ange enheten hanteras av ett moln MDM. |
| LastDirSyncTime |Objektet senast uppdaterades på grund av synkroniserar från den auktoritativa (lokalt kund) directory. |

## <a name="update-device-configuration-attributes"></a>”Uppdatera enhetskonfigurationen” attribut
| Attribut | Beskrivning |
| --- | --- |
| MaximumRegistrationInactivityPeriod |Det maximala antalet dagar som en enhet kan vara inaktiv innan den anses för borttagning. |
| RegistrationQuota |Principen används för att begränsa antalet enheter registreringar tillåts för en enskild användare. |

## <a name="update-service-principal-configuration-attributes"></a>”Uppdatera tjänstens huvudnamn konfiguration” attribut
| Attribut | Beskrivning |
| --- | --- |
| accountEnabled |Anger om ett säkerhetsobjekt kan autentisera. |
| AppPrincipalId |Externa, programdefinierade identitet för ett säkerhetsobjekt. |
| Visningsnamn |Visningsnamnet för ett objekt |
| ServicePrincipalName |Tjänstens huvudnamn, som innehåller ”namn/myndighet” där namn anger ett värde för programmet klass och utfärdare innehåller minst värdnamn [: port] eller ”name” som anger en identifierare för tjänstens huvudnamn. |

## <a name="update-app-attributes"></a>”Uppdatera App” attribut
| Attribut | Beskrivning |
| --- | --- |
| AppAddress |Uppsättningen adresser (omdirigera URL: er) som har tilldelats ett huvudnamn för tjänsten. |
| appId |Program-ID för appen |
| AppIdentifierUri |Programmet URI som identifierar programmet.  Det är vanligtvis åtkomst-URL för programmet. |
| AppLogoUrl |Url för programmet logotyp som lagras i en CDN. |
| AvailableToOtherTenants |True programmet är flera innehavare (dvs. kan användas av andra klienter). |
| Visningsnamn |Visningsnamnet för ett programnamn |
| Rätt |Lista över program rättigheter. |
| ExternalUserAccountDelegationsAllowed |Flagga som anger om resursprogram är en betrodd och kan skapa delegering poster för externa användarkonton. |
| GroupMembershipClaims |Medlemskap i anspråk princip. |
| PublicClient |TRUE om klienten inte kan hålla hemliga (d.v.s. icke-konfidentiella klient i OAuth2.0) |
| RecordConsentConditions |Typer av medgivande villkor, som definieras av avtalsvillkor: Ingen (0), SilentConsentForPartnerManagedApp(1). Det här värdet kommer att exponeras i Graph API-schemat och set eller har ändrats av innehavaradministratörer kan bara vara. |
| RequiredResourceAccess |XML-innehåll för ett värde för egenskapen RequiredResourceAccess. |
| WebApp |Om värdet är true anger du att det här programmet är ett webbprogram. |
| WwwHomepage |Den primära webbsidan. |

## <a name="update-role-attributes"></a>”Uppdatera roll” attribut
| Attribut | Beskrivning |
| --- | --- |
| AppAddress |Uppsättningen adresser (omdirigera URL: er) som har tilldelats ett huvudnamn för tjänsten. |
| BelongsToFirstLoginObjectSet |Om värdet är true anger att det här objektet tillhör en uppsättning objekt som krävs för att aktivera inloggningen för den första administratören av en ny klient. |
| Inbyggd |Anger om livslängden för ett objekt som ägs av systemet. |
| Beskrivning |Läsbart beskrivande fraser om objektet. |
| Visningsnamn |Visningsnamnet för ett objekt |
| mailNickname |Moniker för ett adress book objekt, vanligtvis en del av dess föregående e-postnamn i ”@” symbol. |
| RoleDisabled |Anger om rollen ska ignoreras för åtkomst till kontroller. |
| RoleTemplateId |Identitet för rollmall. |
| ServiceInfo |Tjänstspecifika Etableringsinformation som kan användas av MOAC och/eller andra instanser av tjänsten (samma eller olika typer av tjänster). |
| TaskSetScopeReference |Identifierar en TaskSet och en uppsättning scope som är kopplade till en roll eller RoleTemplate. |
| ValidationError |Information som publicerats av en federerad tjänst som beskriver en inte är tillfällig, tjänstspecifikt fel om de egenskaper eller en länk från en händelse för objektet administratören att lösa. |
| WellKnownObject |Märker ett katalogobjekt ange som en av de fördefinierade. |

## <a name="update-role-definition-attributes"></a>”Uppdatera rolldefinitionen” attribut
| Attribut | Beskrivning |
| --- | --- |
| AssignableScopes |Samling av auktorisering scope som kan refereras när du tilldelar den här RoleDefinition ett säkerhetsobjekt. |
| Visningsnamn |Visningsnamnet för ett objekt |
| GrantedPermissions |Behörigheter som utfärdats av den här RoleDefinition. |

## <a name="update-administrative-unit-attributes"></a>”Uppdatera administrativa enhet” attribut
| Attribut | Beskrivning |
| --- | --- |
| Beskrivning |Den här egenskapen uppdateras när du ändrar beskrivningen av en administrativ enhet. |
| Visningsnamn |Den här egenskapen uppdateras när du ändrar namnet på en administrativ enhet. |

## <a name="update-company-attributes"></a>”Uppdatera företagets” attribut
| Attribut | Beskrivning |
| --- | --- |
| AllowedDataLocation |En plats där företagets användare ska kunna etableras. |
| AuthorizedServiceInstance |Namnen på instanser av tjänsten som en plan kan distribueras. |
| DirSyncEnabled |Anger om synkronisering sker från en auktoritativ (kund, lokalt) directory. |
| DirSyncStatus |Anger om synkronisering av adress book objekt i den här kontexten klient från en auktoritativ (kund, lokalt) katalog. en utökning av egenskapen DirSyncEnabled för företag-objekt. |
| DirSyncFeatures |Flaggan att hålla reda på uppsättning aktiverade och inaktiverade dirsync funktioner för klienten. |
| DirectoryFeatures |Aktiverat/inaktiverat directory-funktioner. |
| DirSyncConfiguration |Innehåller alla DirSync-konfigurationen för den aktuella klienten. |
| Visningsnamn |Visningsnamnet för ett objekt |
| IsMnc |En boolesk flagga värdet ”true” iff som företaget är aktiverad för funktionen flerspråkig företag. |
| ObjectSettings |En samling inställningar som gäller för omfånget för objektet. |
| PartnerCommerceUrl |URL till den Partner commerce plats. |
| PartnerHelpUrl |URL till platsen för partnerns hjälp. |
| PartnerSupportEmail |URL till den Partner stöd för e-post. |
| PartnerSupportTelephone |URL till den Partner stöd telefon. |
| PartnerSupportUrl |URL till supportwebbplats partnerns. |
| StrongAuthenticationDetails |Information som rör StrongAuthentication. |
| StrongAuthenticationPolicy |Stark autentiseringsprincip för företaget. |
| TechnicalNotificationMail |E-postadress för att meddela tekniska problem som rör ett företag. |
| telephoneNumber |Telefonnummer som är kompatibla med ITU rekommendation E.123. |
| TenantType |Typ av en klient. Om det här värdet anges är klienten ett företag. Annars möjliga värden är: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5). |
| VerifiedDomain |En uppsättning av DNS-domännamn som är bunden till ett företag. |

## <a name="update-domain-attributes"></a>”Uppdatera domän” attribut
| Attribut | Beskrivning |
| --- | --- |
| Funktioner |Bit flaggor som beskriver funktionerna i domänen, om sådana finns. |
| Standard |Anger om domänen är standardvärdet; till exempel standard UserPrincipalName suffixet när en administratör skapar en ny användare i MOAC. |
| Inledande |Anger om domänen är den första domänen för företaget som har tilldelats av OCP. Den första domänen är en unik underordnade domän till en Microsoft Online-domän. e.g.contoso3.microsoftonline.com. |
| LiveType |Typ av motsvarande Windows Live namnområdet, om sådana finns. |
| Namn |Identifierare för slutpunkten. |
| PasswordNotificationWindowDays |Antalet dagar innan ett lösenord upphör att gälla användaren meddelas. |
| PasswordValidityPeriodDays |Antal dagar som ett lösenord är bra för innan måste ändras. |

Granskningsloggen är en kontroll som krävs för många regler om överensstämmelse. För kunder som använder Azure Active Directory granska rapporten för att uppfylla sina förordningar, rekommenderas det att kunden skicka en kopia av det här avsnittet i kopian av kundens exporterade kontrollrapport för att förklara anger rapportinformationen. Om granskaren vill förstå förordningar som för närvarande uppfyller Azure direkt granskare till den [kompatibilitet sidan](https://azure.microsoft.com/support/trust-center/compliance/) av Microsoft Azure Trust Center.

