### <a name="configure-a-dns-label-for-hello-public-ip-address"></a>Konfigurera en DNS-etikett för hello offentlig IP-adress

tooconnect toohello SQL Server Database Engine från hello Internet, kan du skapa en DNS-etikett för din offentliga IP-adress. Du kan ansluta med IP-adress, men hello DNS-etikett skapar en A-post som är enklare tooidentify och sammanfattningar hello underliggande offentlig IP-adress.

> [!NOTE]
> DNS-etiketter krävs inte om planen tooonly ansluta toohello SQL Server-instansen inom hello samma virtuella nätverk eller bara lokalt.

toocreate en DNS-etikett först välja **virtuella datorer** hello-portalen. Välj din SQL Server-VM toobring fram egenskaperna.

1. I hello-översikt över virtuella datorer, Välj din **offentliga IP-adressen**.

    ![offentlig IP-adress](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. Expandera i hello egenskaper för din offentliga IP-adress, **Configuration**.

1. Ange ett namn för DNS-etiketten. Det här är en A-post som kan använda tooconnect tooyour SQL Server-VM via namn istället för IP-adressen direkt.

1. Klicka på hello **spara** knappen.

    ![dns-etikett](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a>Ansluta toohello databasmotorn från en annan dator

1. På en dator ansluten toohello internet, Öppna SQL Server Management Studio (SSMS). Om du inte har SQL Server Management Studio kan du ladda ned den [här](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).

1. I hello **ansluta tooServer** eller **ansluta tooDatabase motorn** dialogrutan, redigera hello **servernamn** värde. Ange hello IP-adress eller fullständigt DNS-namn för hello virtuella datorn (bestäms i hello föregående åtgärd). Du kan också lägga till ett kommatecken och ange TCP-porten för SQL Server. Till exempel `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.

1. I hello **autentisering** väljer **SQL Server-autentisering**.

1. I hello **inloggning** rutan, ange hello namnet på en giltig SQL-inloggning.

1. I hello **lösenord** rutan, typen hello lösenordet till hello-inloggning.

1. Klicka på **Anslut**.

    ![ssms anslut](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)