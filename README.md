# template-copy

Actions från template är automatiskt aktiverade.

## How to pull from template parent

Finns lite olika lösningar. Verkar som stora problemet kommer från att när man skapar från template får man inte med commit historik så när man vill göra pull mote basrepo saknas historiken vilket man behöver lösa på olika sätt. https://stackoverflow.com/questions/56577184/github-pull-changes-from-a-template-repository

Jag tänker att man kan göra ett make command för att koppla repot till template. Då kör man alltid den när man skapar ett nytt. Sen kan man göra ett command som gör "pull"?

- I target repo baserat på crontab [Sync action](https://github.com/marketplace/actions/actions-template-sync)
- I source repo baserat på push/PR/osv... Hittar automatiskt de repo som är skapade från template [sync action](https://github.com/ahmadnassri/action-template-repository-sync)
- [fetch till ny branch och sen merge från den till main](https://stackoverflow.com/a/69563752)
- [fetch med --allow-unrelated-history](https://stackoverflow.com/a/56577320) # tror folk har haft problem med denna lösningen pågrund av att commit historik saknas. Annars är denna nog enklast
- [Använd Github importer för att skapa repo. Då kan man göra fetch och merge senare](https://stackoverflow.com/a/66948475)
- [Merge med squash](https://stackoverflow.com/a/75573089)
