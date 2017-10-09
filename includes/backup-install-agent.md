## <a name="download-install-and-register-hello-azure-backup-agent"></a>Hämta, installera och registrera hello Azure Backup-agenten
När du har skapat hello Azure Backup-valv, ska en agent installeras på var och en av dina Windows-datorer (Windows Server, Windows-klient, System Center Data Protection Manager-server eller Azure Backup Server-dator) som möjliggör säkerhetskopiering av data och program tooAzure.

1. Logga in toohello [hanteringsportalen](https://manage.windowsazure.com/)
2. Klicka på **återställningstjänster**och välj hello säkerhetskopieringsvalv som du vill tooregister med en server. Hej snabbstartsidan för att säkerhetskopieringsvalvet visas.
   
    ![Snabbstart](./media/backup-install-agent/quickstart.png)
3. På hello Snabbstart klickar du på hello **för Windows Server eller System Center Data Protection Manager eller Windows-klienten** alternativ **hämta Agent**. Klicka på **spara** toocopy den toohello lokal dator.
   
    ![Spara agent](./media/backup-install-agent/agent.png)
4. När hello-agenten är installerad, dubbelklicka på MARSAgentInstaller.exe toolaunch hello installation av hello Azure Backup-agenten. Välj hello installationsmappen och tillfälliga mapp som krävs för hello agent. hello cacheplats anges måste ha ledigt utrymme som är minst 5% av hello säkerhetskopierade data.
5. Om du använder en proxy server tooconnect toohello internet i hello **proxykonfiguration** anger hello proxy-serverinformation. Om du använder en autentiserad proxyserver ange hello namn och lösenord användarinformation i den här skärmen.
6. hello Azure Backup-agenten installeras .NET Framework 4.5 och Windows PowerShell (om det inte är tillgängligt) toocomplete hello installation.
7. När hello-agenten är installerad klickar du på hello **fortsätta tooRegistration** knappen toocontinue med hello arbetsflöde.
   
   ![Registrera dig](./media/backup-install-agent/register.png)
8. Bläddra i hello valvet autentiseringsuppgifter skärmen tooand väljer hello valvautentiseringsfilen som tidigare har hämtats.
   
    ![Valvautentiseringsuppgifter](./media/backup-install-agent/vc.png)
   
    Hej valvautentiseringsfilen är endast giltig för 48 timmar (när den har hämtats från hello portal). Om det uppstår något fel i den här skärmen (t.ex. ”valvet autentiseringsuppgifterna filen har upphört att gälla”), inloggning toohello Azure-portalen och hämta hello valvautentiseringsuppgifter filen igen.
   
    Se till att hello valvautentiseringsfilen finns på en plats som kan nås av hello installationsprogram. Om du får åtkomst till relaterade fel, kopiera hello valvet autentiseringsuppgifter tooa tillfällig filplats i den här datorn och försök hello igen.
   
    Om det uppstår ett fel för ogiltiga autentiseringsuppgifter (t.ex. ”ogiltiga autentiseringsuppgifterna”) hello-filen är antingen skadad eller har inte har hello senaste autentiseringsuppgifterna som hör till hello återställningstjänsten. Försök igen när du har hämtat en ny valvautentiseringsfil från portalen hello om hello. Det här felet visas vanligen om hello användare klickar på hello **Download valvautentiseringen** alternativ i hello Azure-portalen i snabb följd. I det här fallet gäller endast hello andra valvautentiseringsfilen.
9. I hello **krypteringsinställning** skärmen, kan du antingen skapa en lösenfras eller ange en lösenfras (minst 16 tecken). Kom ihåg toosave hello lösenfras på en säker plats.
   
    ![Kryptering](./media/backup-install-agent/encryption.png)
   
   > [!WARNING]
   > Om hello tappar bort lösenfrasen eller glömt; Microsoft kan inte att återställa hello säkerhetskopierade data. hello användaren äger hello krypteringslösenfrasen och Microsoft har inte insyn i hello lösenfras som används av hello slutanvändaren. Spara hello-filen på en säker plats när den behövs under en återställning.
   > 
   > 
10. När du klickar på hello **Slutför** knappen, hello datorn har registrerats toohello valv och du är nu redo toostart säkerhetskopiera tooMicrosoft Azure.
11. När du använder Microsoft Azure Backup fristående du kan ändra hello-inställningar som anges när ett arbetsflöde för registrering av hello genom att klicka på hello **ändra egenskaper för** alternativ i hello Azure Backup mmc snapin-modulen.
    
    ![Ändra egenskaper för](./media/backup-install-agent/change.png)
    
    Du kan också när du använder Data Protection Manager, du kan ändra hello inställningar anges när ett arbetsflöde för registrering av hello genom att klicka på hello **konfigurera** alternativet genom att välja **Online** under hello **Management** fliken.
    
    ![Konfigurera Azure Backup](./media/backup-install-agent/configure.png)

