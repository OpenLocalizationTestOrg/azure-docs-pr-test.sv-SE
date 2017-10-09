Azure-molnlösningar bygger på virtuella datorer (emulering av fysisk datormaskinvara), vilket möjliggör flexibel förpackning av programvarudistributioner och bättre resurskonsolidering än med fysisk maskinvara. [Docker](https://www.docker.com) behållare och hello docker-ekosystemet har kraftigt utökat hello sätt att utveckla, levereras och hantera distribuerade program. Programkod i en behållare som är isolerad från hello värd virtuell dator och andra behållare på hello samma virtuella dator. Denna isolering ger mer flexibilitet vid utveckling och distribution.

Azure erbjuder hello följande Docker-värden:

* [Många](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) [olika](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) sätt toocreate Docker som värd för behållare toosuit din situation
* Hej [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) skapar kluster av behållare värdar med orchestrators som **marathon** och **swarm**.
* [Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md) och [resurs mallar](../articles/resource-group-authoring-templates.md) toosimplify distribuerar och uppdaterar komplexa distribuerade program
* integrering med en stor matris av verktyg för konfigurationshantering med både upphovsrättsskyddad och öppen källkod

Och eftersom du kan skapa virtuella datorer och Linux programmässigt behållare i Azure, du kan också använda VM och en behållare *orchestration* verktyg toocreate grupper av virtuella datorer (VM) och toodeploy program i både Linux behållare och nu [Windows behållare](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

Den här artikeln beskrivs inte bara dessa begrepp på en hög nivå, innehåller också ton av länkar toomore information, självstudier och produkter relaterade toocontainer och kluster användning på Azure. Om du behöver veta allt detta och bara vill hello länkar, de här finns i [verktyg för att arbeta med behållare](#tools-for-working-with-azure-vms-and-containers).

## <a name="hello-difference-between-virtual-machines-and-containers"></a>hello skillnaden mellan virtuella datorer och behållare
Virtuella datorer körs i en isolerad maskinvaruvirtualiseringsmiljö som tillhandahålls av ett [hypervisor-program](http://en.wikipedia.org/wiki/Hypervisor). I Azure, hello [virtuella datorer](https://azure.microsoft.com/services/virtual-machines/) tjänsten hanterar allt som du: du kan skapa virtuella datorer genom att välja hello operativsystemet och konfigurera &mdash;eller genom att överföra en anpassad VM-avbildning. Virtuella datorer är en teknik för Tidstestat, ”slaget härdat” och det finns många verktyg tillgängliga toomanage hello OS och appar som de innehåller.  Appar i en virtuell dator är dolda från hello värdens operativsystem. Hello utgångspunkt från att ett program eller en användare på en virtuell dator visas hello VM toobe en fristående fysisk dator.

[Linux-behållare](http://en.wikipedia.org/wiki/LXC) och de som skapas och hanteras med hjälp av docker-verktyg, Använd inte en hypervisor tooprovide isolering. Med behållare använder hello behållaren värden processen och filsystem isolering: funktioner för hello Linux kernel tooexpose toohello behållare, dess appar, vissa funktioner i kernel och isolerade filsystemet. Hello utgångspunkt från en App som körs i en behållare, visas hello behållaren toobe en unik OS-instans. En innesluten app kan inte se processer eller andra resurser utanför dess behållare.

Mycket färre resurser används i en Docker-behållare än i en virtuell dator. Docker behållare använder ett program isolering och körningen modell, som inte delar hello kernel för hello Docker-värd. hello behållaren har mycket lägre diskutrymme som saknar hello hela OS. Starttiden och det diskutrymme som krävs är betydligt lägre än i en virtuell dator.
Windows-behållare ange hello samma fördelar som Linux-behållare för appar som körs på Windows. Stöd för Windows-behållare hello Docker bildformat och Docker API, men de kan också hanteras med hjälp av PowerShell. Två behållarkörningar är tillgängliga med Windows-behållare: Windows Server-behållare och Hyper-V-behållare. Hyper-V-behållare ger ett extra lager av isolering genom värdhantering av varje behållare i en superoptimerad virtuell dator. Mer om Windows-behållare, se toolearn [Windows behållare](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview). tooget igång med Windows-behållare i Azure, Lär dig hur för[distribuera ett Azure Container Service-kluster](../articles/container-service/dcos-swarm/container-service-deployment.md).

## <a name="what-are-containers-good-for"></a>Vad är behållare bra för?

Behållare kan förbättra:

* hello hastighet programkod kan utvecklas och delade Allmänt
* hello hastighet och förtroende som en app kan testas
* hello hastighet och förtroende som en app kan distribueras

Behållare körs på en behållarvärd&mdash;ett operativsystem, och i Azure innebär det en virtuell Azure-dator. Även om du gillar redan hello uppfattning om behållare kan du fortfarande kommer tooneed en VM-infrastruktur som värd för hello behållare, men hello fördelar är att behållare inte bryr dig körs vilka VM de (även om huruvida hello behållaren vill Linux- eller Windows körningsmiljö blir viktigt, till exempel).


## <a name="what-are-containers-good-for"></a>Vad är behållare bra för?
De är bra för många olika saker, men de uppmuntra&mdash;som [Azure Cloud Services](https://azure.microsoft.com/services/cloud-services/) och [Azure Service Fabric](../articles/service-fabric/service-fabric-overview.md)&mdash;hello skapandet av en tjänst, mikrotjänster indatavärdena distribuerade program, i vilket program design baseras på fler liten, sammanställningsbara delar istället för på större och mer starkt kopplade komponenter.

Detta gäller särskilt i miljöer med ett offentligt moln som Azure, där du hyr virtuella datorer när och var du vill. Du får inte bara isolering och snabba distributions- och dirigeringsverktyg, utan kan även ta mer effektiva beslut om programinfrastrukturen.

Du kan till exempel för närvarande ha en distribution som består av nio virtuella Azure-datorer med stor storlek för högtillgänglig, distribuerad tillämpning. Om hello komponenter i det här programmet kan distribueras i behållare, kan du toouse kan bara vara 4 virtuella datorer och distribuerar programkomponenterna i 20 behållare för redundans och belastningsutjämning.

Detta är bara ett exempel naturligtvis, men om du kan göra detta i ditt scenario måste du justera toousage toppar med fler behållare i stället för flera virtuella Azure-datorer och använda hello återstående totala CPU-belastningen mer effektivt än tidigare.

Dessutom finns många scenarier som inte lämpar sig tooa mikrotjänster perspektiv. vet du bäst om mikrotjänster och behållare kommer hjälpa dig.

### <a name="container-benefits-for-developers"></a>Behållarfördelar för utvecklare
I allmänhet är det enkelt toosee att behållaren tekniken är framåt ett steg, men det finns också mer specifika fördelar. Låt oss ta hello exempel på Docker-behållare. Det här avsnittet kommer inte fördjupa dig djupt i Docker just nu (läsa [vad är Docker?](https://www.docker.com/whatisdocker/) för den texten eller [wikipedia](http://wikipedia.org/wiki/Docker_%28software%29)), men Docker och dess ekosystem ger betydande fördelar tooboth utvecklare och IT tekniker.

Utvecklare ta tooDocker behållare snabbt, eftersom framför allt den gör det enkelt att använda Linux och Windows-behållare:

* De kan använda enkla, inkrementell kommandon toocreate en fast avbildning som är lätt toodeploy och automatisera skapar dessa avbildningar med hjälp av en dockerfile
* De kan dela dessa bilder enkelt med enkel, [git](https://git-scm.com/)-style push och pull-kommandon för[offentliga](https://registry.hub.docker.com/) eller [privata docker register](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* De kan tänka på isolerade programkomponenter i stället för datorer
* De kan använda ett stort antal verktyg som fungerar bra med Docker-behållare och olika grundläggande avbildningar

### <a name="container-benefits-for-operations-and-it-professionals"></a>Fördelar med behållare för verksamheter och IT-tekniker
IT och operations-tekniker kan också dra nytta av hello kombination av behållare och virtuella datorer.

* inneslutna tjänster är isolerade från den virtuella datorns värdkörningsmiljö
* den inneslutna koden är verifierbart identisk
* inneslutna tjänster kan startas, stoppas och snabbt flyttas mellan utvecklings-, testnings- och produktionsmiljöerna

Funktioner som dessa&mdash;och det finns flera&mdash;hetsas etablerat företag, där Arbetsrelaterad information technology organisationer har hello jobbet passning resurser&mdash;inklusive ren processorkraft&mdash; toohello uppgifter krävs toonot förblir i företag, men öka nöjda endast och nå. Småföretag, ISV: er och nystartade företag har exakt hello samma krav, men de kan beskriva den på olika sätt.

## <a name="what-are-virtual-machines-good-for"></a>Vad är virtuella datorer bra för?
Virtuella datorer innehåller hello stamnät av molntjänster och som inte ändras. Om virtuella datorer starta långsammare, har en större diskutrymme och mappa inte direkt tooa mikrotjänster arkitektur, har de mycket viktiga fördelar:

1. Som standard har de mycket mer stabila standardsäkerhetsskydd för värddatorn
2. De har stöd för alla viktiga operativsystems- och programinställningar
3. De har långvariga verktygsekosystem för kommando och kontroll
4. De ger hello körning miljö toohost behållare

hello sista objektet är viktig, eftersom ett specifikt operativsystem och CPU-typ krävs fortfarande för en innesluten program, beroende hello anrop hello programmet gör. Det är viktigt tooremember installera behållare på virtuella datorer eftersom de innehåller hello-program som du vill toodeploy; behållare är inte ersättningar för virtuella datorer eller operativsystem.

## <a name="high-level-feature-comparison-of-vms-and-containers"></a>Utförlig funktionsjämförelse av virtuella datorer och behållare
hello följande tabell beskrivs en mycket hög nivå hello slags funktionen skillnader som&mdash;utan mycket extra arbete&mdash;finns mellan virtuella datorer och Linux-behållare. Observera att vissa funktioner kan vara mer eller mindre önskvärt beroende på ditt eget program behöver och att extra arbete tillhandahåller precis som med all programvara, ökade funktioner som stöds, särskilt i hello del av säkerhet.

| Funktion | Virtuella datorer | Behållare |
|:--- | --- | --- |
| Stöd för standardsäkerhet |tooa högre grad |tooa något lägre |
| Minnesutrymme på disken som krävs |Fullständigt operativsystem plus appar |Enbart appkrav |
| Tid det tar att toostart |Betydligt längre: Start av operativsystem plus appinläsning |Avsevärt kortare: endast appar behöver toostart eftersom kernel körs redan |
| Portabilitet |Portabilitet med rätt förberedelse |Portabel inom avbildningsformatet; normalt mindre |
| Automatisering av avbildningar |Varierar mycket beroende på operativsystem och appar |[Docker-registret](https://registry.hub.docker.com/); andra |

## <a name="creating-and-managing-groups-of-vms-and-containers"></a>Skapa och hantera grupper av virtuella datorer och behållare
I det här läget kan en arkitekt, utvecklare eller IT-specialist kanske tänka: ”Jag kan automatisera ALLT detta; det här ÄR verkligen datacenter som en tjänst!”.

Du har rätt, det kan vara och det finns många system som du kanske redan använder, som kan hantera grupper med virtuella Azure-datorer och mata in anpassad kod med skript, ofta hello [CustomScriptingExtension för Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) eller Hej [CustomScriptingExtension för Linux](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/). Du kan&mdash;och kanske redan har&mdash;automatiserat dina Azure-distributioner med hjälp av PowerShell- eller Azure CLI-skript.

Dessa funktioner är ofta sedan migrerade tootools som [Puppet](https://puppetlabs.com/) och [Chef](https://www.chef.io/) tooautomate hello skapandet av och konfigurationen för virtuella datorer i större skala. (Här följer några länkar för[använder dessa verktyg med Azure](#tools-for-working-with-containers).)

### <a name="azure-resource-group-templates"></a>Azure-resursgruppsmallar
Azure släppte mer nyligen hello [Azure resource Manager](../articles/resource-manager-deployment-model.md) REST-API och uppdaterade PowerShell och Azure CLI verktyg toouse den enkelt. Du kan distribuera, ändra eller distribuera om hela programmet topologier med hjälp av [Azure Resource Manager-mallar](../articles/resource-group-authoring-templates.md) med hello Azure-resurs hanterings-API:

* Hej [Azure-portalen med hjälp av mallar](https://github.com/Azure/azure-quickstart-templates)&mdash;tipset använder hello ”DeployToAzure”-knappen
* Hej [Azure CLI](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="deployment-and-management-of-entire-groups-of-azure-vms-and-containers"></a>Distribution och hantering av hela grupper av virtuella datorer i Azure och behållare
Det finns flera populära system som kan distribuera hela grupper av virtuella datorer och installera Docker (eller andra Linux-behållarvärdsystem) på dem som en automatiserbar grupp. Direkta länkar finns hello [behållare och verktyg](#containers-and-vm-technologies) avsnittet nedan. Det finns flera system som gör det här tooa större eller mindre utsträckning och den här listan är inte komplett. Beroende på dina kunskaper och scenarier kan de vara mer eller mindre användbara.

Docker har en egen uppsättning verktyg för att skapa virtuella datorer ([docker-machine](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) och ett belastningsutjämnande, docker-behållare-klusterhanteringsverktyg ([swarm](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Dessutom hello [Azure Docker VM-tillägget](https://github.com/Azure/azure-docker-extension/blob/master/README.md) levereras med standardstöd för [ `docker-compose` ](https://docs.docker.com/compose/), som kan distribuera konfigurerats programmet behållare i flera behållare.

Dessutom kan du kan prova [Mesospheres DCOS (Data Center Operating System)](http://docs.mesosphere.com). DC/OS baseras på hello öppen källkod [mesos](http://mesos.apache.org/) ”distribuerade system kernel” som gör att du tootreat ditt datacenter som en adresserbara tjänst. DCOS har inbyggda paket för flera viktiga system som [Spark](http://spark.apache.org/) och [Kafka](http://kafka.apache.org/) (och andra) samt inbyggda tjänster, som [Marathon](https://mesosphere.github.io/marathon/) (ett system för behållarkontroll) och [Chronos](https://mesos.github.io/chronos/) (en distribuerad schemaläggare). Mesos har skapats utifrån erfarenheter från Twitter, AirBnb och andra företag med omfattande webbnärvaro. Du kan också använda **swarm** som hello orchestration-motorn.

[kubernetes](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/) är ett system för hantering av virtuella datorer och behållargrupper med öppen källkod som bygger på erfarenheter på Google. Du kan även använda [kubernetes med väv tooprovide nätverksstöd](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave).

[Deis](http://deis.io/overview/) är en öppen källa ”plattform som en-tjänst” (PaaS) som gör det enkelt toodeploy och hantera program på dina egna servrar. Deis versioner vid Docker och virtuell CoreOS tooprovide en förenklad PaaS med ett Heroku inspirerat arbetsflöde.

[CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html), en Linux-distribution med ett optimerat utrymmeskrav, stöd för Docker och ett eget behållarsystem som heter [rkt](https://github.com/coreos/rkt), har också ett hanteringsverktyg för behållargrupper som kallas för [fleet](https://coreos.com/fleet/docs/latest/).

Ubuntu, en annan mycket populär Linux-distribution, ger bra stöd för Docker, men stöder dessutom [Linux-kluster (LXC-typ)](https://help.ubuntu.com/lts/serverguide/lxc.html).

## <a name="tools-for-working-with-azure-vms-and-containers"></a>Verktyg för att arbeta med virtuella Azure-datorer och behållare
I arbetet med behållare och virtuella datorer på Azure används verktyg. Det här avsnittet innehåller en lista med endast en del av hello mest användbara eller viktiga begrepp och verktyg för behållare, grupper och hello större konfiguration och orchestration-verktyg som används med dem..

> [!NOTE]
> Det här området ändrar otroligt snabbt och medan vi gör våra bästa tookeep det här avsnittet och dess länkar in toodate, kanske också en omöjlig uppgift. Kontrollera att du söker på intressanta ämnen tookeep in toodate!
>
>

### <a name="containers-and-vm-technologies"></a>Behållare och tekniker för virtuella datorer
Vissa Linux-behållartekniker:

* [Docker](https://www.docker.com)
* [LXC](https://linuxcontainers.org/)
* [CoreOS och rkt](https://github.com/coreos/rkt)
* [Öppna behållarprojekt](http://opencontainers.org/)
* [RancherOS](http://rancher.com/rancher-os/)

Windows-behållarlänkar:

* [Windows-behållare](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Visual Studio Docker-länkar:

* [Visual Studio Tools for Docker](https://docs.microsoft.com/en-us/dotnet/core/docker/visual-studio-tools-for-docker)

Docker-verktyg:

* [Daemon för Docker](https://docs.docker.com/installation/#installation)
* Docker-klienter
  * [Windows Docker-klienten på Chocolatey](https://chocolatey.org/packages/docker)
  * [Installationsinstruktioner för Docker](https://docs.docker.com/installation/#installation)

Docker på Microsoft Azure:

* [Docker VM-tillägg för Linux på Azure](../articles/virtual-machines/linux/dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Användarhandbok för Azure Docker VM-tillägg](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
* [Med hjälp av hello Docker VM-tillägget från hello Azure-kommandoradsgränssnittet (Azure CLI)](../articles/virtual-machines/linux/classic/cli-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Med hjälp av hello Docker VM-tillägget från hello Azure-portalen](../articles/virtual-machines/linux/classic/portal-use-docker.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)
* [Hur toouse docker-dator på Azure](../articles/virtual-machines/linux/docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hur toouse docker med swarm i Azure](../articles/virtual-machines/virtual-machines-linux-docker-swarm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Kom igång med Docker och Compose på Azure](../articles/virtual-machines/linux/docker-compose-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Snabbt med en Azure-resurs grupp mallen toocreate en Docker-värd på Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
* [Hej inbyggt stöd för `compose` ](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) för befintliga program
* [Implementera ett privat Docker-register på Azure](../articles/virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Linux-distributioner och Azure-exempel:

* [CoreOS](https://coreos.com/os/docs/latest/booting-on-azure.html)

Konfiguration, klusterhantering och behållardirigering:

* [Flotta på CoreOS](https://coreos.com/fleet/docs/latest/)
* Deis

  * [Slutför guiden tooautomated Kubernetes Klusterdistribution med virtuell CoreOS och redan](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
  * [Kubernetes Visualizer](https://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure/)
* [Mesos](http://mesos.apache.org/)

  * [Mesospheres DCOS (Data Center Operating System)](https://docs.mesosphere.com/1.7/overview/design/azure-container-service/)
* [Jenkins](https://jenkins.io/) och [Hudson](http://hudson-ci.org/)

  * [Plugin-programmet Jenkins VM Agent för Azure](https://wiki.jenkins.io/display/JENKINS/Azure+VM+Agents+plugin)
  * [GitHub-repo: Lagringsplugin-programmet Jenkins för Azure](https://github.com/jenkinsci/windows-azure-storage-plugin)
  * [Tredje part: Det sekundära plugin-programmet Hudson för Azure](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
  * [Tredje part: Plugin-programmet Hudson för lagring på Azure](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
* [Azure Automation](https://azure.microsoft.com/services/automation/)

  * [Video: Hur tooUse Azure Automation med virtuella Linux-datorer](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
* PowerShell DSC för Linux

  * [Blogg: Hur toodo Powershell DSC för Linux](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
  * [GitHub: Docker-klient DSC](https://github.com/anweiss/DockerClientDSC)

## <a name="next-steps"></a>Nästa steg
Titta på [Docker](https://www.docker.com) och [Windows-behållare](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview).

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->
