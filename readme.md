ÜLDISED NÕUDED (koodi oma projektis kasutamiseks)

Branchimine:
1. Main branch peab olema "protected branch" sinna EI TOHI commite teha, merge on lubatud ainult haldurile ja protsess on eelnevalt kokku lepitud. 
2. Kõikide muudatuste loomiseks tuleb luua arendusbranch ja pärast muuta (vt. ka Confluence "versioneerimine"). Branchi nimes EI TOHI olla tühikut. 
3. Branchida tohib AINULT viimase "release" main haru pealt. Ajaloos tagasi liikumiseks tee uus projekt või kopeeri vana haru kood uude branchi!
4. Uue projekti versioonimisega alustamiseks tuleb lisada uus haru "versioning", lisada main branchi tag 0.0.0 ja tööd teha harus "versioning" 
   * integreerida "Projektis millega töötatakse" peatükis toodud muudatused
   * "merge" tagasi "main" harusse
   * kävitada "main" harus "branch" job, tüüpi patch "initialbuild"
   * käivitada build
   * mergeda tagasi "main" harusse
5. Nüüd võiks kõik inital konteinerid, tagid ja released olemas olla. (Kontrolli harborist tulemust)

Projektis millega töötatakse (versioneerimise intgreerimine):
1. Loo fail build.sh Shell runneri build käsud kirjutada faili build.sh NB!!! build.sh execute lipp tuleb püsti panna! ÄRA KASUTA WINDOWSI REAVAHETUSI!!! 
  * vaata versioning projektis build.example
2. Kui on uus projekt mis veel tagimata, tee main branchi start-tag v0.0.0 (ilma selleta hakkad saama igasugu müstilisi urroreid)
3. Kui on veel mingeid muudatusi või kohandusi vaja teha, tee need enne ära ja pushi oma kood MAINi (See on ka viimane kord kui tohid selles projektis midagi main harusse pushida)
1. Kontrolli, et "main" haru oleks "protected" Settings > Repository > Protected branches. Allowed to merge: ja Allowed to push and merge: tuleb panna Maintainers
2. Loo "project access token" : Settings > Access tokens - NB! need aeguvad hiljemalt aastaga.
2. Kopeeri CI muutujate alla, nt. CI_PROJECT_TOKEN
3. kontrolli, kas GENEERIKUTE grupimuutujad on sul projekti kaasa tulnud (vajalik build blokis)
4. Häälesta merge requesti seaded 
  * Projekti seaded: Settings > Merge requests
  * valida "Merge commit with semi-linear history"
  * Merge commit message template muuda kujule
  ```
  %{source_branch} merged into %{target_branch}
  Merge request %{reference}
  ```
5. Projektile määrata ci/cd runner. "shell" tagiga shell runner
6. Seadista projektile versioning projektist konveieri ci_cd väljakutse
  * settings ci/cd > "general pipelines" > ci/cd config ".gitlab-ci.yml@geneerikud/platvormid/versioning"
 

Kasutamine:
1. Tööprojektis run pipeline
  * Avanenud valikutes tuleb valida millist tüüpi ja mis nimega branchi me soovime luua. NB! branchi nimes mitte kasutada sümboleid (.#$- vms)
  * Run pipeline
  * apply job branch
4. Liigume branchi ja teeme ehitamise muudatused dockerfiles ja/või build.sh-s
5. Peale muudatusi käivitada job
6. Ehitatakse ja pushitakse harborisse vastavalt versioonitagide loogikale
7. Kui kõik testitud ja hästi siis mergeda
8. Avada pipeline ja run job
9. Ehitatakse ja pushitakse uus image millel siis muudetakse ver nr ja pannakse näiteks release tag külge
10. Harborist koristada vahepealsed imaged
NB! "release" buildi saab ehitada ainult "upgrade" ja "feature" pealt. "patch" lisatakse viimasele "releasele" 
