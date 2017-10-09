1. <span data-ttu-id="6d4e4-101">Kopiera hello installer tooa lokal mapp (exempelvis C:\Temp) på hello-server som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="6d4e4-101">Copy hello installer tooa local folder (for example, C:\Temp) on hello server that you want tooprotect.</span></span> <span data-ttu-id="6d4e4-102">Kör följande kommandon som administratör i Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="6d4e4-102">Run hello following commands as an administrator at a command prompt:</span></span>

  ```
  cd C:\Temp
  ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
  MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
  cd C:\Temp\Extracted.
  ```
2. <span data-ttu-id="6d4e4-103">tooinstall Mobilitetstjänsten, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6d4e4-103">tooinstall Mobility Service, run hello following command:</span></span>

  ```
  UnifiedAgent.exe /Role "MS" /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
  ```
3. <span data-ttu-id="6d4e4-104">Hello-agenten måste nu toobe som registrerats med hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="6d4e4-104">Now hello agent needs toobe registered with hello Configuration Server.</span></span>

  ```
  cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
  UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
  ```

#### <a name="mobility-service-installer-command-line-arguments"></a><span data-ttu-id="6d4e4-105">Tjänsten Mobility installer kommandoradsargument</span><span class="sxs-lookup"><span data-stu-id="6d4e4-105">Mobility Service installer command-line arguments</span></span>

```
Usage :
UnifiedAgent.exe /Role <MS|MT> /InstallLocation <Install Location> /Platform “VmWare” /Silent
```

| <span data-ttu-id="6d4e4-106">Parameter</span><span class="sxs-lookup"><span data-stu-id="6d4e4-106">Parameter</span></span>|<span data-ttu-id="6d4e4-107">Typ</span><span class="sxs-lookup"><span data-stu-id="6d4e4-107">Type</span></span>|<span data-ttu-id="6d4e4-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6d4e4-108">Description</span></span>|<span data-ttu-id="6d4e4-109">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="6d4e4-109">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="6d4e4-110">/ Roll</span><span class="sxs-lookup"><span data-stu-id="6d4e4-110">/Role</span></span>|<span data-ttu-id="6d4e4-111">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="6d4e4-111">Mandatory</span></span>|<span data-ttu-id="6d4e4-112">Anger om Mobility Service (MS) som ska installeras eller MasterTarget(MT) ska installeras</span><span class="sxs-lookup"><span data-stu-id="6d4e4-112">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="6d4e4-113">MS</span><span class="sxs-lookup"><span data-stu-id="6d4e4-113">MS</span></span> </br> <span data-ttu-id="6d4e4-114">HUVUDMÅLSERVERN</span><span class="sxs-lookup"><span data-stu-id="6d4e4-114">MT</span></span>|
|<span data-ttu-id="6d4e4-115">/InstallLocation</span><span class="sxs-lookup"><span data-stu-id="6d4e4-115">/InstallLocation</span></span>|<span data-ttu-id="6d4e4-116">Valfri</span><span class="sxs-lookup"><span data-stu-id="6d4e4-116">Optional</span></span>|<span data-ttu-id="6d4e4-117">Plats där Mobilitetstjänsten är installerad</span><span class="sxs-lookup"><span data-stu-id="6d4e4-117">Location where Mobility Service is installed</span></span>|<span data-ttu-id="6d4e4-118">En mapp på hello-dator</span><span class="sxs-lookup"><span data-stu-id="6d4e4-118">Any folder on hello computer</span></span>|
|<span data-ttu-id="6d4e4-119">/ Platform</span><span class="sxs-lookup"><span data-stu-id="6d4e4-119">/Platform</span></span>|<span data-ttu-id="6d4e4-120">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="6d4e4-120">Mandatory</span></span>|<span data-ttu-id="6d4e4-121">Anger hello plattform på vilken hello Mobilitetstjänsten komma installeras</span><span class="sxs-lookup"><span data-stu-id="6d4e4-121">Specifies hello platform on which hello Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="6d4e4-122">- **VMware** : Använd det här värdet om du installerar mobilitetstjänsten på en virtuell dator som körs på *vSphere VMware ESXi-värdar*, *Hyper-V-värdar* och *Phsyical servrar*</span><span class="sxs-lookup"><span data-stu-id="6d4e4-122">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="6d4e4-123">- **Azure** : Använd det här värdet om du installerar agenten på en Azure IaaS-VM</span><span class="sxs-lookup"><span data-stu-id="6d4e4-123">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="6d4e4-124">VMware</span><span class="sxs-lookup"><span data-stu-id="6d4e4-124">VMware</span></span> </br> <span data-ttu-id="6d4e4-125">Azure</span><span class="sxs-lookup"><span data-stu-id="6d4e4-125">Azure</span></span>|
|<span data-ttu-id="6d4e4-126">/ Tyst</span><span class="sxs-lookup"><span data-stu-id="6d4e4-126">/Silent</span></span>|<span data-ttu-id="6d4e4-127">Valfri</span><span class="sxs-lookup"><span data-stu-id="6d4e4-127">Optional</span></span>|<span data-ttu-id="6d4e4-128">Anger toorun hello installer i tyst läge</span><span class="sxs-lookup"><span data-stu-id="6d4e4-128">Specifies toorun hello installer in silent mode</span></span>| <span data-ttu-id="6d4e4-129">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="6d4e4-129">NA</span></span>|

>[!TIP]
> <span data-ttu-id="6d4e4-130">hello installationsloggarna finns under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log</span><span class="sxs-lookup"><span data-stu-id="6d4e4-130">hello setup logs can be found under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentInstaller.log</span></span>

#### <a name="mobility-service-registration-command-line-arguments"></a><span data-ttu-id="6d4e4-131">Tjänsten Mobility registrering kommandoradsargument</span><span class="sxs-lookup"><span data-stu-id="6d4e4-131">Mobility Service registration command-line arguments</span></span>

```
Usage :
UnifiedAgentConfigurator.exe”  /CSEndPoint <CSIP> /PassphraseFilePath <PassphraseFilePath>
```

  | <span data-ttu-id="6d4e4-132">Parameter</span><span class="sxs-lookup"><span data-stu-id="6d4e4-132">Parameter</span></span>|<span data-ttu-id="6d4e4-133">Typ</span><span class="sxs-lookup"><span data-stu-id="6d4e4-133">Type</span></span>|<span data-ttu-id="6d4e4-134">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6d4e4-134">Description</span></span>|<span data-ttu-id="6d4e4-135">Möjliga värden</span><span class="sxs-lookup"><span data-stu-id="6d4e4-135">Possible values</span></span>|
  |-|-|-|-|
  |<span data-ttu-id="6d4e4-136">/ CSEndPoint</span><span class="sxs-lookup"><span data-stu-id="6d4e4-136">/CSEndPoint</span></span> |<span data-ttu-id="6d4e4-137">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="6d4e4-137">Mandatory</span></span>|<span data-ttu-id="6d4e4-138">IP-adress för konfigurationsservern hello</span><span class="sxs-lookup"><span data-stu-id="6d4e4-138">IP address of hello configuration server</span></span>| <span data-ttu-id="6d4e4-139">En giltig IP-adress</span><span class="sxs-lookup"><span data-stu-id="6d4e4-139">Any valid IP address</span></span>|
  |<span data-ttu-id="6d4e4-140">/PassphraseFilePath</span><span class="sxs-lookup"><span data-stu-id="6d4e4-140">/PassphraseFilePath</span></span>|<span data-ttu-id="6d4e4-141">Obligatorisk</span><span class="sxs-lookup"><span data-stu-id="6d4e4-141">Mandatory</span></span>|<span data-ttu-id="6d4e4-142">Platsen för hello lösenfras</span><span class="sxs-lookup"><span data-stu-id="6d4e4-142">Location of hello passphrase</span></span> |<span data-ttu-id="6d4e4-143">En giltig UNC- eller lokal filsökväg</span><span class="sxs-lookup"><span data-stu-id="6d4e4-143">Any valid UNC or local file path</span></span>|


>[!TIP]
> <span data-ttu-id="6d4e4-144">Hej AgentConfiguration loggar finns under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log</span><span class="sxs-lookup"><span data-stu-id="6d4e4-144">hello AgentConfiguration logs can be found under %ProgramData%\ASRSetupLogs\ASRUnifiedAgentConfigurator.log</span></span>
