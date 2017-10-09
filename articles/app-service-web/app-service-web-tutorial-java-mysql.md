---
title: aaaBuild en Java och MySQL-webbapp i Azure
description: "Lär dig hur tooget en Java-app som ansluter toohello Azure MySQL database-tjänsten fungerar i Azure apptjänst."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 0820ee9c2b7bf8fcaa22287c27a7ab848a1c4927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Skapa en Java och MySQL-webbapp i Azure

Den här kursen visar hur toocreate en Java webbapp i Azure och koppla den tooa MySQL-databas. När du är klar har du en [Vårversionen Start](https://projects.spring.io/spring-boot/) program som lagrar data i [Azure-databas för MySQL](https://docs.microsoft.com/azure/mysql/overview) körs på [Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview).

![Java-app som körs i Azure apptjänst](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en MySQL-databas i Azure
> * Ansluta en exempeldatabas app toohello
> * Distribuera hello app tooAzure
> * Uppdatera och distribuera hello app
> * Dataströmmen diagnostiska loggar från Azure
> * Övervaka hello app i hello Azure-portalen


## <a name="prerequisites"></a>Krav

1. [Hämta och installera Git](https://git-scm.com/)
1. [Hämta och installera hello JDK för Java-7 eller senare](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Hämta, installera och starta MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="prepare-local-mysql"></a>Förbereda lokala MySQL 

I det här steget skapar du en databas i en lokal MySQL-server för användning i tester hello appen lokalt på din dator.

### <a name="connect-toomysql-server"></a>Anslut tooMySQL server

Anslut tooyour lokala MySQL-servern i ett terminalfönster. Du kan använda den här terminalfönster toorun alla hello-kommandon i den här självstudiekursen.

```bash
mysql -u root -p
```

Om du uppmanas att ange ett lösenord anger hello lösenordet för hello `root` konto. Om du inte kommer ihåg rotlösenordet, se [MySQL: hur tooReset hello Rotlösenordet](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Om kommandot körs utan problem, körs MySQL-servern redan. Om inte, kontrollera att den lokala MySQL-servern har startats med följande hello [MySQL efter installationssteg](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database"></a>Skapa en databas 

I hello `mysql` uppmanar, skapa en databas och en tabell för hello arbetsuppgifter.

```sql
CREATE DATABASE tododb;
```

Avsluta anslutningen till servern genom att skriva `quit`.

```sql
quit
```

## <a name="create-and-run-hello-sample-app"></a>Skapa och köra hello sample-appen 

I det här steget klona exempelappen källan Start, konfigurera den lokala MySQL-databas i toouse hello och körs på datorn. 

### <a name="clone-hello-sample"></a>Klona hello-exempel

Navigera i hello terminalfönster, tooa working directory och klona hello exempel lagringsplatsen. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-hello-app-toouse-hello-mysql-database"></a>Konfigurera hello app toouse hello MySQL-databas

Uppdatera hello `spring.datasource.password` och värde i *spring-boot-mysql-todo/src/main/resources/application.properties* med hello används samma rotlösenordet tooopen hello MySQL prompten:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-hello-sample"></a>Skapa och köra hello-exempel

Skapa och köra hello exempel med hjälp av hello Maven wrapper ingår i hello lagringsplatsen:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Öppna din webbläsare toohttp://localhost:8080 toosee i hello exemplet i åtgärden. När du lägger till toohello uppgiftslistan använda hello följande SQL-kommandon i hello MySQL fråga tooview hello data som lagras i MySQL.

```SQL
use testdb;
select * from todo_item;
```

Stoppa hello program genom att träffa `Ctrl` + `C` i hello terminal. 

## <a name="create-an-azure-mysql-database"></a>Skapa en Azure MySQL-databas

I det här steget skapar du en [Azure-databas för MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) -instans med hello [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). Du konfigurerar hello exempel programmet toouse den här databasen senare i hello kursen.

Använd hello Azure CLI 2.0 i ett terminalfönster toocreate hello resurser behövs toohost Java-program i Azure apptjänst. Logga in tooyour Azure-prenumeration med hello [az inloggningen](/cli/azure/#login) kommando och följ hello på skärmen riktningar. 

```azurecli-interactive 
az login 
```   

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en [resursgruppen](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create) kommando. En Azure-resursgrupp är en logisk behållare där relaterade resurser som webbappar, databaser och storage-konton distribueras och hanteras. 

hello följande exempel skapas en resursgrupp i hello Nordeuropa region:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

toosee hello möjliga värden du kan använda för `--location`, använda hello [az apptjänst lista-platser](/cli/azure/appservice#list-locations) kommando.

### <a name="create-a-mysql-server"></a>Skapa en MySQL-server

Skapa en server i Azure-databas för MySQL (förhandsversion) med hello [az mysql-servern skapa](/cli/azure/mysql/server#create) kommando.    
Ersätt egna unika MySQL namnet på servern där du ser hello `<mysql_server_name>` platshållare. Det här namnet är en del av MySQL-serverns värdnamn, `<mysql_server_name>.mysql.database.azure.com`, så den behöver toobe globalt unika. I stället använda `<admin_user>` och `<admin_password>` med egna värden.

```azurecli-interactive
az mysql server create --name <mysql_server_name> \ 
    --resource-group myResourceGroup \ 
    --location "North Europe" \
    --admin-user <admin_user> \ 
    --admin-password <admin_password>
```

När hello MySQL server skapas visar hello Azure CLI information liknande toohello följande exempel:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Konfigurera server-brandväggen

Skapa en brandväggsregel för MySQL server tooallow klienten anslutningar med hello [az mysql-brandväggsregel skapa](/cli/azure/mysql/server/firewall-rule#create) kommando. 

```azurecli-interactive
az mysql server firewall-rule create \
    --name allIPs \
    --server <mysql_server_name>  \ 
    --resource-group myResourceGroup \ 
    --start-ip-address 0.0.0.0 \ 
    --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure-databas för MySQL (förhandsversion) kan inte för närvarande automatiskt anslutningar från Azure-tjänster. IP-adresser i Azure tilldelas dynamiskt, är det bättre tooenable alla IP-adresser för nu. Eftersom hello tjänsten fortsätter förhandsgranskningen, aktiveras bättre metoder för att skydda databasen.

## <a name="configure-hello-azure-mysql-database"></a>Konfigurera hello Azure MySQL-databas

Anslut toohello MySQL-server i Azure i hello terminalfönster på datorn. Använd hello-värde som du angav tidigare för `<admin_user>` och `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Skapa en databas 

I hello `mysql` uppmanar, skapa en databas och en tabell för hello arbetsuppgifter.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Skapa en användare med behörighet

Skapa en databasanvändare och ge den alla behörigheter i hello `tododb` databas. Ersätt platshållarna hello `<Javaapp_user>` och `<Javaapp_password>` med dina egna unika namn.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* too'<Javaapp_user>';
```

Avsluta anslutningen till servern genom att skriva `quit`.

```sql
quit
```

## <a name="deploy-hello-sample-tooazure-app-service"></a>Distribuera hello exempel tooAzure Apptjänst

Skapa en Azure App Service-plan med hello **lediga** prisnivån med hello [az programtjänstplan skapa](/cli/azure/appservice/plan#create) CLI-kommando. Hej programtjänstplan definierar hello fysiska resurser som används toohost dina appar. Alla program som tilldelats tooan programtjänstplan dela dessa resurser, så att du toosave kostnad när värd för flera appar. 

```azurecli-interactive
az appservice plan create \
    --name myAppServicePlan \ 
    --resource-group myResourceGroup \
    --sku FREE
```

När hello plan är klar, utdata hello Azure CLI visar liknande toohello följande exempel:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Skapa en Azure-webbapp

 Använd hello [az webapp skapa](/cli/azure/appservice/web#create) CLI kommandot toocreate en web app definition i hello `myAppServicePlan` App Service-plan. hello web app definition innehåller en URL-tooaccess ditt program med och konfigurerar flera alternativ toodeploy tooAzure din kod. 

```azurecli-interactive
az webapp create \
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --plan myAppServicePlan
```

Ersätt hello `<app_name>` med dina egna unika namn. Detta unika namn är en del av hello standarddomännamnet för hello webbprogrammet så hello namn måste toobe unikt över alla program i Azure. Du kan mappa ett webbprogram för domänen namnet post toohello innan exponera tooyour användare.

När hello web app definition är klar visar hello Azure CLI information liknande toohello följande exempel: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Konfigurera Java 

Konfigurera hello Java runtime konfiguration som din app behöver med hello [uppdatering az apptjänst web-config](/cli/azure/appservice/web/config#update) kommando.

hello följande kommando konfigurerar hello web app toorun på en senaste Java 8 JDK och [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set \ 
    --name <app_name> \
    --resource-group myResourceGroup \ 
    --java-version 1.8 \ 
    --java-container Tomcat \
    --java-container-version 8.0
```

### <a name="configure-hello-app-toouse-hello-azure-sql-database"></a>Konfigurera hello app toouse hello Azure SQL-databas

Ange inställningar för program innan du kör hello sample-appen på hello web app toouse hello Azure MySQL-databas du skapade i Azure. De här egenskaperna är exponerade toohello webbprogram som miljövariabler och åsidosätta hello värdena i hello application.properties inuti hello paketerade webbprogrammet. 

Ange inställningar för program med hjälp av [az webapp config appsettings](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) i hello CLI:

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" \
    --resource-group myResourceGroup \
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name  \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set \
    --settings SPRING_DATASOURCE_URL=Javaapp_password \
    --resource-group myResourceGroup \ 
    --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>Hämta autentiseringsuppgifter för FTP-distribution 
Du kan distribuera ditt program tooAzure apptjänst på olika sätt, inklusive FTP, lokal Git, GitHub, Visual Studio Team Services och BitBucket. FTP-toodeploy hello i det här exemplet. WAR-filen tidigare bygger på din lokala dator tooAzure Apptjänst.

toodetermine vad autentiseringsuppgifter toopass längs i en FTP-kommandot toohello Web App används [az apptjänst web distributionsåtgärder lista-publicering-profiler](https://docs.microsoft.com/cli/azure/appservice/web/deployment#list-publishing-profiles) kommando: 

```azurecli-interactive
az webapp deployment list-publishing-profiles \ 
    --name <app_name> \ 
    --resource-group myResourceGroup \
    --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" \ 
    --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-hello-app-using-ftp"></a>Överför hello-app med FTP

Använd din favorit toodeploy hello för FTP-verktyget. WAR-filen toohello */site/wwwroot/webapps* mapp på hello serveradress från hello `URL` i hello föregående kommando. Ta bort hello befintliga (rot) programkatalogen och Ersätt hello befintliga ROOT.war med hello. Inbyggda hello tidigare i kursen hello WAR-filen.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected toowaws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-hello-web-app"></a>Testa hello-webbprogram

Bläddra för`http://<app_name>.azurewebsites.net/` och lägga till några uppgifter toohello lista. 

![Java-app som körs i Azure apptjänst](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Grattis!** Du kör en datadrivna Java-app i Azure App Service.

## <a name="update-hello-app-and-redeploy"></a>Uppdatera hello app och distribuera om

Uppdatera hello programmet tooinclude ytterligare en kolumn i hello todo-listan för vilken dag hello objektet skapades. Källan Start hanterar uppdatering hello-databasschemat för dig som hello datamodelländringar utan att ändra din befintliga databasposter.

1. I det lokala systemet öppna *src/main/java/com/example/fabrikam/TodoItem.java* och Lägg till följande hello importerar toohello klass:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Lägg till en `String` egenskapen `timeCreated` för*src/main/java/com/example/fabrikam/TodoItem.java*, initierar med en tidsstämpel när objektet skapas. Lägg till Get/Set-metoder för hello nya `timeCreated` egenskapen när du redigerar filen.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Uppdatera *src/main/java/com/example/fabrikam/TodoDemoController.java* med en rad i hello `updateTodo` metoden tooset hello tidsstämpel:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Lägga till stöd för nya hello-fältet i hello Thymeleaf mall. Uppdatera *src/main/resources/templates/index.html* med ett nytt huvud för hello tidsstämpel och ett nytt fält toodisplay hello-värde för hello tidsstämpeln i varje datarad i tabellen.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Återskapa hello program:

    ```bash
    mvnw clean package 
    ```

6. FTP-hello uppdateras. WAR som tidigare kan ta bort befintliga hello *plats/wwwroot/webbappar/ROOT* directory och *ROOT.war*, och sedan ladda upp hello uppdateras. WAR-filen som ROOT.war. 

När du uppdaterar hello appen, en **Skapad** kolumnen visas nu. När du lägger till en ny uppgift fyller hello app hello tidsstämpel automatiskt. Din befintliga aktiviteter förblir oförändrad och arbeta med hello appen även om hello underliggande datamodellen har ändrats. 

![Java-app som har uppdaterats med en ny kolumn](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Dataströmmen diagnostikloggar 

När Java-programmet körs i Azure App Service kan du få hello konsolen loggar skickas direkt tooyour terminal. På så sätt kan du hello diagnostiska meddelanden för samma toohelp du felsöka programfel.

toostart loggen direktuppspelning, Använd hello [az webapp loggen pilslut](/cli/azure/appservice/web/log#tail) kommando.

```azurecli-interactive 
az webapp log tail \
    --name <app_name> \
    --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Hantera Azure-webbapp

Gå toohello Azure portal toosee hello webbappen du skapade.

toodo, logga in för[https://portal.azure.com](https://portal.azure.com).

Hello vänstra menyn klickar du på **Apptjänst**, klicka på hello namnet på din Azure webbapp.

![Portalen navigering tooAzure webbprogram](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Som standard visas din webbapps blad hello **översikt** sidan. På den här sidan får du en översikt över hur det går för appen. Här kan utföra du också hanteringsuppgifter som att stoppa, starta, starta om och ta bort. hello flikar hello vänster på hello bladet visar hello annan konfigurationssidor som du kan öppna.

![App Service-blad på Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Flikarna i hello bladet innehåller hello många bra funktioner som du kan lägga till tooyour webbprogram. hello följande lista innehåller några av hello möjligheter:
* Mappa ett anpassat DNS-namn
* Bind ett anpassat SSL-certifikat
* Konfigurera kontinuerlig distribution
* Skala upp
* Lägg till användarautentisering

## <a name="clean-up-resources"></a>Rensa resurser

Om du inte behöver dessa resurser för en annan självstudiekursen (se [nästa steg](#next)), kan du ta bort dem genom att köra följande kommando hello: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Nästa steg

> [!div class="checklist"]
> * Skapa en MySQL-databas i Azure
> * Ansluta en exempel Java-app toohello MySQL
> * Distribuera hello app tooAzure
> * Uppdatera och distribuera hello app
> * Dataströmmen diagnostiska loggar från Azure
> * Hantera hello appen i hello Azure-portalen

I förväg toohello nästa självstudiekurs toolearn hur toomap en anpassad DNS namn toohello app.

> [!div class="nextstepaction"] 
> [Mappa en befintlig anpassad DNS-namnet tooAzure Web Apps](app-service-web-tutorial-custom-domain.md)
