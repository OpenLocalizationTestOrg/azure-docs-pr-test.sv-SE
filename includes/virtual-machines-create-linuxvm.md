
1. <span data-ttu-id="28768-101">Logga in tooyour Azure-prenumeration med hjälp av stegen i hello [ansluta tooAzure från hello Azure CLI 1.0](../articles/xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="28768-101">Sign in tooyour Azure subscription using hello steps listed in [Connect tooAzure from hello Azure CLI 1.0](../articles/xplat-cli-connect.md).</span></span>

2. <span data-ttu-id="28768-102">Kontrollera att du är i hello klassisk distribution läge på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="28768-102">Make sure you are in hello Classic deployment mode as follows:</span></span>

    ```azurecli
    azure config mode asm
    ```

3. <span data-ttu-id="28768-103">Ta reda på hello Linux bilden som du vill tooload från hello tillgängliga avbildningar på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="28768-103">Find out hello Linux image that you want tooload from hello available images as follows:</span></span>

   ```azurecli   
    azure vm image list | grep "Linux"
    ```
   
    <span data-ttu-id="28768-104">I ett kommandotolksfönster i Windows använder du **söka** i stället för grep.</span><span class="sxs-lookup"><span data-stu-id="28768-104">In a Windows command-prompt window, use **find** instead of grep.</span></span>
   
4. <span data-ttu-id="28768-105">Använd `azure vm create` toocreate en virtuell dator med hello Linux-avbildning från hello föregående lista.</span><span class="sxs-lookup"><span data-stu-id="28768-105">Use `azure vm create` toocreate a VM with hello Linux image from hello previous list.</span></span> <span data-ttu-id="28768-106">I det här steget skapas en molntjänst och ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="28768-106">This step creates a cloud service and storage account.</span></span> <span data-ttu-id="28768-107">Du kan också ansluta den här Virtuella tooan befintlig molntjänst med en `-c` alternativet.</span><span class="sxs-lookup"><span data-stu-id="28768-107">You could also connect this VM tooan existing cloud service with a `-c` option.</span></span> <span data-ttu-id="28768-108">Skapa en SSH-slutpunkten toolog i toohello virtuell Linux-dator med hello `-e` alternativet.</span><span class="sxs-lookup"><span data-stu-id="28768-108">Create an SSH endpoint toolog in toohello Linux virtual machine with hello `-e` option.</span></span> <span data-ttu-id="28768-109">hello följande exempel skapas en virtuell dator med namnet `myVM` med hello `Ubuntu-14_04_4-LTS` bilden i hello `West US` plats, och lägger till ett användarnamn `ops`:</span><span class="sxs-lookup"><span data-stu-id="28768-109">hello following example creates a VM named `myVM` using hello `Ubuntu-14_04_4-LTS` image in hello `West US` location, and adds a user name `ops`:</span></span>
   
    ```azurecli
    azure vm create myVM \
        b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04_4-LTS-amd64-server-20160516-en-us-30GB \
        -g ops -p P@ssw0rd! -z "Small" -e -l "West US"
    ```

    <span data-ttu-id="28768-110">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="28768-110">hello output is similar toohello following example:</span></span>

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
   > <span data-ttu-id="28768-111">För en virtuell Linux-dator, måste du ange hello `-e` alternativet i `vm create`.</span><span class="sxs-lookup"><span data-stu-id="28768-111">For a Linux virtual machine, you must provide hello `-e` option in `vm create`.</span></span> <span data-ttu-id="28768-112">Det är inte möjligt tooenable SSH efter hello virtuella datorn har skapats.</span><span class="sxs-lookup"><span data-stu-id="28768-112">It is not possible tooenable SSH after hello virtual machine has been created.</span></span> <span data-ttu-id="28768-113">Mer information om SSH läsa [hur tooUse SSH med Linux på Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="28768-113">For more details on SSH, read [How tooUse SSH with Linux on Azure](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

5. <span data-ttu-id="28768-114">Du kan verifiera hello attribut för hello VM med hjälp av hello `azure vm show` kommando.</span><span class="sxs-lookup"><span data-stu-id="28768-114">You can verify hello attributes of hello VM by using hello `azure vm show` command.</span></span> <span data-ttu-id="28768-115">hello följande exempel visar information för hello virtuella datorn med namnet `myVM`:</span><span class="sxs-lookup"><span data-stu-id="28768-115">hello following example lists information for hello VM named `myVM`:</span></span>

    ```azurecli   
    azure vm show myVM
    ```

6. <span data-ttu-id="28768-116">Starta den virtuella datorn med hello `azure vm start` kommandot på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="28768-116">Start your VM with hello `azure vm start` command as follows:</span></span>

    ```azurecli
    azure vm start myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="28768-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28768-117">Next steps</span></span>
<span data-ttu-id="28768-118">Mer information om alla Azure CLI 1.0 virtuella kommandona läsa hello [Using hello Azure CLI 1.0 med hello klassisk distribution API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="28768-118">For details on all these Azure CLI 1.0 virtual machine commands, read hello [Using hello Azure CLI 1.0 with hello Classic deployment API](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

