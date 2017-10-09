### <a name="determine-hello-dns-name-of-hello-virtual-machine"></a>Fastställa hello hello virtuella datorns DNS-namn
tooconnect toohello SQL Server Database Engine från en annan dator, måste du känna till hello Domain Name System (DNS) namn på hello virtuell dator. (Detta är hello namn hello internet använder tooidentify hello virtuell dator. Du kan använda hello IP-adress, men hello IP-adress kan ändras när Azure flyttar resurser för redundans och underhåll. hello DNS-namn är stabil eftersom det kan vara omdirigeras tooa nya IP-adressen.)  

1. I hello Azure-portalen (eller hello föregående steg), Välj **virtuella datorer (klassisk)**.
2. Välj den virtuella SQL-datorn.
3. På hello **virtuella** bladet, kopiera hello **DNS-namnet** för hello virtuella datorn.
   
    ![DNS-namn](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a>Ansluta toohello databasmotorn från en annan dator
1. På en dator ansluten toohello internet, Öppna SQL Server Management Studio.
2. I hello **ansluta tooServer** eller **ansluta tooDatabase motorn** i dialogrutan hello **servernamn** ange hello DNS-namn för hello virtuella datorn (bestäms i hello föregående aktivitet) och en offentlig slutpunkt portnummer i hello-format för *DNSName, portnumber* som **mysqlvm.cloudapp.net,57500**.
   
    ![Ansluta med SSMS](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
   
    Om du inte kommer ihåg hello offentlig slutpunkt portnummer du skapade tidigare, kan du hitta det i hello **slutpunkter** område i hello **virtuella** bladet.
   
    ![Offentlig port](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. I hello **autentisering** väljer **SQL Server-autentisering**.
4. I hello **inloggning** rutan, hello-typnamn för en inloggning som du skapade i en tidigare uppgift.
5. I hello **lösenord** rutan, ange hello lösenord för hello-inloggning som du skapar i en tidigare uppgift.
6. Klicka på **Anslut**.

