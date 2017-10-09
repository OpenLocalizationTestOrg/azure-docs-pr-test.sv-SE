## <a name="how-toocreate-a-virtual-network-using-a-network-config-file-from-powershell"></a>Hur toocreate ett virtuellt nätverk med ett nätverks-config-fil från PowerShell
Azure använder en XML-filen toodefine alla virtuella nätverk tillgängliga tooa prenumeration. Du kan hämta den här filen, redigera den toomodify eller ta bort befintliga virtuella nätverk och skapa nya virtuella nätverk. I kursen får du lära dig hur toodownload den här filen som anges tooas nätverks-konfiguration (eller netcfg) och redigera den toocreate ett nytt virtuellt nätverk. toolearn mer om hello nätverket konfigurationsfilen finns hello [virtuella Azure-nätverket Konfigurationsschemat](https://msdn.microsoft.com/library/azure/jj157100.aspx).

toocreate ett virtuellt nätverk med en netcfg-fil med hjälp av PowerShell fullständig hello följande steg:

1. Om du aldrig har använt Azure PowerShell fullständig hello stegen i hello [hur tooInstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) artikel, och sedan logga in tooAzure och välja din prenumeration.
2. Använd hello från hello Azure PowerShell-konsol, **Get-AzureVnetConfig** cmdlet toodownload hello configuration tooa katalog i nätverket på din dator genom att köra följande kommando hello: 
   
   ```powershell
   Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
   ```
   
   Förväntad utdata:
  
      ```
      XMLConfiguration                                                                                                     
      ----------------                                                                                                     
      <?xml version="1.0" encoding="utf-8"?>...
      ```

3. Öppna hello-filen som du sparade i steg 2 med XML- eller textredigerare och leta efter hello  **<VirtualNetworkSites>**  element. Om du har några nätverk som redan har skapats visas varje nätverk som sin egen  **<VirtualNetworkSite>**  element.
4. toocreate hello virtuellt nätverk i det här scenariot, Lägg till följande XML precis under hello hello  **<VirtualNetworkSites>**  element:

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
   
5. Spara konfigurationsfilen för hello nätverk.
6. Använd hello från hello Azure PowerShell-konsol, **Set AzureVnetConfig** konfigurationsfilen för cmdlet tooupload hello nätverk genom att köra följande kommando hello: 
   
   ```powershell
   Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
   ```
   
   Returnerade utdata:
   
      ```
      OperationDescription OperationId                          OperationStatus
      -------------------- -----------                          ---------------
      Set-AzureVNetConfig  <Id>                                 Succeeded 
      ```
   
   Om **OperationStatus** är inte *lyckades* i hello returnerade utdata, kontrollera hello XML-fil för fel och fullständig steg 6 igen.

7. Använd hello från hello Azure PowerShell-konsol, **Get-AzureVnetSite** cmdlet tooverify som hello nytt nätverk har lagts till genom att köra följande kommando hello: 

   ```powershell
   Get-AzureVNetSite -VNetName TestVNet
   ```
   
   hello returnerade (förkortas) utdata innehåller hello följande text:
  
      ```
      AddressSpacePrefixes : {192.168.0.0/16}
      Location             : Central US
      Name                 : TestVNet
      State                : Created
      Subnets              : {FrontEnd, BackEnd}
      OperationDescription : Get-AzureVNetSite
      OperationStatus      : Succeeded
      ```
