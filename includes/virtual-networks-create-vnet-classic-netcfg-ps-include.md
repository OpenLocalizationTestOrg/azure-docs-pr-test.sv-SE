## <a name="how-toocreate-a-virtual-network-using-a-network-config-file-from-powershell"></a><span data-ttu-id="3e1b7-101">Hur toocreate ett virtuellt nätverk med ett nätverks-config-fil från PowerShell</span><span class="sxs-lookup"><span data-stu-id="3e1b7-101">How toocreate a virtual network using a network config file from PowerShell</span></span>
<span data-ttu-id="3e1b7-102">Azure använder en XML-filen toodefine alla virtuella nätverk tillgängliga tooa prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-102">Azure uses an xml file toodefine all virtual networks available tooa subscription.</span></span> <span data-ttu-id="3e1b7-103">Du kan hämta den här filen, redigera den toomodify eller ta bort befintliga virtuella nätverk och skapa nya virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-103">You can download this file, edit it toomodify or delete existing virtual networks, and create new virtual networks.</span></span> <span data-ttu-id="3e1b7-104">I kursen får du lära dig hur toodownload den här filen som anges tooas nätverks-konfiguration (eller netcfg) och redigera den toocreate ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-104">In this tutorial, you learn how toodownload this file, referred tooas network configuration (or netcfg) file, and edit it toocreate a new virtual network.</span></span> <span data-ttu-id="3e1b7-105">toolearn mer om hello nätverket konfigurationsfilen finns hello [virtuella Azure-nätverket Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span><span class="sxs-lookup"><span data-stu-id="3e1b7-105">toolearn more about hello network configuration file, see hello [Azure virtual network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx).</span></span>

<span data-ttu-id="3e1b7-106">toocreate ett virtuellt nätverk med en netcfg-fil med hjälp av PowerShell fullständig hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-106">toocreate a virtual network with a netcfg file using PowerShell, complete hello following steps:</span></span>

1. <span data-ttu-id="3e1b7-107">Om du aldrig har använt Azure PowerShell fullständig hello stegen i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) artikel, och sedan logga in tooAzure och välja din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-107">If you have never used Azure PowerShell, complete hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azureps-cmdlets-docs) article, then sign in tooAzure and select your subscription.</span></span>
2. <span data-ttu-id="3e1b7-108">Använd hello från hello Azure PowerShell-konsol, **Get-AzureVnetConfig** cmdlet toodownload hello configuration tooa katalog i nätverket på din dator genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-108">From hello Azure PowerShell console, use hello **Get-AzureVnetConfig** cmdlet toodownload hello network configuration file tooa directory on your computer by running hello following command:</span></span> 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="3e1b7-109">Förväntad utdata:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-109">Expected output:</span></span>
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. <span data-ttu-id="3e1b7-110">Öppna hello-filen som du sparade i steg 2 med XML- eller textredigerare och leta efter hello  **<VirtualNetworkSites>**  element.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-110">Open hello file you saved in step 2 using any XML or text editor application, and look for hello **<VirtualNetworkSites>** element.</span></span> <span data-ttu-id="3e1b7-111">Om du har några nätverk som redan har skapats visas varje nätverk som sin egen  **<VirtualNetworkSite>**  element.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-111">If you have any networks already created, each network is displayed as its own **<VirtualNetworkSite>** element.</span></span>
4. <span data-ttu-id="3e1b7-112">toocreate hello virtuellt nätverk i det här scenariot, Lägg till följande XML precis under hello hello  **<VirtualNetworkSites>**  element:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-112">toocreate hello virtual network described in this scenario, add hello following XML just under hello **<VirtualNetworkSites>** element:</span></span>

   ```xml
        <VirtualNetworkSite name="TestVNet" Location="East US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>
   ```
   
5. <span data-ttu-id="3e1b7-113">Spara konfigurationsfilen för hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-113">Save hello network configuration file.</span></span>
6. <span data-ttu-id="3e1b7-114">Använd hello från hello Azure PowerShell-konsol, **Set AzureVnetConfig** konfigurationsfilen för cmdlet tooupload hello nätverk genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-114">From hello Azure PowerShell console, use hello **Set-AzureVnetConfig** cmdlet tooupload hello network configuration file by running hello following command:</span></span> 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   <span data-ttu-id="3e1b7-115">Returnerade utdata:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-115">Returned output:</span></span>
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   <span data-ttu-id="3e1b7-116">Om **OperationStatus** är inte *lyckades* i hello returnerade utdata, kontrollera hello XML-fil för fel och fullständig steg 6 igen.</span><span class="sxs-lookup"><span data-stu-id="3e1b7-116">If **OperationStatus** is not *Succeeded* in hello returned output, check hello xml file for errors and complete step 6 again.</span></span>

7. <span data-ttu-id="3e1b7-117">Använd hello från hello Azure PowerShell-konsol, **Get-AzureVnetSite** cmdlet tooverify som hello nytt nätverk har lagts till genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-117">From hello Azure PowerShell console, use hello **Get-AzureVnetSite** cmdlet tooverify that hello new network was added by running hello following command:</span></span> 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   <span data-ttu-id="3e1b7-118">hello returnerade (förkortas) utdata innehåller hello följande text:</span><span class="sxs-lookup"><span data-stu-id="3e1b7-118">hello returned (abbreviated) output includes hello following text:</span></span>
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
