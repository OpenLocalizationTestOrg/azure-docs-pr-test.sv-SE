I det här steget kan skapa du manuellt hello tillgänglighetsgruppens lyssnare i hanteraren för redundanskluster och SQL Server Management Studio.

1. Öppna Hanteraren för redundanskluster från hello-noden som är värd för hello primära repliken.

2. Välj hello **nätverk** noden och Observera hello klustrets nätverksnamn. Det här namnet används i hello $ClusterNetworkName variabel i hello PowerShell-skript.

3. Expandera klusternamnet hello och klicka sedan på **roller**.

4. I hello **roller** fönstret, högerklicka på hello-tillgänglighetsgrupp och sedan markera **Lägg till resurs** > **klientåtkomstpunkt**.
   
    ![Lägg till klientåtkomstpunkten för tillgänglighetsgruppen](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)

5. I hello **namn** , skapar du ett namn för den här lyssnaren, klickar du på **nästa** två gånger, och klicka sedan på **Slutför**.  
    Tar inte med hello lyssnare eller resursen i det här läget.

6. Klicka på hello **resurser** fliken och expandera sedan hello klientåtkomstpunkt som du nyss skapade. 
    hello IP-adressresurs för varje klusternätverk i klustret visas. Om detta är en Azure-only-lösning, visas endast en IP-adressresurs.

7. Gör något av följande hello:
   
   * tooconfigure hybridlösning:
     
        a. Högerklicka på hello IP-adressresurs som motsvarar tooyour lokala undernät och välj sedan **egenskaper**. Observera hello IP-adress och nätverksnamn.
   
        b. Välj **statisk IP-adress**, tilldela en IP-adress som inte används och klicka sedan på **OK**.
 
   * tooconfigure en endast Azure-lösning:

        a. Högerklicka på hello IP-adressresurs som motsvarar tooyour Azure-undernät och välj sedan **egenskaper**.
       
       > [!NOTE]
       > Du kan konfigurera en statisk IP-adress för i egenskapsfönstret om lyssnaren hello senare misslyckas toocome online på grund av en konflikt IP-adress som valts av DHCP.
       > 
       > 

       b. Hej i samma **IP-adress** egenskapsfönstret, ändra hello **IP-adressnamn**.  
        Det här namnet används i hello $IPResourceName variabel av hello PowerShell-skript. Om din lösning sträcker sig över flera virtuella Azure-nätverk, kan du upprepa det här steget för varje IP-resurs.

