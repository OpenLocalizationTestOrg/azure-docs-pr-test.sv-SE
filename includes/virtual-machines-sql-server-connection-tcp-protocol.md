1. När anslutna toohello virtuella datorn med fjärrskrivbord, söka efter **Configuration Manager**:

    ![Öppna SSCM](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. Konfigurationshanteraren för SQL Server i hello konsolfönstret expanderar **SQL Server-nätverkskonfigurationen**.

1. I konsolfönstret hello klickar du på **protokoll för MSSQLSERVER** (hello namnet för standardinstansen.) Högerklicka i informationsfönstret hello **TCP** och på **aktivera** om den inte redan är aktiverad.

    ![Aktivera TCP](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. I konsolfönstret hello klickar du på **SQL Server Services**. Högerklicka i informationsfönstret hello  **SQL Server (*instansnamn*) ** (hello standardinstansen är **SQL Server (MSSQLSERVER)**), och klicka sedan på **starta om** , toostop och starta om hello-instans av SQL Server.

    ![Starta om databasmotorn](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Stäng Konfigurationshanteraren för SQL Server.

Mer information om hur du aktiverar protokoll för hello SQL Server Database Engine finns [aktivera eller inaktivera nätverksprotokoll Server](http://msdn.microsoft.com/library/ms191294.aspx).
