# template-copy

Actions från template är automatiskt aktiverade.

## How to pull from template parent

Finns lite olika lösningar. Verkar som stora problemet kommer från att när man skapar från template får man inte med commit historik så när man vill göra pull mote basrepo saknas historiken vilket man behöver lösa på olika sätt. https://stackoverflow.com/questions/56577184/github-pull-changes-from-a-template-repository

Jag tänker att man kan göra ett make command för att koppla repot till template. Då kör man alltid den när man skapar ett nytt. Sen kan man göra ett command som gör "pull"?

- [fetch med --allow-unrelated-history](https://stackoverflow.com/a/56577320) # funkade. Behövde göra en merge lokalt
    Efter satt upp upstream kör
    `git fetch --all`
    `git merge template/main --allow-unrelated-histories` | kan nog också köra `git rebase template/main`, ska vara bättre. Kolla om behöver lägga till `--allow-unrelated-histories`.
    verkar som att add remote inte blir det av repot. Måste lägga till det igen på annan dator. Hittar inget med google hur man gör det bara en gång.
    En till fördel med denna är att pull inte försöker skriva över samma filer igen om man har ändrat dem sen sen skapandet. Men om man ändrar i filerna igen i template då försöker den skriva över igen, vilket är bra.
    Kan göra make commands av ovanstående.
- I target repo baserat på crontab [Sync action](https://github.com/marketplace/actions/actions-template-sync)
    - Problem med att uppdatera workflows i copy repo. För att action ska få uppdatera workflows behöver man skapa en PAT med specifika rättigheter. https://github.com/marketplace/actions/actions-template-sync#troubleshooting.
    Kan skapa en PAT i organisation och en SECRET av den som vi kan använda för alla repos. Dock håller ens token MAX ett år så behöver skapa ny varje år.
    - Behöver aktivera `Allow GitHub Actions to create and approve pull requests` för Enterprise, organisation och repo.
    > Open your project `Settings > Actions > General` and select the checkbox `Allow GitHub Actions to create and approve pull requests` under the `Workflow permissions section`.

    - Det skapas en branch med ändringarna som man sen måste gå in och skapa PR och merga.
    - Detta workflow inkluderas också för studenternas forkade repo. Kanske man kan exkludera på något sätt. Eller så gör det inte så mycket eftersom det måste köras manuellt. Man kan nog också fixa med permissions så de inte får köra det kanske.
    - Om man har gjort ändringar i copy repot i filer från template då vill git ändra koden till den från template.
        Kan komma runt detta. Om det är filer vi vet att vi vill använda som en mall och vi ska ändra i, i copy repon. Då kan vi lägga dem i syncignore. Då får vi med mallen när vi skapar copy repot men ändringarna kommer inte skrivas över när Action körs. Det medför dock att om vi skulle vilja göra en "global" ändring i de filerna måste man gå in och göra det manuellt.
        Fördel med manuell sync är att man får merge conflict och kan välja vad som ska ändras. Men detta är bara ett problem om vi har filer i template som vi vill ändra på i copy. Om vi bara har filer som är oförändrade är det inte något problem.
    

- I source repo baserat på push/PR/osv... Hittar automatiskt de repo som är skapade från template [sync action](https://github.com/ahmadnassri/action-template-repository-sync)
    Dennna behöver också PAT. Har inte testat denna än dock.
- [fetch till ny branch och sen merge från den till main](https://stackoverflow.com/a/69563752)
- [Använd Github importer för att skapa repo. Då kan man göra fetch och merge senare](https://stackoverflow.com/a/66948475)
- [Merge med squash](https://stackoverflow.com/a/75573089)
