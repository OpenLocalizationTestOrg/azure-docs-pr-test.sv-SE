


## <a name="tagging-a-virtual-machine-through-templates"></a><span data-ttu-id="de5b3-101">En virtuell dator via mallar-märkning</span><span class="sxs-lookup"><span data-stu-id="de5b3-101">Tagging a Virtual Machine through Templates</span></span>
<span data-ttu-id="de5b3-102">Först ska vi titta på Taggning via mallar.</span><span class="sxs-lookup"><span data-stu-id="de5b3-102">First, let’s look at tagging through templates.</span></span> <span data-ttu-id="de5b3-103">[Den här mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) placerar taggar i följande resurser: bearbetning (virtuell dator), lagring (Storage-konto) och nätverk (offentlig IP-adress, virtuella nätverk och gränssnitt).</span><span class="sxs-lookup"><span data-stu-id="de5b3-103">[This template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) places tags on the following resources: Compute (Virtual Machine), Storage (Storage Account), and Network (Public IP Address, Virtual Network, and Network Interface).</span></span> <span data-ttu-id="de5b3-104">Den här mallen är för en virtuell Windows-dator, men kan anpassas för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="de5b3-104">This template is for a Windows VM but can be adapted for Linux VMs.</span></span>

<span data-ttu-id="de5b3-105">Klicka på den **till Azure** knappen från den [mall länk](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span><span class="sxs-lookup"><span data-stu-id="de5b3-105">Click the **Deploy to Azure** button from the [template link](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags).</span></span> <span data-ttu-id="de5b3-106">Detta kommer att gå till den [Azure-portalen](https://portal.azure.com/) där du kan distribuera den här mallen.</span><span class="sxs-lookup"><span data-stu-id="de5b3-106">This will navigate to the [Azure portal](https://portal.azure.com/) where you can deploy this template.</span></span>

![Enkel distribution med taggar](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

<span data-ttu-id="de5b3-108">Den här mallen innehåller följande taggar: *avdelning*, *programmet*, och *Skapad av*.</span><span class="sxs-lookup"><span data-stu-id="de5b3-108">This template includes the following tags: *Department*, *Application*, and *Created By*.</span></span> <span data-ttu-id="de5b3-109">Du kan lägga till eller redigera dessa taggar direkt i mallen om du vill ha olika taggnamn.</span><span class="sxs-lookup"><span data-stu-id="de5b3-109">You can add/edit these tags directly in the template if you would like different tag names.</span></span>

![Azure taggar i en mall](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

<span data-ttu-id="de5b3-111">Som du ser har taggar definierats som nyckel/värde-par, avgränsade med kolon (:).</span><span class="sxs-lookup"><span data-stu-id="de5b3-111">As you can see, the tags are defined as key/value pairs, separated by a colon (:).</span></span> <span data-ttu-id="de5b3-112">Taggar måste definieras i det här formatet:</span><span class="sxs-lookup"><span data-stu-id="de5b3-112">The tags must be defined in this format:</span></span>

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

<span data-ttu-id="de5b3-113">Spara mallfilen när du har redigerat med taggar du väljer.</span><span class="sxs-lookup"><span data-stu-id="de5b3-113">Save the template file after you finish editing it with the tags of your choice.</span></span>

<span data-ttu-id="de5b3-114">I den **redigera parametrar** avsnittet kan du fylla i värdena för taggarna.</span><span class="sxs-lookup"><span data-stu-id="de5b3-114">Next, in the **Edit Parameters** section, you can fill out the values for your tags.</span></span>

![Redigera taggar i Azure-portalen](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

<span data-ttu-id="de5b3-116">Klicka på **skapa** att distribuera den här mallen med taggvärden.</span><span class="sxs-lookup"><span data-stu-id="de5b3-116">Click **Create** to deploy this template with your tag values.</span></span>

## <a name="tagging-through-the-portal"></a><span data-ttu-id="de5b3-117">Taggning via portalen</span><span class="sxs-lookup"><span data-stu-id="de5b3-117">Tagging through the Portal</span></span>
<span data-ttu-id="de5b3-118">Efter att dina resurser med taggar kan du visa, lägga till och ta bort taggar i portalen.</span><span class="sxs-lookup"><span data-stu-id="de5b3-118">After creating your resources with tags, you can view, add, and delete tags in the portal.</span></span>

<span data-ttu-id="de5b3-119">Välj ikonen taggar för att visa taggarna:</span><span class="sxs-lookup"><span data-stu-id="de5b3-119">Select the tags icon to view your tags:</span></span>

![Ikon för etiketter i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

<span data-ttu-id="de5b3-121">Lägg till en ny tagg via portalen genom att definiera en egen nyckel/värde-par och spara den.</span><span class="sxs-lookup"><span data-stu-id="de5b3-121">Add a new tag through the portal by defining your own Key/Value pair, and save it.</span></span>

![Lägg till ny tagg i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

<span data-ttu-id="de5b3-123">Din nya taggen bör nu visas i listan med taggar för din resurs.</span><span class="sxs-lookup"><span data-stu-id="de5b3-123">Your new tag should now appear in the list of tags for your resource.</span></span>

![Ny tagg som sparats i Azure-portalen](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

