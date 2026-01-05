# This is header
This is not header

Git to kontrola wersji lokalnie na komputerze
Github to zdalny system kontroli wersji. Możemy posiadać wiele repo
Repozytoria/Projekty możemy pushować do githuba lub pullować/clonować do lokalnego gita

README.md plik strony głównej na githubie. opisuje repo

Przy comicie na githubie i tak samo na gicie podajemy opis tego konkretnego comita i jego description. Potem wiemy co zmienił konkretny commit

## Dodawanie lokalnego klucza ssh do githuba
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
Dodawanie lokalnego klucza do gituba autoryzuje nas i pozwala nam modyfikować kod na zdalnym repozytorium pushami itd

ssh-keygen -t ed25519 -C "your_email@example.com"
Ta komenda stworzy klucz prywatny id_ed25519 i publiczny id_ed25519.pub w C:\Users\S\.ssh. Zawartośc plublicznego dodajemy w githubie w ustawieniach.
Musimy potem jescze ten klucz dodac do ssh agenta ale to wszystko jest opisane w linku powyżej

W C:\Users\S\.gitconfig znajduje sie nazwa i email lokalnego commitera. Każdy lokalny comit bedzie podpisywany tym userem.


## git clone git@github.com:filutekartur/Tutorial.git
klonuje całe repo z githuba do miejsca w którym jest obecnie terminal
Bedac w jakimś repo na githubie możemy kliknąć code>SSH i tam mamy link : git@github.com:filutekartur/Tutorial.git

## git status
pokazuje status repozytorium tj. ktore pliki zostały zmodyfikowane, a które zostały dodane całkowicie nowe itp.


## git add .
Rozpoczyna śledzenie plików. Można dodać pojedyncze pliki, całe katalogi lub . która dodaje całą zawartość

## git commit -m "Title" -m "Description"
zapisuje zmiany w śledzonych plikach (add) zmiany zostaną zapisane w lokalnym Git 

## git push
wysyła obecny lokalny comit do repo na githubie. jeżeli użyłeś komendy git init musisz wcześniej połączyć lokalnego gita ze zdalnym repo na githubie żeby widział gdzie wypychać comity
### git push -u origin master
Upstream dla push. Od tej pory można używać samego git push i git bedzie wiedział żeby wysłać to na origin i master. W przypadku pushowania nowej gałęzi będziemy musieli ustawić upstream z nową nazwą gałęzi

## git init
Tworzy lokalne repozytorium kontroli wersji. musimy przejść do tego katalogu w którym nie ma jeszcze folderu .git i po stworzeniu tego repo dodajemy pliki, commitujemy i mozemy potem wysłać to do githuba
### git remote add origin git@github.com:filutekartur/to-delete2.git
Łączy origin ze zdalnym repo. W przypadku tworzenia nowego repo na kompie żeby móc potem wysyłąć commity do zdalnego repo to musimy stworzyć najpierw takie repo na githubie i wyciągnąć SSH.

## git checkout main
Przechodzi pomiedzy gałęziami. --force wymusze przejście do innej gałęzi porzucając zmiany zapisane i również te które czekająw indexie.

### git checkout -b branch2
Tworzy gałąź branch2 i do niej przechodzi. jeżeli mamy różnice w jednym pliku w na jednej gałęzi i na drugiej to przchodząc checkoutem plik w którym są te różnice np. README.md bedzie sie zmieniac.
Oznacza to że każda gałąź to tak jakby osobny projekt
### git branch
pokazuje wszystkie dostępne gałęzie

## git diff test.txt
pokazuje różnica pomiędzy stanem z ostatniego comita a tym co zmieniliśmy w pliku ale nie dodaliśmy do indexu. Jeśli dodamy do indexu to git diff nie pokaże nic
### git diff branch2
Pokazuje różnice pomiędzy ostatnim comitem na obecnej gałęzi i ostatnim comitem gałęzi branch2. Można dopisać drugą nazwę gałęzi żeby sprawdzić dwie konkretne gałęzie a nie tą na której jesteśmy

## git merge branch2
Merguje branch2 do gałezi na której obecnie jesteśmy np. master, domyślnie z funkcją fast-forward merge. Merg z gałęzi feature do master powoduje że do master zostaje dołączona historia całej gałęzi feature.
Ponieważ merge commit posiada dwóch rodziców, jeden z gałęzi master a drugi z gałęzi feature. Ominąć to można używając opcji squash.
### git merge branch2 --no-ff
merguje bez fast-forward merge. Zwykły merge jeśli może to spłaszczy gałąź i włączy ją do głównej gałęzi. Ta metoda pozwala na uniknięcie tej sytuacji i zachowanie branch2 w historii jako widocznej dodatkowej gałęzi. I zawsze tworzy to commit scalający na gałęzi głównej czyli taki comit podsumowywujący włącznie gałęzi branch2 do master

## git pull
pobiera i scala najnowsze comity ze zdalnego repozytorium. Wykonuje git fetch + git merge fetch_head. Fetch_head to poprostu dynamiczny wskaźnik na head najnowszego fetcha. Przydaje się gdy przeskakujemy po różnych repo np. origin1, origin2 wtedy fetch_head bedzie pokazywać na najnowszy pobrany commit a origin1/master zawsze na to repo
### git fetch
pobiera wszystkie zmiany ze zdalnego repo do lokalnej wersji zdalnego repo czyli np. origin/master. lokalna gałąź master jest nie naruszona do tej pory
### git merge fetch_head
scala z obecną gałęzią ostatnio fetchowaną gałąź

## git reset
cofa nas do konkretnego commita lub usuwa pliki z indeksu
### git reset . lub git reset README.md
usuwa wszystkie pliki lub konkretne pliki z indeksu (unstage)
### git reset (hash of commit) lub git reset HEAD~1
cofa nas do konkretnego commitu domyślnie z --mixed lub cofa nas o jeden commit wstecz od head. hash commitu możemy wiciągnąć z git log
#### --soft
uncomit - tak, cofa commit\
unstage - nie, index jest nadal kompletny
#### --mixed
uncomit - tak, cofa commit\
unstage - tak, czyści index\
files - nie, pliki pozostają bez zmian
#### --hard
uncomit - tak, cofa commit\
unstage - tak, czyści index\
files - tak, przywraca pliki z konkretnego commita

## git restore .
przywraca stan w katalogu roboczym lub indexie, nie usuwa comita. Po użyciu restore nadal jesteśmy na ostatnim comicie tj. HEAD
### git restore
przywraca pliki do stanu z indexu
### git restore --staged .
przywraca index do stannu z ostatniego comita (head)
### git restore --staged --worktree .
przywraca pliki i index do stanu z ostatniego comita (head)
### git restore --source HEAD~1 .
cofa zmiany w plikach do stanu z przedostatniego commita HEAD, co ciekawe index pozostaje nie tknięty więc można spowrotem do niego wrócić

## gitk
przeglądarka commitów/gałęzi itd.
### git log --graph
tekstowo/graficzny podlągad gałęzi i comitów można również dodać --oneline dla skróconego widoku
git log main..feature pokaże wszystkie comity feature których nie ma w main czyli taki diff

## git config
Konfiguracja opcji globalnych/repo. --list pokaże obecną konfigurację. Plik zapisuje się w c:\Users\x\.gitconfig
### git config --global user.name "Full Name"
Globalna konfiguracja nazwy uzytkownika którym bedą podpisywane wszystkie comity
### git config --global user.email "email@address.com"
Globalna konfiguracja maila którym bedą podpisywane wszystkie comity

## Vim
Domyślny edytor tekstu dla Gita. 
i komenda włączająca edycję pliku
esc wychodzi z edycji tekstu i przechodzi do lini komend
:wq - write (zapis) i wyjście z pliku
