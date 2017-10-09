# <a name="make-a-remote-connection-tooa-kubernetes-dcos-or-docker-swarm-cluster"></a><span data-ttu-id="cb461-101">Gör en anslutning till tooa Kubernetes, DC/OS eller Docker Swarm-kluster</span><span class="sxs-lookup"><span data-stu-id="cb461-101">Make a remote connection tooa Kubernetes, DC/OS, or Docker Swarm cluster</span></span>
<span data-ttu-id="cb461-102">När du har skapat ett Azure Container Service-kluster måste tooconnect toohello klustret toodeploy och hantera arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="cb461-102">After creating an Azure Container Service cluster, you need tooconnect toohello cluster toodeploy and manage workloads.</span></span> <span data-ttu-id="cb461-103">Den här artikeln beskriver hur tooconnect toohello master VM av hello kluster från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="cb461-103">This article describes how tooconnect toohello master VM of hello cluster from a remote computer.</span></span> 

<span data-ttu-id="cb461-104">Hej Kubernetes DC/OS och Docker Swarm-kluster ger lokalt HTTP-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="cb461-104">hello Kubernetes, DC/OS, and Docker Swarm clusters provide HTTP endpoints locally.</span></span> <span data-ttu-id="cb461-105">Den här slutpunkten exponeras för Kubernetes, på ett säkert sätt på hello internet och du kan komma åt den genom att köra hello `kubectl` kommandoradsverktyget från valfri Internetansluten dator.</span><span class="sxs-lookup"><span data-stu-id="cb461-105">For Kubernetes, this endpoint is securely exposed on hello internet, and you can access it by running hello `kubectl` command-line tool from any internet-connected machine.</span></span> 

<span data-ttu-id="cb461-106">För DC/OS och Docker Swarm rekommenderar vi att du skapar en secure shell (SSH) tunnel från din lokala dator toohello klusterhanteringssystem.</span><span class="sxs-lookup"><span data-stu-id="cb461-106">For DC/OS and Docker Swarm, we recommend that you create a secure shell (SSH) tunnel from your local computer toohello cluster management system.</span></span> <span data-ttu-id="cb461-107">Du kan köra kommandon som använder hello http-slutpunkter och visa hello orchestrator webbgränssnitt (om tillgängligt) från den lokala datorn efter hello tunnel har upprättats.</span><span class="sxs-lookup"><span data-stu-id="cb461-107">After hello tunnel is established, you can run commands which use hello HTTP endpoints and view hello orchestrator's web interface (if available) from your local system.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="cb461-108">Krav</span><span class="sxs-lookup"><span data-stu-id="cb461-108">Prerequisites</span></span>

* <span data-ttu-id="cb461-109">Ett Kubernetes-, DC/OS- eller Docker Swarm-kluster [som distribuerats i Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="cb461-109">A Kubernetes, DC/OS, or Docker Swarm cluster [deployed in Azure Container Service](../articles/container-service/dcos-swarm/container-service-deployment.md).</span></span>
* <span data-ttu-id="cb461-110">SSH-RSA fil för privat nyckel, motsvarande toohello offentliga nyckel tillagda toohello kluster under distributionen.</span><span class="sxs-lookup"><span data-stu-id="cb461-110">SSH RSA private key file, corresponding toohello public key added toohello cluster during deployment.</span></span> <span data-ttu-id="cb461-111">Kommandona förutsätter att hello privata SSH-nyckeln finns i `$HOME/.ssh/id_rsa` på datorn.</span><span class="sxs-lookup"><span data-stu-id="cb461-111">These commands assume that hello private SSH key is in `$HOME/.ssh/id_rsa` on your computer.</span></span> <span data-ttu-id="cb461-112">Mer information finns i dessa anvisningar för [macOS och Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) eller [Windows](../articles/virtual-machines/linux/ssh-from-windows.md).</span><span class="sxs-lookup"><span data-stu-id="cb461-112">See these instructions for [macOS and Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) or [Windows](../articles/virtual-machines/linux/ssh-from-windows.md) for more information.</span></span> <span data-ttu-id="cb461-113">Om hello SSH-anslutningen inte fungerar kanske du måste för [återställa SSH-nycklar](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span><span class="sxs-lookup"><span data-stu-id="cb461-113">If hello SSH connection isn't working, you may need too [reset your SSH keys](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).</span></span>

## <a name="connect-tooa-kubernetes-cluster"></a><span data-ttu-id="cb461-114">Ansluta tooa Kubernetes kluster</span><span class="sxs-lookup"><span data-stu-id="cb461-114">Connect tooa Kubernetes cluster</span></span>

<span data-ttu-id="cb461-115">Följ dessa steg tooinstall och konfigurera `kubectl` på datorn.</span><span class="sxs-lookup"><span data-stu-id="cb461-115">Follow these steps tooinstall and configure `kubectl` on your computer.</span></span>

> [!NOTE] 
> <span data-ttu-id="cb461-116">Du kan behöva toorun hello kommandon i avsnittet med i Linux eller macOS `sudo`.</span><span class="sxs-lookup"><span data-stu-id="cb461-116">On Linux or macOS, you might need toorun hello commands in this section using `sudo`.</span></span>
> 

### <a name="install-kubectl"></a><span data-ttu-id="cb461-117">Installera kubectl</span><span class="sxs-lookup"><span data-stu-id="cb461-117">Install kubectl</span></span>
<span data-ttu-id="cb461-118">Enkelriktade tooinstall det här verktyget är toouse hello `az acs kubernetes install-cli` Azure CLI 2.0-kommando.</span><span class="sxs-lookup"><span data-stu-id="cb461-118">One way tooinstall this tool is toouse hello `az acs kubernetes install-cli` Azure CLI 2.0 command.</span></span> <span data-ttu-id="cb461-119">toorun detta kommando kontrollerar du att du [installerat](/cli/azure/install-az-cli2) hello senaste Azure CLI 2.0 och loggas i tooan Azure-konto (`az login`).</span><span class="sxs-lookup"><span data-stu-id="cb461-119">toorun this command, make sure that you [installed](/cli/azure/install-az-cli2) hello latest Azure CLI 2.0 and logged in tooan Azure account (`az login`).</span></span>

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

<span data-ttu-id="cb461-120">Alternativt kan du hämta hello senaste `kubectl` klient direkt från hello [Kubernetes släpper sidan](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span><span class="sxs-lookup"><span data-stu-id="cb461-120">Alternatively, you can download hello latest `kubectl` client directly from hello [Kubernetes releases page](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md).</span></span> <span data-ttu-id="cb461-121">Mer information finns i [Installera och konfigurera kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span><span class="sxs-lookup"><span data-stu-id="cb461-121">For more information, see [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/).</span></span>

### <a name="download-cluster-credentials"></a><span data-ttu-id="cb461-122">Hämta autentiseringsuppgifter för kluster</span><span class="sxs-lookup"><span data-stu-id="cb461-122">Download cluster credentials</span></span>
<span data-ttu-id="cb461-123">När du har `kubectl` installerat, behöver du toocopy hello klustret autentiseringsuppgifter tooyour datorn.</span><span class="sxs-lookup"><span data-stu-id="cb461-123">Once you have `kubectl` installed, you need toocopy hello cluster credentials tooyour machine.</span></span> <span data-ttu-id="cb461-124">Enkelriktade toodo get hello autentiseringsuppgifter är med hello `az acs kubernetes get-credentials` kommando.</span><span class="sxs-lookup"><span data-stu-id="cb461-124">One way toodo get hello credentials is with hello `az acs kubernetes get-credentials` command.</span></span> <span data-ttu-id="cb461-125">Skicka hello namn hello resursgruppen och hello på hello container service-resurs:</span><span class="sxs-lookup"><span data-stu-id="cb461-125">Pass hello name of hello resource group and hello name of hello container service resource:</span></span>

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

<span data-ttu-id="cb461-126">Detta kommando hämtar hello klustret autentiseringsuppgifter för`$HOME/.kube/config`, där `kubectl` förväntar att det toobe finns.</span><span class="sxs-lookup"><span data-stu-id="cb461-126">This command downloads hello cluster credentials too`$HOME/.kube/config`, where `kubectl` expects it toobe located.</span></span>

<span data-ttu-id="cb461-127">Du kan också använda `scp` toosecurely kopiera hello fil från `$HOME/.kube/config` på hello master VM tooyour lokala dator.</span><span class="sxs-lookup"><span data-stu-id="cb461-127">Alternatively, you can use `scp` toosecurely copy hello file from `$HOME/.kube/config` on hello master VM tooyour local machine.</span></span> <span data-ttu-id="cb461-128">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cb461-128">For example:</span></span>

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

<span data-ttu-id="cb461-129">Om du är i Windows kan använda du Bash på Ubuntu på Windows, hello PuTTy säker fil klienten eller en liknande verktyg.</span><span class="sxs-lookup"><span data-stu-id="cb461-129">If you are on Windows, you can use Bash on Ubuntu on Windows, hello PuTTy secure file copy client, or a similar tool.</span></span>

### <a name="use-kubectl"></a><span data-ttu-id="cb461-130">Använda kubectl</span><span class="sxs-lookup"><span data-stu-id="cb461-130">Use kubectl</span></span>

<span data-ttu-id="cb461-131">När du har `kubectl` konfigurerad, testa hello-anslutning genom att ange hello noder i klustret:</span><span class="sxs-lookup"><span data-stu-id="cb461-131">Once you have `kubectl` configured, test hello connection by listing hello nodes in your cluster:</span></span>

```bash
kubectl get nodes
```

<span data-ttu-id="cb461-132">Du kan prova andra `kubectl`-kommandon.</span><span class="sxs-lookup"><span data-stu-id="cb461-132">You can try other `kubectl` commands.</span></span> <span data-ttu-id="cb461-133">Du kan till exempel visa hello Kubernetes instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="cb461-133">For example, you can view hello Kubernetes Dashboard.</span></span> <span data-ttu-id="cb461-134">Kör en proxy först toohello Kubernetes API-servern:</span><span class="sxs-lookup"><span data-stu-id="cb461-134">First, run a proxy toohello Kubernetes API server:</span></span>

```bash
kubectl proxy
```

<span data-ttu-id="cb461-135">Hej Kubernetes UI är nu tillgänglig på: `http://localhost:8001/ui`.</span><span class="sxs-lookup"><span data-stu-id="cb461-135">hello Kubernetes UI is now available at: `http://localhost:8001/ui`.</span></span>

<span data-ttu-id="cb461-136">Mer information finns i hello [Kubernetes Snabbstart](http://kubernetes.io/docs/user-guide/quick-start/).</span><span class="sxs-lookup"><span data-stu-id="cb461-136">For more information, see hello [Kubernetes quick start](http://kubernetes.io/docs/user-guide/quick-start/).</span></span>

## <a name="connect-tooa-dcos-or-swarm-cluster"></a><span data-ttu-id="cb461-137">Ansluta tooa DC/OS eller Swarm-kluster</span><span class="sxs-lookup"><span data-stu-id="cb461-137">Connect tooa DC/OS or Swarm cluster</span></span>

<span data-ttu-id="cb461-138">Följ dessa instruktioner toocreate en SSH-tunnel från det lokala systemet för Linux, macOS eller Windows toouse hello DC/OS och Docker Swarm-kluster som distribueras med Azure Container Service.</span><span class="sxs-lookup"><span data-stu-id="cb461-138">toouse hello DC/OS and Docker Swarm clusters deployed by Azure Container Service, follow these instructions toocreate a SSH tunnel from your local Linux, macOS, or Windows system.</span></span> 

> [!NOTE]
> <span data-ttu-id="cb461-139">Dessa anvisningar fokuserar på tunnling av TCP-trafik över SSH.</span><span class="sxs-lookup"><span data-stu-id="cb461-139">These instructions focus on tunneling TCP traffic over SSH.</span></span> <span data-ttu-id="cb461-140">Du kan också starta en interaktiv SSH-session med en av hanteringssystem för hello interna kluster, men detta rekommenderas inte.</span><span class="sxs-lookup"><span data-stu-id="cb461-140">You can also start an interactive SSH session with one of hello internal cluster management systems, but we don't recommend this.</span></span> <span data-ttu-id="cb461-141">Det finns en risk för oavsiktliga konfigurationsändringar om du arbetar direkt i ett internt system.</span><span class="sxs-lookup"><span data-stu-id="cb461-141">Working directly on an internal system risks inadvertent configuration changes.</span></span>  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a><span data-ttu-id="cb461-142">Skapa en SSH-tunnel i Linux eller macOS</span><span class="sxs-lookup"><span data-stu-id="cb461-142">Create an SSH tunnel on Linux or macOS</span></span>
<span data-ttu-id="cb461-143">hello första som du kan göra när du skapar en SSH-tunnel i Linux eller macOS är toolocate hello offentliga DNS-namnet på hello belastningsutjämnade huvudservrar.</span><span class="sxs-lookup"><span data-stu-id="cb461-143">hello first thing that you do when you create an SSH tunnel on Linux or macOS is toolocate hello public DNS name of hello load-balanced masters.</span></span> <span data-ttu-id="cb461-144">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="cb461-144">Follow these steps:</span></span>


1. <span data-ttu-id="cb461-145">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello resursgruppen som innehåller din behållartjänstklustret.</span><span class="sxs-lookup"><span data-stu-id="cb461-145">In hello [Azure portal](https://portal.azure.com), browse toohello resource group containing your container service cluster.</span></span> <span data-ttu-id="cb461-146">Expandera hello resursgruppen så att varje resurs visas.</span><span class="sxs-lookup"><span data-stu-id="cb461-146">Expand hello resource group so that each resource is displayed.</span></span> 

2. <span data-ttu-id="cb461-147">Klicka på hello **behållartjänsten** resursen och klickar på **översikt**.</span><span class="sxs-lookup"><span data-stu-id="cb461-147">Click hello **Container service** resource, and click **Overview**.</span></span> <span data-ttu-id="cb461-148">Hej **Master FQDN** av hello kluster visas under **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="cb461-148">hello **Master FQDN** of hello cluster appears under **Essentials**.</span></span> <span data-ttu-id="cb461-149">Spara det här namnet för senare användning.</span><span class="sxs-lookup"><span data-stu-id="cb461-149">Save this name for later use.</span></span> 

    ![Offentligt DNS-namn](./media/container-service-connect/pubdns.png)

    <span data-ttu-id="cb461-151">Du kan också köra hello `az acs show` på behållartjänsten.</span><span class="sxs-lookup"><span data-stu-id="cb461-151">Alternatively, run hello `az acs show` command on your container service.</span></span> <span data-ttu-id="cb461-152">Leta efter hello **Master-profil: fqdn** egenskap i hello kommandoutdata.</span><span class="sxs-lookup"><span data-stu-id="cb461-152">Look for hello **Master Profile:fqdn** property in hello command output.</span></span>

3. <span data-ttu-id="cb461-153">Öppna ett gränssnitt och kör hello `ssh` kommandot genom att ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="cb461-153">Now open a shell and run hello `ssh` command by specifying hello following values:</span></span> 

    <span data-ttu-id="cb461-154">**LOCAL_PORT** är hello TCP-port på hello hello tunnel tooconnect till tjänsten sida.</span><span class="sxs-lookup"><span data-stu-id="cb461-154">**LOCAL_PORT** is hello TCP port on hello service side of hello tunnel tooconnect to.</span></span> <span data-ttu-id="cb461-155">Ange den här too2375 för Swarm.</span><span class="sxs-lookup"><span data-stu-id="cb461-155">For Swarm, set this too2375.</span></span> <span data-ttu-id="cb461-156">Ange den här too80 för DC/OS.</span><span class="sxs-lookup"><span data-stu-id="cb461-156">For DC/OS, set this too80.</span></span> 
    <span data-ttu-id="cb461-157">**REMOTE_PORT** är hello port för hello-slutpunkt som du vill tooexpose.</span><span class="sxs-lookup"><span data-stu-id="cb461-157">**REMOTE_PORT** is hello port of hello endpoint that you want tooexpose.</span></span> <span data-ttu-id="cb461-158">Använd port 2375 för Swarm.</span><span class="sxs-lookup"><span data-stu-id="cb461-158">For Swarm, use port 2375.</span></span> <span data-ttu-id="cb461-159">Använd port 80 för DC/OS.</span><span class="sxs-lookup"><span data-stu-id="cb461-159">For DC/OS, use port 80.</span></span>  
    <span data-ttu-id="cb461-160">**ANVÄNDARNAMNET** är hello användarnamn som angavs när du distribuerade hello klustret.</span><span class="sxs-lookup"><span data-stu-id="cb461-160">**USERNAME** is hello user name that was provided when you deployed hello cluster.</span></span>  
    <span data-ttu-id="cb461-161">**DNSPREFIX** är hello DNS-prefix som du angav när du har distribuerat hello klustret.</span><span class="sxs-lookup"><span data-stu-id="cb461-161">**DNSPREFIX** is hello DNS prefix that you provided when you deployed hello cluster.</span></span>  
    <span data-ttu-id="cb461-162">**REGION** hello region där resursgruppen finns.</span><span class="sxs-lookup"><span data-stu-id="cb461-162">**REGION** is hello region in which your resource group is located.</span></span>  
    <span data-ttu-id="cb461-163">**PATH_TO_PRIVATE_KEY** [valfritt] är hello sökvägen toohello privata nyckeln som motsvarar toohello offentlig nyckel som du angav när du skapade hello klustret.</span><span class="sxs-lookup"><span data-stu-id="cb461-163">**PATH_TO_PRIVATE_KEY** [OPTIONAL] is hello path toohello private key that corresponds toohello public key you provided when you created hello cluster.</span></span> <span data-ttu-id="cb461-164">Använd det här alternativet med hello `-i` flaggan.</span><span class="sxs-lookup"><span data-stu-id="cb461-164">Use this option with hello `-i` flag.</span></span>

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > <span data-ttu-id="cb461-165">hello SSH-anslutningsporten är 2200 och hello inte standard port 22.</span><span class="sxs-lookup"><span data-stu-id="cb461-165">hello SSH connection port is 2200 and not hello standard port 22.</span></span> <span data-ttu-id="cb461-166">I ett kluster med flera master VM är hello anslutning port toohello första huvudservern VM.</span><span class="sxs-lookup"><span data-stu-id="cb461-166">In a cluster with more than one master VM, this is hello connection port toohello first master VM.</span></span>
  > 

  <span data-ttu-id="cb461-167">hello kommando returnerar utan utdata.</span><span class="sxs-lookup"><span data-stu-id="cb461-167">hello command returns without output.</span></span>

<span data-ttu-id="cb461-168">Visa hello exempel för DC/OS och Swarm i hello följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="cb461-168">See hello examples for DC/OS and Swarm in hello following sections.</span></span>    

### <a name="dcos-tunnel"></a><span data-ttu-id="cb461-169">DC/OS-tunnel</span><span class="sxs-lookup"><span data-stu-id="cb461-169">DC/OS tunnel</span></span>
<span data-ttu-id="cb461-170">tooopen en tunnel för DC/OS-slutpunkter, köra ett kommando som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="cb461-170">tooopen a tunnel for DC/OS endpoints, run a command like hello following:</span></span>

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> <span data-ttu-id="cb461-171">Se till att du inte har någon annan lokal process som binder port 80.</span><span class="sxs-lookup"><span data-stu-id="cb461-171">Ensure that you do not have another local process that binds port 80.</span></span> <span data-ttu-id="cb461-172">Om det behövs kan du ange en annan lokal port än port 80, till exempel port 8080.</span><span class="sxs-lookup"><span data-stu-id="cb461-172">If necessary, you can specify a local port other than port 80, such as port 8080.</span></span> <span data-ttu-id="cb461-173">Men vissa länkar i webbgränssnitt kanske inte fungerar när du använder den här porten.</span><span class="sxs-lookup"><span data-stu-id="cb461-173">However, some web UI links might not work when you use this port.</span></span>
>

<span data-ttu-id="cb461-174">Du kan nu komma åt hello DC/OS slutpunkter från det lokala systemet via hello följande URL: er (förutsatt att lokal port 80):</span><span class="sxs-lookup"><span data-stu-id="cb461-174">You can now access hello DC/OS endpoints from your local system through hello following URLs (assuming local port 80):</span></span>

* <span data-ttu-id="cb461-175">DC/OS: `http://localhost:80/`</span><span class="sxs-lookup"><span data-stu-id="cb461-175">DC/OS: `http://localhost:80/`</span></span>
* <span data-ttu-id="cb461-176">Marathon: `http://localhost:80/marathon`</span><span class="sxs-lookup"><span data-stu-id="cb461-176">Marathon: `http://localhost:80/marathon`</span></span>
* <span data-ttu-id="cb461-177">Mesos: `http://localhost:80/mesos`</span><span class="sxs-lookup"><span data-stu-id="cb461-177">Mesos: `http://localhost:80/mesos`</span></span>

<span data-ttu-id="cb461-178">På liknande sätt kan du nå hello rest API: er för varje program via den här tunneln.</span><span class="sxs-lookup"><span data-stu-id="cb461-178">Similarly, you can reach hello rest APIs for each application through this tunnel.</span></span>

### <a name="swarm-tunnel"></a><span data-ttu-id="cb461-179">Swarm-tunnel</span><span class="sxs-lookup"><span data-stu-id="cb461-179">Swarm tunnel</span></span>
<span data-ttu-id="cb461-180">tooopen en toohello Swarm slutpunkt, köra ett kommando som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="cb461-180">tooopen a tunnel toohello Swarm endpoint, run a command like hello following:</span></span>

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> <span data-ttu-id="cb461-181">Se till att du inte har någon annan lokal port som binder post 2375.</span><span class="sxs-lookup"><span data-stu-id="cb461-181">Ensure that you do not have another local process that binds port 2375.</span></span> <span data-ttu-id="cb461-182">Till exempel om du kör hello Docker daemon lokalt kan anges som standardport toouse 2375.</span><span class="sxs-lookup"><span data-stu-id="cb461-182">For example, if you are running hello Docker daemon locally, it's set by default toouse port 2375.</span></span> <span data-ttu-id="cb461-183">Om det behövs kan du ange en annan lokal port än port 2375.</span><span class="sxs-lookup"><span data-stu-id="cb461-183">If necessary, you can specify a local port other than port 2375.</span></span>
>

<span data-ttu-id="cb461-184">Du kan nu komma åt hello Docker Swarm-kluster med hjälp av hello Docker-kommandoradsgränssnittet (Docker CLI) i det lokala systemet.</span><span class="sxs-lookup"><span data-stu-id="cb461-184">Now you can access hello Docker Swarm cluster using hello Docker command-line interface (Docker CLI) on your local system.</span></span> <span data-ttu-id="cb461-185">Installationsinstruktioner finns i [Installera Docker](https://docs.docker.com/engine/installation/).</span><span class="sxs-lookup"><span data-stu-id="cb461-185">For installation instructions, see [Install Docker](https://docs.docker.com/engine/installation/).</span></span>

<span data-ttu-id="cb461-186">Ange din DOCKER_HOST miljö variabeln toohello lokal port du konfigurerade för hello tunnel.</span><span class="sxs-lookup"><span data-stu-id="cb461-186">Set your DOCKER_HOST environment variable toohello local port you configured for hello tunnel.</span></span> 

```bash
export DOCKER_HOST=:2375
```

<span data-ttu-id="cb461-187">Kör kommandon för Docker tunnel toohello Docker Swarm-klustret.</span><span class="sxs-lookup"><span data-stu-id="cb461-187">Run Docker commands that tunnel toohello Docker Swarm cluster.</span></span> <span data-ttu-id="cb461-188">Exempel:</span><span class="sxs-lookup"><span data-stu-id="cb461-188">For example:</span></span>

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a><span data-ttu-id="cb461-189">Skapa en SSH-tunnel i Windows</span><span class="sxs-lookup"><span data-stu-id="cb461-189">Create an SSH tunnel on Windows</span></span>
<span data-ttu-id="cb461-190">Det finns flera olika sätt att skapa SSH-tunnlar i Windows.</span><span class="sxs-lookup"><span data-stu-id="cb461-190">There are multiple options for creating SSH tunnels on Windows.</span></span> <span data-ttu-id="cb461-191">Om du kör Bash på Ubuntu på Windows eller ett liknande verktyg kan du följa hello SSH tunnlar anvisningarna som visas tidigare i den här artikeln för macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="cb461-191">If you are running Bash on Ubuntu on Windows or a similar tool, you can follow hello SSH tunneling instructions shown earlier in this article for macOS and Linux.</span></span> <span data-ttu-id="cb461-192">Som ett alternativ i Windows kan det här avsnittet beskrivs hur toouse PuTTY toocreate hello tunnel.</span><span class="sxs-lookup"><span data-stu-id="cb461-192">As an alternative on Windows, this section describes how toouse PuTTY toocreate hello tunnel.</span></span>

1. <span data-ttu-id="cb461-193">[Hämta PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour Windows-system.</span><span class="sxs-lookup"><span data-stu-id="cb461-193">[Download PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) tooyour Windows system.</span></span>

2. <span data-ttu-id="cb461-194">Kör hello program.</span><span class="sxs-lookup"><span data-stu-id="cb461-194">Run hello application.</span></span>

3. <span data-ttu-id="cb461-195">Ange ett värdnamn som består av hello klustrets adminanvändarnamn och hello offentliga DNS-namnet på hello första huvudservern i hello klustret.</span><span class="sxs-lookup"><span data-stu-id="cb461-195">Enter a host name that is comprised of hello cluster admin user name and hello public DNS name of hello first master in hello cluster.</span></span> <span data-ttu-id="cb461-196">Hej **värdnamn** ser ut ungefär för`azureuser@PublicDNSName`.</span><span class="sxs-lookup"><span data-stu-id="cb461-196">hello **Host Name** looks similar too`azureuser@PublicDNSName`.</span></span> <span data-ttu-id="cb461-197">Ange 2200 för hello **Port**.</span><span class="sxs-lookup"><span data-stu-id="cb461-197">Enter 2200 for hello **Port**.</span></span>

    ![PuTTY-konfiguration 1](./media/container-service-connect/putty1.png)

4. <span data-ttu-id="cb461-199">Välj **SSH > Aut**. Lägg till en sökväg tooyour fil för privat nyckel (.ppk format) för autentisering.</span><span class="sxs-lookup"><span data-stu-id="cb461-199">Select **SSH > Auth**. Add a path tooyour private key file (.ppk format) for authentication.</span></span> <span data-ttu-id="cb461-200">Du kan använda ett verktyg som [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) toogenerate detta filen från hello SSH-nyckel används toocreate hello kluster.</span><span class="sxs-lookup"><span data-stu-id="cb461-200">You can use a tool such as [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) toogenerate this file from hello SSH key used toocreate hello cluster.</span></span>

    ![PuTTY-konfiguration 2](./media/container-service-connect/putty2.png)

5. <span data-ttu-id="cb461-202">Välj **SSH > tunnlar** och konfigurera hello följande vidarebefordrade portar:</span><span class="sxs-lookup"><span data-stu-id="cb461-202">Select **SSH > Tunnels** and configure hello following forwarded ports:</span></span>

    * <span data-ttu-id="cb461-203">**Källport:** Använd 80 för DC/OS eller 2375 för Swarm.</span><span class="sxs-lookup"><span data-stu-id="cb461-203">**Source Port:** Use 80 for DC/OS or 2375 for Swarm.</span></span>
    * <span data-ttu-id="cb461-204">**Mål:** Använd localhost:80 för DC/OS eller localhost:2375 för Swarm.</span><span class="sxs-lookup"><span data-stu-id="cb461-204">**Destination:** Use localhost:80 for DC/OS or localhost:2375 for Swarm.</span></span>

    <span data-ttu-id="cb461-205">följande exempel hello har konfigurerats för DC/OS, men ser ut ungefär för Docker Swarm.</span><span class="sxs-lookup"><span data-stu-id="cb461-205">hello following example is configured for DC/OS, but will look similar for Docker Swarm.</span></span>

    > [!NOTE]
    > <span data-ttu-id="cb461-206">Port 80 får inte användas för något annat när du skapar den här tunneln.</span><span class="sxs-lookup"><span data-stu-id="cb461-206">Port 80 must not be in use when you create this tunnel.</span></span>
    > 

    ![PuTTY-konfiguration 3](./media/container-service-connect/putty3.png)

6. <span data-ttu-id="cb461-208">När du är klar klickar du på **Session > Spara** toosave hello anslutningskonfiguration.</span><span class="sxs-lookup"><span data-stu-id="cb461-208">When you're finished, click **Session > Save** toosave hello connection configuration.</span></span>

7. <span data-ttu-id="cb461-209">tooconnect toohello PuTTY-session genom att klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="cb461-209">tooconnect toohello PuTTY session, click **Open**.</span></span> <span data-ttu-id="cb461-210">När du ansluter kan se du hello portkonfigurationen i PuTTY hello-händelseloggen.</span><span class="sxs-lookup"><span data-stu-id="cb461-210">When you connect, you can see hello port configuration in hello PuTTY event log.</span></span>

    ![PuTTY-händelselogg](./media/container-service-connect/putty4.png)

<span data-ttu-id="cb461-212">När du har konfigurerat hello tunneln för DC/OS kan du använda hello relaterade slutpunkterna på:</span><span class="sxs-lookup"><span data-stu-id="cb461-212">After you've configured hello tunnel for DC/OS, you can access hello related endpoints at:</span></span>

* <span data-ttu-id="cb461-213">DC/OS: `http://localhost/`</span><span class="sxs-lookup"><span data-stu-id="cb461-213">DC/OS: `http://localhost/`</span></span>
* <span data-ttu-id="cb461-214">Marathon: `http://localhost/marathon`</span><span class="sxs-lookup"><span data-stu-id="cb461-214">Marathon: `http://localhost/marathon`</span></span>
* <span data-ttu-id="cb461-215">Mesos: `http://localhost/mesos`</span><span class="sxs-lookup"><span data-stu-id="cb461-215">Mesos: `http://localhost/mesos`</span></span>

<span data-ttu-id="cb461-216">När du har konfigurerat hello tunneln för Docker Swarm kan öppna din Windows-inställningar tooconfigure en system-miljövariabel som heter `DOCKER_HOST` med värdet `:2375`.</span><span class="sxs-lookup"><span data-stu-id="cb461-216">After you've configured hello tunnel for Docker Swarm, open your Windows settings tooconfigure a system environment variable named `DOCKER_HOST` with a value of `:2375`.</span></span> <span data-ttu-id="cb461-217">Du kan sedan komma åt hello Swarm-klustret via hello Docker CLI.</span><span class="sxs-lookup"><span data-stu-id="cb461-217">Then, you can access hello Swarm cluster through hello Docker CLI.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cb461-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cb461-218">Next steps</span></span>
<span data-ttu-id="cb461-219">Distribuera och hantera behållare i klustret:</span><span class="sxs-lookup"><span data-stu-id="cb461-219">Deploy and manage containers in your cluster:</span></span>

* [<span data-ttu-id="cb461-220">Arbeta med Azure Container Service och Kubernetes</span><span class="sxs-lookup"><span data-stu-id="cb461-220">Work with Azure Container Service and Kubernetes</span></span>](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [<span data-ttu-id="cb461-221">Arbeta med Azure Container Service och DC/OS</span><span class="sxs-lookup"><span data-stu-id="cb461-221">Work with Azure Container Service and DC/OS</span></span>](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [<span data-ttu-id="cb461-222">Arbeta med hello Azure Container Service och Docker Swarm</span><span class="sxs-lookup"><span data-stu-id="cb461-222">Work with hello Azure Container Service and Docker Swarm</span></span>](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

