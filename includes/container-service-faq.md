# <a name="container-service-frequently-asked-questions"></a>Vanliga frågor och svar om Container Service

## <a name="orchestrators"></a>Dirigeringsverktyg

### <a name="which-container-orchestrators-do-you-support-on-azure-container-service"></a>Vilka behållardirigeringsverktyg stöder Azure Container Service? 

Det finns stöd för lösningar som är baserade på öppen källkod, som DC/OS, Docker Swarm och Kubernetes. Mer information finns i hello [översikt](../articles/container-service/kubernetes/container-service-intro-kubernetes.md).
 
### <a name="do-you-support-docker-swarm-mode"></a>Finns det stöd för Docker Swarm-läge? 

För närvarande Swarm-läge stöds inte, men den finns på hello service plan. 

### <a name="does-azure-container-service-support-windows-containers"></a>Har Azure Container Service stöd för Windows-behållare?  

För närvarande finns stöd för Linux-behållare med alla dirigeringsverktyg. Stöd för Windows-behållare med Kubernetes är tillgängligt i förhandsversionen.

### <a name="do-you-recommend-a-specific-orchestrator-in-azure-container-service"></a>Rekommenderar ni något specifikt dirigeringsverktyg i Azure Container Service? 
Vanligtvis rekommenderar vi inte något specifikt dirigeringsverktyg. Om du har erfarenhet av någon av hello stöds orchestrators kan du använda dessa erfarenheter i Azure Container Service. Man kan dock säga att DC/OS är produktionstestat för stordata och IoT-arbetsbelastningar, att Kubernetes passar för molnautentiska arbetsbelastningar och att Docker Swarm är känt för sin integrering med Docker-verktyg och korta inlärningskurva.

Beroende på ditt scenario kan du också skapa och hantera anpassade behållarlösningar med andra Azure-tjänster. Till exempel med [Virtual Machines](../articles/virtual-machines/linux/overview.md), [Service Fabric](../articles/service-fabric/service-fabric-overview.md), [Web Apps](../articles/app-service-web/app-service-web-overview.md) och [Batch](../articles/batch/batch-technical-overview.md).  

### <a name="what-is-hello-difference-between-azure-container-service-and-acs-engine"></a>Vad är hello skillnaden mellan Azure Container Service och ACS-motorn? 
Azure Container Service är en SLA-uppbackade Azure-tjänst med funktioner i hello Azure-portalen, Azure kommandoradsverktyg och Azure API: er. hello-tjänsten kan tooquickly implementera och hantera kluster som kör standard behållaren orchestration verktyg med ett relativt litet antal konfigurationsalternativ. 

[ACS-motorn](http://github.com/Azure/acs-engine) är ett projekt med öppen källkod som möjliggör klusterkonfiguration för Privilegierade användare toocustomize hello på varje nivå. Den här möjligheten tooalter hello konfigurationen av både infrastruktur och programvara innebär att vi erbjuder ingen SLA för ACS-motorn. Stöd för hanteras via hello öppen källkod projektet på GitHub i stället för den officiella Microsoft vägen. 

## <a name="cluster-management"></a>Klusterhantering

### <a name="how-do-i-create-ssh-keys-for-my-cluster"></a>Hur skapar jag SSH-nycklar för mitt kluster?

Du kan använda standardverktyg på ditt operativsystem toocreate en SSH-RSA offentlig och privat nyckel för autentisering mot hello Linux virtuella datorer för klustret. Anvisningar finns i hello [OS X- och Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) eller [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) vägledning. 

Om du använder [Azure CLI 2.0 kommandon](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) toodeploy ett container service kluster, SSH nycklar kan genereras automatiskt för klustret.

### <a name="how-do-i-create-a-service-principal-for-my-kubernetes-cluster"></a>Hur skapar jag ett huvudnamn för mitt Kubernetes-kluster?

Ett Azure Active Directory service ägar-ID och lösenord är också nödvändig toocreate ett Kubernetes kluster i Azure Container Service. Mer information finns i [om hello tjänstens huvudnamn för ett kluster med Kubernetes](../articles/container-service/kubernetes/container-service-kubernetes-service-principal.md).

Om du använder [Azure CLI 2.0 kommandon](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md) toodeploy en Kubernetes klustret, service principal autentiseringsuppgifter kan genereras automatiskt för klustret.

### <a name="how-large-a-cluster-can-i-create"></a>Hur stort kluster kan jag skapa?
Du kan skapa ett kluster med 1, 3 eller 5 överordnade noder. Du kan välja upp too100 agent noder.

> [!IMPORTANT]
> För större kluster och beroende på hello VM-storlek som du väljer för noderna behöva tooincrease hello kärnor kvot i din prenumeration. toorequest ökad kvot, öppna ett [online kundsupport](../articles/azure-supportability/how-to-create-azure-support-request.md) utan kostnad. Om du använder ett [kostnadsfritt Azure-konto](https://azure.microsoft.com/free/) kan du bara använda ett begränsat antal Azure Compute-kärnor.
> 

### <a name="how-do-i-increase-hello-number-of-masters-after-a-cluster-is-created"></a>Hur ökar jag hello antal huvudservrar när ett kluster har skapats? 
När hello-kluster skapas hello antal huvudservrar är fast och kan inte ändras. Under hello skapandet av hello klustret, bör du helst välja flera huvudservrar för hög tillgänglighet.

### <a name="how-do-i-increase-hello-number-of-agents-after-a-cluster-is-created"></a>Hur ökar jag hello antalet agenter efter ett kluster skapas? 
Du kan skala hello antalet agenter i hello kluster med hjälp av hello Azure-portalen eller kommandoradsverktyg. Se [Skala ett Azure Container Service-kluster](../articles/container-service/kubernetes/container-service-scale.md).

### <a name="what-are-hello-urls-of-my-masters-and-agents"></a>Vad är hello URL: er för min huvudservrarna och agenter? 
hello URL: er för klustret resurser i Azure Container Service baseras på hello DNS-namnet prefix du anger och hello namnet på hello Azure-region som du valde för distribution. Hello fullständigt kvalificerade domännamnet (FQDN) för hello Huvudnoden är till exempel för det här formuläret:

``` 
DNSnamePrefix.AzureRegion.cloudapp.azure.net
```

Du kan hitta vanliga URL-adresser för klustret i hello Azure-portalen, hello Azure Resursläsaren eller andra Azure-verktyg.

### <a name="how-do-i-tell-which-orchestrator-version-is-running-in-my-cluster"></a>Hur tar jag reda på vilken orchestrator-version som körs i mitt kluster?

* DC/OS: Se hello [Mesosphere-dokumentationen](https://support.mesosphere.com/hc/en-us/articles/207719793-How-to-get-the-DCOS-version-from-the-command-line-)
* Docker Swarm: kör `docker version`
* Kubernetes: kör `kubectl version`

### <a name="how-do-i-upgrade-hello-orchestrator-after-deployment"></a>Hur uppgraderar jag hello orchestrator efter distributionen?

För närvarande innehåller inte Azure Container Service verktyg tooupgrade hello version av hello orchestrator som du har distribuerat i klustret. Om Container Service har stöd för en senare version så kan du distribuera ett nytt kluster. Ett annat alternativ är toouse orchestrator-specifika verktyg om de är tillgängliga tooupgrade ett kluster på plats. Läs exempelvis artikeln om [uppgradering av DC/OS](https://dcos.io/docs/1.8/administration/upgrading/).
 
### <a name="where-do-i-find-hello-ssh-connection-string-toomy-cluster"></a>Var hittar jag hello SSH-anslutning sträng toomy kluster?

Du kan hitta hello anslutningssträngen i hello Azure-portalen eller med hjälp av Azure kommandoradsverktyg. 

1. Navigera toohello resursgruppen för hello Klusterdistribution i hello-portalen.  

2. Klicka på **översikt** och klicka på länken hello **distributioner** under **Essentials**. 

3. I hello **distributionshistoriken** bladet, klickar du på hello-distribution som har ett namn som börjar med **microsoft acs** följt av ett distributionsdatum. Till exempel: microsoft-acs-201701310000.  

4. På hello **sammanfattning** sidan under **utdata**, länkar för flera kluster. **SSHMaster0** ger en SSH-anslutning sträng toohello första huvudservern i din behållartjänstklustret. 

Du kan också använda Azure-verktyg toofind hello FQDN för hello master som tidigare anges. Göra en SSH-anslutning toohello med hello FQDN för hello master och hello användarnamn som du angav när du skapar hello kluster. Exempel:

```bash
ssh userName@masterFQDN –A –p 22 
```

Mer information finns i [Anslut tooan Azure Container Service-kluster](../articles/container-service/kubernetes/container-service-connect.md).

## <a name="next-steps"></a>Nästa steg

* [Läs mer](../articles/container-service/kubernetes/container-service-intro-kubernetes.md) om Azure Container Service.
* Distribuera ett container service-kluster med hjälp av hello [portal](../articles/container-service/dcos-swarm/container-service-deployment.md) eller [Azure CLI 2.0](../articles/container-service/dcos-swarm/container-service-create-acs-cluster-cli.md).
