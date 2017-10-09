


## <a name="tagging-a-virtual-machine-through-templates"></a><span data-ttu-id="75974-101">En virtuell dator via mallar-märkning</span><span class="sxs-lookup"><span data-stu-id="75974-101">Tagging a Virtual Machine through Templates</span></span>
<span data-ttu-id="75974-102">Först ska vi titta på Taggning via mallar.</span><span class="sxs-lookup"><span data-stu-id="75974-102">First, let’s look at tagging through templates.</span></span> <span data-ttu-id="75974-103">[Den här mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) placerar taggar på hello följande resurser: bearbetning (virtuell dator), lagring (Storage-konto) och nätverk (offentlig IP-adress, virtuella nätverk och gränssnitt).</span><span class="sxs-lookup"><span data-stu-id="75974-103">[This template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) places tags on hello following resources: Compute (Virtual Machine), Storage (Storage Account), and Network (Public IP Address, Virtual Network, and Network Interface).</span></span> <span data-ttu-id="75974-104">Den här mallen är för en virtuell Windows-dator, men kan anpassas för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="75974-104">This template is for a Windows VM but can be adapted for Linux VMs.</span></span>

<span data-ttu-id="75974-105">Klicka på hello **distribuera tooAzure** knappen från hello [mall länk](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span><span class="sxs-lookup"><span data-stu-id="75974-105">Click hello **Deploy tooAzure** button from hello [template link](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span></span> <span data-ttu-id="75974-106">Detta ska gå toohello [Azure-portalen](https://portal.azure.com/) där du kan distribuera den här mallen.</span><span class="sxs-lookup"><span data-stu-id="75974-106">This will navigate toohello [Azure portal](https://portal.azure.com/) where you can deploy this template.</span></span>

![Enkel distribution med taggar](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

<span data-ttu-id="75974-108">Den här mallen innehåller hello följande taggar: *avdelning*, *programmet*, och *Skapad av*.</span><span class="sxs-lookup"><span data-stu-id="75974-108">This template includes hello following tags: *Department*, *Application*, and *Created By*.</span></span> <span data-ttu-id="75974-109">Du kan lägga till eller redigera dessa taggar direkt i hello mallen om du vill ha olika taggnamn.</span><span class="sxs-lookup"><span data-stu-id="75974-109">You can add/edit these tags directly in hello template if you would like different tag names.</span></span>

![Azure taggar i en mall](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

<span data-ttu-id="75974-111">Som du ser har hello taggar definierats som nyckel/värde-par, avgränsade med kolon (:).</span><span class="sxs-lookup"><span data-stu-id="75974-111">As you can see, hello tags are defined as key/value pairs, separated by a colon (:).</span></span> <span data-ttu-id="75974-112">hello taggar måste definieras i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="75974-112">hello tags must be defined in this format:</span></span>

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

<span data-ttu-id="75974-113">Spara filen med hello mallen när du har redigerat med hello taggar du väljer.</span><span class="sxs-lookup"><span data-stu-id="75974-113">Save hello template file after you finish editing it with hello tags of your choice.</span></span>

<span data-ttu-id="75974-114">Nästa i hello **redigera parametrar** avsnittet kan du fylla i hello värden för taggarna.</span><span class="sxs-lookup"><span data-stu-id="75974-114">Next, in hello **Edit Parameters** section, you can fill out hello values for your tags.</span></span>

![Redigera taggar i Azure-portalen](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

<span data-ttu-id="75974-116">Klicka på **skapa** toodeploy den här mallen med taggvärden.</span><span class="sxs-lookup"><span data-stu-id="75974-116">Click **Create** toodeploy this template with your tag values.</span></span>

## <a name="tagging-through-hello-portal"></a><span data-ttu-id="75974-117">Taggning via hello Portal</span><span class="sxs-lookup"><span data-stu-id="75974-117">Tagging through hello Portal</span></span>
<span data-ttu-id="75974-118">Efter att dina resurser med taggar kan du visa, lägga till och ta bort taggar i hello portal.</span><span class="sxs-lookup"><span data-stu-id="75974-118">After creating your resources with tags, you can view, add, and delete tags in hello portal.</span></span>

<span data-ttu-id="75974-119">Välj hello taggar ikonen tooview taggarna:</span><span class="sxs-lookup"><span data-stu-id="75974-119">Select hello tags icon tooview your tags:</span></span>

![Ikon för etiketter i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

<span data-ttu-id="75974-121">Lägg till en ny tagg hello-portalen genom att definiera en egen nyckel/värde-par och spara den.</span><span class="sxs-lookup"><span data-stu-id="75974-121">Add a new tag through hello portal by defining your own Key/Value pair, and save it.</span></span>

![Lägg till ny tagg i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

<span data-ttu-id="75974-123">Din nya taggen bör nu visas i hello lista med taggar för din resurs.</span><span class="sxs-lookup"><span data-stu-id="75974-123">Your new tag should now appear in hello list of tags for your resource.</span></span>

![Ny tagg som sparats i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

