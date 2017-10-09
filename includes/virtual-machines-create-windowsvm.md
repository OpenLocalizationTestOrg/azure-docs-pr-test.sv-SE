1. Logga in toohello [Azure-portalen](https://portal.azure.com).

2. Från och med hello övre vänstra hörnet, klickar du på **New > Compute > Windows Server 2016 Datacenter**.

    ![Navigera toohello Azure VM-avbildningar i hello-portalen](./media/virtual-machines-common-portal-create-fqdn/marketplace-new.png)

3. Välj hello klassiska distributionsmodellen på hello Windows Server 2016 Datacenter. Klicka på Skapa.

    ![Skärmbild som visar hello Azure VM-avbildningar som är tillgänglig i hello-portalen](./media/virtual-machines-common-portal-create-fqdn/deployment-classic-model.png)

## <a name="1-basics-blade"></a>1. Bladet Grundläggande inställningar

hello grunderna bladet begär hello virtuella datorns administrativ information.

1. Ange en **namn** för hello virtuella datorn. I exemplet hello _HeroVM_ är hello hello virtuella datorns namn. hello namn måste vara 1 och 15 tecken långt och det får inte innehålla specialtecken.

2. Ange en **användarnamn** och ett starkt **lösenord** som är toocreate används ett lokalt konto på hello VM. hello lokalt konto används toosign i tooand hantera hello VM. I exemplet hello _azureuser_ är hello användarnamn.

 hello lösenordet måste vara 8 och 123 tecken och uppfylla tre utanför hello fyra följande krav på komplexitet: en gemen, en versal, en siffra och ett specialtecken. Läs mer om [krav för användarnamn och lösenord](../articles/virtual-machines/windows/faq.md).

3. Hej **prenumeration** är valfritt. En vanlig inställning är ”Betala per användning”.

4. Välj en befintlig **resursgruppen** eller hello-typnamn för en ny. I exemplet hello _HeroVMRG_ är hello namnet på hello resursgrupp.

5. Välj ett Azure-datacenter **plats** där du vill att hello VM toorun. I exemplet hello **östra USA** är hello plats.

6. När du är klar klickar du på **nästa** toocontinue toohello nästa bladet.

    ![Skärmbild som visar hello inställningar på bladet för hello grunderna för att konfigurera en Azure VM](./media/virtual-machines-common-portal-create-fqdn/basics-blade-classic.png)

## <a name="2-size-blade"></a>2. Bladet Storlek

hello storlek bladet identifierar hello konfigurationsinformation för hello VM och visar en lista över olika alternativ som innehåller OS, antal processorer, disktyp för lagring och uppskattade kostnaderna för månatlig användning.  

Välj en VM-storlek och klicka sedan på **Välj** toocontinue. I det här exemplet _DS1_\__V2 Standard_ är hello VM-storlek.

  ![Skärmbild av hello storlek blad som visar hello Azure VM-storlekar som du kan välja](./media/virtual-machines-common-portal-create-fqdn/vm-size-classic.png)


## <a name="3-settings-blade"></a>3. Bladet Inställningar

hello inställningsbladet begär alternativ för lagring och nätverk. Du kan acceptera standardinställningarna för hello. Azure skapar relevanta poster om det behövs.

Om du valde en VM-storlek som stöder det kan du prova Azure Premium Storage genom att välja Premium (SSD) i Disktyp.

När du har gjort önskade ändringar klickar du på **OK**.

## <a name="4-summary-blade"></a>4. Bladet Sammanfattning

hello sammanfattning bladet visar hello inställningarna i hello föregående blad. Klicka på **OK** när du är klar toomake hello avbildningen.

 ![Översikt över bladet rapporter med angivna inställningarna för hello virtuell dator](./media/virtual-machines-common-portal-create-fqdn/summary-blade-classic.png)

När hello virtuell dator skapas hello portal visar hello ny virtuell dator under **alla resurser**, och visar en panel för hello virtuell dator på hello instrumentpanelen. hello motsvarande cloud service och storage-konto också skapas och visas. Både hello virtuella datorn och Molntjänsten startas automatiskt och deras status visas som **kör**.

 ![Konfigurera VM-agenten och hello slutpunkter för hello virtuell dator](./media/virtual-machines-common-portal-create-fqdn/portal-with-new-vm.png)
