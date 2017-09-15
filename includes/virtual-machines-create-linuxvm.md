
1. <span data-ttu-id="c346c-101">Logga in på din Azure-prenumeration med hjälp av anvisningarna i [Connect to Azure from the Azure CLI 1.0](../articles/xplat-cli-connect.md) (Ansluta till Azure från Azure CLI 1.0).</span><span class="sxs-lookup"><span data-stu-id="c346c-101">Sign in to your Azure subscription using the steps listed in [Connect to Azure from the Azure CLI 1.0](../articles/xplat-cli-connect.md).</span></span>

2. <span data-ttu-id="c346c-102">Se till att du använder läget för klassisk distribution:</span><span class="sxs-lookup"><span data-stu-id="c346c-102">Make sure you are in the Classic deployment mode as follows:</span></span>

    ```azurecli
    azure config mode asm
    ```

3. <span data-ttu-id="c346c-103">Ta reda på den Linux-avbildning som du vill läsa in från de tillgängliga avbildningarna på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="c346c-103">Find out the Linux image that you want to load from the available images as follows:</span></span>

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    <span data-ttu-id="c346c-104">I ett kommandotolksfönster i Windows använder du **söka** i stället för grep.</span><span class="sxs-lookup"><span data-stu-id="c346c-104">In a Windows command-prompt window, use **find** instead of grep.</span></span>
   
4. <span data-ttu-id="c346c-105">Använd `azure vm create` för att skapa en virtuell dator med Linux-avbildningen från föregående lista.</span><span class="sxs-lookup"><span data-stu-id="c346c-105">Use `azure vm create` to create a VM with the Linux image from the previous list.</span></span> <span data-ttu-id="c346c-106">I det här steget skapas en molntjänst och ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="c346c-106">This step creates a cloud service and storage account.</span></span> <span data-ttu-id="c346c-107">Du kan också ansluta den virtuella datorn till en befintlig molntjänst med alternativet `-c`.</span><span class="sxs-lookup"><span data-stu-id="c346c-107">You could also connect this VM to an existing cloud service with a `-c` option.</span></span> <span data-ttu-id="c346c-108">Skapa en SSH-slutpunkt för att logga in på den virtuella Linux-datorn med alternativet `-e`.</span><span class="sxs-lookup"><span data-stu-id="c346c-108">Create an SSH endpoint to log in to the Linux virtual machine with the `-e` option.</span></span> <span data-ttu-id="c346c-109">I följande exempel skapas en virtuell dator med namnet `myVM` med hjälp av avbildningen `Ubuntu-14_04_4-LTS` på platsen `West US`, och lägger till ett användarnamn `ops`:</span><span class="sxs-lookup"><span data-stu-id="c346c-109">The following example creates a VM named `myVM` using the `Ubuntu-14_04_4-LTS` image in the `West US` location, and adds a user name `ops`:</span></span>
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    <span data-ttu-id="c346c-110">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="c346c-110">The output is similar to the following example:</span></span>

    ```azurecli
    info:    Executing command vm create
    + Looking up image b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB
    + Looking up cloud service
    info:    cloud service myVM not found.
    + Creating cloud service
    + Retrieving storage accounts
    + Creating VM
    info:    vm create command OK
    ```
   
   > [!NOTE]
   > <span data-ttu-id="c346c-111">För en virtuell Linux-dator måste du ange alternativet `-e` i `vm create`.</span><span class="sxs-lookup"><span data-stu-id="c346c-111">For a Linux virtual machine, you must provide the `-e` option in `vm create`.</span></span> <span data-ttu-id="c346c-112">Det går inte att aktivera SSH när den virtuella datorn har skapats.</span><span class="sxs-lookup"><span data-stu-id="c346c-112">It is not possible to enable SSH after the virtual machine has been created.</span></span> <span data-ttu-id="c346c-113">Mer information om SSH finns i [Så här använder du SSH med Linux på Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c346c-113">For more details on SSH, read [How to Use SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

5. <span data-ttu-id="c346c-114">Du kan verifiera attribut för den virtuella datorn med hjälp av kommandot `azure vm show`.</span><span class="sxs-lookup"><span data-stu-id="c346c-114">You can verify the attributes of the VM by using the `azure vm show` command.</span></span> <span data-ttu-id="c346c-115">I följande exempel visas information för den virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="c346c-115">The following example lists information for the VM named `myVM`:</span></span>

    ```azurecli   
    azure vm show myVM
    ```

6. <span data-ttu-id="c346c-116">Starta den virtuella datorn med kommandot `azure vm start` enligt följande:</span><span class="sxs-lookup"><span data-stu-id="c346c-116">Start your VM with the `azure vm start` command as follows:</span></span>

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="c346c-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c346c-117">Next steps</span></span>
<span data-ttu-id="c346c-118">Mer information om alla dessa Azure CLI 1.0-kommandon för virtuella datorer finns i [Using the Azure CLI 1.0 with the Classic deployment API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) (Använda Azure CLI 1.0 med klassisk API-distribution).</span><span class="sxs-lookup"><span data-stu-id="c346c-118">For details on all these Azure CLI 1.0 virtual machine commands, read the [Using the Azure CLI 1.0 with the Classic deployment API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

