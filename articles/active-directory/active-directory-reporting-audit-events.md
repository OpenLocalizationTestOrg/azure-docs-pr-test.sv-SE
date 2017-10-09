---
title: "aaaAzure Active Directory-granskningsrapporthändelser | Microsoft Docs"
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
ms.openlocfilehash: 4a84cce2be56bde006164abf10ad1e72ca6e99bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-report-events"></a>Azure Active Directory granska rapporthändelser
*Den här dokumentationen är en del av hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

hello Azure Active Directory-kontrollrapport hjälper kunder att identifiera Privilegierade åtgärder som uppstått i sina Azure Active Directory. Privilegierade åtgärder omfattar höjning ändringar (till exempel skapa användarrollen eller återställning av lösenord), ändra principkonfigurationer (till exempel lösenordsprinciper) eller ändringar toodirectory konfigurationen (till exempel ändrar toodomain federation inställningar). hello-rapporter tillhandahåller hello Granskningspost för hello händelsenamnet, hello aktören som utförde hello åtgärd, hello målresurs påverkas av hello ändringen och hello datum och tid (i UTC). Kunder är kan tooretrieve hello lista över granskningshändelser för sina Azure Active Directory via hello [Azure Portal](https://portal.azure.com/), enligt beskrivningen i [visa granskningsloggarna](active-directory-reporting-azure-portal.md).

## <a name="list-of-audit-report-events"></a>Lista över Granskningsrapporthändelser
<!--- audit event descriptions should be in hello past tense --->

| Händelser | Händelsebeskrivning |
| --- | --- |
| **Användarhändelser** | |
| Lägg till användare |Lägga till en användarens toohello katalog. |
| Ta bort användare |Ta bort en användare från hello directory. |
| Ange egenskaper för licens |Ange hello licens egenskaperna för en användare i hello directory. |
| Återställa användarlösenord |Återställa hello lösenord för en användare i hello directory. |
| Ändra användarens lösenord |Ändra hello lösenordet för en användare i hello directory. |
| Ändra användarlicens |Ändra hello tooa användare i katalogen för hello-licens. vilka licenser har uppdaterats, titta på hello toosee [uppdatering användaren](#update-user-attributes) egenskaper nedan |
| Uppdatera användare |Uppdatera en användare i hello directory. [Se nedan](#update-user-attributes) för attribut som inte kan uppdateras. |
| Ange kraft ändra användarens lösenord |Egenskapen hello som tvingar en användare toochange sitt lösenord vid inloggningen. |
| Uppdatera autentiseringsuppgifterna |Användare ändrade hello lösenord |
| **Gruppera händelser** | |
| Lägg till grupp |Skapa en grupp i hello directory. |
| Uppdateringsgrupp |Uppdatera en grupp i hello directory. toosee vilka gruppegenskaper uppdaterades finns för[grupp egenskaper granskas](#update-group-attributes) i hello avsnitt |
| Ta bort grupp |Ta bort en grupp från hello directory. |
| CreateGroupSettings |Skapade gruppinställningar |
| UpdateGroupSettings |Uppdatera inställningarna. toosee vilka gruppinställningar uppdaterades finns för[grupp egenskaper granskas](#update-group-attributes) i hello avsnitt |
| DeleteGroupSettings |Borttagen gruppinställningar |
| SetGroupLicense |Ange gruppen licens. |
| SetGroupManagedBy |Ange grupp toobe hanteras av användaren |
| AddGroupMember |Lade till medlem toogroup |
| RemoveGroupMember |Ta bort medlemmen från gruppen |
| AddGroupOwner |Tillagda ägare toogroup |
| RemoveGroupOwner |Borttagna ägare av gruppen |
| **Programhändelser** | |
| Lägga till tjänstens huvudnamn |Lägga till en service principal toohello katalog. |
| Ta bort tjänstens huvudnamn |Ta bort ett huvudnamn för tjänsten från hello directory. |
| Lägga till tjänstens huvudnamn autentiseringsuppgifter |Tillagd referenserna tooa tjänstens huvudnamn. |
| Ta bort service principal autentiseringsuppgifter |Borttagna autentiseringsuppgifter från en tjänstens huvudnamn. |
| Lägg till delegeringspost |Skapa en [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) i hello directory. |
| Delegering transaktion |Uppdatera en [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) i hello directory. |
| Ta bort posten för delegering |Ta bort en [OAuth2PermissionGrant](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permissiongrant-entity) i hello directory. |
| **Rollhändelser** | |
| Lägg till rollen medlem tooRole |Lägga till en användarroll tooa directory. |
| Ta bort Rollmedlem i rollen |Ta bort en användare från en roll i katalogen. |
| AddRoleDefinition |Tillagda rolldefinitionen. |
| UpdateRoleDefinition |Uppdaterade rolldefinitionen. toosee vilka rollinställningar uppdaterades finns för[rollen Definition egenskaper granskas](#update-role-definition-attributes) i hello avsnitt |
| DeleteRoleDefinition |Ta bort rolldefinitionen. |
| AddRoleAssignmentToRoleDefinition |Lägga till rollen tilldelning toorole definition. |
| RemoveRoleAssignmentFromRoleDefinition |Borttagna rolltilldelningen från rolldefinitionen. |
| AddRoleFromTemplate |Tillagda roll från mallen. |
| UpdateRole |Uppdaterade roll. |
| AddRoleScopeMemberToRole |Lägga till begränsade medlem toorole. |
| RemoveRoleScopedMemberFromRole |Ta bort begränsade medlem från rollen. |
| **Enhetshändelser (alla nya händelser)** | |
| AddDevice |Tillagd enhet. |
| UpdateDevice |Uppdaterade enhet. toosee vilka enhetsegenskaper uppdaterades finns för[enhetsegenskaper Audited](#update-device-attributes) i hello avsnitt |
| DeleteDevice |Borttagna enhet. |
| AddDeviceConfiguration |Tillagda enhetskonfigurationen. |
| UpdateDeviceConfiguration |Uppdaterade enhetskonfigurationen. toosee vilka enhetsegenskaper konfiguration uppdaterades finns för[enhetsegenskaper configuration Audited](#update-device-configuration-attributes) i hello avsnitt |
| DeleteDeviceConfiguration |Borttagna enhetskonfigurationen. |
| AddRegisteredOwner |Lägga till registrerad ägare toodevice. |
| AddRegisteredUsers |Lägga till registrerade användare toodevice. |
| RemoveRegisteredOwner |Ta bort registrerade ägaren från enheten. |
| RemoveRegisteredUsers |Ta bort registrerade användare från enheten. |
| RemoveDeviceCredentials |Ta bort enheten autentiseringsuppgifter. |
| **B2B-händelser** | |
| Inbjudan har skickats ut batch upp. |En administratör har överfört en fil som innehåller inbjudningar toobe skickas toopartner användare. |
| Batch inbjudan har skickats ut bearbetas. |En fil som innehåller inbjudningar toopartner användare har bearbetats. |
| Bjud in externa användare. |En extern användare har inbjudna toohello directory. |
| Lösa externa användare inbjudan. |En extern användare har lösts in sina inbjudan toohello kataloger. |
| Lägg till externa användare toogroup. |En extern användare har tilldelats medlemskap tooa grupp i hello directory. |
| Tilldela tooapplication för externa användare. |En extern användare har tilldelats direktåtkomst tooan program. |
| Viral klient skapas. |En ny klient har skapats i Azure AD med hello inbjudan åtgärd. |
| Skapa viral användare. |En användare har skapats i en befintlig klient i Azure AD med hello inbjudan åtgärd. |
| **Administrativa enheter (alla nya händelser)** | |
| AddAdministrativeUnit |Lägg till administrativa enhet. |
| UpdateAdministrativeUnit |Uppdatera administrativa enhet. toosee vilka administrativa enhet egenskaper uppdaterades finns för[enhet Administrationsegenskaper granskas](#update-administrative-unit-attributes) i hello avsnitt |
| DeleteAdministrativeUnit |Ta bort administrativa enhet. |
| AddMemberToAdministrativeUnit |Lägga till medlem tooadministrative enhet. |
| RemoveMemberFromAdministrativeUnit |Ta bort medlemmen från administrativa enhet. |
| **Directory händelser** | |
| Lägg till partner toocompany |Lägga till en partner toohello katalog. |
| Ta bort Partner från företaget |Ta bort en partner från hello directory. |
| DemotePartner |Degradera partner. |
| Lägg till domän toocompany |Lägga till en domän toohello katalog. |
| Ta bort domänen från företaget |Ta bort en domän från hello directory. |
| Uppdatera domänen |Uppdatera en domän på hello-katalogen. vilka egenskaper uppdaterades toosee finns för[egenskaper för domän granskas](#update-domain-attributes) i hello avsnitt |
| Ange domänautentisering |Ändra hello domän standardinställningen för hello företag. |
| Ange företagets kontaktuppgifter |Ange företagsnivån kontaktinställningar. Detta inkluderar e-postadresser för marknadsföring, samt tekniska meddelanden om Microsoft Online Services. |
| Ange inställningar på domänen för federation |Uppdatera hello federation inställningar för en domän. |
| Verifiera domän |Verifiera en domän på hello-katalogen. |
| Kontrollera e-verifierad domän |Verifiera en domän i hello katalogen med hjälp av e-verifiering. |
| Flaggan DirSyncEnabled på företaget |Egenskapen hello som aktiverar en katalog för Azure AD Sync. |
| Ange lösenordsprincip |Ange villkor för längd och tecken för lösenord. |
| Ange företagsinformation |Uppdaterade hello företagsnivån information. Se hello [Get-MsolCompanyInformation](https://msdn.microsoft.com/library/azure/dn194126.aspx) PowerShell-cmdleten för mer information. |
| SetCompanyAllowedDataLocation |Ange företagets tillåtna Dataplats. |
| SetCompanyDirSyncEnabled |Flaggan DirSyncEnabled. |
| SetCompanyDirSyncFeature |Ange DirSync-funktionen. |
| SetCompanyInformation |Ange företagsinformation. |
| SetCompanyMultiNationalEnabled |Ange företagets flerspråkig funktionen aktiverad. |
| SetDirectoryFeatureOnTenant |Ange directory funktionen innehavaren. |
| SetTenantLicenseProperties |Ange egenskaper för klient-licens. |
| CreateCompanySettings |Skapa Företagsinställningar |
| UpdateCompanySettings |Uppdatera Företagsinställningar. vilka egenskaper du företagets uppdaterades toosee finns för[företagets egenskaper granskas](#update-company-attributes) i hello avsnitt |
| DeleteCompanySettings |Ta bort Företagsinställningar |
| SetAccidentalDeletionThreshold |Ange tröskelvärdet för oavsiktlig borttagning. |
| SetRightsManagementProperties |Ange rights management-egenskaper. |
| PurgeRightsManagementProperties |Rensa egenskaper för rights management. |
| UpdateExternalSecrets |Uppdatera externa hemligheter |
| **Principhändelser (alla nya händelser)** | |
| AddPolicy |Lägg till princip. |
| UpdatePolicy |Uppdatera princip. |
| Ta bort princip |Ta bort principen. |
| AddDefaultPolicyApplication |Lägg till princip tooapplication. |
| AddDefaultPolicyServicePrincipal |Lägg till princip tooservice huvudnamn. |
| RemoveDefaultPolicyApplication |Ta bort principen från programmet. |
| RemoveDefaultPolicyServicePrincipal |Ta bort principen från tjänstens huvudnamn. |
| RemovePolicyCredentials |Ta bort princip autentiseringsuppgifter. |

## <a name="audit-report-retention"></a>Granska rapporten kvarhållning

Hello senaste information om kvarhållning finns [rapportkvarhållningsregler i Azure Active Directory](active-directory-reporting-retention.md).


För kunder som vill lagra sina granskningshändelser under längre bevarandeperioder, kan hello Reporting API vara används tooregularly pull granskningshändelser i ett separat datalager. Se [komma igång med hello Reporting API](active-directory-reporting-api-getting-started.md) mer information.

## <a name="properties-included-with-each-audit-event"></a>Egenskaper som ingår i varje händelse
| Egenskap | Beskrivning |
| --- | --- |
| Datum och tid |hello datum och tid som hello granskningshändelse uppstod |
| aktören |hello användar- eller tjänstens huvudnamn som utförde åtgärden hello |
| Åtgärd |hello-åtgärd som utfördes |
| mål |hello användar- eller tjänstens huvudnamn som hello åtgärd utfördes på |

## <a name="update-user-attributes"></a>”Uppdatera användare” attribut
Hej ”uppdatering användare” granskningshändelse innehåller ytterligare information om vilka användarattribut har uppdaterats. För varje attribut både hello tidigare värde och nytt värde för hello ingår.

| Attribut | Beskrivning |
| --- | --- |
| accountEnabled |hello användaren kan logga in. |
| AssignedLicense |Alla licenser som har tilldelats toohello användare. |
| AssignedPlan |Serviceplaner som uppstår till följd av hello licenser tilldelad toohello användare. |
| LicenseAssignmentDetail |Information om licenser tilldelad toohello användare. Till exempel om gruppbaserade licensiering var inblandad även detta hello-grupp som beviljas hello licens. |
| Mobil |hello användarens mobiltelefon. |
| OtherMail |Hej användarens alternativa e-postadress. |
| otherMobile |Hej användarens alternativa mobiltelefon. |
| StrongAuthenticationMethod |En lista över verifieringsmetoderna som konfigurerats av hello användare för Multifaktorautentisering, till exempel röst anropa, SMS eller verifiering kod från en mobil app. |
| StrongAuthenticationRequirement |Om Multifaktorautentisering har framtvingats aktiveras eller inaktiveras för den här användaren. |
| StrongAuthenticationUserDetails |hello användarens telefon number, alternativa telefonnummer och e-postadress används för Multifaktorautentisering och verifiering för återställning av lösenord. |
| StrongAuthenticationPhoneAppDetail |Information om forof phone-appar registrerade tooperform 2FA inloggning |
| telephoneNumber |hello användarens telefonnummer. |
| AlternativeSecurityId |Ett alternativt säkerhets-ID för hello-objektet. |
| CreationType |Metod för skapande av hello användare (antingen av inbjudan eller viral). |
| InviteTicket |Lista över inbjudan biljetter för hello användare. |
| InviteReplyUrl |Lista över URL: er tooreply efter inbjudan mottagandet. |
| InviteResources |Lista över resurser toowhich hello har bjudits in. |
| LastDirSyncTime |Tid för senaste hello objekt har uppdaterats på grund av synkroniserar från hello auktoritära (kund, lokalt) directory. |
| MSExchRemoteRecipientType |Mappar tooMSO mottagande typer. Se för [MS mottagande typer] https://msdn.microsoft.com/library/microsoft.office.interop.outlook.recipient.type.aspx för mottagande typer |
| PreferredDataLocation |hello önskad plats för hello användare, grupp, kontaktens, offentlig mapp eller enhetens data. |
| ProxyAddresses |hello-adress som ett mottagande Exchange Server-objekt kan identifieras i en främmande e-postsystemet. |
| StsRefreshTokensValidFrom |Utför uppdatera token återkallningsinformation - några STS uppdaterings-tokens som utfärdats innan den här gången betraktas som upphört att gälla. |
| UserPrincipalName |hello UPN som är ett inloggningsnamn för Internet-format för en användare. |
| UserState |Status för användaren väntar på godkännande/PendingAcceptance/accepteras/PendingVerification. |
| UserStateChangedOn |Tidsstämpel för senaste ändring tooUserState. Använda tootrigger livscykel arbetsflöden. |
| UserType |Typ av användare. Medlem (0), Gäst (1), Viral(2). |

## <a name="update-group-attributes"></a>”Uppdateringsgrupp”-attribut
| Attribut | Beskrivning |
| --- | --- |
| Klassificering |hello klassificering för en enhetlig grupp (HBI, MBI osv.). |
| Beskrivning |Läsbart beskrivande fraser om hello-objekt. |
| Visningsnamn |hello visningsnamn för ett objekt |
| DirSyncEnabled |Anger om synkronisering sker från en auktoritativ (kund, lokalt) directory. |
| GroupLicenseAssignment |Licenstilldelning i en grupp |
| groupType |Typ av grupp, enhetlig (0) |
| IsMembershipRuleLocked |Anger att hello MembershipRule egenskapen anges av hello självbetjäning management-tjänsten och kan inte ändras av användare. Gäller endast toogroups där GroupType innehåller GroupType.DynamicMembership |
| IsPublic |Flagga tooindicate om hello är offentliga och privata. |
| LastDirSyncTime |Tid för senaste hello objekt uppdaterades till följd av synkroniserar från hello auktoritära (lokalt kund) directory. |
| Post |Primär e-postadress |
| MailEnabled |Anger om den här gruppen har e-funktionen. |
| mailNickname |Moniker för ett adress book objekt, vanligtvis hello del av sin e-postnamn föregående hello ”@” symbol. |
| MembershipRule |En sträng som uttrycka hello kriterier som hello självbetjäning management service toodetermine vilka medlemmar som ska tillhöra toothis grupp. Se även IsMembershipRuleLocked. Gäller endast toogroups där GroupType innehåller GroupType.DynamicMembership. |
| MembershipRuleProcessingState |Ett uppräkningsvärde som definieras av hanteringstjänsten för självbetjäning av hello definierar hello status för medlemskap för den här gruppen bearbetades. Gäller endast toogroups där GroupType innehåller GroupType.DynamicMembership. |
| ProxyAddresses |hello-adress som ett mottagande Exchange Server-objekt kan identifieras i en främmande e-postsystemet. |
| RenewedDateTime |Tidsstämpel logg över när en grupp senast förnyas. |
| securityEnabled |Anger om medlemskap i gruppen hello kan påverka auktoriseringsbeslut. |
| WellKnownObject |Märker ett katalogobjekt ange som en av de fördefinierade. |

## <a name="update-device-attributes"></a>”Uppdatera enheten” attribut
| Attribut | Beskrivning |
| --- | --- |
| accountEnabled |Anger om ett säkerhetsobjekt kan autentisera. |
| CloudAccountEnabled |Anger om ett säkerhetsobjekt kan autentisera. Skrivs av InTune när hello enheten hanteras lokalt. |
| CloudDeviceOSType |Typ av hello enhet baserat på hello OS t.ex. Windows RT, iOS. Ange när en tjänst i molnet (till exempel Intune) måste blir det här attributet auktoritära i hello directory för DeviceOSType. |
| CloudDeviceOSVersion |Hello OS-versionen. Ange när en tjänst i molnet (till exempel Intune) måste blir det här attributet auktoritära i hello directory för DeviceOSVersion. |
| CloudDisplayName |Värdet för hello displayName LDAP-attribut. Ange när en tjänst i molnet (till exempel Intune) måste blir det här attributet auktoritära i hello för displayName. |
| CloudCreated |Anger om hello-objektet har skapats av molntjänster. |
| CompliantUntil |Vilka tills enheten anses kompatibla. |
| DeviceMetadata |Anpassade metadata för hello-enhet |
| DeviceObjectVersion |Detta attribut är används tooidentify hello schemaversion av hello enhet. |
| DeviceOSType |Typ av hello enhet baserat på hello OS t.ex. Windows RT, iOS. Skrivits av hello Registreringstjänst och avsedda toobe uppdateras av hello MDM management-tjänsten eller STS lysa management-tjänsten. |
| DeviceOSVersion |Hello OS-versionen. Skrivits av hello Registreringstjänst och avsedda toobe uppdateras av hello MDM management-tjänsten eller STS lysa management-tjänsten. |
| DevicePhysicalIds |Flervärdesattribut avsedd toostore identifierare för hello fysisk enhet. Detta kan inkludera BIOS-ID, TPM-tumavtryck, maskinvara specifikt ID: N, osv. |
| DirSyncEnabled |Anger om synkronisering sker från en auktoritativ (kund, lokalt) directory. |
| Visningsnamn |hello visningsnamn för ett objekt |
| IsCompliant |Detta attribut är används toomanage hello mobila Enhetsstatus för hantering av hello enhet. |
| IsManaged |Det här attributet används tooindicate hello enheten hanteras av ett moln MDM. |
| LastDirSyncTime |Tid för senaste hello objekt har uppdaterats på grund av synkroniserar från hello auktoritära (lokalt kund) directory. |

## <a name="update-device-configuration-attributes"></a>”Uppdatera enhetskonfigurationen” attribut
| Attribut | Beskrivning |
| --- | --- |
| MaximumRegistrationInactivityPeriod |Hej hur många dagar en enhet kan vara inaktiv innan den anses för borttagning. |
| RegistrationQuota |Principen används toolimit hello antal enheten registreringar tillåts för en enskild användare. |

## <a name="update-service-principal-configuration-attributes"></a>”Uppdatera tjänstens huvudnamn konfiguration” attribut
| Attribut | Beskrivning |
| --- | --- |
| accountEnabled |Anger om ett säkerhetsobjekt kan autentisera. |
| AppPrincipalId |Externa, programdefinierade identitet för ett säkerhetsobjekt. |
| Visningsnamn |hello visningsnamn för ett objekt |
| ServicePrincipalName |Tjänstens huvudnamn, som innehåller ”namn/myndighet” där namn anger ett värde för programmet klass och utfärdare innehåller minst värdnamn [: port] eller ”name” som anger en identifierare för hello tjänstens huvudnamn. |

## <a name="update-app-attributes"></a>”Uppdatera App” attribut
| Attribut | Beskrivning |
| --- | --- |
| AppAddress |hello uppsättning adresser (omdirigera URL: er) som har tilldelats tooa tjänstens huvudnamn. |
| appId |Program-ID för hello App |
| AppIdentifierUri |Programmet URI som identifierar hello program.  Det är vanligtvis åtkomst-URL för hello. |
| AppLogoUrl |hello-url för hello logotypen programavbildning lagras i en CDN. |
| AvailableToOtherTenants |True hello programmet är flera innehavare (dvs. kan användas av andra klienter). |
| Visningsnamn |hello visningsnamn för ett programnamn |
| Rätt |Lista över program rättigheter. |
| ExternalUserAccountDelegationsAllowed |Flagga som anger om resursprogram är en betrodd och kan skapa delegering poster för externa användarkonton. |
| GroupMembershipClaims |hello medlemskap anspråk Grupprincip. |
| PublicClient |TRUE om hello-klienten inte kan hålla hemliga (d.v.s. icke-konfidentiella klient i OAuth2.0) |
| RecordConsentConditions |Typer av medgivande villkor, som definieras av hello minimera villkor: Ingen (0), SilentConsentForPartnerManagedApp(1). Det här värdet kommer att exponeras i hello Graph API-schemat och kan bara vara set eller har ändrats av innehavaradministratörer. |
| RequiredResourceAccess |XML-innehåll för ett värde för hello RequiredResourceAccess egenskapen. |
| WebApp |Om värdet är true anger du att det här programmet är ett webbprogram. |
| WwwHomepage |hello primära webbsida. |

## <a name="update-role-attributes"></a>”Uppdatera roll” attribut
| Attribut | Beskrivning |
| --- | --- |
| AppAddress |hello uppsättning adresser (omdirigera URL: er) som har tilldelats tooa tjänstens huvudnamn. |
| BelongsToFirstLoginObjectSet |Om värdet är true anger att det här objektet tillhör toohello uppsättning objekt krävs tooenable inloggningen för hello första administratören av en ny klient. |
| Inbyggd |Anger om hello livslängden för ett objekt som ägs av hello system. |
| Beskrivning |Läsbart beskrivande fraser om hello-objekt. |
| Visningsnamn |hello visningsnamn för ett objekt |
| mailNickname |Moniker för ett adress book objekt, vanligtvis hello del av sin e-postnamn föregående hello ”@” symbol. |
| RoleDisabled |Anger om hello roll ska ignoreras för åtkomst till kontroller. |
| RoleTemplateId |Hello rollmall identitet. |
| ServiceInfo |Tjänstspecifika Etableringsinformation som kan användas av MOAC och/eller andra instanser av tjänsten (av hello samma eller olika tjänsttyper). |
| TaskSetScopeReference |Identifierar en TaskSet och en uppsättning scope som är kopplade till en roll eller RoleTemplate. |
| ValidationError |Information som publicerats av en federerad tjänst som beskriver en inte är tillfällig, tjänstspecifikt fel om hello egenskaper eller länk från ett objekt administratör åtgärden tooresolve. |
| WellKnownObject |Märker ett katalogobjekt ange som en av de fördefinierade. |

## <a name="update-role-definition-attributes"></a>”Uppdatera rolldefinitionen” attribut
| Attribut | Beskrivning |
| --- | --- |
| AssignableScopes |Samling av auktorisering scope som kan refereras när du tilldelar RoleDefinition tooa säkerhetsobjektet. |
| Visningsnamn |hello visningsnamn för ett objekt |
| GrantedPermissions |Behörigheter som utfärdats av den här RoleDefinition. |

## <a name="update-administrative-unit-attributes"></a>”Uppdatera administrativa enhet” attribut
| Attribut | Beskrivning |
| --- | --- |
| Beskrivning |Den här egenskapen uppdateras när du ändrar hello beskrivning av en administrativ enhet. |
| Visningsnamn |Den här egenskapen uppdateras när du ändrar hello namnet på en administrativ enhet. |

## <a name="update-company-attributes"></a>”Uppdatera företagets” attribut
| Attribut | Beskrivning |
| --- | --- |
| AllowedDataLocation |En plats i vilka hello företagets användare tillåts toobe etableras. |
| AuthorizedServiceInstance |Namn på tjänsten instanser toowhich en plan kan distribueras. |
| DirSyncEnabled |Anger om synkronisering sker från en auktoritativ (kund, lokalt) directory. |
| DirSyncStatus |Anger om synkronisering av adress book objekt i den här kontexten klient från en auktoritativ (kund, lokalt) katalog. en utökning av hello DirSyncEnabled-egenskapen för företag-objekt. |
| DirSyncFeatures |Bitars flaggan tookeep reda på aktiverade och inaktiverade dirsync-funktioner för hello-klient. |
| DirectoryFeatures |Aktiverat/inaktiverat directory-funktioner. |
| DirSyncConfiguration |Innehåller alla DirSync configuration specifika toohello aktuella klienten. |
| Visningsnamn |hello visningsnamn för ett objekt |
| IsMnc |Ett boolesk flagga set för ”true” iff hello för företag är aktiverade för hello flerspråkig företagets funktionen. |
| ObjectSettings |En samling inställningar gäller toohello omfattning hello-objektet. |
| PartnerCommerceUrl |URL: en toohello Partner commerce-webbplats. |
| PartnerHelpUrl |Webbplats för URL: en toohello Partner. |
| PartnerSupportEmail |URL: en toohello Partner-stöd för e-post. |
| PartnerSupportTelephone |URL: en toohello Partner support via telefon. |
| PartnerSupportUrl |URL: en toohello Partner supportwebbplats. |
| StrongAuthenticationDetails |Information om relaterade tooStrongAuthentication. |
| StrongAuthenticationPolicy |Stark autentiseringsprincip för hello företag. |
| TechnicalNotificationMail |E-postadress toonotify tekniska problem som rör tooa företag. |
| telephoneNumber |Telefonnummer som är kompatibla med hello ITU rekommendation E.123. |
| TenantType |hello typ av en klient. Om det här värdet anges är hello-klient ett företag. Annars möjliga värden är: MicrosoftSupport (0), SyndicatePartner (1), BreadthPartner (2) BreadthPartnerDelegatedAdmin (3) ResellerPartnerDelegatedAdmin (4) ValueAddedResellerPartnerDelegatedAdmin (5). |
| VerifiedDomain |En uppsättning av DNS-domännamn bunden tooa företag. |

## <a name="update-domain-attributes"></a>”Uppdatera domän” attribut
| Attribut | Beskrivning |
| --- | --- |
| Funktioner |Bitars flaggor som beskriver hello funktioner för hello domän, om en sådan. |
| Standard |Anger om hello domän är standardvärdet för hello; till exempel hello standard UserPrincipalName suffix när en administratör skapar en ny användare i MOAC. |
| Inledande |Anger om hello är hello inledande domän för hello företag som har tilldelats av OCP. hello är inledande en unik underordnade domän i en Microsoft Online-domän. e.g.contoso3.microsoftonline.com. |
| LiveType |Typ av hello motsvarande Windows Live namnområde, om sådana finns. |
| Namn |Identifierare för hello slutpunkt. |
| PasswordNotificationWindowDays |hello antal dagar innan ett lösenord upphör att gälla hello användaren meddelas. |
| PasswordValidityPeriodDays |hello antal dagar som ett lösenord är bra för innan måste ändras. |

Granskningsloggen är en kontroll som krävs för många regler om överensstämmelse. För kunder som använder hello Azure Active Directory granska rapporten toomeet sina förordningar, rekommenderas hello kunden skicka en kopia av det här avsnittet med hello kopia av hello kundens exporteras kontrollrapport toohelp förklarar hello-rapport information. Om hello granskare vill toounderstand hello förordningar som Azure uppfyller för närvarande kan dirigera hello granskare toohello [kompatibilitet sidan](https://azure.microsoft.com/support/trust-center/compliance/) av hello Microsoft Azure Trust Center.

