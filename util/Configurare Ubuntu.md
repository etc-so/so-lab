## Configurarea unui sistem bazat pe Ubuntu pentru laborator

Pentru laboratorul de „Sisteme de Operare”, veți avea nevoie de o serie de pachete
pe care trebuie să le instalați în sistemele voastre. În funcție de distribuția
folosită, unele din ele pot fi instalate deja implicit, altele însă lipsesc, de
obicei, pentru că vizează aspecte (de ex., compilarea programelor) de care
utilizatorii obișnuiți nu se lovesc.

Este recomandat să instalați pachetele menționate mai jos cu comanda aferentă.

#### git

Pentru a salva munca voastră pe GitHub.

```
sudo apt-get install git gitk git-man
```

#### Compilatorul C

În majoritatea distribuițiilor destinate utilizatorilor comuni, compilatoarele nu sunt
incluse în instalarea implicită. Excepție fac distribuțiile special destinate pentru
ingineri, cercetători sau hackeri. Din fericire, însă, Ubuntu oferă un metapachet care
include toate utilitarele de care aveți nevoie pentru a compila programe C (gcc, binutils, make, etc...)

```
sudo apt-get install build-essential
```

#### Manuale

Paginile de manual (_manpages_) sunt o necesitate atât pentru programatori, cât și pentru
administratori de sistem, pentru că ele descriu detaliat toate funcțiile și programele
folosite în sistem. Din motivele prezentate mai sus, însă, nici ele nu sunt instalate implicit
pe sistemele destinate utilizatorilor comuni.

```
sudo apt-get install manpages manpages-dev manpages-posix manpages-posix-dev
```
