---
title: "Snabbstart: Skapa Azure-databas för MySQL-server – Azure Portal | Microsoft Docs"
description: "Den här artikeln steg som du använder hello Azure portal tooquickly skapa ett exempel på Azure-databas för MySQL-servern om fem minuter."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.custom: mvc
ms.topic: hero-article
ms.date: 08/15/2017
ms.openlocfilehash: d5754fe7a6f0f0f4b3fa19d456c4e15e64ca396c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Skapa en Azure Database för MySQL med Azure Portal
Azure MySQL-databas är en hanterad tjänst som gör att du toorun, hantera och skala högtillgänglig MySQL-databaser i hello molnet. Den här snabbstarten visar hur toocreate en Azure-databas för MySQL-server med hello Azure-portalen om fem minuter. 

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

## <a name="log-in-tooazure"></a>Logga in tooAzure
Öppna webbläsaren och gå toohello [Microsoft Azure-portalen](https://portal.azure.com/). Ange dina autentiseringsuppgifter toosign toohello-portalen. hello standardvyn är instrumentpanelen service.

## <a name="create-azure-database-for-mysql-server"></a>Skapa en Azure-databas för MySQL-servern
En Azure Database för MySQL-server skapas med en definierad uppsättning [compute- och lagringsresurser](./concepts-compute-unit-and-storage.md). hello server skapas inom en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md).

Följ dessa steg toocreate en Azure-databas för MySQL-server:

1. Klicka på hello **ny** knappen (+) finns på hello övre vänstra hörnet av hello Azure-portalen.

2. Välj **databaser** från hello **ny** och väljer **Azure-databas för MySQL** från hello **databaser** sidan. Du kan också skriva **MySQL** i hello ny sida rutan toofind hello söktjänsten.
![Azure Portal – ny – databas – MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

3. Fyll i formuläret om hello nya servern information med hello följande information som visas i föregående bild hello:

    **Inställning** | **Föreslaget värde** | **Fältbeskrivning** 
    ---|---|---
    servernamn | myserver4demo | Välj ett unikt namn för Azure Database för MySQL-server. hello domännamn *mysql.database.azure.com* är tillagda toohello servernamn som du anger för program tooconnect till. hello servernamnet får innehålla endast små bokstäver, siffror och hello bindestreck (-) och det måste innehålla mellan 3 och 63 tecken.
    Prenumeration | Din prenumeration | hello Azure-prenumeration som du vill toouse för servern. Om du har flera prenumerationer, Välj hello lämpliga prenumeration där hello resurs debiteras för.
    Resursgrupp | myresourcegroup | Du kan skapa ett nytt resursgruppnamn eller använda ett befintligt namn i prenumerationen.
    inloggning för serveradministratör | myadmin | Se egna inloggningen konto toouse när du ansluter toohello server. Hej administratör inloggningsnamnet får inte vara 'azure_superuser', ”admin”, 'administratör', 'rot', 'gäst' eller 'public'.
    Lösenord | *Ditt val* | Skapa ett nytt lösenord för administratörskontot för hello-server. Måste innehålla från 8 too128 tecken. Lösenordet måste innehålla tecken från tre av hello följande kategorier-engelska versala bokstäver, engelska gemena bokstäver, siffror (0-9) och icke-alfanumeriska tecken (!, $, #, % osv.).
    Bekräfta lösenord | *Ditt val*| Bekräfta hello admin kontolösenord.
    Plats | *hello region närmaste tooyour användare*| Välj hello-plats som är närmast tooyour användare eller andra Azure-program.
    Version | *Välj hello senaste version*| Välj hello senaste versionen om du inte har särskilda krav.
    Prisnivå | **Basic**, **50 Compute-enheter** **50 GB** | Klicka på **prisnivå** toospecify hello tjänstnivå och prestandanivå servicenivå för den nya databasen. Välj **grundläggande nivån** hello fliken hello överst. Klicka på hello vänstra ände hello **Compute enheter** skjutreglaget tooadjust hello värdet toohello minst belopp tillgängliga för denna Snabbstart. Klicka på **Ok** toosave hello priser för val av. Se hello följande skärmbild.
    PIN-kod toodashboard | Markera | Kontrollera hello **PIN-kod toodashboard** alternativet tooallow enkel spårning av din server på hello främre instrumentpanelssida på Azure-portalen.

    > [!IMPORTANT]
    > hello inloggning för serveradministratör och lösenord som du anger här är nödvändig toolog i toohello server och dess databaser senare i den här snabbstarten. Kom ihåg eller skriv ned den här informationen så att du kan använda den senare.
    > 

    ![Azure portal – skapa MySQL genom att tillhandahålla hello krävs formulärindata](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

4.  Klicka på **skapa** tooprovision hello server. Etablering tar några minuter, in too20 minuter maximalt.
   
5.  På verktygsfältet hello **meddelanden** (klockikonen) toomonitor hello distributionsprocessen.

## <a name="configure-a-server-level-firewall-rule"></a>Konfigurera en brandväggsregel på servernivå

hello Azure-databas för MySQL-tjänst som skapar en brandvägg på servernivå för hello. Den här brandväggen förhindrar externa program och verktyg ansluter toohello server och alla databaser på servern hello, såvida inte en brandväggsregel skapas tooopen hello-brandväggen för specifika IP-adresser. 

1.  Leta upp din server när hello distributionen är klar. Om det behövs kan du söka efter den. Klicka till exempel **alla resurser** från hello vänstra menyn och Skriv hello servernamn (till exempel hello exempel *myserver4demo*) toosearch för den nya servern. Klicka på namnet på servern som anges i hello sökresultatet. Hej **översikt** sidan för servern öppnas och visar alternativ för ytterligare konfiguration.

2. Hello server på sidan Välj **anslutningssäkerhet**.

3.  Under hello **regler i brandväggen** rubrik, klicka i hello tom textruta i hello **Regelnamn** kolumnen toobegin skapar hello brandväggsregel. 

    Denna Snabbstart vi tillåter alla IP-adresser i hello-servern genom att fylla i hello textruta i varje kolumn med hello följande värden:

    Regelnamn | Start-ip | Slut-ip 
    ---|---|---
    AllowAllIps |  0.0.0.0 | 255.255.255.255

4. Hello övre verktygsfältet för hello **anslutningssäkerhet** klickar du på **spara**. Vänta en stund och meddelande hello-meddelande visar att uppdatera anslutningssäkerhet har slutförts innan du fortsätter.

    > [!NOTE]
    > Anslutningar tooAzure för MySQL-databas kommunicerar via port 3306. Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 3306 inte av ditt nätverks brandvägg. I så fall, blir inte kan tooconnect tooyour server om din IT-avdelning öppnar port 3306.
    > 

## <a name="get-hello-connection-information"></a>Hämta hello anslutningsinformation
tooconnect tooyour databasserver, behöver du tooremember hello fullständig server name och admin inloggningsuppgifter. Du kan märka dessa värden i hello Quickstart ovan. Om du inte gjorde du lätt kan hitta hello server name och logga in information från hello server **översikt** sida eller hello **egenskaper** sida i hello Azure-portalen.

1. Öppna serverns **Översikt**-sida. Anteckna hello **servernamn** och **serverinloggningsnamnet för admin**. 
    Håller markören över varje fält och hello kopiera-ikonen visas toohello höger hello text. Klicka på hello kopiera-ikonen som behövs toocopy hello värden.

    I det här exemplet hello servernamnet är *myserver4demo.mysql.database.azure.com*, och hello inloggning för serveradministratör är  *myadmin@myserver4demo* .

## <a name="connect-toomysql-using-mysql-command-line-tool"></a>Ansluta med hjälp av kommandoradsverktyget för mysql tooMySQL
Det finns flera program kan du använda tooconnect tooyour Azure-databas för MySQL-servern. Först ska vi använda hello [mysql](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) kommandoradsverktyget verktyget tooillustrate hur tooconnect toohello server.  Du kan använda en webbläsare och hello Azure Cloud Shell som beskrivs här utan hello måste tooinstall ytterligare programvara. Om du har installerats lokalt på din egen dator hello mysql-verktyget kan ansluta du samt i därifrån.

1. Starta hello Azure Cloud Shell via terminal hello-ikonen (> _) på hello uppifrån höger om hello Azure portal webbsida.

2. hello Azure Cloud Shell öppnas i webbläsaren, vilket gör att du tootype bash shell-kommandon.

    ![Kommandotolk – exempel på mysql-kommandorad](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

3. I Kommandotolken hello molnet Shell ansluta tooyour Azure-databas för MySQL-servern genom att ange hello mysql-kommandoraden hello grön i Kommandotolken.

    hello följande format är används tooconnect tooan Azure-databas för MySQL-server med hello mysql-verktyget:
    ```bash
    mysql --host <yourserver> --user <server admin login> --password
    ```

    Till exempel ansluter följande kommando hello tooour exempel server:
    ```azurecli-interactive
    mysql --host myserver4demo.mysql.database.azure.com --user myadmin@myserver4demo --password
    ```

    mysql-parameter |Föreslaget värde|Beskrivning
    ---|---|---
    --host | *servernamn* | Ange hello server namn-värde som används när du skapade hello Azure-databas för MySQL tidigare. Exempelservern som visas är myserver4demo.mysql.database.azure.com. Använd hello fullständigt kvalificerade domännamnet (\*. mysql.database.azure.com) enligt hello exempel. Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg namnet på servern. 
    --användare | *inloggning för serveradministratör* |Skriv i hello server inloggningen administratörsanvändarnamnet angavs när du skapade hello Azure-databas för MySQL tidigare. Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg hello användarnamn.  hello-formatet är  *username@servername* .
    --lösenord | *vänta på uppmaning* | Du uppmanas att för ”ange lösenord” när du anger hello-kommandot. När du uppmanas ange hello samma lösenord som du angav när du skapade hello-server.  Obs hello angett tecken inte visas på hello bash fråga när du skriver dem lösenord. Tryck på RETUR när du har angett alla hello tecken tooauthenticate och ansluta.

   När du är ansluten, hello mysql-verktyget visar en `mysql>` fråga efter du tootype kommandon. 

    Exempel på mysql-utdata:
    ```bash
    Welcome toohello MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 65505
    Server version: 5.6.26.0 MySQL Community Server (GPL)
    
    Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.

    Type 'help;' or '\h' for help. Type '\c' tooclear hello current input statement.
    
    mysql>
    ```
    > [!TIP]
    > Om hello-brandväggen inte är konfigurerad tooallow hello IP-adressen för hello Azure Cloud Shell hello följande fel inträffar:
    >
    > FEL 2003 (28000): Klienten med IP-adressen 123.456.789.0 tillåts inte tooaccess hello server.
    >
    > tooresolve hello fel, kontrollera att hello server configuration matchar hello stegen i hello *konfigurera en brandväggsregel på servernivå* i hello artikeln.

4. Visa status tooensure hello serveranslutning fungerar. Typen `status` på hello mysql > fråga när den är ansluten.
    ```sql
    status
    ```

   > [!TIP]
   > Fler kommandon finns i [referenshandboken för MySQL 5.7 – kapitel 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

5.  Skapa en tom databas på hello mysql > fråga genom att skriva följande kommando hello:
    ```sql
    CREATE DATABASE quickstartdb;
    ```
    hello-kommandot kan ta några ögonblick toocomplete. 

    Du kan skapa en eller flera databaser på en Azure Database för MySQL-server. Du kan välja toocreate en enskild databas per server tooutilize alla hello resurser eller skapa flera databaser tooshare hello resurser. Det finns ingen gräns toohello antalet databaser som kan skapas, men flera databaser dela hello resurser på samma server. 

6. Lista hello databaser på hello mysql > fråga genom att skriva följande kommando hello:

    ```sql
    SHOW DATABASES;
    ```

7.  Typen `\q` och tryck sedan på RETUR tooquit hello mysql-verktyget. När du är klar kan du stänga hello Azure Cloud-gränssnittet.

Du har nu anslutna toohello Azure-databas för MySQL och skapat en tom databas. Fortsätta toohello nästa avsnitt toorepeat en liknande Övning tooconnect toohello samma server med ett annat gemensamma verktyg, MySQL-arbetsstationen.

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a>Ansluta toohello servern med hjälp av hello MySQL arbetsstationen GUI-verktyg
tooconnect tooAzure MySQL server verktyget hello GUI MySQL arbetsstationen:

1.  Starta hello MySQL arbetsstationen programmet på klientdatorn. Du kan ladda ned och installera MySQL Workbench [här](https://dev.mysql.com/downloads/workbench/).

2.  I **installera ny anslutning** dialogrutan Ange följande information hello **parametrar** fliken:

    ![konfigurera ny anslutning](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

    | **Inställning** | **Föreslaget värde** | **Fältbeskrivning** |
    |---|---|---|
    |   Anslutningsnamn | Demoanslutning | Ange ett namn på anslutningen. |
    | Anslutningsmetod | Standard (TCP/IP) | Standard (TCP/IP) är tillräckligt. |
    | Värdnamn | *servernamn* | Ange hello server namn-värde som används när du skapade hello Azure-databas för MySQL tidigare. Exempelservern som visas är myserver4demo.mysql.database.azure.com. Använd hello fullständigt kvalificerade domännamnet (\*. mysql.database.azure.com) enligt hello exempel. Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg namnet på servern.  |
    | Port | 3306 | Använd alltid port 3306 vid anslutning tooAzure databas för MySQL. |
    | Användarnamn |  *inloggning för serveradministratör* | Skriv i hello server inloggningen administratörsanvändarnamnet angavs när du skapade hello Azure-databas för MySQL tidigare. Vår användarnamn i exemplet är myadmin@myserver4demo. Åtgärderna hello i hello föregående avsnitt tooget hello anslutningsinformationen om du inte kommer ihåg hello användarnamn. hello-formatet är  *username@servername* .
    | Lösenord | ditt lösenord | Klicka i valvet... knappen toosave hello lösenordet lagras. |

    Klicka på **Testanslutningen** tootest om alla parametrar är korrekt konfigurerade. Klicka på OK toosave hello anslutning. 

    > [!NOTE]
    > SSL används som standard på din server och kräver ytterligare konfiguration i ordning tooconnect har. Se [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](./howto-configure-ssl.md).  Om du vill toodisable SSL för den här snabbstarten finns hello Azure-portalen och klicka hello anslutning säkerhet sidan toodisable hello framtvinga SSL-anslutning växla.

## <a name="clean-up-resources"></a>Rensa resurser
Rensa hello-resurser som du skapade i hello quickstart antingen genom att ta bort hello [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md), som innehåller alla hello resurser i hello resursgruppen eller resursen hello en server om du vill använda tookeep hello andra resurser är intakt.

> [!TIP]
> De andra snabbstarterna i den här samlingen bygger på den här snabbstarten. Om du tänker toocontinue toowork med efterföljande hello Snabbstart, vill inte rensa resurser som skapats i denna Snabbstart. Om du inte planerar toocontinue använda hello efter steg toodelete alla resurser som skapats av denna Snabbstart i hello Azure-portalen.
>

toodelete hello hela resursgruppen inklusive hello nyskapad server:
1.  Leta upp din resursgrupp i hello Azure-portalen. Hello vänstra menyn i hello Azure-portalen klickar du på **resursgrupper** och klicka sedan på hello namnet på resursgruppen, till exempel vårt exempel **myresourcegroup**.
2.  Klicka på **Ta bort** på din resursgruppssida. Sedan hello-typnamn för resursgruppen, till exempel vårt exempel **myresourcegroup**i hello text rutan tooconfirm tas bort och klicka sedan på **ta bort**.

Eller i stället toodelete hello nyskapad server:
1.  Leta upp din server i hello Azure-portalen om du inte har öppnat. Hello vänstra menyn i Azure-portalen klickar du på **alla resurser**, och sök sedan efter hello-server som du skapade.
2.  På hello **översikt** klickar du på hello **ta bort** hello övre fönstret-knappen.
![Azure Database för MySQL – Ta bort server](./media/quickstart-create-mysql-server-database-using-azure-portal/delete-server.png)
3.  Bekräfta hello servernamnet du vill toodelete och visa hello databaserna under den som påverkas. Skriv namnet på servern hello i textrutan, till exempel vårt exempel **myserver4demo**, och klicka sedan på **ta bort**.

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Skapa din första Azure-databas för MySQL](./tutorial-design-database-using-portal.md)

