### <a name="create-a-tcp-endpoint-for-hello-virtual-machine"></a>Skapa en TCP-slutpunkt för hello virtuell dator
I ordning tooaccess SQL Server från hello internet, hello virtuella datorn måste ha en slutpunkt toolisten för inkommande TCP-kommunikation. Den här Azure konfigurationssteget dirigerar inkommande TCP-port trafik tooa TCP-port som är tillgänglig toohello virtuell dator.

> [!NOTE]
> Om du ansluter inom hello samma cloud service eller virtuellt nätverk, behöver du inte toocreate en offentligt tillgänglig slutpunkt. I så fall kan du fortsätta toohello nästa steg. Mer information finns i [Connection Scenarios](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios) (Anslutningsscenarier).
> 
> 

1. På hello Azure Portal, väljer **virtuella datorer (klassisk)**.
2. Välj sedan den virtuella SQL Server-datorn.
3. Välj **slutpunkter**, och klicka sedan på hello **Lägg till** knappen hello överst på bladet för hello-slutpunkter.
   
    ![Steg för att skapa en slutpunkt i Azure Portal](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. På hello **Lägg till slutpunkt** bladet ger en **namn** , till exempel SQLEndpoint.
5. Välj **TCP** för hello **protokollet**.
6. För **Offentlig port** anger du ett portnummer, till exempel **57500**.
7. För **privat port**, ange lyssnande port för SQL Server, där standardinställningen för**1433**.
8. Klicka på **Ok** toocreate hello slutpunkt.

