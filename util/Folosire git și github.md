## Folosirea programului `git` și a platformei _GitHub_

Lucrările de laborator "Sisteme de Operare" sunt distribuite studenților prin platforma GitHub Classroom, de aceea prima lucrare de laborator este cel de inițiere în folosirea sistemului de versionare `git` și a platformei _GitHub_.

În acest document, găsiți informații niște informații suplimentare legate de modul de folosire a
acestora.

### git

**git** este un sistem de control al versiunilor descentralizat, inițial dezvoltat de către
Linus Torvalds pentru a facilita dezvoltarea kernelului Linux. Aceasta iese în evidență dintre celelalte
sisteme prin branching-ul foarte ieftin, ce permite o dezvoltare paralelă foarte ușoară în cazul
proiectelor mari.

#### Repository

Un _repository_ sau, mai pe scurt, _repo_ este depozitul în care este stocat întreg istoricul dezvoltării
unui proiect. Acesta conține toate fișierele ce au fost adăugate vreodată la proiect, respectiv
toate stadiile de modificare a acestora. _Repo_-ul se regăsește în directorul rădăcină al proiectului,
într-un subdirector numit `.git`, și conține fișierele în formă de un lanț de modificări pornind de
la prima versiune adăugată într-o formă comprimată și criptată. Criptarea este importantă pentru a
păstra un istoric intact, fără a permite modificări ulterioare, respectiv pentru ca orice modificare să
poată fi legată la dezvoltatorul care a efectuat acea modificare. De asemenea, în acest repo se regăsesc
și informații de configurare a proiectului, date de identificare și alte informații necesare pentru
buna funcționare a sistemului.

Interacțiunea cu repo-ul este realizată prin intermediul unor comenzi trimise la programul **git**.
Mai întâi, însă, trebuie precizate niște noțiuni de bază:

* __istoric__ este tot istoricul de modificări a fișierelor adăugate la repo; acesta poate să aibă
o structură arborescentă și, de obicei, este reprezentat ca un graf aciclic direcționat;
* __commit__ este un nod de pe acest istoric; conține modificările operate de dezvoltator într-o etapă.
Fiecare commit are un identificator unic (un cod SHA-1) cu care puteți să accesați commit-ul respectiv.
Fiind un cod SHA-1, e destul de lung, dar pentru identificare sunt suficiente primele câteva caractere
care să identifice unic (evident, la un proiect mai lung, e nevoie de mai multe caractere);
* __branch__ este o bifurcație în istoric, unde sunt adăugate commit-uri în paralel;
* __merge__ reintegrarea unui branch în părinte (sau în alt branch);
* __cherry-pick__ integrarea unui singur commit de pe un branch pe altul;
* __working copy__ este directorul de lucru; acesta poate conține modificări față de ultimul commit din
repo, care urmează să fie comise în repo. De asemenea, aici pot să fie și fișiere care sunt ignorate:
de exemplu, fișiere binare compilate ce ar ocupa inutil spațiu pentru că pot fi oricând recompilate
din sursă;
* __staging area__ este prima etapă într-un commit; aici sunt puse fișierele ce urmează să fie comise.
Este folosit pentru a permite o granularitate mai bună a commit-urilor prin selectarea fișierelor
(respectiv, chiar a anumitor linii din fișiere) ce să fie incluse în commit.

Comenzile cele mai frecvent folosite pentru legarea proiectului în lucru cu repo-ul asociat sunt:

```
git add [-a] [nume_fișier]
```
Cu această comandă, se pot adăuga fișiere (nominalizate unul câte unul sau folosind wildcard-uri de shell)
sau, cu opțiunea `-a`, toate fișierele modificate. Fișierele neincluse, încă, în git trebuie nominalizate
obligatoriu.

```
git status
```
Arată starea fișierelor în directorul de lucru. Acestea pot fi modificate sau neincluse (non-tracked)
în git.

```
git checkout [nume_fișier]
```
Înlocuiește fișierul curent (cu orice modificare existentă) cu copia existentă în repo. Aceasta, practic,
se folosește pentru a recrea starea curentă a repo-ului (dacă sunt niște modificări sau experimentări
nedorite).

```
git commit
```
Va introduce modificările aflate în staging area (de după `git add`) în repo. Fiecare commit trebuie
să aibă un mesaj asociat care explică ce modificare aduce commit-ul respectiv, ce poate fi vizualizat
împreună cu istoricul proiectului și, astfel, permite găsirea mai ușoară a anumitor commit-uri.
De aceea, comanda va porni un editor de text în care se poate introduce acest mesaj. Editorul pornește
cu niște comentarii care includ și lista fișierelor comise, astfel ușurând construirea mesajului oportun.
Aceste comentarii (liniile care încep cu `#`) nu vor fi incluse în mesaj.

Dacă se dorește adăugarea unui mesaj de un singur rând, aceasta se poate realiza și prin opțiunea
`-m "mesaj"`.


#### Branch-uri și merge-uri

Avantajul major al sistemului git față de celelalte sisteme este ușurința de a crea și de a reintegra
branch-uri în istoricul repo-ului. Aceasta permite crearea unor puncte de dezvoltare paralelă a anumitor
funcționalități care, ulterior, pot fi reintroduse în trunchiul comun când sunt considerate a fi gata.

Pentru a crea un nou branch, se folosește comanda:
```
git branch BRANCH_NAME
```
unde `BRANCH_NAME` este o denumire a branchului
(de preferat, să indice și ce funcționalitate oferă branch-ul).

Schimbarea între branch-uri se realizează tot prin comanda:
```
git checkout BRANCH_NAME
```
de data aceasta, denumind branch-ul ce trebuie adus în directorul de lucru.

Branch-ul primar din care se pornește tot istoricul în git se numește `master` și este,
practic, trunchiul comun în ierarhia arborescentă.

După ce s-a făcut dezvoltarea pe branch, acesta poate fi reintegrat în branch-ul de unde s-a pornit
(sau, eventual, în master) cu comanda:

```
git merge NUME_BRANCH
```
Aceasta trebuie emisă pe branch-ul pe care se dorește a se reveni (deci, înainte trebuie schimbat
branch-ul actual la acesta cu comanda `checkout`) și va include un nou commit pe acest branch care,
practic, va conține tot ceea ce s-a modificat în branch-ul respectiv.

În mod normal, git este suficient de deștept ca să adune toate modificările realizate pe branch-uri
paralele și să salveze rezultatul ca și cum s-ar fi întâmplat una după cealaltă.
Se poate întâmpla, totuși, ca pe amândouă branch-urile ce sunt integrate, să fie modificate aceleași linii de cod;
în acest caz, git nu poate ghici care ar trebui să fie rezultatul. De aceea, semnalează un conflict.
Acesta este marcat în fișierul sursă prin includerea liniilor modificate în ambele branch-uri,
între linii marcate cu `<<<<<<<`, respectiv `>>>>>>>`. Liniile respective trebuie editate manual de
către dezvoltator, după care fișierele rezolvate sunt adăugate în staging area (cu `add`) și, la final,
comise manual în repo.

#### Sincronizare cu ceilalți

Sistemul git, fiind descentralizat, înseamnă că, în cazul dezvoltării proiectului în mod colaborativ,
e nevoie și de sincronizarea repo-urilor individuale ale dezvoltatorilor.
Această sincronizare se realizează prin comenzile:

```
git pull
```
pentru a aduce modificările de pe repo-ul de la distanță în repo-ul local;

```
git push
```
pentru a trimite modificările locale în repo-ul de la distanță.

Bineînțeles, cele două repo-uri de la dezvoltatori diferiți sunt considerate și ele ca niște branch-uri
pentru că, practic, ele sunt construite în paralel; de aceea, comanda `pull` este echivalentă cu
un merge (cu posibilitate de conflict ce trebuie rezolvat în modul prezentat anterior). Din cauză
că conflictele pot fi rezolvate numai local prin intervenția utilizatorului uman, nu se poate
executa `push` dacă acesta ar avea nevoie de un merge pe calculatorul de la distanță (adică, sunt
prezente acolo modificări ce nu se regăsesc în repo-ul local). Astfel, înainte de `push`, întotdeauna
trebuie realizat și un `pull` (se sincronizează, mai întâi, local și, după aceea, la distanță).


Mai multe informații și o descriere detaliată a tuturor comenzilor, găsiți la [1].

#### Bibliografie
[1] [http://git-scm.com/doc](http://git-scm.com/doc)

### GitHub

Platforma **GitHub** este o platformă online ce permite integrarea unui repository central de **git** cu
o rețea de socializare, ușurând, astfel, colaborarea între dezvoltatori sau grupuri de dezvoltatori,
împreună cu o sincronizare facilă a repo-urilor individuale ale acelor dezvoltatori.

Punctul de plecare într-un proiect GitHub o reprezintă repo-ul git asociat. Fiecare repo este identificat
cu un URL de forma `github.com/username/reponame`, unde `username` este numele utilizatorul proprietar
al proiectului, iar `reponame` este numele proiectului. Fiecare repo poate să aibă mai mulți colaboratori
cu drept de scriere/citire, caz în care colaboratorii pot să interacționeze direct cu
repo-ul prin intermediul programului git,
sau poate fi doar citit de ceilalți utilizatori, în afară de proprietar, caz în care utilizatorii
trebuie să interacționeze prin intermediul platformei online.

#### Repo-uri personale

Repo-ul poate fi descărcat pe calculatorul dezvoltatorului folosind comanda:

```
git clone https://github.com/username/reponame
```

Această comandă va permite interacțiunea cu GitHub prin protocolul HTTPS securizat, ceea ce înseamnă
că va trebui introdus numele de utilizator și parola de fiecare dată când se comunică cu serverul.
Parola folosită pe această interfață însă nu este parola asociată cu contul GitHub de pe pagina web. 
Pentru a folosi uneltele din linia de comandă fiecare utilizator va trebui să creeze un _Personal Access Token_ pe pagina de setări (secțiune _Developer settings_) a profilului de utilizator.

Deși această metodă aduce mai multă povară din cauza introducerii perechii username/parolă de fiecare
dată, ea este metoda preferată în timpul laboratorului, unde studenții lucrează pe calculatoare partajate,
și este de preferat evitarea refolosirii datelor de identificare de către ceilalți colegi.
Tot din această cauză, este recomandată setarea datelor de identificare locală în repo, iar după terminarea
lucrului în laborator, ștergerea completă a proiectului (acesta fiind păstrat oricum pe serverul GitHub).

Astfel, după clonare, trebuie să configurați și datele de identificare cu comenzile:

```
git config --local user.name "NUME PRENUME"
git config --local user.email EMAIL@ADDRESS
```

Evident, înlocuiți NUME PRENUME, respectiv EMAIL@ADDRESS, cu numele complet și adresa de e-mail folosită
la înregistrarea pe GitHub.

Cei care lucrează pe calculatoarele personale de acasă pot să evite acest overhead prin
configurarea globală a identității și folosirea unui acces prin SSH cu cheie publică. Detalii despre 
acest mod, găsiți la [1].

Repo-ul local poate fi sincronizat cu serverul GitHub folosind comenzile `pull` și `push`. Pentru
a aduce ultima versiune de pe server pe mașina locală, se folosește comanda:

```
git pull
```

Aceasta va aduce toate modificările de pe server și va muta toate modificările locale (din commit-uri
netrimise la server) după ultimul commit de pe server, folosind tehnica rebase. Dacă commit-urile
locale afectează același fișier (mai exac,t aceleași linii de cod din fișier), atunci poate fi raportat
un conflict de merge ce trebuie rezolvat în modul prezentat anterior.

Pentru a trimite modificările locale către serverul GitHub, se folosește comanda:

```
git push
```

Aceasta va funcționa doar dacă pe server nu există commit-uri care să nu fie prezente și local. De aceea,
înainte de `push`, trebuie să vă asigurați că aveți cea mai nouă versiune de pe server cu comanda `pull`.

#### Repo-uri colaborative

În aceste reporui modul de lucru este puțin diferit, în sensul că repoul are un proprietar care răspunde de conținutul repoului. Aceste repo-uri pot fi citite, respectiv clonate de către ceilalți utilizatori. În schimb, nu poate fi
efectuat `push` pe ele. De aceea, ele au nevoie de o altă modalitate de a interacționa. Această
modalitate este **fork/pull-request**, disponibilă ca operații direct pe platforma online.

Pentru a realiza operații cu astfel de repo, utilizatorul, mai întâi, trebuie să creeze o copie proprie
a repo-ului respectiv. Aceasta se realizează prin intermediul butonului **Fork** de pe pagina repo-ului.
Aceasta, după niște întrebări de confirmare, va crea un nou repository sub numele utilizatorului care
este la momentul respectiv. Este o clonă identică cu repo-ul primar.
Repo-ul, fiind creat în proprietatea utilizatorului, poate fi folosit în modul prezentat la punctul
precedent (clonat pe mașina locală, modificat și comis, respectiv încărcat înapoi pe server).

Pentru o operativitate mai bună, se recomandă ca orice modificare în repo-ul propriu să fie realizată
pe un branch care, în cele din urmă, să ajungă înapoi în repo-ul părinte, iar master-ul fork-ului să fie
păstrat pentru sincronizare continuă cu părintele.

Astfel, după ce ați clonat repo-ul și ați configurat identitatea, creați un nou branch pentru
funcționalitatea pe care vreți să lucrați:

```
git branch FUNCTIE_NOUA
```

Bineînțeles, FUNCTIE_NOUA este o denumire oarecare care ar trebui aleasă astfel încât să reflecte la
ce vreți să lucrați.

După ce ați implementat funcționalitatea și ați trimis la server, puteți să cereți ca modificările
să fie integrate în repo-ul părinte. Pentru aceasta, trebuie să foloșiți butonul de `Create pull request`
în pagina repo-ului părinte, indicând branch-ul pe care vreți să fie integrat. Împreună cu pull-request,
puteți adăuga și un comentariu în care să explicați ce ați modificat, de ce vreți să fie integrat,
poate și issue-ul pe care l-ați rezolvat.
Proprietarul repo-ului va fi notificat de acest pull-request și poate să examineze modificările. Dacă
consideră modificările oportune, atunci el acceptă pull-request-ul și îl integrează în repo.

Altfel, dacă consideră că mai lipsește ceva, sau nu întreg codul este după standardul așteptat, atunci
poate să facă comentarii la pull-request. În acest caz, utilizatorul poate să amendeze acele comentarii
și să încarce o nouă versiune modificată. Pentru aceasta, este suficient să facă `push` pe branch-ul
respectiv. Toate commit-urile ulterioare pull-requestului vor fi automat adăugate la pull-request-ul
curent. Nu e nevoie să se deschidă un nou pull-request.

După ce pull-request-ul a fost acceptat și integrat în repo-ul părinte, nu mai e nevoie de branch-ul
de dezvoltare, care poate fi șters (este chiar recomandată ștergerea acestor branch-uri). Trebuie,
totuși, făcută sincronizarea între părinte și fork. Pentru aceasta, e nevoie de următoarele comenzi:

```
git checkout master
```
pentru a reveni înapoi la trunchiul repo-ului;

```
git remote add upstream https://github.com/parentuser/parentrepo
```
Cu aceasta, se va configura calea către repo-ul părinte (evident, `parentuser` și `parentrepo` trebuie
înlocuite cu user și repo corespunzătoare); aceasta comandă e suficient să fie dată o singură dată, la început;

```
git pull upstream master
git push
```
Aceasta va aduce modificările de pe părinte în repo-ul local, după care le mută și pe server, în repo-ul
propriu.

#### Bibliografie

[1] [https://help.github.com/](https://help.github.com/)
