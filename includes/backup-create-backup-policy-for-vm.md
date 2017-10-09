## <a name="defining-a-backup-policy"></a>Definiera en princip för säkerhetskopiering
En princip för säkerhetskopiering definierar en matris över när ögonblicksbilder av hello data tas och hur länge de kvarhålls. När du definierar en princip för säkerhetskopiering av en VM, kan du utlösa ett säkerhetskopieringsjobb *en gång per dag*. När du skapar en ny princip är tillämpade toohello valvet. hello-gränssnittet för säkerhetskopieringsprincipen ser ut så här:

![Princip för säkerhetskopiering](./media/backup-create-policy-for-vms/backup-policy.png)

toocreate en princip:

1. Ange ett namn för hello **principnamn**.
2. Ögonblicksbilder av data kan tas med dags- eller veckointervall. Använd hello **säkerhetskopieringsfrekvens** nedrullningsbara menyn toochoose om ögonblicksbilder av data ska tas dagligen eller veckovis.
   
   * Om du väljer dagsintervall använda hello markerade kontrollen tooselect hello tidpunkten hello för hello ögonblicksbild. toochange hello timme, avmarkerar hello timmen och väljer hello ny timme.
     
     ![Princip för daglig säkerhetskopiering](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>
   * Om du väljer veckointervall använda hello markerade kontrollerna tooselect hello dag i veckan hello och hello tid på dagen tootake hello ögonblicksbild. Välj en eller flera dagar i dagmenyn hello. Välj en timme i timmenyn hello. toochange hello timme, avmarkerar hello timmen och väljer hello ny timme.
     
     ![Princip för veckovis säkerhetskopiering](./media/backup-create-policy-for-vms/backup-policy-weekly.png)
3. Som standard är alla alternativ för **Kvarhållningsintervall** markerade. Avmarkera alla kvarhållningsintervallet som du inte vill toouse. Ange sedan hello intervall toouse.
   
    Månatliga och årliga Kvarhållningsintervall låter dig toospecify hello ögonblicksbilderna baserat på veckovisa eller dagliga inkrement.
   
   > [!NOTE]
   > Ett säkerhetskopieringsjobb körs en gång per dag när du skyddar en VM. hello tid när hello säkerhetskopiering körs är hello samma för varje Kvarhållningsintervall.
   > 
   > 
4. När du har angett alla alternativ för hello principen hello överkant hello-bladet klickar du på **spara**.
   
    hello nya principen är omedelbart tillämpade toohello valvet.

