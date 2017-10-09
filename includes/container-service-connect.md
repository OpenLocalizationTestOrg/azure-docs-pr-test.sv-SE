# <a name="make-a-remote-connection-tooa-kubernetes-dcos-or-docker-swarm-cluster"></a>Gör en anslutning till tooa Kubernetes, DC/OS eller Docker Swarm-kluster
När du har skapat ett Azure Container Service-kluster måste tooconnect toohello klustret toodeploy och hantera arbetsbelastningar. Den här artikeln beskriver hur tooconnect toohello master VM av hello kluster från en fjärrdator. 

Hej Kubernetes DC/OS och Docker Swarm-kluster ger lokalt HTTP-slutpunkter. Den här slutpunkten exponeras för Kubernetes, på ett säkert sätt på hello internet och du kan komma åt den genom att köra hello `kubectl` kommandoradsverktyget från valfri Internetansluten dator. 

För DC/OS och Docker Swarm rekommenderar vi att du skapar en secure shell (SSH) tunnel från din lokala dator toohello klusterhanteringssystem. Du kan köra kommandon som använder hello http-slutpunkter och visa hello orchestrator webbgränssnitt (om tillgängligt) från den lokala datorn efter hello tunnel har upprättats. 

## <a name="prerequisites"></a>Krav

* Ett Kubernetes-, DC/OS- eller Docker Swarm-kluster [som distribuerats i Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).
* SSH-RSA fil för privat nyckel, motsvarande toohello offentliga nyckel tillagda toohello kluster under distributionen. Kommandona förutsätter att hello privata SSH-nyckeln finns i `$HOME/.ssh/id_rsa` på datorn. Mer information finns i dessa anvisningar för [macOS och Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) eller [Windows](../articles/virtual-machines/linux/ssh-from-windows.md). Om hello SSH-anslutningen inte fungerar kanske du måste för [återställa SSH-nycklar](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

## <a name="connect-tooa-kubernetes-cluster"></a>Ansluta tooa Kubernetes kluster

Följ dessa steg tooinstall och konfigurera `kubectl` på datorn.

> [!NOTE] 
> Du kan behöva toorun hello kommandon i avsnittet med i Linux eller macOS `sudo`.
> 

### <a name="install-kubectl"></a>Installera kubectl
Enkelriktade tooinstall det här verktyget är toouse hello `az acs kubernetes install-cli` Azure CLI 2.0-kommando. toorun detta kommando kontrollerar du att du [installerat](/cli/azure/install-az-cli2) hello senaste Azure CLI 2.0 och loggas i tooan Azure-konto (`az login`).

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Alternativt kan du hämta hello senaste `kubectl` klient direkt från hello [Kubernetes släpper sidan](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md). Mer information finns i [Installera och konfigurera kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).

### <a name="download-cluster-credentials"></a>Hämta autentiseringsuppgifter för kluster
När du har `kubectl` installerat, behöver du toocopy hello klustret autentiseringsuppgifter tooyour datorn. Enkelriktade toodo get hello autentiseringsuppgifter är med hello `az acs kubernetes get-credentials` kommando. Skicka hello namn hello resursgruppen och hello på hello container service-resurs:

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Detta kommando hämtar hello klustret autentiseringsuppgifter för`$HOME/.kube/config`, där `kubectl` förväntar att det toobe finns.

Du kan också använda `scp` toosecurely kopiera hello fil från `$HOME/.kube/config` på hello master VM tooyour lokala dator. Exempel:

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Om du är i Windows kan använda du Bash på Ubuntu på Windows, hello PuTTy säker fil klienten eller en liknande verktyg.

### <a name="use-kubectl"></a>Använda kubectl

När du har `kubectl` konfigurerad, testa hello-anslutning genom att ange hello noder i klustret:

```bash
kubectl get nodes
```

Du kan prova andra `kubectl`-kommandon. Du kan till exempel visa hello Kubernetes instrumentpanelen. Kör en proxy först toohello Kubernetes API-servern:

```bash
kubectl proxy
```

Hej Kubernetes UI är nu tillgänglig på: `http://localhost:8001/ui`.

Mer information finns i hello [Kubernetes Snabbstart](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-tooa-dcos-or-swarm-cluster"></a>Ansluta tooa DC/OS eller Swarm-kluster

Följ dessa instruktioner toocreate en SSH-tunnel från det lokala systemet för Linux, macOS eller Windows toouse hello DC/OS och Docker Swarm-kluster som distribueras med Azure Container Service. 

> [!NOTE]
> Dessa anvisningar fokuserar på tunnling av TCP-trafik över SSH. Du kan också starta en interaktiv SSH-session med en av hanteringssystem för hello interna kluster, men detta rekommenderas inte. Det finns en risk för oavsiktliga konfigurationsändringar om du arbetar direkt i ett internt system.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a>Skapa en SSH-tunnel i Linux eller macOS
hello första som du kan göra när du skapar en SSH-tunnel i Linux eller macOS är toolocate hello offentliga DNS-namnet på hello belastningsutjämnade huvudservrar. Följ de här stegen:


1. I hello [Azure-portalen](https://portal.azure.com), bläddra toohello resursgruppen som innehåller din behållartjänstklustret. Expandera hello resursgruppen så att varje resurs visas. 

2. Klicka på hello **behållartjänsten** resursen och klickar på **översikt**. Hej **Master FQDN** av hello kluster visas under **Essentials**. Spara det här namnet för senare användning. 

    ![Offentligt DNS-namn](./media/container-service-connect/pubdns.png)

    Du kan också köra hello `az acs show` på behållartjänsten. Leta efter hello **Master-profil: fqdn** egenskap i hello kommandoutdata.

3. Öppna ett gränssnitt och kör hello `ssh` kommandot genom att ange hello följande värden: 

    **LOCAL_PORT** är hello TCP-port på hello hello tunnel tooconnect till tjänsten sida. Ange den här too2375 för Swarm. Ange den här too80 för DC/OS. 
    **REMOTE_PORT** är hello port för hello-slutpunkt som du vill tooexpose. Använd port 2375 för Swarm. Använd port 80 för DC/OS.  
    **ANVÄNDARNAMNET** är hello användarnamn som angavs när du distribuerade hello klustret.  
    **DNSPREFIX** är hello DNS-prefix som du angav när du har distribuerat hello klustret.  
    **REGION** hello region där resursgruppen finns.  
    **PATH_TO_PRIVATE_KEY** [valfritt] är hello sökvägen toohello privata nyckeln som motsvarar toohello offentlig nyckel som du angav när du skapade hello klustret. Använd det här alternativet med hello `-i` flaggan.

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > hello SSH-anslutningsporten är 2200 och hello inte standard port 22. I ett kluster med flera master VM är hello anslutning port toohello första huvudservern VM.
  > 

  hello kommando returnerar utan utdata.

Visa hello exempel för DC/OS och Swarm i hello följande avsnitt.    

### <a name="dcos-tunnel"></a>DC/OS-tunnel
tooopen en tunnel för DC/OS-slutpunkter, köra ett kommando som hello nedan:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Se till att du inte har någon annan lokal process som binder port 80. Om det behövs kan du ange en annan lokal port än port 80, till exempel port 8080. Men vissa länkar i webbgränssnitt kanske inte fungerar när du använder den här porten.
>

Du kan nu komma åt hello DC/OS slutpunkter från det lokala systemet via hello följande URL: er (förutsatt att lokal port 80):

* DC/OS: `http://localhost:80/`
* Marathon: `http://localhost:80/marathon`
* Mesos: `http://localhost:80/mesos`

På liknande sätt kan du nå hello rest API: er för varje program via den här tunneln.

### <a name="swarm-tunnel"></a>Swarm-tunnel
tooopen en toohello Swarm slutpunkt, köra ett kommando som hello nedan:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> Se till att du inte har någon annan lokal port som binder post 2375. Till exempel om du kör hello Docker daemon lokalt kan anges som standardport toouse 2375. Om det behövs kan du ange en annan lokal port än port 2375.
>

Du kan nu komma åt hello Docker Swarm-kluster med hjälp av hello Docker-kommandoradsgränssnittet (Docker CLI) i det lokala systemet. Installationsinstruktioner finns i [Installera Docker](https://docs.docker.com/engine/installation/).

Ange din DOCKER_HOST miljö variabeln toohello lokal port du konfigurerade för hello tunnel. 

```bash
export DOCKER_HOST=:2375
```

Kör kommandon för Docker tunnel toohello Docker Swarm-klustret. Exempel:

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Skapa en SSH-tunnel i Windows
Det finns flera olika sätt att skapa SSH-tunnlar i Windows. Om du kör Bash på Ubuntu på Windows eller ett liknande verktyg kan du följa hello SSH tunnlar anvisningarna som visas tidigare i den här artikeln för macOS och Linux. Som ett alternativ i Windows kan det här avsnittet beskrivs hur toouse PuTTY toocreate hello tunnel.

1. [Hämta PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour Windows-system.

2. Kör hello program.

3. Ange ett värdnamn som består av hello klustrets adminanvändarnamn och hello offentliga DNS-namnet på hello första huvudservern i hello klustret. Hej **värdnamn** ser ut ungefär för`azureuser@PublicDNSName`. Ange 2200 för hello **Port**.

    ![PuTTY-konfiguration 1](./media/container-service-connect/putty1.png)

4. Välj **SSH > Aut**. Lägg till en sökväg tooyour fil för privat nyckel (.ppk format) för autentisering. Du kan använda ett verktyg som [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) toogenerate detta filen från hello SSH-nyckel används toocreate hello kluster.

    ![PuTTY-konfiguration 2](./media/container-service-connect/putty2.png)

5. Välj **SSH > tunnlar** och konfigurera hello följande vidarebefordrade portar:

    * **Källport:** Använd 80 för DC/OS eller 2375 för Swarm.
    * **Mål:** Använd localhost:80 för DC/OS eller localhost:2375 för Swarm.

    följande exempel hello har konfigurerats för DC/OS, men ser ut ungefär för Docker Swarm.

    > [!NOTE]
    > Port 80 får inte användas för något annat när du skapar den här tunneln.
    > 

    ![PuTTY-konfiguration 3](./media/container-service-connect/putty3.png)

6. När du är klar klickar du på **Session > Spara** toosave hello anslutningskonfiguration.

7. tooconnect toohello PuTTY-session genom att klicka på **öppna**. När du ansluter kan se du hello portkonfigurationen i PuTTY hello-händelseloggen.

    ![PuTTY-händelselogg](./media/container-service-connect/putty4.png)

När du har konfigurerat hello tunneln för DC/OS kan du använda hello relaterade slutpunkterna på:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

När du har konfigurerat hello tunneln för Docker Swarm kan öppna din Windows-inställningar tooconfigure en system-miljövariabel som heter `DOCKER_HOST` med värdet `:2375`. Du kan sedan komma åt hello Swarm-klustret via hello Docker CLI.

## <a name="next-steps"></a>Nästa steg
Distribuera och hantera behållare i klustret:

* [Arbeta med Azure Container Service och Kubernetes](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [Arbeta med Azure Container Service och DC/OS](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [Arbeta med hello Azure Container Service och Docker Swarm](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

