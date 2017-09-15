1. <span data-ttu-id="843c8-101">Kopiera installationsprogrammet till en lokal mapp (till exempel /tmp) på den server som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="843c8-101">Copy the installer to a local folder (for example, /tmp) on the server that you want to protect.</span></span> <span data-ttu-id="843c8-102">Kör följande kommandon i en terminal:</span><span class="sxs-lookup"><span data-stu-id="843c8-102">In a terminal, run the following commands:</span></span>
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. <span data-ttu-id="843c8-103">För att installera Mobilitetstjänsten, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="843c8-103">To install Mobility Service, run the following command:</span></span>

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. <span data-ttu-id="843c8-104">När installationen är klar, måste Mobilitetstjänsten registreras på konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="843c8-104">Once installation is complete, the Mobility Service needs to get registered to the configuration server.</span></span> <span data-ttu-id="843c8-105">Kör följande kommando för att registrera Mobilitetstjänsten med konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="843c8-105">Run the following command to register the Mobility Service with Configuration server.</span></span>

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a><span data-ttu-id="843c8-106">Mobilitetstjänstens installationsprogram kommandoraden</span><span class="sxs-lookup"><span data-stu-id="843c8-106">Mobility Service installer command-line</span></span>

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|<span data-ttu-id="843c8-107">Parameter</span><span class="sxs-lookup"><span data-stu-id="843c8-107">Parameter</span></span>|<span data-ttu-id="843c8-108">Typ</span><span class="sxs-lookup"><span data-stu-id="843c8-108">Type</span></span>|<span data-ttu-id="843c8-109">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="843c8-109">Description</span></span>|<span data-ttu-id="843c8-110">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="843c8-110">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="843c8-111">-r</span><span class="sxs-lookup"><span data-stu-id="843c8-111">-r</span></span> |<span data-ttu-id="843c8-112">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="843c8-112">Mandatory</span></span>|<span data-ttu-id="843c8-113">Anger om Mobility Service (MS) som ska installeras eller MasterTarget(MT) ska installeras</span><span class="sxs-lookup"><span data-stu-id="843c8-113">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="843c8-114">MS</span><span class="sxs-lookup"><span data-stu-id="843c8-114">MS</span></span> </br> <span data-ttu-id="843c8-115">HUVUDMÅLSERVERN</span><span class="sxs-lookup"><span data-stu-id="843c8-115">MT</span></span>|
|<span data-ttu-id="843c8-116">-d</span><span class="sxs-lookup"><span data-stu-id="843c8-116">-d</span></span> |<span data-ttu-id="843c8-117">Valfri</span><span class="sxs-lookup"><span data-stu-id="843c8-117">Optional</span></span>|<span data-ttu-id="843c8-118">Plats där Mobilitetstjänsten kommer att installeras</span><span class="sxs-lookup"><span data-stu-id="843c8-118">Location where Mobility Service will be installed</span></span>|<span data-ttu-id="843c8-119">/usr/local/ASR</span><span class="sxs-lookup"><span data-stu-id="843c8-119">/usr/local/ASR</span></span>|
|<span data-ttu-id="843c8-120">-v</span><span class="sxs-lookup"><span data-stu-id="843c8-120">-v</span></span>|<span data-ttu-id="843c8-121">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="843c8-121">Mandatory</span></span>|<span data-ttu-id="843c8-122">Anger plattformen där Mobilitetstjänsten komma installeras</span><span class="sxs-lookup"><span data-stu-id="843c8-122">Specifies the platform on which the Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="843c8-123">- **VMware** : Använd det här värdet om du installerar mobilitetstjänsten på en virtuell dator som körs på *vSphere VMware ESXi-värdar*, *Hyper-V-värdar* och *Phsyical servrar*</span><span class="sxs-lookup"><span data-stu-id="843c8-123">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="843c8-124">- **Azure** : Använd det här värdet om du installerar agenten på en Azure IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="843c8-124">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="843c8-125">VMware</span><span class="sxs-lookup"><span data-stu-id="843c8-125">VMware</span></span> </br> <span data-ttu-id="843c8-126">Azure</span><span class="sxs-lookup"><span data-stu-id="843c8-126">Azure</span></span>|
|<span data-ttu-id="843c8-127">-q</span><span class="sxs-lookup"><span data-stu-id="843c8-127">-q</span></span>|<span data-ttu-id="843c8-128">Valfri</span><span class="sxs-lookup"><span data-stu-id="843c8-128">Optional</span></span>|<span data-ttu-id="843c8-129">Anger om du vill köra installer i tyst läge</span><span class="sxs-lookup"><span data-stu-id="843c8-129">Specifies to run installer in silent mode</span></span>| <span data-ttu-id="843c8-130">Saknas</span><span class="sxs-lookup"><span data-stu-id="843c8-130">N/A</span></span>|


#### <a name="mobility-service-configuration-command-line"></a><span data-ttu-id="843c8-131">Mobility tjänstkonfiguration kommandoraden</span><span class="sxs-lookup"><span data-stu-id="843c8-131">Mobility Service configuration command-line</span></span>

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|<span data-ttu-id="843c8-132">Parameter</span><span class="sxs-lookup"><span data-stu-id="843c8-132">Parameter</span></span>|<span data-ttu-id="843c8-133">Typ</span><span class="sxs-lookup"><span data-stu-id="843c8-133">Type</span></span>|<span data-ttu-id="843c8-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="843c8-134">Description</span></span>|<span data-ttu-id="843c8-135">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="843c8-135">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="843c8-136">-i</span><span class="sxs-lookup"><span data-stu-id="843c8-136">-i</span></span> |<span data-ttu-id="843c8-137">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="843c8-137">Mandatory</span></span>|<span data-ttu-id="843c8-138">Konfigurationsservern IP</span><span class="sxs-lookup"><span data-stu-id="843c8-138">IP of the Configuration Server</span></span>|<span data-ttu-id="843c8-139">Vilken giltig IP-adress som helst</span><span class="sxs-lookup"><span data-stu-id="843c8-139">Any valid IP Address</span></span>|
|<span data-ttu-id="843c8-140">-P</span><span class="sxs-lookup"><span data-stu-id="843c8-140">-P</span></span> |<span data-ttu-id="843c8-141">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="843c8-141">Mandatory</span></span>|<span data-ttu-id="843c8-142">Fullständig sökväg där anslutning lösenfrasen sparas filen</span><span class="sxs-lookup"><span data-stu-id="843c8-142">Full file path the file where the connection passphrase is saved</span></span>|<span data-ttu-id="843c8-143">En giltig mapp</span><span class="sxs-lookup"><span data-stu-id="843c8-143">Any valid folder</span></span>|
