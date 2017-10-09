1. <span data-ttu-id="97654-101">Kopiera hello installer tooa lokal mapp (till exempel /tmp) på hello-server som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="97654-101">Copy hello installer tooa local folder (for example, /tmp) on hello server that you want tooprotect.</span></span> <span data-ttu-id="97654-102">Kör följande kommandon hello i en terminal:</span><span class="sxs-lookup"><span data-stu-id="97654-102">In a terminal, run hello following commands:</span></span>
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. <span data-ttu-id="97654-103">tooinstall Mobilitetstjänsten, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="97654-103">tooinstall Mobility Service, run hello following command:</span></span>

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. <span data-ttu-id="97654-104">När installationen är klar måste hello Mobilitetstjänsten tooget registrerade toohello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="97654-104">Once installation is complete, hello Mobility Service needs tooget registered toohello configuration server.</span></span> <span data-ttu-id="97654-105">Kör följande kommando tooregister hello Mobilitetstjänsten med konfigurationsservern hello.</span><span class="sxs-lookup"><span data-stu-id="97654-105">Run hello following command tooregister hello Mobility Service with Configuration server.</span></span>

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a><span data-ttu-id="97654-106">Mobilitetstjänstens installationsprogram kommandoraden</span><span class="sxs-lookup"><span data-stu-id="97654-106">Mobility Service installer command-line</span></span>

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|<span data-ttu-id="97654-107">Parameter</span><span class="sxs-lookup"><span data-stu-id="97654-107">Parameter</span></span>|<span data-ttu-id="97654-108">Typ</span><span class="sxs-lookup"><span data-stu-id="97654-108">Type</span></span>|<span data-ttu-id="97654-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="97654-109">Description</span></span>|<span data-ttu-id="97654-110">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="97654-110">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="97654-111">-r</span><span class="sxs-lookup"><span data-stu-id="97654-111">-r</span></span> |<span data-ttu-id="97654-112">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="97654-112">Mandatory</span></span>|<span data-ttu-id="97654-113">Anger om Mobility Service (MS) som ska installeras eller MasterTarget(MT) ska installeras</span><span class="sxs-lookup"><span data-stu-id="97654-113">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="97654-114">MS</span><span class="sxs-lookup"><span data-stu-id="97654-114">MS</span></span> </br> <span data-ttu-id="97654-115">HUVUDMÅLSERVERN</span><span class="sxs-lookup"><span data-stu-id="97654-115">MT</span></span>|
|<span data-ttu-id="97654-116">-d</span><span class="sxs-lookup"><span data-stu-id="97654-116">-d</span></span> |<span data-ttu-id="97654-117">Valfri</span><span class="sxs-lookup"><span data-stu-id="97654-117">Optional</span></span>|<span data-ttu-id="97654-118">Plats där Mobilitetstjänsten kommer att installeras</span><span class="sxs-lookup"><span data-stu-id="97654-118">Location where Mobility Service will be installed</span></span>|<span data-ttu-id="97654-119">/usr/local/ASR</span><span class="sxs-lookup"><span data-stu-id="97654-119">/usr/local/ASR</span></span>|
|<span data-ttu-id="97654-120">-v</span><span class="sxs-lookup"><span data-stu-id="97654-120">-v</span></span>|<span data-ttu-id="97654-121">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="97654-121">Mandatory</span></span>|<span data-ttu-id="97654-122">Anger hello plattform på vilken hello Mobilitetstjänsten komma installeras</span><span class="sxs-lookup"><span data-stu-id="97654-122">Specifies hello platform on which hello Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="97654-123">- **VMware** : Använd det här värdet om du installerar mobilitetstjänsten på en virtuell dator som körs på *vSphere VMware ESXi-värdar*, *Hyper-V-värdar* och *Phsyical servrar*</span><span class="sxs-lookup"><span data-stu-id="97654-123">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="97654-124">- **Azure** : Använd det här värdet om du installerar agenten på en Azure IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="97654-124">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="97654-125">VMware</span><span class="sxs-lookup"><span data-stu-id="97654-125">VMware</span></span> </br> <span data-ttu-id="97654-126">Azure</span><span class="sxs-lookup"><span data-stu-id="97654-126">Azure</span></span>|
|<span data-ttu-id="97654-127">-q</span><span class="sxs-lookup"><span data-stu-id="97654-127">-q</span></span>|<span data-ttu-id="97654-128">Valfri</span><span class="sxs-lookup"><span data-stu-id="97654-128">Optional</span></span>|<span data-ttu-id="97654-129">Anger toorun installer i tyst läge</span><span class="sxs-lookup"><span data-stu-id="97654-129">Specifies toorun installer in silent mode</span></span>| <span data-ttu-id="97654-130">Saknas</span><span class="sxs-lookup"><span data-stu-id="97654-130">N/A</span></span>|


#### <a name="mobility-service-configuration-command-line"></a><span data-ttu-id="97654-131">Mobility tjänstkonfiguration kommandoraden</span><span class="sxs-lookup"><span data-stu-id="97654-131">Mobility Service configuration command-line</span></span>

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|<span data-ttu-id="97654-132">Parameter</span><span class="sxs-lookup"><span data-stu-id="97654-132">Parameter</span></span>|<span data-ttu-id="97654-133">Typ</span><span class="sxs-lookup"><span data-stu-id="97654-133">Type</span></span>|<span data-ttu-id="97654-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="97654-134">Description</span></span>|<span data-ttu-id="97654-135">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="97654-135">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="97654-136">-i</span><span class="sxs-lookup"><span data-stu-id="97654-136">-i</span></span> |<span data-ttu-id="97654-137">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="97654-137">Mandatory</span></span>|<span data-ttu-id="97654-138">Hello konfigurationsservern IP</span><span class="sxs-lookup"><span data-stu-id="97654-138">IP of hello Configuration Server</span></span>|<span data-ttu-id="97654-139">Vilken giltig IP-adress som helst</span><span class="sxs-lookup"><span data-stu-id="97654-139">Any valid IP Address</span></span>|
|<span data-ttu-id="97654-140">-P</span><span class="sxs-lookup"><span data-stu-id="97654-140">-P</span></span> |<span data-ttu-id="97654-141">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="97654-141">Mandatory</span></span>|<span data-ttu-id="97654-142">Fullständig sökväg hello fil där hello anslutning lösenfras sparas</span><span class="sxs-lookup"><span data-stu-id="97654-142">Full file path hello file where hello connection passphrase is saved</span></span>|<span data-ttu-id="97654-143">En giltig mapp</span><span class="sxs-lookup"><span data-stu-id="97654-143">Any valid folder</span></span>|
