hello tillgänglighetsgruppens lyssnare är en IP-adress och namn som hello SQL Server-tillgänglighetsgrupp som lyssnar på. toocreate hello tillgänglighetsgruppens lyssnare, hello följande:

1. <a name="getnet"></a>Hämta hello namn för hello klusterresurs för nätverket.

    a. Använd RDP tooconnect toohello virtuell Azure-dator som är värd för hello primära repliken. 

    b. Öppna Hanteraren för redundanskluster.

    c. Välj hello **nätverk** nod och Observera hello klustrets nätverksnamn. Använd det här namnet i hello `$ClusterNetworkName` variabeln i hello PowerShell-skript. Hello följande bild hello Klusternätverksnamnet är **klustret nätverk 1**:

   ![Klustrets nätverksnamn](./media/virtual-machines-ag-listener-configure/90-clusternetworkname.png)

2. <a name="addcap"></a>Lägg till hello klientåtkomstpunkt.  
    hello klientåtkomstpunkt är hello nätverksnamn att program använder tooconnect toohello databaser i en tillgänglighetsgrupp. Skapa hello klientåtkomstpunkt i hanteraren för redundanskluster.

    a. Expandera klusternamnet hello och klicka sedan på **roller**.

    b. I hello **roller** fönstret, högerklicka på hello-tillgänglighetsgrupp och sedan markera **Lägg till resurs** > **klientåtkomstpunkt**.

   ![Klientåtkomstpunkt](./media/virtual-machines-ag-listener-configure/92-addclientaccesspoint.png)

    c. I hello **namn** skapar du ett namn för den här lyssnaren. 
   hello namnet hello lyssnaren är hello nätverksnamn att program använder tooconnect toodatabases i hello SQL Server-tillgänglighetsgrupp.
   
    d. toofinish skapar hello lyssnare, klickar du på **nästa** två gånger, och klicka sedan på **Slutför**. Tar inte med hello lyssnare eller resursen i det här läget.

3. <a name="congroup"></a>Konfigurera hello IP-resurs för hello-tillgänglighetsgruppen.

    a. Klicka på hello **resurser** fliken och expandera sedan hello klientåtkomstpunkt som du skapade.  
    hello klientåtkomstpunkt är offline.

   ![Klientåtkomstpunkt](./media/virtual-machines-ag-listener-configure/94-newclientaccesspoint.png) 

    b. Högerklicka på hello IP-resurs och klicka sedan på Egenskaper. Anteckna namnet på hello hello IP-adress och använda det i hello `$IPResourceName` variabeln i hello PowerShell-skript.

    c. Under **IP-adress**, klickar du på **statisk IP-adress**. Ange hello IP-adress som hello samma adress som du använde när du har ställt in hello-adressen för belastningsutjämnaren på hello Azure-portalen.

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/96-ipresource.png) 

    <!-----------------------I don't see this option on server 2016
    1. Disable NetBIOS for this address and click **OK**. Repeat this step for each IP resource if your solution spans multiple Azure VNets. 
    ------------------------->

4. <a name = "dependencyGroup"></a>Gör hello SQL Server tillgänglighetsgruppsresursen beroende hello klientåtkomstpunkt.

    a. I hanteraren för redundanskluster, klickar du på **roller**, och klicka sedan på tillgänglighetsgruppen.

    b. På hello **resurser** fliken, under **andra resurser**högerklickar du på hello tillgänglighet resursgruppen och klicka sedan på **egenskaper**. 

    c. Lägg till hello namnet på hello klienten åtkomst punkt (hello lyssnare) resursen hello beroenden på fliken.

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/97-propertiesdependencies.png) 

    d. Klicka på **OK**.

5. <a name="listname"></a>Se hello klientåtkomst peka resurs som är beroende av hello IP-adress.

    a. I hanteraren för redundanskluster, klickar du på **roller**, och klicka sedan på tillgänglighetsgruppen. 

    b. På hello **resurser** , högerklickar på hello klienten åtkomst punkt resurs under **servernamn**, och klicka sedan på **egenskaper**. 

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/98-dependencies.png) 

    c. Klicka på hello **beroenden** fliken. Kontrollera att hello IP-adress är ett beroende. Om det inte ange ett beroende på hello IP-adress. Om det finns flera resurser i listan, kontrollera att hello IP-adresser har eller inte och beroenden. Klicka på **OK**. 

   ![IP-resurs](./media/virtual-machines-ag-listener-configure/98-propertiesdependencies.png) 

    d. Högerklicka på hello grupplyssnarens namn och klicka sedan på **Anslut**. 

    >[!TIP]
    >Du kan verifiera att hello beroenden är korrekt konfigurerade. I hanteraren för redundanskluster, gå tooRoles, högerklicka på hello tillgänglighetsgruppen, klickar du på **fler åtgärder**, och klicka sedan på **visa beroenderapport**. När hello beroenden är korrekt konfigurerade hello tillgänglighetsgruppen är beroende av hello nätverksnamnet och hello nätverksnamn är beroende av hello IP-adress. 


6. <a name="setparam"></a>Ange hello Klusterparametrar i PowerShell.
    
    a. Kopiera hello följande PowerShell-skriptet tooone av SQL Server-instanser. Uppdatera hello variabler för din miljö.     
    
    ```PowerShell
    $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
    $IPResourceName = "<IPResourceName>" # hello IP Address resource name
    $ILBIP = “<n.n.n.n>” # hello IP Address of hello Internal Load Balancer (ILB). This is hello static IP address for hello load balancer you configured in hello Azure portal.
    [int]$ProbePort = <nnnnn>
    
    Import-Module FailoverClusters
    
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
    ```

    b. Ange hello Klusterparametrar genom att köra hello PowerShell-skript på en av klusternoderna hello.  

    > [!NOTE]
    > Om SQL Server-instanser i olika regioner, måste toorun hello PowerShell-skriptet två gånger. Hej första gången, använda hello `$ILBIP` och `$ProbePort` från hello första region. Hej gång, Använd hello `$ILBIP` och `$ProbePort` från andra hello-region. hello Klusternätverksnamnet och hello klusternamnet IP-resurs är hello samma. 
