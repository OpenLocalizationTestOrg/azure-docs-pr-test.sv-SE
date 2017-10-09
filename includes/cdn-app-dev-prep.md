## <a name="prerequisites"></a>Krav
Innan vi kan skriva kod för CDN-hantering, måste toodo vissa förberedelser tooenable vår kod toointeract med hello Azure Resource Manager.  toodo, måste du:

* Skapa en resurs grupp toocontain hello CDN-profilen i den här självstudiekursen skapar vi
* Konfigurera Azure Active Directory tooprovide autentisering i appen
* Tillämpa behörigheter toohello resursgruppen så att endast behöriga användare från våra Azure AD-klient kan interagera med våra CDN-profilen

### <a name="creating-hello-resource-group"></a>Skapa hello resursgruppen.
1. Logga in på hello [Azure Portal](https://portal.azure.com).
2. Klicka på hello **ny** -knappen i övre vänstra hörnet hello, och sedan **Management**, och **resursgruppen**.

    ![Skapa en ny resursgrupp](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Anropa resursgruppen *CdnConsoleTutorial*.  Välj din prenumeration och välj en plats nära dig.  Om du vill kan du klicka på hello **PIN-kod toodashboard** kryssrutan toopin hello resurs grupp toohello instrumentpanelen i hello-portalen.  Det gör det enklare toofind senare.  När du har gjort dina val klickar du på **skapa**.

    ![Namnge hello resursgruppen.](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Efter hello resursgruppen skapas om du inte fäster den tooyour instrumentpanelen, du kan hitta den genom att klicka på **Bläddra**, sedan **resursgrupper**.  Klicka på hello resurs grupp tooopen den.  Anteckna din **prenumerations-ID**.  Vi behöver den senare.

    ![Namnge hello resursgruppen.](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-hello-azure-ad-application-and-applying-permissions"></a>Skapa hello Azure AD-program och tillämpa behörigheter
Det finns två tillvägagångssätt tooapp autentisering med Azure Active Directory: enskilda användare eller ett huvudnamn för tjänsten. Ett huvudnamn för tjänsten är liknande tooa-tjänstkontot i Windows.  I stället för att ge en viss användare behörighet toointeract med hello CDN profiler ge vi i stället hello behörigheter toohello tjänstens huvudnamn.  Tjänstens huvudnamn används normalt för automatisk, icke-interaktiv processer.  Även om den här kursen är skrivs en interaktiv konsolprogram, ska vi fokusera på hello service principal metod.

Skapa ett huvudnamn för tjänsten består av flera steg, inklusive att skapa ett Azure Active Directory-program.  toodo kan vi för[i den här kursen](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Vara säker på att toofollow alla hello steg i hello [länkade kursen](../articles/resource-group-create-service-principal-portal.md).  Det är *mycket viktigt* du slutför den exakt som beskrivs.  Se till att toonote din **klient-ID**, **klientdomännamn** (vanligtvis en *. onmicrosoft.com* domän om du har angett en anpassad domän), **klient-ID** , och **klienten autentiseringsnyckel**, som vi behöver dessa senare.  Vara mycket försiktig tooguard din **klient-ID** och **klienten autentiseringsnyckel**, enligt de här autentiseringsuppgifterna kan vara används av alla tooexecute åtgärder som hello tjänstens huvudnamn.
>
> När du får toohello steg med namnet konfigurera program för flera innehavare kan välja **nr**.
>
> När du får toohello steg [tilldela programmet toorole](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), Använd hello resursgruppen som vi skapade tidigare, *CdnConsoleTutorial*, men i stället för hello **Reader** roll, tilldela Hej **CDN-profilen deltagare** roll.  När du har tilldelat hello programmet hello **CDN-profilen deltagare** -rollen på din resursgrupp, returnerar toothis kursen. 
>
>

När du har skapat din service principal och tilldelade hello **CDN-profilen deltagare** roll, hello **användare** bladet för resursgruppen bör se ut ungefär toothis.

![Bladet användare](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interaktiv användarautentisering
Om du i stället för ett huvudnamn för tjänsten har du i stället enskilda interaktiv användarautentisering, är hello processen mycket lik toothat för ett huvudnamn för tjänsten.  I praktiken måste toofollow hello samma procedur men göra några mindre ändringar.

> [!IMPORTANT]
> Följ anvisningarna nästa endast om du väljer toouse enskilda användarautentisering i stället för en tjänstens huvudnamn.
>
>

1. När du skapar ditt program i stället för **webbprogram**, Välj **programspecifika**.

    ![Det ursprungliga programmet](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. På nästa sida hello uppmanas du att för en **omdirigerings-URI**.  hello URI: N kommer inte att verifiera, men kom ihåg vad du angett.  Du behöver det senare.
3. Det finns inget behov av toocreate en **klienten autentiseringsnyckel**.
4. I stället för att tilldela en service principal toohello **CDN-profilen deltagare** roll, vi tooassign enskilda användare eller grupper.  I det här exemplet ser du att det har tilldelats *CDN Demo användaren* toohello **CDN-profilen deltagare** roll.  

    ![Enskilda användaråtkomst](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
