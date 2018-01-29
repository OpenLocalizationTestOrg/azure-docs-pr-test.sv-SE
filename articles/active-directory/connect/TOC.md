# Översikt
## [Vad är Azure AD Connect?](active-directory-aadconnect.md)
## [Vad är Azure AD Connect Sync?](active-directory-aadconnectsync-whatis.md)
### [Användare och kontakter](active-directory-aadconnectsync-understanding-users-and-contacts.md)
### [Arkitektur](active-directory-aadconnectsync-understanding-architecture.md)
### [Deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning.md)
#### [Uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
### [Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md)
## [Vad är Azure AD Connect och Federation?](active-directory-aadconnectfed-whatis.md)


# Kom igång
## [Förutsättningar](active-directory-aadconnect-prerequisites.md)
## [Installera Azure AD Connect](active-directory-aadconnect-select-installation.md)
### [Standardinställningar](active-directory-aadconnect-get-started-express.md)
### [Anpassade inställningar](active-directory-aadconnect-get-started-custom.md)
### [Uppgradera från DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md)
### [Uppgradera från en tidigare version](active-directory-aadconnect-upgrade-previous-version.md)
### [Installera med hjälp av en befintlig ADSync-databas](active-directory-aadconnect-existing-database.md)

# Gör så här för att
## Planera och designa
### [Designbegrepp](active-directory-aadconnect-design-concepts.md)
### [Topologier för Azure AD Connect](active-directory-aadconnect-topologies.md)
### [Active Directory Federation Services i Azure](active-directory-aadconnect-azure-adfs.md)
### [Att tänka på för instanser](active-directory-aadconnect-instances.md)
### [När du redan har Azure AD](active-directory-aadconnect-existing-tenant.md)
## [Hantera Azure AD Connect](active-directory-aadconnect-whats-next.md)
### [Förnya certifikat för O365 och Azure AD](active-directory-aadconnect-o365-certs.md)
### [Uppdatera SSL-certifikatet för en AD FS-servergrupp (Active Directory Federation Services)](active-directory-aadconnectfed-ssl-update.md)
### [Aktivera tillbakaskrivning av enheter](active-directory-aadconnect-feature-device-writeback.md)
### [Alternativ för användarinloggning](active-directory-aadconnect-user-signin.md)
#### [Smidig enkel inloggning](active-directory-aadconnect-sso.md)
##### [Snabbstart](active-directory-aadconnect-sso-quick-start.md)
##### [Hur fungerar det?](active-directory-aadconnect-sso-how-it-works.md)
##### [Vanliga frågor och svar](active-directory-aadconnect-sso-faq.md)
##### [Felsöka](active-directory-aadconnect-troubleshoot-sso.md)
#### [Direktautentisering](active-directory-aadconnect-pass-through-authentication.md)
##### [Snabbstart](active-directory-aadconnect-pass-through-authentication-quick-start.md)
##### [Aktuella begränsningar](active-directory-aadconnect-pass-through-authentication-current-limitations.md)
##### [Hur fungerar det?](active-directory-aadconnect-pass-through-authentication-how-it-works.md)
##### [Uppgradera agenter i förhandsversion](active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md)
##### [Smart Lockout](active-directory-aadconnect-pass-through-authentication-smart-lockout.md)
##### [Vanliga frågor och svar](active-directory-aadconnect-pass-through-authentication-faq.md)
##### [Felsöka](active-directory-aadconnect-troubleshoot-pass-through-authentication.md)
##### [Djupgående om säkerhet](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md)
### [Stöd för flera domäner vid federering](active-directory-aadconnect-multiple-domains.md)
### [Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md)
### [Använda en SAML 2.0-identitetsprovider (IdP) för enkel inloggning](active-directory-aadconnect-federation-saml-idp.md)



## Hantera Azure AD Connect Sync
### [Förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
### [Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md)
### [Azure AD-tjänstkonto](active-directory-aadconnectsync-howto-azureadaccount.md)
### [Installationsguide](active-directory-aadconnectsync-installation-wizard.md)
### [Ändra standardkonfigurationen](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)
### [Konfigurera filtrering](active-directory-aadconnectsync-configure-filtering.md)
### [Scheduler](active-directory-aadconnectsync-feature-scheduler.md)
### [Katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md)

### [Ändra lösenordet för Azure AD Sync-tjänstkontot](active-directory-aadconnectsync-change-serviceacct-pass.md)
### [Ändra AD DS-kontolösenordet](active-directory-aadconnectsync-change-addsacct-pass.md)
### [Aktivera AD-papperskorgen](active-directory-aadconnectsync-recycle-bin.md)

### [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md)
#### [Åtgärder](active-directory-aadconnectsync-service-manager-ui-operations.md)
#### [Anslutningsappar](active-directory-aadconnectsync-service-manager-ui-connectors.md)
#### [Metaversumdesigner](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)
#### [Metaversumsökning](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)


## Hantera Federation Services
### [Hantera och anpassa](active-directory-aadconnect-federation-management.md)
### [Federera flera instanser av Azure AD med en enda instans av AD FS](active-directory-aadconnectfed-single-adfs-multitenant-federation.md)


## Felsöka
### [Anslutning](active-directory-aadconnect-troubleshoot-connectivity.md)
### [Fel under synkronisering](active-directory-aadconnect-troubleshoot-sync-errors.md)
### [Objektet har inte synkroniserats](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md)
### [Lösenordssynkronisering](active-directory-aadconnectsync-troubleshoot-password-synchronization.md)
### [LargeObject-fel som orsakats av userCertificate](active-directory-aadconnectsync-largeobjecterror-usercertificate.md)
### [Så här återställer du från LocalDB med en gräns på 10 GB](active-directory-aadconnect-recover-from-localdb-10gb-limit.md)

# Referens
## [Kodexempel](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory)
## [Identitetssynkronisering och duplicerad attributåterhämtning](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
## [Portar och protokoll som krävs för hybrididentitet](active-directory-aadconnect-ports.md)
## [Funktioner i förhandsgranskningen](active-directory-aadconnect-feature-preview.md)
## [Versionshistorik](active-directory-aadconnect-version-history.md)
## [Konton och behörigheter](active-directory-aadconnect-accounts-permissions.md)

## Azure AD Connect Sync
### [Attribut som synkroniseras till Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md)
### [Versionshistorik för anslutningsappen](active-directory-aadconnectsync-connector-version-history.md)
### [Referens för funktioner](active-directory-aadconnectsync-functions-reference.md)
### [Driftåtgärder och saker att tänka på](active-directory-aadconnectsync-operations.md)
### [Kompatibilitetslista för Azure AD-federation](active-directory-aadconnect-federation-compatibility.md)
### [Tekniska begrepp](active-directory-aadconnectsync-technical-concepts.md)
### [Tjänstfunktioner](active-directory-aadconnectsyncservice-features.md)




# Relaterat
## [Övervaka den lokala identitetsinfrastrukturen och synkroniseringstjänster i molnet](../connect-health/active-directory-aadconnect-health.md)
## [Designguide för hybrididentitet](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/)


# Resurser
## [Azure-översikt](https://azure.microsoft.com/roadmap/?category=security-identity)
##[Vanliga frågor och svar om Azure AD Connect](active-directory-aadconnect-faq.md)
##[DirSync-utfasning](active-directory-aadconnect-dirsync-deprecated.md)
## [Priskalkylator](https://azure.microsoft.com/pricing/calculator/)

