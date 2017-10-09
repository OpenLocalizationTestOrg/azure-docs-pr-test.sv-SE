---
title: "Självstudier: Konfigurera Workday för automatisk användaretablering med lokala Active Directory och Azure Active Directory | Microsoft Docs"
description: "Lär dig hur toouse Workday som datakälla identitet för Active Directory och Azure Active Directory."
services: active-directory
author: asmalser-msft
documentationcenter: na
manager: femila
ms.assetid: 1a2c375a-1bb1-4a61-8115-5a69972c6ad6
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/26/2017
ms.author: asmalser
ms.openlocfilehash: d4eb3237b8fe7614606c58b39fbefcb44f4060fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-configure-workday-for-automatic-user-provisioning-with-on-premises-active-directory-and-azure-active-directory"></a>Självstudier: Konfigurera Workday för automatisk användaretablering med lokala Active Directory och Azure Active Directory
hello syftet med den här kursen är tooshow du hello stegen tooperform tooimport personer från Workday till både Active Directory och Azure Active Directory med valfria tillbakaskrivning av vissa attribut tooWorkday. 



## <a name="overview"></a>Översikt

Hej [Azure Active Directory-användare som etableras](active-directory-saas-app-provisioning.md) kan integreras med hello [Workday personal API](https://community.workday.com/sites/default/files/file-hosting/productionapi/Human_Resources/v21.1/Get_Workers.html) ordning tooprovision användarkonton. Azure AD använder den här anslutningen tooenable hello efter användaren etablering arbetsflöden:

* **Etablera användare tooActive Directory** -Synkronisera markerade uppsättningar med användare från Workday till en eller flera Active Directory-skogar. 

* **Endast molnbaserad användare tooAzure Active Directory-etablering** -Hybrid-användare som finns i både Active Directory och Azure Active Directory kan etableras till hello senare med hjälp av [AAD Connect](connect/active-directory-aadconnect.md). Användare som är molnbaserad kan dock etableras direkt från Workday tooAzure Active Directory med hjälp av hello Azure AD-användare som etableras.

* **Tillbakaskrivning av e-post adresser tooWorkday** -hello Azure AD-användaren som etablerar tjänsten kan skriva valt Azure AD användaren attribut tillbaka tooWorkday, till exempel hello e-postadress.

### <a name="scenarios-covered"></a>Scenarier som tas upp

Hej Workday användaretablering arbetsflöden som stöds av etableras hello Azure AD-användare kan automatisering av hello efter personal identity livscykel scenarier och:

* **Uthyrning nyanställda** – när en ny medarbetare läggs tooWorkday, ett konto skapas automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD ](active-directory-saas-app-provisioning.md), vid skrivning baksidan hello e-postadress tooWorkday.

* **Medarbetare-attribut och profilen uppdateringar** – när en anställd uppdateras i Workday (till exempel deras namn, avdelning eller manager), sitt användarkonto uppdateras automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).

* **Medarbetare uppsägningar** – när en anställd avslutas i arbetsdagen, sitt användarkonto inaktiveras automatiskt i Active Directory, Azure Active Directory och eventuellt Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).

* **Medarbetare nytt anlitar** – när en anställd rehired i Workday, sina gamla kontot kan återaktiveras automatiskt eller etablerade igen (beroende på dina inställningar) tooActive Directory, Azure Active Directory, och du kan också Office 365 och [andra SaaS-program som stöds av Azure AD](active-directory-saas-app-provisioning.md).


## <a name="planning-your-solution"></a>Planerar din lösning

Innan du börjar din Workday-integrering, kontrollera hello krav nedan och Läs hello följande anvisningar om hur toomatch nuvarande Active Directory-arkitektur och användaretablering krav med hello solution(s) som tillhandahålls av Azure Active Katalog.

### <a name="prerequisites"></a>Krav

hello-scenario som beskrivs i den här kursen förutsätter att du redan har hello följande objekt:

* En giltig Azure AD Premium P1-prenumeration med global administratörsbehörighet
* En klient för implementering av Workday för testning och integrering
* Administratörsbehörighet i Workday toocreate en systemanvändare integrering och se ändringar tootest medarbetardata för testning
* En domänansluten server som kör tjänsten Windows 2012 eller högre är obligatoriska toohost hello för användaren etablering tooActive Directory [lokalt lösenordssynkroniseringsagenten](https://go.microsoft.com/fwlink/?linkid=847801)
* [Azure AD Connect](connect/active-directory-aadconnect.md) för synkronisering mellan Active Directory och Azure AD

> [!NOTE]
> Om Azure AD-klienten finns i Europa finns hello [kända problem](#known-issues) nedan.


### <a name="solution-architecture"></a>Lösningsarkitektur

Azure AD innehåller en omfattande uppsättning etablering kopplingar toohelp du lösa etablering och identitet livscykelhantering från Workday tooActive Directory, Azure AD, SaaS-appar och framåt. Vilka funktioner du använder och hur du ställer in hello lösning varierar beroende på din organisations miljö och behov. Går igenom hur många av hello följande är närvarande och distribuerade i din organisation som ett första steg:

* Hur många Active Directory-skogar finns i använda?
* Hur många Active Directory-domäner finns i använda?
* Hur många Active Directory organisationsenheter (OU) används?
* Hur många Azure Active Directory-klienter finns i använda?
* Finns det användare som behöver toobe etableras tooboth Active Directory och Azure Active Directory (t.ex. ”hybrid” användare)?
* Finns det användare som behöver toobe etableras tooAzure Active Directory, men inte Active Directory (t.ex. ”endast molnbaserad” användare)?
* Behöver användare e-postadresser toobe skrivs tillbaka tooWorkday?

När du har svaren toothese frågor kan du planera din arbetsdag allokering distribution av hello vägledningen nedan.

#### <a name="using-provisioning-connector-apps"></a>Använda etablering connector appar

Azure Active Directory stöder förintegrerade etablering kopplingar för Workday och många andra SaaS-program. 

En enkel etablering koppling gränssnitt med hello API i en enda källsystemet och hjälper etablera data tooa enda målsystemet. De flesta etablering kopplingar som har stöd för Azure AD är för en enda källa och målsystem (t.ex. Azure AD-tooServiceNow) och kan konfigureras genom att lägga till hello appen i fråga från hello Azure AD app-galleriet (t.ex. ServiceNow). 

Det finns en 1: 1-relation mellan etablering koppling instanser och app-instanser i Azure AD:

| Källsystemet | Målsystem |
| ---------- | ---------- | 
| Azure AD-klient | SaaS-program |


När du arbetar med Workday och Active Directory, finns det dock flera käll- och system toobe-anses vara:

| Källsystemet | Målsystem | Anteckningar |
| ---------- | ---------- | ---------- |
| Arbetsdagar | Active Directory-skog | Varje skog behandlas som ett distinkta målsystem |
| Arbetsdagar | Azure AD-klient | Som krävs för endast molnbaserad användare |
| Active Directory-skog | Azure AD-klient | Detta flöde hanteras av AAD Connect idag |
| Azure AD-klient | Arbetsdagar | För tillbakaskrivning av e-postadresser |

toofacilitate flera arbetsflöden toomultiple käll- och systemen, Azure AD innehåller flera etablering connector-appar som du kan lägga till från hello Azure AD app-galleriet:

![AAD App-galleriet](./media/active-directory-saas-workday-inbound-tutorial/WD_Gallery.PNG)

* **Arbetsdagar tooActive Directory etablering** -den här appen förenklar användaretablering konto från Workday tooa Active Directory-skog. Om du har flera skogar, kan du lägga till en instans av den här appen från hello Azure AD app-galleriet för varje Active Directory-skog som du behöver tooprovision till.

* **Arbetsdagar tooAzure AD etablering** -medan AAD Connect är hello-verktyget som ska använda toosynchronize Active Directory-användare tooAzure Active Directory, så den här appen kan använda toofacilitate etablering av endast molnbaserad användare från Workday tooa enda Azure Active Directory-klient.

* **Arbetsdagar tillbakaskrivning** -den här appen förenklar tillbakaskrivning av användarens e-postadresser från Azure Active Directory tooWorkday.

> [!TIP]
> hello reguljära ”Workday” app används för att konfigurera enkel inloggning mellan Workday och Azure Active Directory. 

Hur tooset upp och konfigurera dessa särskilda etablering connector appar är hello ämnet för hello återstående avsnitten i den här kursen. Vilka appar som du väljer tooconfigure beror på vilka system måste Active Directory-skogar och Azure AD för tooprovision till, och hur många finns klienter i din miljö.

![Översikt](./media/active-directory-saas-workday-inbound-tutorial/WD_Overview.PNG)

## <a name="configure-a-system-integration-user-in-workday"></a>Konfigurera en systemanvändare integrering i Workday
Ett vanligt krav för alla hello Workday etablering kopplingar är de kräver autentiseringsuppgifter för en arbetsdag system integration konto tooconnect toohello Workday personal API. Det här avsnittet beskrivs hur toocreate en systemintegreraren konto i Workday.

> [!NOTE]
> Det är möjligt toobypass den här proceduren och i stället använda ett globalt administratörskonto för Workday som hello systemkonto för integrering. Detta fungerar bra för demonstrationer, men det rekommenderas inte för Produktionsdistribution.

### <a name="create-an-integration-system-user"></a>Skapa en integration system-användare

**toocreate en integration systemanvändare:**

1. Logga in på din Workday-klient med ett administratörskonto. I hello **Workday arbetsstationen**, ange skapa användare i hello sökrutan och klicka sedan på **skapa Integration systemanvändare**. 
   
    ![Skapa användare](./media/active-directory-saas-workday-inbound-tutorial/IC750979.png "skapa användare")
2. Fullständig hello **skapa Integration systemanvändare** aktiviteten genom att ange ett användarnamn och lösenord för en ny Integration systemanvändare.  
 * Lämna hello **kräver nya lösenord vid nästa inloggning** alternativet är avmarkerat, eftersom den här användaren loggar in via programmering. 
 * Lämna hello **antal minuter för sessionens Timeout** med standardvärdet 0, vilket förhindrar att hello användarsessioner timeout för tidigt. 
   
    ![Skapa Integration systemanvändare](./media/active-directory-saas-workday-inbound-tutorial/IC750980.png "skapa Integration systemanvändare")

### <a name="create-a-security-group"></a>Skapa en säkerhetsgrupp
Du behöver toocreate en säkerhetsgrupp för obegränsad integration system och tilldela hello användaren tooit.

**toocreate en säkerhetsgrupp:**

1. Ange skapa säkerhetsgrupp i hello sökrutan och klicka sedan på **skapa säkerhetsgruppen**. 
   
    ![CreateSecurity grupp](./media/active-directory-saas-workday-inbound-tutorial/IC750981.png "CreateSecurity grupp")
2. Fullständig hello **skapa säkerhetsgrupp** aktivitet.  
3. Välj Integration Systemsäkerhetsgrupp – obegränsad från hello **typ av innehavare säkerhetsgrupp** listrutan.
4. Skapa en säkerhet grupp toowhich medlemmar läggs explicit. 
   
    ![CreateSecurity grupp](./media/active-directory-saas-workday-inbound-tutorial/IC750982.png "CreateSecurity grupp")

### <a name="assign-hello-integration-system-user-toohello-security-group"></a>Tilldela hello integration system toohello säkerhetsgrupp

**tooassign hello integration systemanvändare:**

1. Ange redigera säkerhetsgrupp i hello sökrutan och klicka sedan på **redigera säkerhetsgrupp**. 
   
    ![Redigera säkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750983.png "redigera säkerhetsgrupp")
2. Sök efter och välj hello ny säkerhetsgrupp för integrering av namn. 
   
    ![Redigera säkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750984.png "redigera säkerhetsgrupp")
3. Lägg till hello nya integration system toohello ny säkerhetsgrupp. 
   
    ![Systemsäkerhetsgrupp](./media/active-directory-saas-workday-inbound-tutorial/IC750985.png "Systemsäkerhetsgrupp")  

### <a name="configure-security-group-options"></a>Konfigurera alternativ för säkerhet
I det här steget kan du bevilja toohello nya security gruppbehörigheter för **hämta** och **placera** hello-objekt som skyddas av hello följande säkerhetsprinciper för domänen:

* Externa konto-etablering
* Worker Data: Public Worker rapporter
* Worker Data: Alla positioner
* Worker Data: Current bemanning Information
* Worker Data: Business titel på Worker profil

**tooconfigure säkerhetsalternativ till gruppen:**

1. Ange säkerhetsprinciper för domänen i hello sökrutan och klicka sedan på hello länken **domän säkerhetsprinciper för funktionsområde**.  
   
    ![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750986.png "säkerhetsprinciper för domänen")  
2. Sök efter system och välj hello **System** funktionsområde.  Klicka på **OK**.  
   
    ![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750987.png "säkerhetsprinciper för domänen")  
3. Hello lista över säkerhetsprinciper för hello System funktionsområde, expandera **säkerhetsadministration** och välj hello säkerhetsprincip **externa Kontoetablering**.  
   
    ![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750988.png "säkerhetsprinciper för domänen")  
4. Klicka på **Redigera behörigheter**, och klicka sedan på hello **Redigera behörigheter**dialogrutan Lägg till hello nya säkerhet toohello grupplistan säkerhetsgrupper med **hämta** och **Placera** integrering behörigheter. 
   
    ![Redigera behörighet](./media/active-directory-saas-workday-inbound-tutorial/IC750989.png "redigera behörighet")  
5. Upprepa steg 1 ovan tooreturn toohello skärmen för att välja huvudområden och nu, Sök efter personal, Välj hello **bemanning funktionsområde** och på **OK**.
   
    ![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750990.png "säkerhetsprinciper för domänen")  
6. Expandera i hello lista över säkerhetsprinciper för hello Staffing funktionsområde, **Worker Data: Staffing** och upprepa ovanstående steg 4 för varje återstående säkerhetsprinciper:

   * Worker Data: Public Worker rapporter
   * Worker Data: Alla positioner
   * Worker Data: Current bemanning Information
   * Worker Data: Business titel på Worker profil
   
7. Upprepa steg 1 ovan tooreturn toohello skärmen för att välja huvudområden och den här tiden kan söka efter **kontaktinformation**, Välj hello Staffing funktionsområde och på **OK**.

8.  Hello lista över säkerhetsprinciper för hello Staffing funktionsområde, expandera **Worker Data: Kontakta arbetsinformation**, och upprepa steg 4 ovan för hello säkerhetsprinciper nedan:

    * Worker Data: E-post

    ![Domän säkerhetsprinciper](./media/active-directory-saas-workday-inbound-tutorial/IC750991.png "säkerhetsprinciper för domänen")  
    
### <a name="activate-security-policy-changes"></a>Aktivera ändringar i säkerhet

**ändringar i tooactivate säkerhet:**

1. Ange aktiverar i hello sökrutan och klicka sedan på hello länken **aktivera väntande ändringar i säkerhet**. 
   
    ![Aktivera](./media/active-directory-saas-workday-inbound-tutorial/IC750992.png "aktivera") 
2. Begin hello aktivera väntande ändringar i säkerhet genom att ange en kommentar för granskningsändamål, och klicka sedan på **OK**. 
   
    ![Aktivera väntande säkerhet](./media/active-directory-saas-workday-inbound-tutorial/IC750993.png "aktivera väntande säkerhet")   
3. Fullständig hello aktivitet på nästa hello-skärmen genom att markera kryssrutan hello **Bekräfta**, och klicka sedan på **OK**. 
   
    ![Aktivera väntande säkerhet](./media/active-directory-saas-workday-inbound-tutorial/IC750994.png "aktivera väntande säkerhet")  

## <a name="configuring-user-provisioning-from-workday-tooactive-directory"></a>Konfigurering av användarförsörjning från Workday tooActive Directory
Följ dessa instruktioner tooconfigure användarkonto etablering från Workday tooeach Active Directory-skog som du behöver etablering till.

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>Del 1: Lägga till hello etablering connector appen och skapa hello anslutning tooWorkday

**tooconfigure Workday tooActive Directory etablering:**

1.  Gå för<https://portal.azure.com>

2.  Välj i hello vänstra navigeringsfältet **Azure Active Directory**

3.  Välj **företagsprogram**, sedan **alla program**.

4.  Välj **lägga till ett program**, och välj hello **alla** kategori.

5.  Sök efter **Workday etablering tooActive Directory**, och Lägg till appen från hello-galleriet.

6.  När hello app har lagts till och hello app information visas, Välj **etablering**

7.  Ändra hello **etablering** **läge** för**automatisk**

8.  Fullständig hello **administratörsautentiseringsuppgifter** avsnittet på följande sätt:

   * **Admin Username** – ange hello användarnamnet för hello systemkontot för Workday-integrering med hello klient domännamnet tillagt. **Ska se ut ungefär:username@contoso4**

   * **Administratörslösenordet –** ange hello lösenord för hello systemkontot för Workday-integrering

   * **Klient URL –** ange hello URL toohello Workday web services-slutpunkten för din klient. Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med hello rätt miljö sträng.

   * **Active Directory-skogar -** hello ”Name” för Active Directory-skog som returneras av hello Get-ADForest powershell cmdlet. Detta är vanligtvis en sträng som: *contoso.com*

   * **Active Directory-behållare -** ange hello behållaren sträng som innehåller alla användare i din AD-skog. Exempel: *OU = standardanvändare, OU = Users, DC = contoso, DC = test*

   * **E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.

   * Klicka på hello **Testanslutningen** knappen. Om anslutningstestet hello lyckas klickar du på hello **spara** knappen hello överst. Om det misslyckas, kan du kontrollera att hello Workday-autentiseringsuppgifter är giltiga i Workday. 

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_1.PNG)

### <a name="part-2-configure-attribute-mappings"></a>Del 2: Konfigurera attributmappning 

I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Active Directory.

1.  Hello etablering på fliken **mappningar**, klickar du på **synkronisera Workday arbetare tooOnPremises**.

2.  I hello **källa Objektområde** fält, kan du välja vilka uppsättningar med användare i Workday ska vara i omfånget för att etablera tooAD, genom att definiera en uppsättning attributbaserad filter. hello standardscope är ”alla användare i Workday”. Exempel filter:

   * Exempel: Scope toousers med Worker-ID: N mellan 1000000 och 2000000

      * Attributet: WorkerID

      * Operatorn: REGEX-matchning

      * Värde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Exempel: Endast anställda och inte contingent personer 

      * Attributet: EmployeeID

      * Operatorn: Inte är NULL

3.  I hello **mål objektåtgärder** fält du kan globalt filtrera vilka åtgärder tillåts toobe utförs på Active Directory. **Skapa** och **uppdatering** är de vanligaste.

4.  I hello **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappa tooActive katalogattribut.

5. Klicka på ett befintligt attribut mappning tooupdate den, eller klicka **Lägg till ny mappning** längst hello hello skärmen tooadd nya mappningar. En enskild attributmappning stöder dessa egenskaper:

      * **Avbildningstyp**

         * **Direkt** – skriver hello värdet för hello Workday attributet toohello AD-attributet, utan ändringar

         * **Konstant** -skriva ett statiska, konstant strängvärde till hello AD-attribut

         * **Uttrycket** – kan du toowrite ett anpassat värde till hello AD-attribut, baserat på en eller flera Workday-attribut. [Mer information finns i den här artikeln på uttryck](active-directory-saas-writing-expressions-for-attribute-mappings.md).

      * **Källattributet** -hello användarattribut från Workday.

      * **Standardvärde** – det är valfritt. Om hello källattribut har ett tomt värde, skrivs hello mappningen det här värdet i stället.
            De vanligaste konfigurationen är tooleave detta tomt.

      * **Målattribut** – hello användarattribut i Active Directory.

      * **Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen används toouniquely identifiera användare mellan Workday och Active Directory. Detta anges normalt Worker ID-fältet för Workday som vanligtvis är mappad till hello anställnings-ID-attribut i Active Directory.

      * **Matchar prioritet** – flera matchande attribut kan anges. När det finns flera, utvärderas de i den ordning som anges av det här fältet. När en matchning hittas matchar ingen ytterligare attribut utvärderas.

      * **Använd den här mappningen**
       
         * **Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder

         * **Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder

6. toosave kopplingar, klicka på **spara** hello överst i avsnittet attributmappning.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_2.PNG)

**Här följer några exempel attributmappning mellan Workday och Active Directory med vissa vanliga uttryck**

-   hello-uttrycket som mappar toohello parentDistinguishedName AD-attributet kan vara används tooprovision en användare tooa Organisationsenheten baserat på en eller flera Workday källa attribut. Det här exemplet placerar användare i olika organisationsenheter beroende på deras stad data i Workday.

-   hello-uttryck som mappar toohello userPrincipalName AD-attributet skapar en UPN för firstName.LastName@contoso.com. Den ersätter också ogiltiga specialtecken.

-   [Det finns dokumentationen om hur du skriver här uttryck](active-directory-saas-writing-expressions-for-attribute-mappings.md)

  
| ARBETSDAGAR ATTRIBUT | ACTIVE DIRECTORY-ATTRIBUT |  MATCHANDE ID? | SKAPA / UPPDATERA |
| ---------- | ---------- | ---------- | ---------- |
|  **WorkerID**  |  EmployeeID | **Ja** | Skrivas i Skapa endast | 
|  **Namnet**   |   L   |     | Skapa och uppdatera |
|  **Företag**         | Företag   |     |  Skapa och uppdatera |
|  **CountryReferenceTwoLetter**      |   CO |     |   Skapa och uppdatera |
| **CountryReferenceTwoLetter**    |  C  |     |         Skapa och uppdatera |
| **SupervisoryOrganization**  | Avdelning  |     |  Skapa och uppdatera |
|  **PreferredNameData**  |  Visningsnamn |     |   Skapa och uppdatera |
| **EmployeeID**    |  CN    |   |   Skrivas i Skapa endast |
| **Fax**      | facsimileTelephoneNumber     |     |    Skapa och uppdatera |
| **Förnamn**   | givenName       |     |    Skapa och uppdatera |
| **Växel (\[Active\],, ”0”, ”True”, ”1”)** |  AccountDisabled      |     | Skapa och uppdatera |
| **Mobile**  |    mobila       |     |       Skrivas i Skapa endast |
| **E-postadress**    | E-post    |     |     Skapa och uppdatera |
| **ManagerReference**   | Manager  |     |  Skapa och uppdatera |
| **WorkSpaceReference** | physicalDeliveryOfficeName    |     |  Skapa och uppdatera |
| **Postnummer**  |   Postnummer  |     | Skapa och uppdatera |
| **LocalReference** |  preferredLanguage  |     |  Skapa och uppdatera |
| ** Ersätta (Mid (Ersätt (\[EmployeeID\]”, (\[ \\ \\ / \\ \\ \\ \\ \\ \\ \[\\\\\]\\\\:\\\\;\\\\|\\\\=\\\\,\\\\+\\\\\*\\\\?\\ \\ &lt; \\ \\ &gt; \]) ””, ”,), 1, 20)”, ([\\\\.) \* \$] (file:///\\.) *$)", , "", , )**      |    SAMAccountName            |     |         Skrivas i Skapa endast |
| **Efternamn**   |   SN   |     |  Skapa och uppdatera |
| **CountryRegionReference** |  St     |     | Skapa och uppdatera |
| **AddressLineData**    |  StreetAddress  |     |   Skapa och uppdatera |
| **PrimaryWorkTelephone**  |  telephoneNumber   |     | Skrivas i Skapa endast |
| **BusinessTitle**   |  Rubrik     |     |  Skapa och uppdatera |
| **Ansluta (”@”, Ersätt (Ersätt (ersätta (Ersätt (ersätta (Ersätt (ersätta (ersätta (Ersätt ((Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt (Ersätt ( Ersätt (ansluta till (””., [Förnamn], [Efternamn]), ”([Øø])”, ”Outlook Express”,), ”[Ææ]”, ”ae”,), ”([äãàâãåáąÄÃÀÂÃÅÁĄA])”, ”a”,), ”[B]”, ”b”,), ”([CçčćÇČĆ])”, ”c”,), ”([ďĎD])”, ”d”,), ”([ëèéêęěËÈÉÊĘĚE])”, ”e”,), ”[F]”, ”f”,), ”([G])” ”g”,), ”[H]”, ”h”,), ”([ïîìíÏÎÌÍI])”, ”i”,), ”[J]”, ”j”,), ”([K])”, ”k”,), ”([ľłŁĽL])”, ”l”,), ”([M])”, ”m”,), ”([ñńňÑŃŇN])”, ”n”,), ”([öòőõôóÖÒŐÕÔÓO])”, ”o”,), ”([P])”, ”p”,), ”([Q])”, ”q”,),  ”([ŘŘR])”, ”r”,), ”([ßšśŠŚS])”, ”s”,), ”([TŤť])”, ”t”,), ”([üùûúůűÜÙÛÚŮŰU])”, ”u”,), ”([V])”, ”v”,), ”([B])”, ”b”,), ”([ýÿýŸÝY])”, ”y”,), ”([źžżŹŽŻZ])”, ”z”,) ”,”, ””,), ”contoso.com”)**   | UserPrincipalName     |     | Skapa och uppdatera                                                   
| **Växel (\[namnet\]”, OU standardanvändare, OU = = användare, OU = standard, OU = platser, DC = contoso, DC = com”, ”Dallas” ”, OU standardanvändare, OU = = användare, OU Dallas, OU = platser, DC = = contoso, DC = com”, ”Austin” ”OU standardanvändare, OU = = Användare, OU Austin, OU = platser, DC = = contoso, DC = com ”,” Seattle ””, OU standardanvändare, OU = = användare, OU Seattle, OU = = platser, DC = contoso, DC = com ”,” London ””, OU standardanvändare, OU = = användare, OU London, OU = platser, DC = = contoso, DC = com ”)**  | parentDistinguishedName     |     |  Skapa och uppdatera |
  
### <a name="part-3-configure-hello-on-premises-synchronization-agent"></a>Del 3: Konfigurera hello lokalt synkronisering agent

En agent måste installeras på en domänansluten server i hello önskan Active Directory-skog i ordning tooprovision tooActive Directory lokalt. Domänadministratörer (eller företagsadministratör) autentiseringsuppgifter krävs för att slutföra hello proceduren.

**[Du kan hämta hello lokalt lösenordssynkroniseringsagenten här](https://go.microsoft.com/fwlink/?linkid=847801)**

När du har installerat agenten, kör du hello Powershell-kommandon nedan tooconfigure hello agent för din miljö.

**Kommandot #1**

> CD C:\\programfiler\\Microsoft Azure Active Directory-synkronisering Agent\\moduler\\AADSyncAgent

> import-module AADSyncAgent.psd1

**Kommandot #2**

> Lägg till ADSyncAgentActiveDirectoryConfiguration

* Indata: För ”katalognamn” ange hello AD skogsnamnet som har angetts i del \#2
* Indata: Admin användarnamn och lösenord för Active Directory-skog

**Kommandot #3**

> Lägg till ADSyncAgentAzureActiveDirectoryConfiguration

* Indata: Global administratörsanvändarnamnet och lösenordet för din Azure AD-klient

**Kommandot #4**

> Get-AdSyncAgentProvisioningTasks

* Åtgärd: Kontrollera data returneras. Det här kommandot upptäcker automatiskt Workday etablering appar i Azure AD-klienten. Exempel på utdata:

> Namn: Min AD-skog
>
> Aktiverad: True
>
> DirectoryName: mydomain.contoso.com
>
> Trovärdig: False
>
> ID: WDAYdnAppDelta.c2ef8d247a61499ba8af0a29208fb853.4725aa7b-1103-41e6-8929-75a5471a5203

**Kommandot #5**

> Start-AdSyncAgentSynchronization-automatisk

**Kommandot #6**

> net stop aadsyncagent

**Kommandot #7**

> net start aadsyncagent

### <a name="part-4-start-hello-service"></a>Del 4: Start hello service
När du delar 1-3 har slutförts, kan du starta hello etableras i hello Azure-hanteringsportalen.

1.  I hello **etablering** fliken, ange hello **Status för etablering** till **på**.

2. Klicka på **Spara**.

3. Detta startar hello inledande synkronisering, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.

4. Enskilda sync händelser, t.ex vilka användare läses från Workday och sedan senare lagts till eller uppdaterats tooActive Directory, kan visas i hello **granskningsloggar** fliken. **[Se hello etablering reporting vägledning för detaljerade anvisningar om hur tooread hello granskningsloggar](active-directory-saas-provisioning-reporting.md)**

5.  hello Windows programlogg hello agentdator visas alla åtgärder som utförs via hello-agenten.

6. En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.

![Azure Portal](./media/active-directory-saas-workday-inbound-tutorial/WD_3.PNG)


## <a name="configuring-user-provisioning-tooazure-active-directory"></a>Konfigurera användaretablering tooAzure Active Directory
Hur du konfigurerar etablering tooAzure Active Directory beror på dina etablering krav som anges i hello nedan.

| Scenario | Lösning |
| -------- | -------- |
| **Användare behöver toobe etableras tooActive Directory och Azure AD** | Använd  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Användare behöver toobe etableras tooActive Directory endast** | Använd  **[AAD Connect](connect/active-directory-aadconnect.md)** |
| **Användare behöver toobe etableras tooAzure AD endast (moln endast)** | Använd hello **Workday tooAzure Active Directory-etablering** app i hello app-galleriet |

Anvisningar om hur du konfigurerar Azure AD Connect finns hello [dokumentation för Azure AD Connect](connect/active-directory-aadconnect.md).

hello följande avsnitt beskrivs hur du konfigurerar en anslutning mellan Workday och Azure AD tooprovision endast molnbaserad användare.

> [!IMPORTANT]
> Följ bara hello proceduren nedan om du har endast molnbaserad användare som behöver toobe etableras tooAzure AD och inte lokala Active Directory.

### <a name="part-1-adding-hello-azure-ad-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>Del 1: Lägga till hello Azure AD etablering connector appen och skapa hello anslutning tooWorkday

**tooconfigure Workday tooAzure Active Directory-etablering för endast molnbaserad användare:**

1.  Gå för<https://portal.azure.com>.

2.  Välj i hello vänstra navigeringsfältet **Azure Active Directory**

3.  Välj **företagsprogram**, sedan **alla program**.

4.  Välj **lägga till ett program**, och välj sedan hello **alla** kategori.

5.  Sök efter **Workday tooAzure AD etablering**, och Lägg till appen från hello-galleriet.

6.  När hello app har lagts till och hello app information visas, Välj **etablering**

7.  Ändra hello **etablering** **läge** för**automatisk**

8.  Fullständig hello **administratörsautentiseringsuppgifter** avsnittet på följande sätt:

   * **Admin Username** – ange hello användarnamnet för hello systemkontot för Workday-integrering med hello klient domännamnet tillagt. Ska se ut ungefär:username@contoso4

   * **Administratörslösenordet –** ange hello lösenord för hello systemkontot för Workday-integrering

   * **Klient URL –** ange hello URL toohello Workday web services-slutpunkten för din klient. Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med hello rätt miljö sträng (vid behov).

   * **E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.

   * Klicka på hello **Testanslutningen** knappen.

   * Om anslutningstestet hello lyckas klickar du på hello **spara** knappen hello överst. Om det misslyckas, kontrollera att hello Workday-URL och autentiseringsuppgifter är giltiga i Workday.


### <a name="part-2-configure-attribute-mappings"></a>Del 2: Konfigurera attributmappning 

I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Azure Active Directory för endast molnbaserad användare.

1.  Hello etablering på fliken **mappningar**, klickar du på **synkronisera arbetare tooAzure AD**.

2.   I hello **källa Objektområde** fält, kan du välja vilka uppsättningar med användare i Workday ska vara i omfånget för att etablera tooAzure AD, genom att definiera en uppsättning attributbaserad filter. hello standardscope är ”alla användare i Workday”. Exempel filter:

   * Exempel: Scope toousers med Worker-ID: N mellan 1000000 och 2000000

      * Attributet: WorkerID

      * Operatorn: REGEX-matchning

      * Värde: (1[0-9][0-9][0-9][0-9][0-9][0-9])

   * Exempel: Endast contingent arbetare och inte fast anställda

      * Attributet: ContingentID

      * Operatorn: Inte är NULL

3.  I hello **mål objektåtgärder** fält du kan globalt filtrera vilka åtgärder tillåts toobe utförs på Azure AD. **Skapa** och **uppdatering** är de vanligaste.

4.  I hello **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappa tooActive katalogattribut.

5. Klicka på ett befintligt attribut mappning tooupdate den, eller klicka **Lägg till ny mappning** längst hello hello skärmen tooadd nya mappningar. En enskild attributmappning stöder dessa egenskaper:

   * **Avbildningstyp**

      * **Direkt** – skriver hello värdet för hello Workday attributet toohello AD-attributet, utan ändringar

      * **Konstant** -skriva ett statiska, konstant strängvärde till hello AD-attribut

      * **Uttrycket** – kan du toowrite ett anpassat värde till hello AD-attribut, baserat på en eller flera Workday-attribut. [Mer information finns i den här artikeln på uttryck](active-directory-saas-writing-expressions-for-attribute-mappings.md).

   * **Källattributet** -hello användarattribut från Workday.

   * **Standardvärde** – det är valfritt. Om hello källattribut har ett tomt värde, skrivs hello mappningen det här värdet i stället.
            De vanligaste konfigurationen är tooleave detta tomt.

   * **Målattribut** – hello användarattribut i Azure AD.

   * **Matcha objekt med hjälp av det här attributet** – oavsett om den här mappningen används toouniquely identifiera användare mellan Workday och Azure AD. Detta anges normalt Worker ID-fältet för Workday som vanligtvis är mappad till hello anställnings-ID-attributet (nya) eller ett tillägg-attributet i Azure AD.

   * **Matchar prioritet** – flera matchande attribut kan anges. När det finns flera, utvärderas de i den ordning som anges av det här fältet. När en matchning hittas matchar ingen ytterligare attribut utvärderas.

   * **Använd den här mappningen**

     * **Alltid** – tillämpa den här mappningen för både skapa användare och uppdatera åtgärder

     * **Endast under skapa** -mappningen tillämpas endast på användare creation-åtgärder

6. toosave kopplingar, klicka på **spara** hello överst i avsnittet attributmappning.

### <a name="part-3-start-hello-service"></a>Del 3: Start hello service
När du delar 1 – 2 har slutförts, kan du starta hello etableras.

1.  I hello **etablering** fliken, ange hello **Status för etablering** till **på**.

2. Klicka på **Spara**.

3. Detta startar hello inledande synkronisering, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.

4. Enskilda sync händelser kan visas i hello **granskningsloggar** fliken. **[Se hello etablering reporting vägledning för detaljerade anvisningar om hur tooread hello granskningsloggar](active-directory-saas-provisioning-reporting.md)**

5. En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.


## <a name="configuring-writeback-of-email-addresses-tooworkday"></a>Konfigurera tillbakaskrivning av e-postadresser tooWorkday
Följ dessa instruktioner tooconfigure tillbakaskrivning av användare e-postadresser från Azure Active Directory tooWorkday.

### <a name="part-1-adding-hello-provisioning-connector-app-and-creating-hello-connection-tooworkday"></a>Del 1: Lägga till hello etablering connector appen och skapa hello anslutning tooWorkday

**tooconfigure Workday tooActive Directory etablering:**

1.  Gå för<https://portal.azure.com>

2.  Välj i hello vänstra navigeringsfältet **Azure Active Directory**

3.  Välj **företagsprogram**, sedan **alla program**.

4.  Välj **lägga till ett program**och välj hello **alla** kategori.

5.  Sök efter **Workday tillbakaskrivning**, och Lägg till appen från hello-galleriet.

6.  När hello app har lagts till och hello app information visas, Välj **etablering**

7.  Ändra hello **etablering** **läge** för**automatisk**

8.  Fullständig hello **administratörsautentiseringsuppgifter** avsnittet på följande sätt:

   * **Admin Username** – ange hello användarnamnet för hello systemkontot för Workday-integrering med hello klient domännamnet tillagt. Ska se ut ungefär:username@contoso4

   * **Administratörslösenordet –** ange hello lösenord för hello systemkontot för Workday-integrering

   * **Klient URL –** ange hello URL toohello Workday web services-slutpunkten för din klient. Detta ska se ut: https://wd3-impl-services1.workday.com/ccx/service/contoso4 där contoso4 ersätts med rätt-klientnamn och wd3 impl ersätts med hello rätt miljö sträng (vid behov).

   * **E-postmeddelande –** ange din e-postadress och markera kryssrutan ”Skicka e-post om fel inträffar”.

   * Klicka på hello **Testanslutningen** knappen. Om anslutningstestet hello lyckas klickar du på hello **spara** knappen hello överst. Om det misslyckas, kontrollera att hello Workday-URL och autentiseringsuppgifter är giltiga i Workday.


### <a name="part-2-configure-attribute-mappings"></a>Del 2: Konfigurera attributmappning 


I det här avsnittet ska du konfigurera hur informationen flödar från Workday till Active Directory.

1.  Hello etablering på fliken **mappningar**, klickar du på **synkronisera Azure AD-användare tooWorkday**.

2.  I hello **källa Objektområde** fält du kan du filtrera vilka uppsättningar med användare i Azure Active Directory ska ha sina e-postadresser skrivs tillbaka tooWorkday. hello standardscope är ”alla användare i Azure AD”. 

3.  I hello **attributet mappningar** avsnitt, kan du definiera hur enskilda Workday attribut mappa tooActive katalogattribut. Det finns en mappning för hello e-postadress som standard. Hello matchande ID måste vara uppdaterade toomatch användare i Azure AD med sina motsvarande poster i Workday. En populär matchande metod är toosynchronize hello Workday worker-ID eller medarbetare ID tooextensionAttribute1-15 i Azure AD och sedan använda det här attributet toomatch Azure AD-användare i Workday.

4.  toosave kopplingar, klicka på **spara** hello överst i hello attributmappning avsnitt.

### <a name="part-3-start-hello-service"></a>Del 3: Start hello service
När du delar 1 – 2 har slutförts, kan du starta hello etableras.

1.  I hello **etablering** fliken, ange hello **Status för etablering** till **på**.

2. Klicka på **Spara**.

3. Detta startar hello inledande synkronisering, vilket kan ta flera timmar beroende på hur många användare finns i Workday variabeln.

4. Enskilda sync händelser kan visas i hello **granskningsloggar** fliken. **[Se hello etablering reporting vägledning för detaljerade anvisningar om hur tooread hello granskningsloggar](active-directory-saas-provisioning-reporting.md)**

5. En klar skrivs en översikt över kontrollrapport den **etablering** fliken enligt nedan.

## <a name="known-issues"></a>Kända problem

* **Granskningsloggar i europeiska språk** - hello-versionen av den här tekniska förhandsversionen är ett känt problem med hello [granskningsloggar](active-directory-saas-provisioning-reporting.md) för hello Workday connector appar visas inte i hello [Azure-portalen](https://portal.azure.com) om hello Azure AD-klient finns i ett Europeiska datacenter. En korrigering för problemet är kommande. Kontrollera blanksteg igen hello nära framtiden för uppdateringar. 

## <a name="additional-resources"></a>Ytterligare resurser
* [Självstudier: Konfigurera enkel inloggning mellan Workday och Azure Active Directory](active-directory-saas-workday-tutorial.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar med Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md)

## <a name="next-steps"></a>Nästa steg

* [Lär dig hur tooreview loggar och få rapporter om etablering aktivitet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-provisioning-reporting)
