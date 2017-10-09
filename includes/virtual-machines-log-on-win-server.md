1. Om du klickar på **Anslut** så skapas och hämtas en protokollfil för fjärrskrivbord (.rdp-fil). Klicka på **öppna** toouse den här filen.
2. Du får en varning om att hello RDP-filen kommer från en okänd utgivare. Detta är normalt. På hello fjärrskrivbordet i fönstret **Anslut** toocontinue.
   
    ![Skärmbild med ett varning som meddelar att utgivaren är okänd.](./media/virtual-machines-log-on-win-server/rdp-warn.png)
3. I hello **Windows-säkerhet** , anger hello autentiseringsuppgifterna för ett konto på hello virtuella datorn och klicka sedan på **OK**.
   
     **Lokalt konto** -detta är vanligtvis hello lokalt konto, användarnamn och lösenord som du angav när du skapade hello virtuell dator. I det här fallet hello domänen hello hello virtuella datorns namn och anges den som *vmname*&#92; *användarnamnet*.  
   
    **Domänansluten VM** - om hello VM tillhör tooa domän, ange hello användarnamn i formatet hello *domän*&#92; *Användarnamnet*. hello-kontot måste också tooeither att gruppera i hello administratörer eller ha beviljats fjärråtkomst privilegier toohello VM.
   
    **Domänkontrollanten** - om hello VM är en domänkontrollant, Skriv hello användarnamn och lösenord för ett domänadministratörskonto för domänen.
4. Klicka på **Ja** tooverify hello hello virtuella datorns identitet och slutföra inloggning.
   
   ![Skärmbild som visar ett meddelande möts verifierar hello identitet hello VM.](./media/virtual-machines-log-on-win-server/cert-warning.png)

