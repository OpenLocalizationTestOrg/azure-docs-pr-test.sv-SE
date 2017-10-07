---
title: aaaConfigure Azure Active Directory-autentisering - SQL | Microsoft Docs
description: "Lär dig hur tooconnect tooSQL Database och SQL Data Warehouse med hjälp av Azure Active Directory-autentisering."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 7e2508a1-347e-4f15-b060-d46602c5ce7e
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/10/2017
ms.author: rickbyh
ms.openlocfilehash: d6222da0b840f96d4bcfbc02964dc7c54d5ea1ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-manage-azure-active-directory-authentication-with-sql-database-or-sql-data-warehouse"></a>Konfigurera och hantera Azure Active Directory-autentisering med SQL Database eller SQL Data Warehouse

Den här artikeln beskrivs hur du toocreate och fylla i Azure AD och sedan använda Azure AD med Azure SQL Database och SQL Data Warehouse. En översikt finns [Azure Active Directory Authentication](sql-database-aad-authentication.md).

>  [!NOTE]  
>  Ansluter tooSQL Server som körs på en Azure VM inte stöds med ett Azure Active Directory-konto. Använd en domän Active Directory-konto i stället.

## <a name="create-and-populate-an-azure-ad"></a>Skapa och fylla i en Azure AD
Skapa en Azure AD och fylla det med användare och grupper. Azure AD kan vara hello initiala domänen Azure AD-hanterad domän. Azure AD kan också vara en lokal Active Directory Domain Services som är federerat med hello Azure AD.

Mer information finns i [integrera dina lokala identiteter med Azure Active Directory](../active-directory/active-directory-aadconnect.md), [lägga till egna domänen namnet tooAzure AD](../active-directory/active-directory-add-domain.md), [nu federation med stöd för Microsoft Azure Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [administrera Azure AD-katalogen](https://msdn.microsoft.com/library/azure/hh967611.aspx), [hantera Azure AD med hjälp av Windows PowerShell](/powershell/azure/overview?view=azureadps-2.0), och [Hybrididentitet Krävs portar och protokoll](../active-directory/active-directory-aadconnect-ports.md).

## <a name="optional-associate-or-change-hello-active-directory-that-is-currently-associated-with-your-azure-subscription"></a>Valfritt: Koppla eller ändra hello active directory som associeras med din Azure-prenumeration
tooassociate databasen med hello Azure AD-katalogen för din organisation, se hello directory en betrodd katalog för hello Azure värd hello prenumerationsdatabasen. Mer information finns i [Hur Azure-prenumerationer är associerade med Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx).

**Ytterligare information:** var Azure-prenumerationen har en förtroenderelation med en Azure AD-instans. Detta innebär att den litar den directory tooauthenticate användare, tjänster och enheter. Flera prenumerationer kan lita hello samma katalog, men en prenumeration litar bara på en katalog. Du kan se vilken katalog som är betrodd av din prenumeration på hello **inställningar** högst [https://manage.windowsazure.com/](https://manage.windowsazure.com/). Den här förtroenderelationen med en prenumeration med en katalog skiljer sig från hello-relation med en prenumeration med alla andra resurser i Azure (webbplatser, databaser och så vidare), som är mer som underordnade resurser till en prenumeration. Om en prenumeration går ut och sedan komma åt toothose andra resurser som är associerade med prenumerationen hello stoppas även. Men hello katalogen finns kvar i Azure och du kan associera en annan prenumeration med katalogen och fortsätta toomanage hello directory-användare. Mer information om resurser finns [förstå resursåtkomst i Azure](https://msdn.microsoft.com/library/azure/dn584083.aspx).

hello visar följande procedurer hur toochange hello associerade katalog för en viss prenumeration.
1. Ansluta tooyour [klassiska Azure-portalen](https://manage.windowsazure.com/) med hjälp av en administratör för Azure-prenumeration.
2. Markera på hello vänstra banderoll **inställningar**.
3. Dina prenumerationer visas på skärmen för hello-inställningar. Eventuellt hello prenumerationen inte visas klickar du på **prenumerationer** hello överst listrutan hello **FILTER av DIRECTORY** rutan och välj hello-katalog som innehåller dina prenumerationer och klicka sedan på **Tillämpa**.
   
    ![Välj prenumeration][4]
4. I hello **inställningar** området klickar du på din prenumeration och klicka sedan på **redigera katalog** på hello hello sidans nederkant.
   
    ![AD-inställningar-portal][5]
5. I hello **redigera katalog** rutan Välj hello Azure Active Directory som är associerad med din SQL Server eller SQL Data Warehouse och klicka sedan på pilen hello för nästa.
   
    ![Redigera katalog val][6]
6. I hello **BEKRÄFTA** directory mappning dialogrutan Bekräfta att ”**alla medadministratörer tas bort.**”
   
    ![Redigera katalog bekräfta][7]
7. Klicka på hello Kontrollera tooreload hello portal.

   > [!NOTE]
   > När du ändrar hello directory access tooall medadministratörer, Azure AD-användare och grupper och resource backas upp av directory användarna tas bort och de har inte längre åtkomst toothis prenumeration eller dess resurser. Endast kan du, som tjänstadministratör Konfigurera åtkomst för säkerhetsobjekt baserat på nya hello-katalogen. Den här ändringen kan ta lång tid toopropagate tooall resurser. Ändra hello directory även ändringar hello Azure AD-administratör för SQL Database och SQL Data Warehouse och neka åtkomst till databasen för alla befintliga Azure AD-användare. hello Azure AD-administratör måste vara återställning (som beskrivs nedan) och nya Azure AD-användare måste skapas.
   >  

## <a name="create-an-azure-ad-administrator-for-azure-sql-server"></a>Skapa en Azure AD-administratör för Azure SQL server
Varje Azure SQL-server (som värd för en SQL Database eller SQL Data Warehouse) börjar med ett administratörskonto för enskild server Hej administratör hello hela Azure SQL Server. En andra SQL Server-administratören måste skapas, som är en Azure AD-kontot. Detta säkerhetsobjekt skapas som en innesluten databasanvändare i hello master-databasen. Som administratör hello server-administratörskonton som är medlemmar i hello **db_owner** roll i varje användare databasen och ange varje användardatabas som hello **dbo** användare. Läs mer om administratörskonton i hello server [hantera databaser och inloggningar i Azure SQL Database](sql-database-manage-logins.md).

När du använder Azure Active Directory med geo-replikering, måste hello Azure Active Directory-administratör konfigureras för både hello primära och sekundära hello-servrar. Om en server inte har en Azure Active Directory-administratör, sedan får Azure Active Directory-inloggningar och användare felet ”Det går inte att ansluta” tooserver.

> [!NOTE]
> Användare som inte är baserade på en Azure AD-kontot (inklusive hello Azure SQL server-administratörskontot) kan inte skapa Azure AD-baserade användare, eftersom de inte har behörighet toovalidate föreslagna databasanvändare med hello Azure AD.
> 

## <a name="provision-an-azure-active-directory-administrator-for-your-azure-sql-server"></a>Etablera en Azure Active Directory-administratör för din Azure SQL-server

hello följande två procedurer visar hur tooprovision administratör Azure Active Directory för din Azure SQL-server i hello Azure-portalen och med hjälp av PowerShell.

### <a name="azure-portal"></a>Azure Portal
1. I hello [Azure-portalen](https://portal.azure.com/), hello övre högra hörnet i, klicka på din anslutning toodrop ned en lista över möjliga Active kataloger. Välj hello korrigera Active Directory som standard hello Azure AD. Det här steget länkar hello prenumeration association med Active Directory med Azure SQL-server kontrollerar som hello samma prenumeration som används för både Azure AD och SQL Server. (hello Azure SQL-server kan vara värd för Azure SQL Database eller Azure SQL Data Warehouse.)   
    ![Välj ad][8]   
    
2. I hello kvar banderoll väljer **SQL-servrar**väljer din **SQLServer**, och klicka sedan på hello **SQL Server** bladet, klickar du på **Active Directory-administratör**.   
3. I hello **Active Directory-administratör** bladet, klickar du på **ange admin**.   
    ![Välj active directory](./media/sql-database-aad-authentication/select-active-directory.png)  
    
4. I hello **lägga till administratören** bladet söka efter en användare, Välj hello användare eller grupp toobe administratör och klicka sedan på **Välj**. (hello Active Directory-administratör bladet visar alla medlemmar och grupper i Active Directory. Användare eller grupper som är nedtonade kan inte väljas eftersom de inte stöds som Azure AD-administratörer. (Se hello lista över stöds administratörer i hello **Azure AD-funktioner och begränsningar** avsnitt i [Använd Azure Active Directory-autentisering för autentisering med SQL Database eller SQL Data Warehouse](sql-database-aad-authentication.md).) Rollbaserad åtkomstkontroll (RBAC) är inte spridda tooSQL Server gäller toohello-portalen.   
    ![Välj admin](./media/sql-database-aad-authentication/select-admin.png)  
    
5. Hello överst i hello **Active Directory-administratör** bladet, klickar du på **spara**.   
    ![Spara admin](./media/sql-database-aad-authentication/save-admin.png)   

hello processen att ändra Hej administratör kan ta några minuter. Sedan hello ny administratör visas i hello **Active Directory-administratör** rutan.

   > [!NOTE]
   > När du konfigurerar hello Azure AD admin kan inte hello nytt admin namn (användare eller grupp) redan finnas i hello virtuell master-databasen som en användare för SQL Server-autentisering. Om den finns, misslyckas hello Azure AD admin installationen; återställer skapades och som anger att sådana administratör (namn) redan finns. Alla arbete tooconnect toohello servern med hjälp av Azure AD-autentisering misslyckas eftersom SQL Server-autentisering användaren inte är en del av hello Azure AD.
   > 


toolater ta bort en administratör hello överst i hello **Active Directory-administratör** bladet, klickar du på **ta bort admin**, och klicka sedan på **spara**.

### <a name="powershell"></a>PowerShell
toorun PowerShell-cmdlets måste toohave Azure PowerShell installerade och körs. Detaljerad information finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

tooprovision en Azure AD-administratör kör hello följande Azure PowerShell-kommandon:

* Add-AzureRmAccount
* SELECT-AzureRmSubscription

Cmdlet: ar används tooprovision och hantera Azure AD-administratör:

| Cmdlet-namn | Beskrivning |
| --- | --- |
| [Ange AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/set-azurermsqlserveractivedirectoryadministrator) |Etablerar en Azure Active Directory-administratör för Azure SQL server eller Azure SQL Data Warehouse. (Måste vara från hello aktuell prenumeration.) |
| [Ta bort AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/remove-azurermsqlserveractivedirectoryadministrator) |Tar bort en Azure Active Directory-administratör för Azure SQL server eller Azure SQL Data Warehouse. |
| [Get-AzureRmSqlServerActiveDirectoryAdministrator](/powershell/module/azurerm.sql/get-azurermsqlserveractivedirectoryadministrator) |Returnerar information om en administratör för Azure Active Directory som har konfigurerats för hello Azure SQL server eller Azure SQL Data Warehouse. |

Använd PowerShell-kommandot get-help toosee mer detaljerad information för var och en av dessa kommandon, till exempel ``get-help Set-AzureRmSqlServerActiveDirectoryAdministrator``.

Hej följande skript tillhandahåller en administratör Azure AD-grupp med namnet **DBA_Group** (objekt-id `40b79501-b343-44ed-9ce7-da4c8cc7353f`) för hello **demo_server** server i en resursgrupp med namnet **grupp-23** :

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group"
```

Hej **DisplayName** Indataparametern accepterar hello Azure AD visningsnamn eller hello UPN-namnet. Till exempel ``DisplayName="John Smith"`` och ``DisplayName="johns@contoso.com"``. Visningsnamn stöd för Azure AD grupper endast hello Azure AD.

> [!NOTE]
> hello Azure PowerShell-kommandot ```Set-AzureRmSqlServerActiveDirectoryAdministrator``` förhindrar inte att du etablerar Azure AD-administratörer för användare som inte stöds. En användare som inte stöds kan etableras, men kan inte ansluta tooa databas. 
> 

hello följande exempel används hello valfria **ObjectID**:

```
Set-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23"
-ServerName "demo_server" -DisplayName "DBA_Group" -ObjectId "40b79501-b343-44ed-9ce7-da4c8cc7353f"
```

> [!NOTE]
> hello Azure AD **ObjectID** krävs när hello **DisplayName** är inte unikt. tooretrieve hello **ObjectID** och **DisplayName** värden, hello Active Directory-delen av den klassiska Azure-portalen och visar hello egenskaperna för en användare eller grupp.
> 

hello följande exempel returnerar information om Hej aktuella Azure AD-administratör för Azure SQL server:

```
Get-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server" | Format-List
```

hello följande exempel tar bort en Azure AD-administratör:

```
Remove-AzureRmSqlServerActiveDirectoryAdministrator -ResourceGroupName "Group-23" -ServerName "demo_server"
```

Du kan också etablera ett Azure Active Directory-administratör med hjälp av hello REST API: er. Mer information finns i [Service Management REST API-referens och åtgärder för Azure SQL-databaser för Azure SQL-databaser](https://msdn.microsoft.com/library/azure/dn505719.aspx)

### <a name="cli"></a>CLI  
Du kan också etablera en Azure AD-administratör genom att anropa hello följande CLI-kommandon:
| Kommando | Beskrivning |
| --- | --- |
|[Skapa AZ sql server ad-administratör](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#create) |Etablerar en Azure Active Directory-administratör för Azure SQL server eller Azure SQL Data Warehouse. (Måste vara från hello aktuell prenumeration.) |
|[ta bort AZ sql server ad-administratör](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#delete) |Tar bort en Azure Active Directory-administratör för Azure SQL server eller Azure SQL Data Warehouse. |
|[AZ sql server ad-admin-lista](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#list) |Returnerar information om en administratör för Azure Active Directory som har konfigurerats för hello Azure SQL server eller Azure SQL Data Warehouse. |
|[AZ sql server ad-admin-uppdatering](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#update) |Uppdaterar hello Active Directory-administratör för en Azure SQL-server eller Azure SQL Data Warehouse. |

Mer information om CLI-kommandon finns [SQL - az sql](https://docs.microsoft.com/cli/azure/sql/server).  


## <a name="configure-your-client-computers"></a>Konfigurera klientdatorer
På alla klientdatorer som dina program eller användare ansluta tooAzure SQL-databas eller Azure SQL Data Warehouse med hjälp av Azure AD identiteter, måste du installera hello följande programvara:

* .NET framework 4.6 eller senare från [https://msdn.microsoft.com/library/5a4x27ek.aspx](https://msdn.microsoft.com/library/5a4x27ek.aspx).
* Azure Active Directory Authentication Library för SQLServer (**ADALSQL. DLL-filen**) är tillgängligt på flera språk (x86 och amd64) hello download Center på [Microsoft Active Directory Authentication Library för Microsoft SQL Server](http://www.microsoft.com/download/details.aspx?id=48742).

Du kan uppfylla dessa krav genom att:

* Installera antingen [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) eller [SQL Server Data Tools för Visual Studio 2015](https://msdn.microsoft.com/library/mt204009.aspx) uppfyller hello .NET Framework 4.6 krav.
* SSMS installerar hello x86 version av **ADALSQL. DLL-filen**.
* SSDT installerar hello amd64-versionen av **ADALSQL. DLL-filen**.
* hello senaste Visual Studio från [Visual Studio-hämtningar](https://www.visualstudio.com/downloads/download-visual-studio-vs) uppfyller hello .NET Framework 4.6, men installerar inte hello krävs amd64-versionen av **ADALSQL. DLL-filen**.

## <a name="create-contained-database-users-in-your-database-mapped-tooazure-ad-identities"></a>Skapa oberoende databasanvändare i din databas mappas tooAzure AD identiteter

Azure Active Directory-autentisering kräver databasen användare toobe skapas som oberoende databasanvändare. En innesluten databasanvändare baserat på en Azure AD-identitet är en databasanvändare som inte har en inloggning i hello master-databasen och vilken maps tooan identitet i hello Azure AD-katalog som är associerad med hello-databasen. hello Azure AD identity kan vara ett eget användarkonto eller en grupp. Läs mer om oberoende databasanvändare [innehöll databasanvändare-göra din databas bärbara](https://msdn.microsoft.com/library/ff929188.aspx).

> [!NOTE]
> Databasanvändare (med undantag för hello av administratörer) kan inte skapas med hjälp av portalen. RBAC-roller är inte spridda tooSQL Server, SQL Database eller SQL Data Warehouse. Azure RBAC-roller som används för att hantera Azure-resurser och tillämpa inte toodatabase behörigheter. Till exempel hello **SQL Server-deltagare** roll inte bevilja åtkomst tooconnect toohello SQL Database eller SQL Data Warehouse. hello åtkomstbehörighet måste beviljas direkt i hello-databas med Transact-SQL-uttryck.
>

toocreate en Azure AD-baserad finns databasanvändare (andra än hello serveradministratören som äger hello databasen), ansluta toohello databasen med en Azure AD-identitet som en användare med minst hello **ALTER ANY USER** behörighet. Använd sedan hello följande Transact-SQL-syntax:

```
CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
```

*Azure_AD_principal_name* kan vara hello användarens huvudnamn för en Azure AD användar- eller hello visningsnamn för en Azure AD-grupp.

**Exempel:** toocreate en innesluten databasanvändare som representerar en Azure AD federerade eller hanterade domänanvändare:
```
CREATE USER [bob@contoso.com] FROM EXTERNAL PROVIDER;
CREATE USER [alice@fabrikam.onmicrosoft.com] FROM EXTERNAL PROVIDER;
```

toocreate en innesluten databas användaren som representerar en Azure AD eller externa domängrupp, ange hello namn i en säkerhetsgrupp:
```
CREATE USER [ICU Nurses] FROM EXTERNAL PROVIDER;
```

toocreate en innesluten databasanvändare som representerar ett program som ansluter med hjälp av en Azure AD-token:

```
CREATE USER [appName] FROM EXTERNAL PROVIDER;
```

>  [!TIP]
>  Du kan inte direkt skapa en användare från en Azure Active Directory än hello Azure Active Directory som är associerad med din Azure-prenumeration. Dock medlemmar i andra Active kataloger som är importerade användare i hello associerade Active Directory (kallas externa användare) kan läggas till tooan Active Directory-grupp i hello klient Active Directory. Genom att skapa en innesluten databasanvändare för den AD-gruppen hello användare från hello externa Active Directory kan få åtkomst till tooSQL databasen.   

Mer information om hur du skapar innehåller databasen användare baserat på Azure Active Directory identiteter, se [skapa användare (Transact-SQL)](http://msdn.microsoft.com/library/ms173463.aspx).

> [!NOTE]
> Att ta bort hello Azure Active Directory-administratör för Azure SQL-servern förhindrar alla Azure AD authentication-användare ansluter toohello-servern. Vid behov går inte att använda Azure AD-användare kan tas bort manuellt av en administratör för SQL-databas.   

>  [!NOTE]
>  Om du får en **tidsgränsen uppnåddes för anslutningen**, du kan behöva tooset hello `TransparentNetworkIPResolution` parametern för hello anslutning sträng toofalse. Mer information finns i [timeout anslutningsproblem med .NET Framework 4.6.1 - TransparentNetworkIPResolution](https://blogs.msdn.microsoft.com/dataaccesstechnologies/2016/05/07/connection-timeout-issue-with-net-framework-4-6-1-transparentnetworkipresolution/).   

   
När du skapar en databasanvändare som användare får hello **Anslut** behörighet och kan ansluta toothat databasen som en medlem i hello **offentliga** roll. Hej ursprungligen endast behörigheter som är tillgängliga toohello användare har alla behörigheter beviljas toohello **offentliga** , eller alla behörigheter beviljas tooany Windows-grupper att de är medlem i. När du etablerar en Azure AD-baserad finns databasanvändare, kan du bevilja ytterligare hello användarbehörigheter, hello samma sätt som du bevilja behörighet tooany andra typer av användare. Vanligtvis bevilja behörighet toodatabase roller och Lägg till användare tooroles. Mer information finns i [Database Engine behörighet grunderna](http://social.technet.microsoft.com/wiki/contents/articles/4433.database-engine-permission-basics.aspx). Mer information om den speciella SQL-databas roller finns [hantera databaser och inloggningar i Azure SQL Database](sql-database-manage-logins.md).
En federerad domänanvändare som har importerats till en hantera domän måste använda hello hanterade domän identitet.

> [!NOTE]
> Azure AD-användare har markerats i hello databasens metadata med typ E (EXTERNAL_USER) och för grupper med typen X (EXTERNAL_GROUPS). Mer information finns i [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx). 
>

## <a name="connect-toohello-user-database-or-data-warehouse-by-using-ssms-or-ssdt"></a>Ansluta toohello användaren databasen eller data warehouse med hjälp av SSMS eller SSDT  
tooconfirm hello Azure AD-administratör har konfigurerats korrekt, ansluta toohello **master** databasen med hjälp av hello Azure AD-administratörskonto.
tooprovision en Azure AD-baserad finns databasanvändare (andra än hello serveradministratören som äger hello databasen), ansluta toohello databasen med en Azure AD-identitet som har åtkomst till toohello databas.

> [!IMPORTANT]
> Stöd för Azure Active Directory-autentisering finns i [SQL Server 2016 Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) och [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) i Visual Studio 2015. hello augusti 2016-versionen av SSMS finns också stöd för Active Directory Universal-autentisering, vilket gör att administratörer toorequire Multi-Factor Authentication med ett telefonsamtal, textmeddelande, smartkort PIN-kod eller meddelande i mobilappen.
 
## <a name="using-an-azure-ad-identity-tooconnect-using-ssms-or-ssdt"></a>Med hjälp av en Azure AD identity tooconnect med hjälp av SSMS eller SSDT  

hello följande procedurer visar hur tooconnect tooa SQL-databas med en Azure AD-identitet med hjälp av SQL Server Management Studio eller verktyg för SQL Server-databasen.

### <a name="active-directory-integrated-authentication"></a>Active Directory-integrerad autentisering

Använd den här metoden om du är inloggad i tooWindows med hjälp av Azure Active Directory-autentiseringsuppgifter från en federerad domän.

1. Starta Management Studio eller Dataverktyg och i hello **ansluta tooServer** (eller **ansluta tooDatabase motorn**) i dialogrutan hello **autentisering** väljer **Active Directory - integrerade**. Inget lösenord krävs eller kan anges eftersom din befintliga autentiseringsuppgifter kommer att visas för hello-anslutning.   

    ![Välj AD-integrerad autentisering][11]
2. Klicka på hello **alternativ** knappen, och på hello **anslutningsegenskaper** i hello sidan **ansluta toodatabase** rutan, hello typnamn hello-databasen som du vill tooconnect Så här. (hello **AD domän namn eller klient-ID**”alternativet stöds bara för **Universal med MFA anslutning** alternativ, annars den är nedtonat.)  

    ![Välj hello databasnamn][13]

## <a name="active-directory-password-authentication"></a>Active Directory-lösenordsautentisering

Använd den här metoden när du ansluter med en Azure AD-huvudnamn med hjälp av hello Azure AD hanterade domän. Du kan också använda det för federerat konto utan åtkomst toohello domän, till exempel när du arbetar via fjärranslutning.

Använd den här metoden om du är inloggad tooWindows med autentiseringsuppgifter från en domän som inte är federerat med Azure, eller när du använder Azure AD för autentisering med Azure AD utifrån hello inledande hello klienten domän.

1. Starta Management Studio eller Dataverktyg och i hello **ansluta tooServer** (eller **ansluta tooDatabase motorn**) i dialogrutan hello **autentisering** väljer **Active Directory - lösenord**.
2. I hello **användarnamn** Skriv ditt Azure Active Directory-användarnamn i formatet hello  **username@domain.com** . Det här måste vara ett konto från hello Azure Active Directory eller ett konto från en domän federera med hello Azure Active Directory.
3. I hello **lösenord** Skriv ditt användarnamn lösenord för hello Azure Active Directory-konto eller federerad domänkonto.

    ![Välj autentisering av AD-lösenord][12]
4. Klicka på hello **alternativ** knappen, och på hello **anslutningsegenskaper** i hello sidan **ansluta toodatabase** rutan, hello typnamn hello-databasen som du vill tooconnect Så här. (Se hello bilden i hello föregående alternativ.)

## <a name="using-an-azure-ad-identity-tooconnect-from-a-client-application"></a>Med hjälp av en Azure AD identity tooconnect från ett klientprogram

hello följande procedurer visar hur tooconnect tooa SQL-databas med en Azure AD identity från ett klientprogram.

###  <a name="active-directory-integrated-authentication"></a>Active Directory-integrerad autentisering

toouse integrerad Windows-autentisering, Active Directory för din domän måste vara federerade med Azure Active Directory. Ditt klientprogram (eller en tjänst) ansluter toohello databasen måste köras på en domänansluten dator under en användares domänautentiseringsuppgifter.

tooconnect tooa en databas med hjälp av integrerad autentisering och en Azure AD-identitet, hello autentisering nyckelord i hello databasanslutningssträng måste anges tooActive Directory Integrated. hello använder följande kodexempel för C# ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Integrated; Initial Catalog=testdb;";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Hej anslutningssträngsnyckelordet ``Integrated Security=True`` stöds inte för att ansluta tooAzure SQL-databas. När du gör en ODBC-anslutning du behöver tooremove blanksteg och ange autentisering too'ActiveDirectoryIntegrated'.

### <a name="active-directory-password-authentication"></a>Active Directory-lösenordsautentisering

tooconnect tooa en databas med hjälp av integrerad autentisering och en Azure AD-identitet, hello autentisering nyckelordet måste anges tooActive Directory-lösenord. hello anslutningssträngen måste innehålla användar-ID: T/UID och lösenord/PWD nyckelord och värden. hello använder följande kodexempel för C# ADO .NET.

```
string ConnectionString =
@"Data Source=n9lxnyuzhv.database.windows.net; Authentication=Active Directory Password; Initial Catalog=testdb;  UID=bob@contoso.onmicrosoft.com; PWD=MyPassWord!";
SqlConnection conn = new SqlConnection(ConnectionString);
conn.Open();
```

Lär dig mer om Azure AD-autentiseringsmetoder som använder hello demo-kodexempel finns på [Azure AD Authentication GitHub Demo](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/security/azure-active-directory-auth).

## <a name="azure-ad-token"></a>Azure AD-token
Den här autentiseringsmetoden låter mellannivå services tooconnect tooAzure SQL-databas eller Azure SQL Data Warehouse genom att hämta en token från Azure Active Directory (AAD). Det gör det möjligt för avancerade scenarier inklusive certifikatbaserad autentisering. Du måste slutföra fyra grundläggande steg toouse Azure AD-tokenautentisering:

1. Registrera ditt program med Azure Active Directory och hämta hello klient-id för din kod. 
2. Skapa en databas som representerar hello användarprogram. (Utföra tidigare i steg 6).
3. Skapa ett certifikat på hello klienten datorn kör hello program.
4. Lägga till hello certifikatet som en nyckel för ditt program.

Exempel anslutningssträng:

```
string ConnectionString =@"Data Source=n9lxnyuzhv.database.windows.net; Initial Catalog=testdb;"
SqlConnection conn = new SqlConnection(ConnectionString);
connection.AccessToken = "Your JWT token"
conn.Open();
```

Mer information finns i [SQL Server-Säkerhetsblogg](https://blogs.msdn.microsoft.com/sqlsecurity/2016/02/09/token-based-authentication-support-for-azure-sql-db-using-azure-ad-auth/).

### <a name="sqlcmd"></a>sqlcmd

Hej följande instruktioner, ansluta med version 13,1 av sqlcmd som är tillgängliga från hello [Download Center](http://go.microsoft.com/fwlink/?LinkID=825643).

```
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net  -G  
sqlcmd -S Target_DB_or_DW.testsrv.database.windows.net -U bob@contoso.com -P MyAADPassword -G -l 30
```

## <a name="next-steps"></a>Nästa steg
- En översikt över åtkomst och kontroll i SQL Database finns i [Åtkomst och kontroll för SQL Database](sql-database-control-access.md).
- En översikt över inloggningar, användare och databasroller i SQL Database finns i [Inloggningar, användare och databasroller](sql-database-manage-logins.md).
- Mer information om huvudkonton finns i [Huvudkonton](https://msdn.microsoft.com/library/ms181127.aspx).
- Mer information om databasroller finns [Databasroller](https://msdn.microsoft.com/library/ms189121.aspx).
- Mer information om brandväggsregler i SQL Database finns [SQL Database-brandväggsregler](sql-database-firewall-configure.md).

<!--Image references-->

[1]: ./media/sql-database-aad-authentication/1aad-auth-diagram.png
[2]: ./media/sql-database-aad-authentication/2subscription-relationship.png
[3]: ./media/sql-database-aad-authentication/3admin-structure.png
[4]: ./media/sql-database-aad-authentication/4select-subscription.png
[5]: ./media/sql-database-aad-authentication/5ad-settings-portal.png
[6]: ./media/sql-database-aad-authentication/6edit-directory-select.png
[7]: ./media/sql-database-aad-authentication/7edit-directory-confirm.png
[8]: ./media/sql-database-aad-authentication/8choose-ad.png
[10]: ./media/sql-database-aad-authentication/10choose-admin.png
[11]: ./media/sql-database-aad-authentication/active-directory-integrated.png
[12]: ./media/sql-database-aad-authentication/12connect-using-pw-auth2.png
[13]: ./media/sql-database-aad-authentication/13connect-to-db2.png

