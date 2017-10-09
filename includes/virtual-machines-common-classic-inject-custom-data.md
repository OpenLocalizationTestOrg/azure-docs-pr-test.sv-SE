


<span data-ttu-id="20022-101">Det här avsnittet beskrivs hur du:</span><span class="sxs-lookup"><span data-stu-id="20022-101">This topic describes how to:</span></span>

* <span data-ttu-id="20022-102">Mata in data i en Azure virtuell dator (VM) när den har etablerats.</span><span class="sxs-lookup"><span data-stu-id="20022-102">Inject data into an Azure virtual machine (VM) when it is being provisioned.</span></span>
* <span data-ttu-id="20022-103">Hämta den för både Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="20022-103">Retrieve it for both Windows and Linux.</span></span>
* <span data-ttu-id="20022-104">Använd särskilda verktyg som finns på vissa system toodetect och hantera anpassade data automatiskt.</span><span class="sxs-lookup"><span data-stu-id="20022-104">Use special tools available on some systems toodetect and handle custom data automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="20022-105">Den här artikeln beskriver hur anpassade data kan matas in med hjälp av en virtuell dator som skapats med hello Azure Service Management API.</span><span class="sxs-lookup"><span data-stu-id="20022-105">This article describes how custom data can be injected by using a VM created with hello Azure Service Management API.</span></span> <span data-ttu-id="20022-106">hur toouse hello Azure Resource Management API Se toosee [hello exempelmall](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span><span class="sxs-lookup"><span data-stu-id="20022-106">toosee how toouse hello Azure Resource Management API, see [hello example template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).</span></span>
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a><span data-ttu-id="20022-107">Placera anpassade data i ditt virtuella Azure-datorn</span><span class="sxs-lookup"><span data-stu-id="20022-107">Injecting custom data into your Azure virtual machine</span></span>
<span data-ttu-id="20022-108">Den här funktionen stöds bara i hello [Azure-kommandoradsgränssnittet](https://github.com/Azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="20022-108">This feature is currently supported only in hello [Azure Command-Line Interface](https://github.com/Azure/azure-xplat-cli).</span></span> <span data-ttu-id="20022-109">Här kan vi skapa en `custom-data.txt` -fil som innehåller våra data sedan mata in som i toohello VM under etableringen.</span><span class="sxs-lookup"><span data-stu-id="20022-109">Here we create a `custom-data.txt` file that contains our data, then inject that in toohello VM during provisioning.</span></span> <span data-ttu-id="20022-110">Även om du kan använda någon av hello alternativ för hello `azure vm create` kommandot hello följande visar ett grundläggande tillvägagångssätt:</span><span class="sxs-lookup"><span data-stu-id="20022-110">Although you may use any of hello options for hello `azure vm create` command, hello following demonstrates one very basic approach:</span></span>

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-hello-virtual-machine"></a><span data-ttu-id="20022-111">Med hjälp av anpassade data i hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="20022-111">Using custom data in hello virtual machine</span></span>
* <span data-ttu-id="20022-112">Om din Azure VM är en Windows-baserad virtuell dator och sedan hello anpassade data sparas för`%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span><span class="sxs-lookup"><span data-stu-id="20022-112">If your Azure VM is a Windows-based VM, then hello custom data file is saved too`%SYSTEMDRIVE%\AzureData\CustomData.bin`.</span></span> <span data-ttu-id="20022-113">Även om det var base64-kodad tootransfer från hello lokal dator toohello ny virtuell dator, är det automatiskt avkodas och som kan öppnas eller används omedelbart.</span><span class="sxs-lookup"><span data-stu-id="20022-113">Although it was base64-encoded tootransfer from hello local computer toohello new VM, it is automatically decoded and can be opened or used immediately.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="20022-114">Om hello finns skrivs över.</span><span class="sxs-lookup"><span data-stu-id="20022-114">If hello file exists, it is overwritten.</span></span> <span data-ttu-id="20022-115">hello på hello directory säkerhetsnivån för**System: fullständig kontroll** och **administratörer: fullständig kontroll**.</span><span class="sxs-lookup"><span data-stu-id="20022-115">hello security on hello directory is set too**System:Full Control** and **Administrators:Full Control**.</span></span>
  > 
  > 
* <span data-ttu-id="20022-116">Om din Azure VM är en Linux-baserade Virtuella hello anpassade datafilen ska finnas i placerar något av följande hello beroende på din distro.</span><span class="sxs-lookup"><span data-stu-id="20022-116">If your Azure VM is a Linux-based VM, then hello custom data file will be located in one of hello following places depending on your distro.</span></span> <span data-ttu-id="20022-117">hello data kan vara base64-kodade, så du kanske måste toodecode hello data först:</span><span class="sxs-lookup"><span data-stu-id="20022-117">hello data may be base64-encoded, so you may need toodecode hello data first:</span></span>
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a><span data-ttu-id="20022-118">Molnet initiering på Azure</span><span class="sxs-lookup"><span data-stu-id="20022-118">Cloud-init on Azure</span></span>
<span data-ttu-id="20022-119">Om din Azure VM är från en Ubuntu eller virtuell CoreOS-avbildning, kan du använda CustomData toosend en moln-config toocloud initiering.</span><span class="sxs-lookup"><span data-stu-id="20022-119">If your Azure VM is from an Ubuntu or CoreOS image, then you can use CustomData toosend a cloud-config toocloud-init.</span></span> <span data-ttu-id="20022-120">Eller om anpassade datafilen är ett skript, sedan molnet init kan bara köra den.</span><span class="sxs-lookup"><span data-stu-id="20022-120">Or if your custom data file is a script, then cloud-init can simply execute it.</span></span>

### <a name="ubuntu-cloud-images"></a><span data-ttu-id="20022-121">Ubuntu molnet bilder</span><span class="sxs-lookup"><span data-stu-id="20022-121">Ubuntu Cloud Images</span></span>
<span data-ttu-id="20022-122">I de flesta Azure Linux-avbildningar, skulle du redigera ”/ etc/waagent.conf” tooconfigure hello tillfälliga disk och växla resursfil.</span><span class="sxs-lookup"><span data-stu-id="20022-122">In most Azure Linux images, you would edit "/etc/waagent.conf" tooconfigure hello temporary resource disk and swap file.</span></span> <span data-ttu-id="20022-123">Se [Azure Linux-agenten användarhandboken](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för mer information.</span><span class="sxs-lookup"><span data-stu-id="20022-123">See [Azure Linux Agent user guide](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for more information.</span></span>

<span data-ttu-id="20022-124">Dock på hello Ubuntu molnet bilder, måste du använda molnet init tooconfigure hello resurs disk (det vill säga hello ”tillfälliga” disk) och växla partition.</span><span class="sxs-lookup"><span data-stu-id="20022-124">However, on hello Ubuntu Cloud Images, you must use cloud-init tooconfigure hello resource disk (that is, hello "ephemeral" disk) and swap partition.</span></span> <span data-ttu-id="20022-125">Se hello följande sida på hello Ubuntu wiki för mer information: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span><span class="sxs-lookup"><span data-stu-id="20022-125">See hello following page on hello Ubuntu wiki for more details: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).</span></span>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps-using-cloud-init"></a><span data-ttu-id="20022-126">Nästa steg: med molnet initiering</span><span class="sxs-lookup"><span data-stu-id="20022-126">Next steps: Using cloud-init</span></span>
<span data-ttu-id="20022-127">Mer information finns i hello [moln init-dokumentationen för Ubuntu](https://help.ubuntu.com/community/CloudInit).</span><span class="sxs-lookup"><span data-stu-id="20022-127">For further information, see hello [cloud-init documentation for Ubuntu](https://help.ubuntu.com/community/CloudInit).</span></span>

<!--Link references-->
[<span data-ttu-id="20022-128">Lägg till rollen Service Management REST API-referens</span><span class="sxs-lookup"><span data-stu-id="20022-128">Add Role Service Management REST API Reference</span></span>](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[<span data-ttu-id="20022-129">Azure-kommandoradsgränssnittet</span><span class="sxs-lookup"><span data-stu-id="20022-129">Azure Command-line Interface</span></span>](https://github.com/Azure/azure-xplat-cli)

