## <a name="how-toocreate-a-classic-vnet-using-azure-cli"></a>Hur toocreate ett klassiska VNet med Azure CLI
Du kan använda hello Azure CLI toomanage Azure-resurser från hello Kommandotolken från valfri dator som kör Windows, Linux eller OSX. toocreate ett VNet med hjälp av hello Azure CLI, följ hello stegen nedan.

1. Om du aldrig har använt Azure CLI, se [installera och konfigurera hello Azure CLI](../articles/cli-install-nodejs.md) och följer instruktionerna för hello in toohello punkt där du väljer Azure-konto och prenumeration.
2. Kör hello **azure network vnet skapa** kommandot toocreate ett VNet och undernät, enligt nedan. hello-listan som visas efter hello utdata förklarar hello parametrar som används.
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Förväntad utdata:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Namnet på hello VNet toobe skapas. I vårt exempel, *TestVNet*
   * **-e (eller---adressutrymmet)**. VNet-adressutrymmet. I vårt scenario, *192.168.0.0*
   * **-i (eller - cidr)**. Nätverksmasken i CIDR-format. I vårt scenario, *16*.
   * **-n (eller--undernätsnamn**). Namnet på hello första undernätet. I vårt scenario, *FrontEnd*.
   * **-p (eller--undernät-start-ip)**. Första IP-adressen för undernätet eller undernätsadressutrymmet. I vårt scenario, *192.168.1.0*.
   * **-r (eller--undernät cidr)**. Nätverksmasken i CIDR-format för undernätet. I vårt scenario, *24*.
   * **-l (eller --location)**. Azure-region där hello VNet kommer att skapas. I vårt scenario, *centrala USA*.
3. Kör hello **azure network vnet subnet skapa** kommandot toocreate ett undernät som visas nedan. hello-listan som visas efter hello utdata förklarar hello parametrar som används.
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    Här är förväntat hello utdata för hello ovanstående kommando:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (eller--vnet-name**. Namnet på hello VNet där undernätet hello kommer att skapas. I vårt scenario, *TestVNet*.
   * **-n (eller --name)**. Namnet på hello Nytt undernät. I vårt scenario, *BackEnd*.
   * **-a (eller --address-prefix)**. CIDR-block för undernätet. Fyra vårt scenario, *192.168.2.0/24*.
4. Kör hello **azure network vnet show** kommandot tooview hello egenskaper för hello nya vnet, enligt nedan.
   
            azure network vnet show
   
    Här är förväntat hello utdata för hello ovanstående kommando:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

