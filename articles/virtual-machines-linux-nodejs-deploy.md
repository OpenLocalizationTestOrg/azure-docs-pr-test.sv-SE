---
title: aaaDeploy en Node.js-programmet tooLinux virtuella datorer i Azure
description: "Lär dig hur toodeploy en Node.js programmet tooLinux virtuella datorer i Azure."
services: 
documentationcenter: nodejs
author: stepro
manager: dmitryr
editor: 
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: /azure
ms.assetid: 857a812d-c73e-4af7-a985-2d0baf8b6f71
ms.service: multiple
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2016
ms.author: stephpr
ms.openlocfilehash: e876c8170a06f102f7f32ec7cb930b876647a707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-nodejs-application-toolinux-virtual-machines-in-azure"></a>Distribuera virtuella datorer i Azure för tooLinux en Node.js-program
Den här kursen visar hur tootake ett Node.js-program och distribuerar den tooLinux virtuella datorer som körs i Azure. hello anvisningarna i den här kursen kan tillämpas på alla operativsystem som kan köra Node.js.

Du lär dig hur du:

* Förgrening och klona en GitHub-databas som innehåller en enkel TODO ansökan.
* Skapa och konfigurera två virtuella Linux-datorer i Azure toorun hello program.
* Iterera hello program genom att trycka på uppdateringar toohello web klientdel virtuell dator.

> [!NOTE]
> toocomplete den här självstudiekursen kommer du behöver en GitHub-konto och ett Microsoft Azure-konto och hello möjlighet toouse Git från en utvecklingsdator.
> 
> Om du inte har en GitHub-konto kan du registrera dig [här](https://github.com/join).
> 
> Om du inte har en [Microsoft Azure](https://azure.microsoft.com/) konto, du kan registrera dig för en kostnadsfri utvärderingsversion [här](https://azure.microsoft.com/pricing/free-trial/). Detta också leder dig igenom hello inloggningsprocessen för en [Account](http://account.microsoft.com) om du inte redan har en. Om du prenumererar på Visual Studio, du kan också [aktivera MSDN-förmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
> 
> Om du inte har git på utvecklingsdatorn om du använder en Macintosh eller Windows-dator, installerar git från [här](http://www.git-scm.com). Om du använder Linux, installera git med hello mekanism som är bäst för dig, till exempel `sudo apt-get install git`.
> 
> 

## <a name="forking-and-cloning-hello-todo-application"></a>Onödiga kostnader och klona hello TODO-program
hello TODO-program som används av den här kursen implementerar en enkel web-klientdel över en MongoDB-instans som övervakas av en att göra-lista. Efter inloggningen tooGitHub [här](https://github.com/stepro/node-todo) toofind hello program och duplicera den med hello länken i hello uppe till höger. Detta bör du skapa en databas i ditt konto med namnet *accountname*/node-todo.

Nu på utvecklingsdatorn, klona lagringsplatsen:

    git clone https://github.com/accountname/node-todo.git

Vi använder den här lokala kloning av hello databasen lite senare när du ändrar toohello källkoden.

## <a name="creating-and-configuring-hello-linux-virtual-machines"></a>Skapa och konfigurera hello virtuella Linux-datorer
Azure har bra stöd för raw beräkning med hjälp av virtuella Linux-datorer. Den här delen av hello självstudiekurs visar hur du kan lätt få igång två virtuella Linux-datorer och distribuerar hello TODO programmet toothem kör hello web-klientdel på en och hello hello andra MongoDB-instansen.

### <a name="creating-virtual-machines"></a>Skapa virtuella datorer
hello enklaste sättet toocreate en ny virtuell dator i Azure är toouse hello Azure-portalen. Klicka på [här](https://portal.azure.com) toosign i och starta hello Azure-portalen i webbläsaren. När hello Azure-portalen har lästs in, Slutför hello följande steg:

* Klicka på hello ”+ nytt” länka;
* Välj hello ”Compute” kategori och välj ”Ubuntu Server 14.04 LTS”;
* Välj distributionsmodell för hello ”Resource Manager” och klicka på ”Skapa”;
* Fyll i hello grunderna följa dessa riktlinjer:
  * Ange ett namn som du lätt kan identifiera senare.
  * För den här kursen Välj lösenordsautentisering;
  * Skapa en ny resursgrupp med ett namn som kan identifieras.
* För hello storlek för virtuell dator är ”A1 Standard” ett rimligt val för den här självstudiekursen.
* För ytterligare inställningar, kontrollera hello disktyp är ”Standard” och acceptera alla hello återstående standardvärdena.
* Startar hello skapas på hello sammanfattningssidan.

Utför ovanstående hello bearbeta två gånger toocreate två virtuella Linux-datorer, en för hello web klientdel och en för hello MongoDB-instansen. Skapa hello virtuella datorer tar cirka 5-10 minuter.

### <a name="assigning-a-dns-entry-for-virtual-machines"></a>Tilldela en DNS-post för virtuella datorer
Virtuella datorer som skapats i Azure är som standard endast nås via en offentlig IP-adress som 1.2.3.4. Vi behöver kontrollera hello datorer lättare kan identifieras genom att tilldela dem DNS-poster.

När hello portal anger att hello virtuella datorer har skapats, klickar du på hello ”virtuella datorer” länk i hello vänstra navigeringsfält för och leta reda på dina datorer. För varje dator:

* Leta upp hello Essentials-fliken och klicka på hello offentliga IP-adress.
* Tilldela en DNS-namnetikett i hello offentliga IP-adresskonfiguration, och spara.

hello portal säkerställer att hello-namn som du anger är tillgänglig. När du har sparat hello konfiguration, virtuella datorer har liknande värdnamn för`machinename.region.cloudapp.azure.com`.

### <a name="connecting-toohello-virtual-machines"></a>Ansluta toohello virtuella datorer
När dina virtuella datorer har etablerats var de förkonfigurerade tooallow fjärranslutningar via SSH. Detta är hello mekanism som vi använder tooconfigure hello virtuella datorer. Om du använder Windows för din utveckling behöver tooget en SSH-klienten om du inte redan har en. Ett vanligt val är PuTTY som kan hämtas från [här](http://www.chiark.greenend.org.uk/~sgtatham/putty/). Macintosh- och Linux OSes levereras med en version av SSH förinstallerat.

### <a name="configuring-hello-web-frontend-virtual-machine"></a>Konfigurera hello Web klientdel virtuell dator
SSH toohello klientdel datorn på webbservern du skapat med PuTTY, ssh kommandorad eller din andra favorit SSH-verktyget. Du bör se ett välkomstmeddelande följt av en kommandotolk.

Först se till att git och noden både är installerade:

    sudo apt-get install -y git
    curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
    sudo apt-get install -y nodejs

Eftersom hello programmets web klientdel är beroende av vissa inbyggda Node.js-moduler, måste vi också tooinstall hello grundläggande uppsättning verktyg:

    sudo apt-get install -y build-essential

Slutligen ska vi installera ett Node.js-program som kallas *alltid*, som hjälper toorun Node.js server-program:

    sudo npm install -g forever

Detta är alla hello beroenden som krävs på den här virtuella datorn toobe kan toorun hello programmets web klientdel, så det är dags att köras. toodo kan vi först skapar en utan kloning av hello GitHub-lagringsplats som du tidigare forked så att du kan enkelt publicera uppdateringar toohello virtuell dator (vi tar upp det här scenariot uppdateringen senare) och klona hello utan kloning tooprovide en version av hello databasen som faktiskt kan utföras.

Från hello arbetskatalog (~), kör följande kommandon hello (ersätter *accountname* med ditt användarkonto i GitHub):

    git clone --bare https://github.com/accountname/node-todo.git
    git clone node-todo.git

Nu ange hello nod todo-katalogen och köra följande kommandon:

    npm install
    forever start server.js

hello programmets web klientdel körs nu, men det finns ytterligare ett steg innan du kan komma åt programmet hello från en webbläsare. hello virtuell dator som du skapade skyddas av en Azure-resurs kallas en *nätverkssäkerhetsgruppen*, som skapades för dig när du etablerat hello virtuell dator. Den här resursen kan för närvarande endast externa begäranden tooport 22 toobe dirigeras toohello virtuell dator, vilket gör att SSH-kommunikation med hello datorn men inget annat. Så i ordning tooview hello TODO-program som är konfigurerade toorun på port 8080, måste den här porten också toobe öppnat.

Returnera toohello Azure-portalen och slutför hello följande steg:

* Klicka på ”resursgrupper” i vänster navigeringsfält för hello;
* Välj hello resursgruppen som innehåller den virtuella datorn;
* Välj i hello resulterande lista över resurser, hello nätverkssäkerhetsgruppen (hello en med en sköldikon);
* I hello egenskaper, väljer du ”inkommande säkerhetsregler”;
* I hello i verktygsfältet klickar du på ”Lägg till”;
* Ange ett namn som ”standard-Tillåt-todo”;
* Ange hello-protokollet för ”TCP”;
* Ange hello Målportintervall för ”8080”;
* Klicka på OK och vänta hello säkerhet regeln toobe skapas.

När du har skapat den här säkerhetsregeln hello TODO program visas offentligt på hello internet och du kan bläddra tooit, till exempel med en URL som:

    http://machinename.region.cloudapp.azure.com:8080

Du ser att även om vi ännu inte har konfigurerat hello MongoDB virtuella hello TODO program visas toobe helt funktionell. Detta beror på att hello källdatabasen är hårdkodad toouse före distribuerade MongoDB-instansen. När vi har konfigurerat hello MongoDB virtuell dator, ska vi gå tillbaka och ändra hello källa kod tooutilize våra privata MongoDB-instansen i stället.

### <a name="configuring-hello-mongodb-virtual-machine"></a>Konfigurera hello MongoDB virtuell dator
SSH toohello andra dator du skapat med PuTTY, ssh kommandorad eller din andra favorit SSH-verktyget. Installera MongoDB efter hello välkomstmeddelande och Kommandotolken, (instruktionerna togs från [här](https://docs.mongodb.org/master/tutorial/install-mongodb-on-ubuntu/)):

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
    sudo apt-get update
    sudo apt-get install -y mongodb-org

Som standard konfigureras MongoDB så att det bara kan kommas åt lokalt. För den här självstudiekursen kommer vi konfigurera MongoDB så att den kan nås från hello programmets virtuella datorn. I en kontext som sudo, öppna hello /etc/mongod.conf filen och leta upp hello `# network interfaces` avsnitt. Ändra hello `net.bindIp` configuration värdet för`0.0.0.0`.

> [!NOTE]
> Den här konfigurationen är hello avsedd endast den här kursen. Det är **inte** en rekommenderad säkerhetsrutin och ska inte användas i produktionsmiljöer.
> 
> 

Nu ska du kontrollera hello MongoDB-tjänsten har startats:

    sudo service mongod restart

MongoDB fungerar över port 27017 som standard. Så i hello samma sätt som vi behövde tooopen port 8080 på hello web klientdel virtuell dator, vi behöver tooopen port 27017 på hello MongoDB virtuella datorn.

Returnera toohello Azure-portalen och slutför hello följande steg:

* Klicka på ”resursgrupper” i vänster navigeringsfält för hello;
* Välj hello resursgruppen som innehåller hello MongoDB virtuella maskinen.
* I hello resulterande lista över resurser, väljer du hello nätverkssäkerhetsgruppen (hello en med en sköldikon) med hello samma namn som du gav toohello MongoDB virtuella maskinen.
* I hello egenskaper, väljer du ”inkommande säkerhetsregler”;
* I hello i verktygsfältet klickar du på ”Lägg till”;
* Ange ett namn som ”standard-Tillåt-mongo”;
* Ange hello-protokollet för ”TCP”;
* Ange hello Målportintervall för ”27017”;
* Klicka på OK och vänta hello säkerhet regeln toobe skapas.

## <a name="iterating-on-hello-todo-application"></a>Iteration av hello TODO-program
Hittills har vi har etablerat två virtuella Linux-datorer: en som kör hello programmets web klientdel och en som kör en MongoDB-instansen. Men det finns ett problem - hello web klientdel använder inte hello etableras MongoDB-instansen ännu. Korrigeras genom att uppdatera hello web klientdel koden toouse en miljövariabel i stället för en hårdkodad instans.

### <a name="changing-hello-todo-application"></a>Ändra hello TODO-program
Öppna hello på utvecklingsdatorn där du först har klonat hello nod-göra databasen, `node-todo/config/database.js` filen i din favorit-redigeraren och ändra hello url-värdet från hello hårdkodat värde som `mongodb://...` för`process.env.MONGODB`.

Genomför ändringarna och skicka toohello GitHub master:

    git commit -am "Get MongoDB instance from env"
    git push origin master

Tyvärr Publicera inte detta hello ändra toohello web klientdel virtuell dator. Låt oss se några fler ändringar toothat virtuella tooenable en tydlig och effektiv mekanism för att publicera uppdateringar så att du kan snabbt se hello effekten av hello ändringar i hello live-miljö.

### <a name="configuring-hello-web-frontend-virtual-machine"></a>Konfigurera hello Web klientdel virtuell dator
Kom ihåg att vi tidigare har skapat en utan kloning av hello nod-göra databasen på hello web klientdel virtuella datorn. Det visar sig att den här åtgärden skapas en ny Git remote toowhich ändringar kan skickas. Dock får bara trycka toothis remote ganska inte hello snabb iteration modell som utvecklare behöver när du arbetar med koden.

Vad vi vill toobe kan toodo se till att när ett push toohello fjärrdatabasen på den virtuella datorn hello inträffar hello körs TODO programmet uppdateras automatiskt. Detta är som tur är kan enkelt tooachieve med git.

Git exponerar ett antal kod som kan anropas på en viss tidpunkt tooreact tooactions vidtagits hello-databasen. Dessa anges med kommandoskript i hello databas `hooks` mapp. hello hook som är mest användbart för hello automatisk uppdatering scenariot är hello `post-update` händelse.

I en SSH-session toohello web klientdel virtuell dator, ändrar du toohello `~/node-todo.git/hooks` directory och lägga till hello efter innehåll tooa fil med namnet `post-update` (ersätter `machinename` och `region` med informationen om MongoDB virtuella):

    #!/bin/bash

    forever stopall
    unset 'GIT_DIR'
    export MONGODB="mongodb://machinename.region.cloudapp.azure.com:27017/tododb"
    cd ~/node-todo && git fetch origin && git pull origin master && npm install && forever start ~/node-todo/server.js
    exec git update-server-info

Se till att den här filen är körbara genom att köra följande kommando hello:

    chmod 755 post-update

Det här skriptet säkerställer att hello aktuella serverprogrammet har stoppats, hello koden i hello klonade databasen är uppdaterad toohello senaste, eventuella uppdaterade beroenden är uppfyllda och hello-servern har startats om. Det innebär också att hello-miljö har konfigurerats som förberedelse för att ta emot vår första programmet uppdatering tooget hello MongoDB-instansen från en miljövariabel.

### <a name="configuring-your-development-machine"></a>Konfigurera utvecklingsdatorn
Nu ska vi gå utvecklingsdatorn ansluten toohello web klientdel virtuell dator. Detta är så enkelt som att lägga till hello utan databasen på hello virtuell dator som fjärransluten. Kör följande kommando toodo hello detta (ersätter *användare* med ditt inloggningsnamn för web klientdel virtuell dator och *machinename* och *region* efter behov):

    git remote add azure user@machinename.region.cloudapp.azure.com:node-todo.git

Det här är allt som behövs tooenable push-installation eller publicering i praktiken, ändras toohello web klientdel virtuell dator.

### <a name="publishing-updates"></a>Publicera uppdateringar
Vi publicerar hello en ändring har gjorts hittills så att programmet hello kommer att använda våra egna MongoDB-instansen:

    git push azure master

Du bör se utdata liknande toothis:

    Counting objects: 4, done.
    Delta compression using up too4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (4/4), 406 bytes | 0 bytes/s, done.
    Total 4 (delta 1), reused 0 (delta 0)
    remote: info:    Forever stopped processes:
    remote: data:        uid  command         script    forever pid  id logfile                         uptime
    remote: data:    [0] 0Lyh /usr/bin/nodejs server.js 9064    9301    /home/username/.forever/0Lyh.log 0:0:3:17.487
    remote: From /home/username/node-todo
    remote:    5f31fd7..5bc7be5  master     -> origin/master
    remote: From /home/username/node-todo
    remote:  * branch            master     -> FETCH_HEAD
    remote: Updating 5f31fd7..5bc7be5
    remote: Fast-forward
    remote:  config/database.js | 2 +-
    remote:  1 file changed, 1 insertion(+), 1 deletion(-)
    remote: npm WARN package.json node-todo@0.0.0 No repository field.
    remote: npm WARN package.json node-todo@0.0.0 No license field.
    remote: warn:    --minUptime not set. Defaulting to: 1000ms
    remote: warn:    --spinSleepTime not set. Your script will exit if it does not stay up for at least 1000ms
    remote: info:    Forever processing file: /home/username/node-todo/server.js
    toousername@machinename.region.cloudapp.azure.com:node-todo.git
    5f31fd7..5bc7be5  master -> master

När det här kommandot har slutförts, försök att uppdatera programmet hello i en webbläsare. Du ska kunna toosee att hello TODO-listan som visas här är tom och inte längre bundet toohello delade distribueras MongoDB-instansen.

toocomplete hello kursen ska vi ändra en annan, tydligare. På utvecklingsdatorn, öppna hello node-todo/public/index.html filen med din favorit-redigeraren. Leta upp hello jumbotron sidhuvud och ändra hello rubrik från ”jag Todo-aholic” för ”jag Todo-aholic i Azure”!.

Nu ska vi genomför:

    git commit -am "Azurify hello title"

Den här gången, vi publicerar hello ändra tooAzure innan du skickar det till GitHub-repo-tooback toohello:

    git push azure master

När det här kommandot har slutförts, ser uppdatera hello webbsida och du hello ändringar. Eftersom de passar push hello ändra tillbaka toohello ursprung fjärråtkomst: 

    git push origin master

## <a name="next-steps"></a>Nästa steg
Den här artikeln visar hur tootake ett Node.js-program och distribuerar den tooLinux virtuella datorer som körs i Azure. toolearn mer information om Linux-datorer i Azure, se [introduktion tooLinux på Azure](/documentation/articles/virtual-machines-linux-introduction/).

Mer information om hur toodevelop Node.js-program i Azure, se hello [Node.js Developer Center](/develop/nodejs/).

