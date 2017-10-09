### <a name="prepare-for-a-push-installation-on-a-linux-server"></a>Förbereda en push-installation på en Linux-server

1. Kontrollera att det finns en nätverksanslutning mellan hello Linux-dator och hello processervern.
2. Skapa ett konto som hello processervern kan använda tooaccess hello-dator. hello kontot ska vara en **rot** användare på hello Linux källservern. (Använd det här kontot bara för hello push-installation och uppdateringar.)
3. Kontrollera att hello/etc/hosts-filen på hello källan Linux-servern har poster som mappar hello lokala värdnamnet tooIP adresser som är associerade med alla nätverkskort.
4. Installera hello senaste openssh, openssh-server och openssl paket på hello-dator som du vill tooreplicate.
5. Kontrollera att Secure Shell (SSH) är aktiverat och körs på port 22.
6. Aktivera autentisering med SFTP undersystemet och lösenord i hello sshd_config fil:
  1.  Logga in som **rot**.
  2.  I hello filen/etc/ssh/sshd_config filen, hitta hello rad som börjar med **PasswordAuthentication**.
  3.  Ta bort kommentarerna hello rad och ändra hello värdet för**Ja**.
  4.  Hitta hello rad som börjar med **undersystemet** och Avkommentera hello rad.

     ![Linux](./media/site-recovery-prepare-push-install-mob-svc-lin/mobility2.png)
  5. Starta om hello **sshd** service.

7. Lägg till hello-konto som du skapade i CSPSConfigtool.
    1.  Logga in tooyour konfigurationsservern.
    2.  Öppna **cspsconfigtool.exe**. (Det är tillgängligt som en genväg på skrivbordet hello och i hello %ProgramData%\home\svsystems\bin mapp.)
    3.  På hello **hantera konton** klickar du på **Lägg till konto**.
    4.  Lägg till hello-konto som du skapade. 
    5.  Ange hello autentiseringsuppgifter som du använder när du aktiverar replikering för en dator.
