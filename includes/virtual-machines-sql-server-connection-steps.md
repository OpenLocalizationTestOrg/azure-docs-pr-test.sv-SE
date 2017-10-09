### <a name="open-tcp-ports-in-hello-windows-firewall-for-hello-default-instance-of-hello-database-engine"></a>Öppna TCP-portar i hello Windows-brandväggen för hello standardinstansen av hello databasmotor
1. Ansluta toohello virtuella datorn med fjärrskrivbord. Detaljerade instruktioner om hur du ansluter toohello VM finns [öppna en SQL-VM med Remote Desktop](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md#open-the-vm-with-remote-desktop).
2. När inloggad på hello startskärmen skriver **WF.msc**, och tryck sedan på RETUR.
   
    ![Starta hello brandväggsprogram](./media/virtual-machines-sql-server-connection-steps/12Open-WF.png)
3. I hello **Windows-brandväggen med avancerad säkerhet**i hello vänstra fönstret, högerklicka på **regler för inkommande trafik**, och klicka sedan på **ny regel** hello åtgärdsfönstret.
   
    ![Ny regel](./media/virtual-machines-sql-server-connection-steps/13New-FW-Rule.png)
4. I hello **ny inkommande regel för** dialogrutan under **regeltyp**väljer **Port**, och klicka sedan på **nästa**.
5. I hello **protokoll och portar** dialogrutan, Använd hello standard **TCP**. I hello **specifika lokala portar** rutan och typen hello portnummer hello hello Database Engine-instansen (**1433** för hello standardinstansen eller dina val för hello privat port i hello endpoint steg).
   
    ![TCP-port 1433](./media/virtual-machines-sql-server-connection-steps/14Port-1433.png)
6. Klicka på **Nästa**.
7. I hello **åtgärd** dialogrutan **Tillåt hello anslutning**, och klicka sedan på **nästa**.
   
    **Säkerhetsmeddelande:** Selecting **Tillåt hello anslutning om den är skyddad** kan ge ytterligare säkerhet. Välj det här alternativet om du vill tooconfigure ytterligare säkerhetsalternativ i din miljö.
   
    ![Tillåta anslutningar](./media/virtual-machines-sql-server-connection-steps/15Allow-Connection.png)
8. I hello **profil** dialogrutan **offentliga**, **privata**, och **domän**. Klicka sedan på **Nästa**.
   
    **Säkerhetsmeddelande:** Selecting **offentliga** tillåter åtkomst via hello internet. Välj alltid en mer restriktiv profil om det är möjligt.
   
    ![Offentlig profil](./media/virtual-machines-sql-server-connection-steps/16Public-Private-Domain-Profile.png)
9. I hello **namn** dialogrutan Skriv ett namn och beskrivning för den här regeln och klickar sedan på **Slutför**.
   
    ![Regelnamn](./media/virtual-machines-sql-server-connection-steps/17Rule-Name.png)

Öppna ytterligare portar för andra komponenter efter behov. Mer information finns i [konfigurera hello Windows-brandväggen tooAllow SQL Server-åtkomst](http://msdn.microsoft.com/library/cc646023.aspx).

### <a name="configure-sql-server-toolisten-on-hello-tcp-protocol"></a>Konfigurera SQL Server toolisten på hello TCP-protokoll

[!INCLUDE [Enable TCP](virtual-machines-sql-server-connection-tcp-protocol.md)]

### <a name="configure-sql-server-for-mixed-mode-authentication"></a>Konfigurera SQL Server för blandat läge-autentisering
hello SQL Server Database Engine kan inte använda Windows-autentisering utan domänmiljö. tooconnect toohello databasmotorn från en annan dator, konfigurerar SQL Server för verifiering i blandat läge. Med blandat läge-autentisering tillåts både SQL Server-autentisering och Windows-autentisering.

> [!NOTE]
> Det kanske inte är nödvändigt att konfigurera blandat läge-autentisering om du har konfigurerat ett virtuellt Azure-nätverk med en konfigurerad domänmiljö.
> 
> 

1. När anslutna toohello virtuell dator på hello startsida, Skriv **SQL Server Management Studio** och klickar på valda hello-ikonen.
   
    hello första gången du öppnar Management Studio måste det skapa hello användare Management Studio miljö. Det kan ta en stund.
2. Management Studio visar hello **ansluta tooServer** dialogrutan. I hello **servernamn** rutan, hello-typnamn för hello virtuella tooconnect toohello databasmotorn med hello Object Explorer (i stället för hello virtuellt datornamn som du kan också använda **(lokal)** eller en enskild period som hello **servernamn**). Välj **Windowsautentisering**, och lämna  ***your_VM_name*\your_local_administrator** i hello **användarnamn** rutan. Klicka på **Anslut**.
   
    ![Ansluta tooServer](./media/virtual-machines-sql-server-connection-steps/19Connect-to-Server.png)
3. Högerklicka på hello namnet på hello instans av SQL Server (hello virtuellt datornamn) i SQL Server Management Studio Object Explorer och klicka sedan på **egenskaper**.
   
    ![Serveregenskaper](./media/virtual-machines-sql-server-connection-steps/20Server-Properties.png)
4. På hello **säkerhet** sidan under **serverautentisering**väljer **SQL Server och Windows-autentisering läge**, och klicka sedan på **OK** .
   
    ![Välja autentiseringsläge](./media/virtual-machines-sql-server-connection-steps/21Mixed-Mode.png)
5. I dialogrutan för hello SQL Server Management Studio klickar du på **OK** tooacknowledge hello krav toorestart SQL Server.
6. Högerklicka på din server i Object Explorer och klicka sedan på **Starta om**. (Om SQL Server Agent körs måste den också startas om.)
   
    ![Starta om](./media/virtual-machines-sql-server-connection-steps/22Restart2.png)
7. I dialogrutan för hello SQL Server Management Studio klickar du på **Ja** tooagree som du vill toorestart SQL Server.

### <a name="create-sql-server-authentication-logins"></a>Skapa SQL Server-autentiseringsinloggningar
tooconnect toohello databasmotorn från en annan dator, måste du skapa minst en inloggning för SQL Server-autentisering.

1. Expandera hello mapp hello server-instans som du vill toocreate hello ny inloggning i SQL Server Management Studio Object Explorer.
2. Högerklicka på hello **säkerhet** mappen, peka för**ny**, och välj **inloggning...** .
   
    ![Ny inloggning](./media/virtual-machines-sql-server-connection-steps/23New-Login.png)
3. I hello **inloggning - ny** på hello dialogrutan **allmänna** anger hello namn för den nya användaren i hello i hello **inloggningsnamnet** rutan.
4. Välj **SQL Server-autentisering**.
5. I hello **lösenord** anger ett lösenord för hello ny användare. Ange lösenordet igen i hello **Bekräfta lösenord** rutan.
6. Välj hello tvingande lösenordsalternativ krävs (**genomdriva lösenordsprinciper**, **genomdriva Lösenordets giltighetstid**, och **användaren måste byta lösenord vid nästa inloggning**). Om du använder den här inloggningen själv behöver inte toorequire en lösenordsändring vid hello nästa inloggning.
7. Från hello **standarddatabasen** väljer du en standarddatabas för hello inloggningen. **Master** är hello standardvärde för det här alternativet. Om du inte har skapat en användardatabas lämna det ställa in för**master**.
   
    ![Inloggningsegenskaper](./media/virtual-machines-sql-server-connection-steps/24Test-Login.png)
8. Om det här är första hello inloggning som du skapar kanske du vill toodesignate inloggningen som en SQL Server-administratör. I så fall på hello **serverroller** sidan Kontrollera **sysadmin**.
   
   > [!NOTE]
   > Medlemmar i hello fasta serverrollen sysadmin har fullständig kontroll över hello databasmotorn. Du bör noggrant begränsa medlemskapet i den här rollen.
   > 
   > 
   
   ![sysadmin](./media/virtual-machines-sql-server-connection-steps/25sysadmin.png)
9. Klicka på OK.

Mer information om SQL Server-inloggningar finns i [Skapa en inloggning](http://msdn.microsoft.com/library/aa337562.aspx).

