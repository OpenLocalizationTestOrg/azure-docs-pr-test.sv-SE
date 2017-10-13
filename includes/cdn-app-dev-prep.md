## <a name="prerequisites"></a>Krav
Innan vi kan skriva kod för CDN-hantering, som vi behöver göra vissa förberedelser för att aktivera vår kod för att interagera med Azure Resource Manager.  Om du vill göra detta måste du:

* Skapa en resursgrupp att innehålla CDN-profilen i den här självstudiekursen skapar vi
* Konfigurera Azure Active Directory för att tillhandahålla autentisering i appen
* Tillämpa behörigheter för resursgruppen så att endast auktoriserade användare från våra Azure AD-klient kan interagera med våra CDN-profilen

### <a name="creating-the-resource-group"></a>Skapa resursgruppen.
1. Logga in på den [Azure-portalen](https://portal.azure.com).
2. Klicka på den **ny** knappen i det övre vänstra och sedan **Management**, och **resursgruppen**.

    ![Skapa en ny resursgrupp](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Anropa resursgruppen *CdnConsoleTutorial*.  Välj din prenumeration och välj en plats nära dig.  Om du vill kan du klicka på den **fäst på instrumentpanelen** om du vill fästa resursgruppen på instrumentpanelen i portalen.  Det gör det lättare att hitta senare.  När du har gjort dina val klickar du på **skapa**.

    ![Namngivning av resursgruppen.](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. När resursgruppen har skapats kan du inte fästa på instrumentpanelen, du kan hitta den genom att klicka på **Bläddra**, sedan **resursgrupper**.  Klicka på resursgruppen för att öppna den.  Anteckna din **prenumerations-ID**.  Vi behöver den senare.

    ![Namngivning av resursgruppen.](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Skapa Azure AD-program och tillämpa behörigheter
Det finns två metoder för autentisering i appen med Azure Active Directory: enskilda användare eller ett huvudnamn för tjänsten. En tjänst som liknar ett tjänstkonto i Windows.  I stället för att bevilja behörigheter att interagera med CDN-profiler för en viss användare, bevilja vi i stället behörighet till tjänstens huvudnamn.  Tjänstens huvudnamn används normalt för automatisk, icke-interaktiv processer.  Även om den här kursen är skrivs en interaktiv konsolprogram, ska vi fokusera på service principal metod.

Skapa ett huvudnamn för tjänsten består av flera steg, inklusive att skapa ett Azure Active Directory-program.  Detta gör vi [i den här kursen](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Se till att följa alla steg i den [länkade kursen](../articles/resource-group-create-service-principal-portal.md).  Det är *mycket viktigt* du slutför den exakt som beskrivs.  Se till att Observera din **klient-ID**, **klientdomännamn** (vanligtvis en *. onmicrosoft.com* domän om du har angett en anpassad domän), **klient-ID** , och **klienten autentiseringsnyckel**, som vi behöver dessa senare.  Var försiktig skydda din **klient-ID** och **klienten autentiseringsnyckel**, enligt de här autentiseringsuppgifterna kan användas av alla för att utföra åtgärder som tjänstens huvudnamn.
>
> När du kommer till steget med namnet konfigurera program för flera innehavare, välja **nr**.
>
> När du kommer till steg [tilldela program till rollen](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), använda den resursgrupp som vi skapade tidigare, *CdnConsoleTutorial*, men i stället för den **Reader** roll, tilldela  **CDN-profilen deltagare** roll.  När du har tilldelat programmet den **CDN-profilen deltagare** -rollen på resursgruppens namn, gå tillbaka till den här kursen. 
>
>

När du har skapat tjänstens huvudnamn och tilldelas den **CDN-profilen deltagare** roll, den **användare** bladet för resursgruppen bör se ut ungefär så här.

![Bladet användare](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interaktiv användarautentisering
Om du i stället för ett huvudnamn för tjänsten har du i stället enskilda interaktiv användarautentisering, påminner processen som för ett huvudnamn för tjänsten.  I praktiken behöver du följer samma procedur men göra några mindre ändringar.

> [!IMPORTANT]
> Följ bara dessa nästa steg om du väljer att använda användarautentisering för enskilda i stället för en tjänstens huvudnamn.
>
>

1. När du skapar ditt program i stället för **webbprogram**, Välj **programspecifika**.

    ![Det ursprungliga programmet](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. På nästa sida, uppmanas du att för en **omdirigerings-URI**.  URI: N kommer inte att verifiera, men kom ihåg vad du angett.  Du behöver det senare.
3. Det finns inget behov av att skapa en **klienten autentiseringsnyckel**.
4. I stället för att tilldela ett huvudnamn för tjänsten i **CDN-profilen deltagare** roll, är det dags att tilldela enskilda användare eller grupper.  I det här exemplet ser du att det har tilldelats *CDN Demo användaren* till den **CDN-profilen deltagare** roll.  

    ![Enskilda användaråtkomst](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
