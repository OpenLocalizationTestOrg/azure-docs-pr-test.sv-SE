Organisationer som använder flera [programvara som en tjänst (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) program för produktivitet eftersom molnteknik och verktyg blir mer tillgänglig. När hello antalet SaaS-appar växer blir svårt för hello administratörer toomanage konton och behörigheter och för hello användare tooremember olika lösenord. Hantera programmen individuellt skapar extra arbete och är mindre säkert.

* Anställda som har tookeep reda på många lösenord tenderar toouse säkra mindre metoder tooremember dem antingen skriva ned lösenord eller använda hello samma lösenord för många konton.
* När en nyanställd anländer eller en lämnar måste sina konton vara individuellt etablerats eller tas bort.
* Dessutom anställda kan börja använda SaaS-appar för arbetet utan att gå via IT-avdelningen, vilket betyder att de skapar sina egna konton på system som hello IT-administratörer inte har godkänts och är inte övervakning.  

En lösning för alla dessa problem är enkel inloggning (SSO). Det har hello enklaste sättet toomanage flera appar och ge användarna en konsekvent upplevelse inloggning. Azure Active Directory (AD Azure) är en robust lösning för enkel inloggning och har många tillgängliga redan integrerade program, med självstudier för administratörer tooquickly ställa in en ny app och starta etablering användare.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Hur integrera appar i Azure Active Directory?
Azure AD kan toointegrate dina appar och etablerade konton. Detta kan göras på något av två sätt.

* Om hello app redan är integrerade i hello app galleriet kan du gå igenom den portal tooset in appar och konfigurera hello inställningar tooallow SSO. Du kan komma igång med hello enkla steg för steg-anvisningar presenteras i hello app-galleriet och hello Azure portal tooenable enkel inloggning för för galleriet-appen.
* Om hello app inte är i hello galleriet måste skapa du fortfarande de flesta appar i Azure AD som en anpassad app. Detta kräver lite mer tekniska kunskaper tooconfigure. Du kan lägga till alla program som stöder SAML 2.0 som en federerad app eller alla program som har en HTML-baserad-inloggningssida som lösenord SSO app.

I hello fall där användarna har skapat sina egna konton för SaaS-appar som inte hanteras av IT-avdelningen, hello [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) verktyget tillhandahåller en lösning. Det här verktyget övervakar hello web trafik tooidentify vilka appar som används i hela organisationen hello och hello antalet personer som använder dem. IT kan använda denna information toolearn vilka appar hello användare föredrar och bestämma vilka toointegrate till Azure AD för enkel inloggning.  

När du integrerar en app till Azure AD kan du mappa hello användarnas etablerade programmet identiteter tootheir respektive Azure AD-identitet.  

