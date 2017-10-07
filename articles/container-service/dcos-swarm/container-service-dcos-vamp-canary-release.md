---
title: "aaaCanary version med Vamp på Azure DC/OS-kluster | Microsoft Docs"
description: "Hur toouse Vamp toocanary versionen tjänster och tillämpa smart trafik filtrering på ett Azure Container Service DC/OS-kluster"
services: container-service
author: gggina
manager: rasquill
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/17/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: e7b8658a161a7cddcf718e3e1c12a889a330d3d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="canary-release-microservices-with-vamp-on-an-azure-container-service-dcos-cluster"></a>Kanarieöarna versionen mikrotjänster med Vamp på ett Azure Container Service DC/OS-kluster

I den här genomgången ska konfigurera vi Vamp på Azure Container Service med ett DC/OS-kluster. Vi Kanarieöarna släpp hello Vamp demo service ”sava” och sedan matcha inkompatibilitet hello tjänsten med Firefox genom att använda smart trafikfiltrering. 

> [!TIP] 
> I den här genomgången Vamp körs på en DC/OS-klustret, men du kan också använda Vamp med Kubernetes som hello orchestrator.
>

## <a name="about-canary-releases-and-vamp"></a>Om Kanarieöarna versioner och Vamp


[Kanarieöarna släppa](https://martinfowler.com/bliki/CanaryRelease.html) är en smart distributionsstrategi antas av innovativa organisationer som Netflix, Facebook och Spotify. Det är en lösning som passar eftersom det minskar problem, introducerar säkerhet nät och ökar innovation. Så varför inte alla företag som använder den? Utöka en CI/CD-pipeline tooinclude Kanarieöarna strategier lägger till komplexitet och kräver omfattande devops kunskap och erfarenhet. Som är tillräckligt med tooblock mindre företag och företag som är både innan de även komma igång. 

[Vamp](http://vamp.io/) är en öppen källkod system utformade tooease övergången och ta Kanarieöarna släppa funktioner tooyour önskade behållaren Schemaläggaren. Vamps Kanarieöarna funktioner går utöver procent-baserade installationer. Trafik kan filtreras och dela på en mängd olika villkor, till exempel tootarget specifika användare, IP-adressintervall eller enheter. Vamp spårar och analyserar prestandamått, så att för automatisering som bygger på verkliga data. Du kan ställa in automatisk återställning vid fel eller skala enskild tjänst varianter utifrån belastningen eller svarstid.

## <a name="set-up-azure-container-service-with-dcos"></a>Konfigurera Azure Container Service med DC/OS



1. [Distribuera ett DC/OS-kluster](container-service-deployment.md) med en överordnad och två agenterna av standardstorleken. 

2. [Skapa en SSH-tunnel](../container-service-connect.md) tooconnect toohello DC/OS-klustret. Den här artikeln förutsätter att du tunnel toohello klustret på lokal port 80.


## <a name="set-up-vamp"></a>Ställ in Vamp

Nu när du har ett DC/OS-kluster som körs, kan du installera Vamp från hello DC/OS-Gränssnittet (http://localhost:80). 

![DC/OS-gränssnitt:](./media/container-service-dcos-vamp-canary-release/01_set_up_vamp.png)

Installationen sker i två steg:

1. **Distribuera Elasticsearch**.

2. Sedan **distribuera Vamp** genom att installera hello Vamp DC/OS universe paketet.

### <a name="deploy-elasticsearch"></a>Distribuera Elasticsearch

Vamp kräver Elasticsearch för insamling av mätvärden och aggregering. Du kan använda hello [magneticio Docker bilder](https://hub.docker.com/r/magneticio/elastic/) toodeploy en kompatibel Vamp Elasticsearch stack.

1. Hello DC/OS-Gränssnittet, gå för**Services** och på **distribuera tjänst**.

2. Välj **JSON-läget** från hello **distribuera nya tjänster** popup.

  ![Välj JSON-läge](./media/container-service-dcos-vamp-canary-release/02_deploy_service_json_mode.png)

3. Klistra in i hello följande JSON. Den här konfigurationen kör hello behållare med 1 GB RAM-minne och grundläggande hälsokontrollen på hello Elasticsearch port.
  
  ```JSON
  {
    "id": "elasticsearch",
    "instances": 1,
    "cpus": 0.2,
    "mem": 1024.0,
    "container": {
      "docker": {
        "image": "magneticio/elastic:2.2",
        "network": "HOST",
        "forcePullImage": true
      }
    },
    "healthChecks": [
      {
        "protocol": "TCP",
        "gracePeriodSeconds": 30,
        "intervalSeconds": 10,
        "timeoutSeconds": 5,
        "port": 9200,
        "maxConsecutiveFailures": 0
      }
    ]
  }
  ```
  

3. Klicka på **distribuera**.

  DC/OS distribuerar hello Elasticsearch behållare. Du kan följa förloppet på hello **Services** sidan.  

  ![distribuera e? Elasticsearch](./media/container-service-dcos-vamp-canary-release/03_deply_elasticsearch.png)

### <a name="deploy-vamp"></a>Distribuera Vamp

När Elasticsearch rapporterar som **kör**, kan du lägga till hello Vamp DC/OS Universe paketet. 

1. Gå för**Universe** och Sök efter **vamp**. 
  ![Vamp på DC/OS universe](./media/container-service-dcos-vamp-canary-release/04_universe_deploy_vamp.png)

2. Klicka på **installera** nästa toohello vamp paketet och välj **Installation i Avancerat**.

3. Bläddra nedåt och ange hello följande elasticsearch-url: `http://elasticsearch.marathon.mesos:9200`. 

  ![Ange Elasticsearch URL](./media/container-service-dcos-vamp-canary-release/05_universe_elasticsearch_url.png)

4. Klicka på **granska och installera**, klicka på **installera** toostart hello distribution.  

  DC/OS distribuerar alla nödvändiga komponenter för Vamp. Du kan följa förloppet på hello **Services** sidan.
  
  ![Distribuera Vamp som universe paket](./media/container-service-dcos-vamp-canary-release/06_deploy_vamp.png)
  
5. När distributionen är klar, kan du komma åt hello Vamp UI:

  ![Vamp-tjänsten på DC/OS](./media/container-service-dcos-vamp-canary-release/07_deploy_vamp_complete.png)
  
  ![Vamp UI](./media/container-service-dcos-vamp-canary-release/08_vamp_ui.png)


## <a name="deploy-your-first-service"></a>Distribuera din första tjänst

Nu att Vamp är igång kan du distribuera en tjänst från ett utkast. 

I sin enklaste form är en [Vamp modell](http://vamp.io/documentation/using-vamp/blueprints/) beskriver hello slutpunkter (gateway), kluster och tjänster toodeploy. Vamp använder kluster toogroup olika varianter av hello samma tjänst i logiska grupper för Kanarieöarna släppa eller A / B-testning.  

Det här scenariot använder ett monolitisk exempelprogram som kallas [ **sava**](https://github.com/magneticio/sava), vilket är version 1.0. Hej monolitvolym paketeras i en dockerbehållare med som finns i Docker under magneticio/sava:1.0.0. hello appen körs normalt på port 8080, men du vill tooexpose den under port 9050 i det här fallet. Distribuera hello appen via Vamp med en enkel modell.

1. Gå för**distributioner**.

2. Klicka på **Lägg till**.

3. Klistra in följande hello utkast YAML. Det här utkastet innehåller ett kluster med en enda service variant som vi ändras i ett senare steg:

  ```YAML
  name: sava                        # deployment name
  gateways:
    9050: sava_cluster/webport      # stable endpoint
  clusters:
    sava_cluster:               # cluster toocreate
     services:
        -
          breed:
            name: sava:1.0.0        # service variant name
            deployable: magneticio/sava:1.0.0
            ports:
              webport: 8080/http # cluster endpoint, used for canary releasing
  ```

4. Klicka på **Spara**. Vamp initierar hello-distribution.

hello distribution visas på hello **distributioner** sidan. Klicka på hello distribution toomonitor dess status.

![Vamp gränssnitt – distribuera sava](./media/container-service-dcos-vamp-canary-release/09_sava100.png)

![sava service i Användargränssnittet för Vamp](./media/container-service-dcos-vamp-canary-release/09a_sava100.png)

Två gateways skapas, som visas på hello **Gateways** sidan:

* en stabil endpoint tooaccess hello som kör tjänsten (9050-port) 
* en Vamp hanteras internt gateway (mer om den här gatewayen senare). 

![Vamp UI - sava gateways](./media/container-service-dcos-vamp-canary-release/10_vamp_sava_gateways.png)

Hej sava tjänsten nu har distribuerats, men du kan inte komma åt den externt eftersom hello Azure belastningsutjämnare inte vet tooforward trafik tooit ännu. tooaccess hello tjänst update hello Azure nätverkskonfigurationen.


## <a name="update-hello-azure-network-configuration"></a>Uppdatera hello Azure nätverkskonfiguration

Vamp distribuerade hello sava-tjänsten på noder hello DC/OS-agenten, exponera en stabil slutpunkt på port 9050. tooaccess hello tjänst från utanför hello DC/OS-klustret, att Hej efter ändringar toohello Azure nätverkskonfigurationen i kluster-distributionen: 

1. **Konfigurera hello Azure belastningsutjämnare** för hello-agenter (hello resurs med namnet **DC/OS-agent-lb-xxxx**) med en hälsoavsökningen och en regel tooforward trafik på port 9050 toohello sava instanser. 

2. **Uppdatera hello nätverkssäkerhetsgruppen** för hello offentliga agenter (hello resurs med namnet **XXXX-agent-offentliga-nsg-XXXX**) tooallow trafik på port 9050.

För detaljerade anvisningar toocomplete hello dessa uppgifter med hjälp av Azure portal, se [aktiverar offentlig åtkomst tooan Azure Container Service-programmet](container-service-enable-public-access.md). Ange port 9050 för alla portinställningar.


När allt har skapats, gå toohello **översikt** bladet för hello DC/OS-agentens belastningsutjämnare (hello resurs med namnet **DC/OS-agent-lb-xxxx**). Hitta hello **offentliga IP-adressen**, och använder hello adress tooaccess sava på port 9050.

![Azure portal – hämta offentlig IP-adress](./media/container-service-dcos-vamp-canary-release/18_public_ip_address.png)

![sava](./media/container-service-dcos-vamp-canary-release/19_sava100.png)


## <a name="run-a-canary-release"></a>Kör en version av Kanarieöarna

Anta att du har en ny version av det här programmet som du vill toocanary övergång till produktion. Du har den behållare som magneticio/sava:1.1.0 och är redo toogo. Vamp kan du enkelt lägga till nya tjänster toohello kör distribution. Tjänsterna ”kopplade” distribuerats tillsammans med befintliga hello-tjänster i hello kluster, och tilldelas en vikt på 0%. Ingen trafik är routade tooa nyligen samman tjänsten tills du justera hello trafikfördelning. hello vikt skjutreglaget i hello Vamp UI ger fullständig kontroll över hello distribution, så att för inkrementell justeringar (Kanarieöarna utgåva) eller en omedelbar återställning.

### <a name="merge-a-new-service-variant"></a>Koppla en ny service-variant

toomerge hello nya sava 1.1 service med hello kör distribution:

1. I hello Vamp UI, klickar du på **ritningarna**.

2. Klicka på **Lägg till** och klistra in följande hello utkast YAML: den här utkast som beskriver en ny service variant (sava: 1.1.0) toodeploy inom hello befintligt kluster (sava_cluster).

  ```YAML
  name: sava:1.1.0      # blueprint name
  clusters:
    sava_cluster:       # cluster tooupdate
      services:
        -
          breed:
            name: sava:1.1.0    # service variant name
            deployable: magneticio/sava:1.1.0    
            ports:
              webport: 8080/http # cluster endpoint tooupdate
  ```
  
3. Klicka på **Spara**. hello utkast lagras och visas på hello **ritningarna** sidan.

4. Öppna hello Åtgärd-menyn i hello sava: 1.1 plan och klickar på **koppla till**.

  ![Vamp UI - ritningarna](./media/container-service-dcos-vamp-canary-release/20_sava110_mergeto.png)

5. Välj hello **sava** distribution och klickar på **sammanfoga**.

  ![Vamp UI - sammanfoga utkast toodeployment](./media/container-service-dcos-vamp-canary-release/21_sava110_merge.png)

Vamp distribuerar hello ny sava: 1.1.0 service variant beskrivs i hello utkast tillsammans med sava: 1.0.0 i hello **sava_cluster** av hello kör distribution. 

![Vamp UI - uppdaterade sava distribution](./media/container-service-dcos-vamp-canary-release/22_sava_cluster.png)

Hej **webport-sava/sava_cluster** gateway (hello klusterslutpunkten) har också uppdaterats, lägga till en väg toohello nyligen distribuerade sava: 1.1.0. Nu ingen trafik dirigeras här (hello **vikt** anges too0%).

![Vamp UI - klustret gateway](./media/container-service-dcos-vamp-canary-release/23_sava_cluster_webport.png)

### <a name="canary-release"></a>Kanarieöarna versionen

Med båda versionerna av sava distribuerats i hello samma kluster, justera hello fördelningen av trafiken mellan dem genom att flytta hello **vikt** skjutreglaget.

1. Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) nästa för**vikt**.

2. Ange hello vikten Distributionsplatsens too50%/50% och på **spara**.

  ![Vamp UI - gateway vikt skjutreglaget](./media/container-service-dcos-vamp-canary-release/24_sava_cluster_webport_weight.png)

3. Gå tillbaka tooyour webbläsare och uppdatera hello sava sidan några gånger. Hej sava programmet nu växlar mellan en sava: 1.0 och en sava: 1.1-sidan.

  ![Alternerande sava1.0 och sava1.1 tjänster](./media/container-service-dcos-vamp-canary-release/25_sava_100_101.png)


  > [!NOTE]
  > Den här växlingar hello sidan fungerar bäst med hello ”Incognito” eller ”anonym” läge i webbläsaren på grund av hello cachelagring av statiska tillgångar.
  >

### <a name="filter-traffic"></a>Filtrera trafik

Anta att efter distributionen du identifierade inkompatibilitet i sava: 1.1.0 att visas i Firefox webbläsare. Du kan ange Vamp toofilter inkommande trafik och dirigera alla Firefox användare tillbaka toohello kända stabil sava: 1.0.0. Det här filtret bättre direkt matchar hello avbrott för Firefox användare, medan alla andra fortsätter tooenjoy hello fördelarna med hello sava: 1.1.0.

Vamp använder **villkor** toofilter trafik mellan vägar i en gateway. Trafik filtreras först och dirigeras bl.a toohello villkor som gäller tooeach vägen. Alla återstående trafiken fördelas enligt toohello gateway vikt inställningen.

Du kan skapa ett villkor toofilter alla Firefox användare och dirigera dem toohello gamla sava: 1.0.0:

1. På hello sava/sava_cluster/webport **Gateways** klickar du på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) tooadd en **VILLKORET** toohello väg sava/sava_cluster/sava:1.0.0/webport. 

2. Ange villkor för hello **användaragent == Firefox** och på ![Vamp UI - spara](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png).

  Vamp lägger till hello villkor med en standardvärdet 0%. Filtrera trafik på toostart, behöver du tooadjust hello villkoret styrka.

3. Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) toochange hello **styrkan** tillämpas toohello villkor.
 
4. Ange hello **styrkan** too100% och klickar på ![Vamp UI - spara](./media/container-service-dcos-vamp-canary-release/vamp_ui_save.png) toosave.

  Vamp nu skickar all trafik som matchar hello villkor (alla Firefox användare) toosava:1.0.0.

  ![Vamp UI - använda villkoret toogateway](./media/container-service-dcos-vamp-canary-release/26_apply_condition.png)

5. Slutligen justera hello gateway vikt toosend alla återstående trafik (alla användare med icke-Firefox) toohello nya sava: 1.1.0. Klicka på ![Vamp UI - redigera](./media/container-service-dcos-vamp-canary-release/vamp_ui_edit.png) nästa för**vikt** och ange hello viktfördelningen så 100% är riktat toohello väg sava/sava_cluster/sava:1.1.0/webport.

  All trafik som inte filtreras genom hello villkoret är nu dirigerad toohello nya sava: 1.1.0.

6. toosee hello filter i åtgärden, öppna två olika webbläsare (en Firefox och en annan webbläsare) och hello sava tjänsten från både. Alla Firefox begäranden skickas toosava:1.0.0, medan alla andra webbläsare som är riktade toosava:1.1.0.

  ![Vamp UI - filtrera trafik](./media/container-service-dcos-vamp-canary-release/27_filter_traffic.png)

## <a name="summing-up"></a>Sammanfattningsvis

Den här artikeln är en snabb introduktion tooVamp på ett DC/OS-kluster. Börja du har fått Vamp och körs på din Azure Container Service DC/OS-kluster distribuerat en tjänst med en Vamp modell och används på hello exponeras slutpunkt (gateway).

Vi också har använts på vissa kraftfulla funktioner i Vamp: Koppla en ny service variant toohello kör distribution och introduktion till inkrementellt sedan filtrera trafik tooresolve kända kompatibilitetsproblem.


## <a name="next-steps"></a>Nästa steg

* Lär dig mer om hur du hanterar Vamp åtgärder via hello [Vamp REST API](http://vamp.io/documentation/api/api-reference/).

* Skapa Vamp automatiseringsskript i Node.js och köra dem som [Vamp arbetsflöden](http://vamp.io/documentation/tutorials/create-a-workflow/).

* Se ytterligare [VAMP självstudier](http://vamp.io/documentation/tutorials/overview/).

