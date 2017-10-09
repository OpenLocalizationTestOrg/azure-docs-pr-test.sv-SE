## <a name="push-tooazure-from-git"></a>Push-tooAzure från Git

Lägg till en Azure remote tooyour lokal Git-lagringsplats.

```bash
git remote add azure <URI from previous step>
```

Skicka toohello Azure remote toodeploy din app. Du ombeds hello lösenord som du skapade tidigare när du skapade hello distribution användare. Se till att ange hello lösenordet du skapade i [konfigurerar en distribution användare](#configure-a-deployment-user), inte hello lösenord du använder toolog i toohello Azure-portalen.

```bash
git push azure master
```

hello Visar föregående kommando information liknande toohello följande exempel:
