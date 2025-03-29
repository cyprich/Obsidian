# Databazove Systemy

## Basics

DML - Select, Update, Delete, Insert  
DDL (Data Definition Language) - Create, Alter, Drop - automaticky robi commit  
DCL - Grant, Revoke
TCL (Transaction Control Language, niekedy aj DAS) - Commit, Rollback

SQL je neproceduralny jazyk -

### Datove typy

- `VARCHAR(5)` - ako keby string s variabilnou dlzkou (max 5 znakov)
- `VARCHAR2(5)` - novsia verzia `VARCHAR`
- `CHAR(5)` - ako keby string s pevne danou dlzkou 5

- `NUMBER(x, y)` - ciselna hodnota
  - `x` - maximalny pocet znakov ktore bude mat cislo
  - `y` - pocet znakov za desatinnou ciarkou
  - `NUMBER(10, 0) = NUMBER(10) = INTEGER(10)`
- `SMALLINT`, `FLOAT`, `DOUBLE`

- `DATE` - datum
- `TIME` - cas
- `DATETIME` - datum aj cas
- `TIMESTAMP(9)` - presnejsie ako sekundy (`9 = sekunda *`$10^{-9}$ )
- Oracle ma iba `DATE`, ktore je akokeby `DATETIME`

Formaty datumov

- `YYYY` / `YY` / `RRRR` / `RR`
- `MM`
- `DD`
- `HH` (12-hodinovy cas) / `HH24` (24-hodinovy cas)
- `MI`
- `SS`
- ...

| Skratka | Meaning                   | Popis                                                                                                                                 |
| ------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `NN`    | Not Null                  | Hodnota musi byt zadana, nemoze byt Null                                                                                              |
| `PK`    | Primary Key               | Primarny, unikatny kluc v tabulke, je `NN`, **je najviac jeden** (moze byt kompozitny - skladat sa z viacerych atributov?), minimalny |
| `FK`    | Foreign Key               | Odkazuje na primarny kluc z inej tabulky, moze byt aj Null                                                                            |
| `PFK`   | Primary Foreign Key       | Primary + Foreign                                                                                                                     |
| `KPK`   | Kandidat primarneho kluca | Nieco, co teoreticky moze byt tiez primarny kluc                                                                                      |

## SELECT

```sql
SELECT * FROM studenti  -- vypise vsetko o vsetkych zaznamoch z tabulky studenti

SELECT meno, priezvisko, os_cislo FROM studenti  -- vypise iba meno, priezvisko a osobne cislo z tabulky studenti

SELECT meno, priezvisko
FROM studenti
WHERE meno='Peter'
-- vypise vsetkych Petrov z tabulky

SELECT meno AS m FROM studenti  -- alias, namiesto "meno" sa odteraz pouziva "m"

SELECT meno, rocnik+1 AS novy_rocnik FROM studenti
```

Praca s `Null`

```sql
-- NEMOZEME SPRAVIT TOTO
SELECT * FROM students WHERE ulica!=null

-- Treba spravit toto
SELECT * FROM students WHERE ulica IS NOT null
```

Zliepanie viac tabuliek dokopy - `JOIN`

```sql
SELECT *
FROM osobne_udaje
JOIN student
USING (rodne_cislo)
-- spoji tabulky "osobne_udaje" a "student" podla kluca "rodne_cislo"
-- toto sa da pouzit iba ak obidve tabulky obsahuju "rodne_cislo", teda PK a FK su rovnake

SELECT *
FROM osobne_udaje
JOIN student
ON (osobne_udaje.cislo = student.cislo)
```

Popis tabulky - `description`

```sql
DESCRIBE osobne_udaje
DESC osobne_udaje
```

`Like` - pouzitie akoze wildcards spolu so znakmi `%` a `_`

```sql
-- Znak % specifikuje lubovolny pocet znakov
SELECT *
FROM osobne_udaje
WHERE meno LIKE "%a%"  -- vsetci co maju v mene "a"

-- Znak _ specifikuje jeden znak
SELECT *
FROM osobne_udaje
WHERE meno LIKE "_a%"  -- vsetci co maju na druhom mieste "a"
```

`Dual` - specialna tabulka s jednym zaznamom (dummy)

```sql
-- Ak by sme spustili nasledovny prikaz, vratilo by nam to tolko odpovedi kolko je zaznamov v tabulke
SELECT 1+1 FROM studenti

-- Ak chceme vratit iba jednu odpoved, mozeme pouzit tabulku dual, v ktorej je iba jeden zaznam
SELECT 1+1 FROM dual

```

Datumy

```sql
SELECT to_date('05-01-2025', 'DD-MM-YYYY') FROM dual -- formatovanie datumu

SELECT to_char(sysdate, 'DD.MM.YY') FROM dual  -- dnesny datum

SELECT sysdate+1 FROM dual  -- dnesny datum + 1 den
SELECT add_months(sysdate, 2)  -- za 2 mesiace
```

Nejake selectiky

```sql
select meno,priezvisko
    from os_udaje join student(using rod_cislo)
    where rocnik=2;

select meno,priezvisko
    from os_udaje
    where rod_cislo IN (select rod_cislo from student where rocnik=2);
```

```sql
select meno,priezvisko
    from os_udaje
    where rod_cislo not in (select rod_cislo from student);

select meno,priezvisko
    from os_udaje
    where rod_cislo in (select rod_cislo from student where rocnik is null);
    -- cez IN sa neda spravit, vnoreny select nikdy nic nevrati lebo rocnik je NN

select meno,priezvisko
    from os_udaje o
    where NOT EXISTS
    (select 'x' from student s where s.rod_cislo=o.rod_cislo);
    -- univerzalnejsie riesenie - mozeme dat viac podmienok
```

Toto bude dolezite pri INSERT, DELETE a UPDATE

## INSERT

Vztahuje sa iba na jednu tabulku

```sql
INSERT INTO os_udaje
    values('560123/1234', 'Michal', 'Kvet', 'Univerzitna', '01026', 'Zilina')

INSERT INTO os_udaje
    values('560123/1235', 'Michal', 'Kvet', null, null, null)

INSERT INTO os_udaje(meno, priezvisko, rod_cislo)
    values('Michal', 'Kvet', '560123/1236')
```

Ulozim vysledok selectu do tabulky

```sql
select *
    from priklad_db2.os_udaje  -- z inej db, tabulka pouzivatela "priklad_db2"

insert into os_udaje(rod_cislo, meno, priezvisko)
    select rod_cislo, meno, priezvisko
        from priklad_db2.os_udaje;
        where priklad_db2.rod_cislo not in
            (select rod_cislo from os_udaje)

insert into os_udaje(rod_cislo, meno, priezvisko)
    select rod_cislo, meno, priezvisko
        from priklad_db2.os_udaje p;
        where p.rod_cislo not in
            (select rod_cislo from os_udaje)
```

VALUES vlozi jeden riadkok, presne definujem co idem vlozit  
Pomocou SELECT mozem vlozit lubovolny pocet udajov

```sql
desc student
insert into student
    values(500123412, 100, 0, '311234/1234', 1, '5ZI011', 'S', null, sysdate)
    -- error - narusene referencen obmedzenie - v druhej tabulke neexistuje uvedene rodne cislo
```

## UPDATE

Vzdy modifikuje jednu tabulku

```sql
update student
    set rocnik=3
    where os_cislo = 123456;

update student
    set rocnik=3,st_skupina=5'5ZI031'
    where os_cislo = 123456
```

Vylucim vsetkych petrov zo studia

```sql
update student
    set ukoncenie=sysdate
    where rod_cislo in
        (select rod_cislo from os_udaje
            where meno='Peter')

-- nepouzivat JOIN

update student
    set ukoncenie=sysdate
    where EXISTS
        (select 'x' from os_udaje
            where meno='Peter' AND os_udaje.rod_cislo=student.rod_cislo)
```

```sql
update student
    set stav='x'
    where exists
        (select 'x' from zap_predmety join predmet using (cis_premdetu)
        where nazov='Zaklady databazovych Systemy')
```

## DELETE

Vzdy pracujem s jednou tabulkou  
Ziadny JOIN  
Ak su nejake FK na polozku ktoru chcem vymazat tak to neprejde - ak student existuje v zapisanych predmetoch, studenta nemozem vymazat

```sql
delete -- nepisu sa ziadne atributy - vymazavam cely riadok
    from zap_predmety
    where os_cislo=501313

delete from student
    where os_cislo=501313
```

Chcem vymazat predmet DBS

```sql
delete from predmety where nazov='DBS'
-- toto nemozem spravit lebo predmet je este v inych tabulkach

delete from zap_predmety
    where cislo_predmetu in
        (select cis_premdetu from predmet
            where nazov='DBS')
delete from st_program
    where cislo_predmetu in
        (select cis_premdetu from predmet
            where nazov='DBS')
delete from predmet_bod
    where cislo_predmetu in
        (select cis_premdetu from predmet
            where nazov='DBS')
delete from predmet where nazov='DBS'
```

Chcem vymazat studijny odbor

```sql
delete from st_program p
    where EXISTS
        (select 'x' from st_odbory o
        where popis_odboru='Manazment' -- iba takto sa vymaze vsetko!!!
        and p.st_odbor = o.st_odbor
        and p.st_zameranie = o.st_zameranie) -- treba toto doplnit!
delete from zap_predmety zp
    where EXISTS
        (select 'x' from st_odbory o join student using(st_odbor, st_zameranie)
        where popis_odboru='Manazment'
        and student.os_cislo = zp.os_cislo
delete from student s
    where EXISTS
        (select 'x' from st_odbory o
        where popis_odboru='Manazment' -- iba takto sa vymaze vsetko!!!
        and s.st_odbor = o.st_odbor
        and s.st_zameranie = o.st_zameranie) -- treba toto doplnit!

delete from st_odbory where popis_odboru='Manazment'
```

---

Chcem zmenit rodne cislo studentovi (`801234/1234` -> `8801234/1234`)

```sql
insert into os_udaje(rod_cislo, meno, priezvisko, ulica, psc, obec)
    select '880123/1234', meno, priezvisko, ulica, psc, obec
        from os_udaje
            where rod_cislo = '801234/1234'

update student
    set rod_cislo='881234/1234'
    where rod_cislo='801234/1234'

delete -- vymazem povodneho studenta
```

## TCL

Chcem aby presla operacia bud cela, alebo vobec  
Napr. bankove transakcie

- `COMMIT` - potvrdit zmeny
- `ROLLBACK` - zahodit vsetky zmeny

Ak raz spravim commit, uz mi ziadny rollback nepomoze  
Rollback sa vracia ku poslednemu commitu (resp. na zaciatok?)

Vlastnost **atomicita** - bud transakcia prebehne cela alebo vobec - **A**  
Vlastnost **konzistencia** - vsetky obmedzenia (PK, NN, ) su zachovane - **C**  
Vlastnost **izolovanost** - ked mame 2 sessions - ak spravim zmeny v jednej session, v druhej to budem vidiet iba ak dam commit - **I**  
Vlastnost **durabilita** - **D**

= **ACID**

## Datove Modelovanie

- Struktura dat - ake tabulky chcem vytvorit,
- Manipulacia dat (DML (insert, update, delete, select)) - mozno nebudem chciet DELETE, alebo UPDATE alebo co ja viem co z nejakeho dovodu
- Integritne obmedzenia - musim obmedzit datovy typ na nejaku hodnotu - PSC nemoze byt `ABCDE`, rodne cislo nemoze byt `000000_0000`

### ERA Model

ER = Entity Relation - konceptualny model

Entita - je objekt realneho sveta, schopny nezavislej existencie, jednoznacne odlisitelny od ostatnych objektov  
Vztah - vazba medzi 2+ entitami  
Popisny typ - dvojica mnoziny hodnot a mnoziny operacii (napr. rocnik = cislo, mozem ho zmenit)  
Atribut - funkcia priradujuca entitam alebo vztahom hodnotu  
Typ entity - mnozina vlastnoti entity (osoba, ma 6 atributov (rodne cislo, meno, priezvisko, ...) )  
Typ vztahu - mnozina vlastnoti vztahu  
Instancia entity - samotna realizacia entity - samotny zaznam ktory ma svoje hodnoty  
Instancia vztahu - podobne ako instancia entity  
Identifikacny kluc - KPK, jeho hodnota sluzi na indentifikaciu

Typy atributov

- Atomnicke - neda sa delit na mensie zlozky (Rodne cislo - sklada sa sice z casti ale jednotlive casti maju rovnocenny vyznam)
- Neatomnicke - skupinove (strukturovane), viachodnotove

Superkluc - unikatny, ale nemusi byt minimalny

Definovanie kompozitneho PK

```sql
create table Tab(
    a integer primary key,
    b integer primary key  -- toto nemozem spravit - definujem 2 PK
)

create table Tab2(
    a integer,
    b integer,
    primary_key(a, b)  -- takto sa to robi
)
```

Definovanie FK

```sql
alter table Tab2 add foreign key (id) references Tab (id)
```

### Typ, Kardinalita, Clenstvo

**Typ vztahu**

Identifikacny vztah - prislusny FK sa stane castou PK
Neidentifikacny vztah -prislusny FK sa **ne**stane castou PK

**Kardinalita** - integritne obmedzenie, ktore vyjadruje (jedna osoba moze byt n-krat studentom)

- 1:1 - napr. jeden ujo moze byt starosta jedneho mesta
- 1:n - napr. v jednom dome moze byt viac ludi, ale jeden clovek moze byt iba v jednom dome
- m:n - napr. student a predmet - jeden student moze mat viac predmetov, jeden predmet moze mat viac studentov - v tomto pripade musi vzniknut nova tabulka - zap_predmety

**Clenstvo** - povinne vs. nepovinne - ci FK moze nabudat null alebo nie

V grafe

- Plna ciara = identifikacny vztah
- Prerusovana ciara = neidentifikacny vztah
- Ak je kardinalita 1:n, tak tam kde je `n` su "metlicky"
- Nepovinne clenstvo = kruzok

### Slabe entitne typy

Aby som definoval \_ tak potrebujem \_ z inej tabulky

### Rekurzivny vztah

Vzdy neindetifikacny

### ISA hierarchia

#### Generalizacia

#### Specializacia

## Select 2

### Join

#### Inner join

Doteraz sme pouzivali **Inner Join** - vysledok spojenia bol iba prienik tabuliek - co mali rovnake

```sql
select * from os_udaje join student using(rod_cislo);
select * from os_udaje inner join student using(rod_cislo);  -- to iste, nepovinny parameter inner
```

**Semi Join** - hladam tie osoby, ktore niekedy boli studentami - `in`, `exists`

```sql
select * from os_udaje
    where rod_cislo in (select rod_cislo fro mstudent);

select * from st_odbory
    where exists (select 'x' from student
        where student.st_odbory=st_odbory.st_odbor and student.st_zameranie=st_odbory.st_zameranie)  -- tolko podmienok, kolko atributov ma PK
```

**Anti Join** - osoby, ktore nikdy studentami neboli - `not in`, `not exists`

```sql
select * from os_udaje
    where rod_cislo not in (select rod_cislo from student);

select * from os_udaje o
    where not exists (select 'x' from student s where s.rod_cislo=o.rod_cislo);
```

---

```sql
select os_udaje.*
    from os_udaje join student on (os_udaje.rod_cislo=student.rod_cislo);  -- tento vrati viac hodnot - kardinalita 1:n

-- fix - doplnime distinct
select distinct os_udaje.* ...

select *
    from os_udaje
        where rod_cislo in (select rod_cislo form student);  -- toto bude rychlejsie
```

#### Outer join - Left, Right, Full

**Left** Join - zoberiem vsetko z lavej tabulky (os_udaje) + to co sa da z pravej tabulky (student)

```sql
select * from os_udaje
    LEFT join student using (rod_cislo);
-- pri nejakej osobe mame studentske hodnoty NULL - nikdy neboli studentami

-- cize teoreticky ak chcem zistit osoby ktore neboli studentami - NEPOUZIVAT - NEEFEKTIVNE!!
select *
    from os_udaje
    left join student using (rod_cislo)
    where os_cislo is null
```

`left join` = `left outer join`

**Right** Join - to iste len naopak

```sql
select * from student
    right join os_udaje using (rod_cislo);


select * from os_udaje  -- to iste co priklad v LEFT, ale pouzijem RIGHT - v tomto pripade to nema zmysel - nemoze existovat student bez os_udajov
    RIGHT join student using (rod_cislo);
-- v tomto pripade by bolo efektivnejsie INNER join
```

**Full** Join

```sql
select ico, rod_cislo, id_platitela
    from p_platitel
        join p_osoba on (id_platitela=rod_cislo)
        join p_zamestnavatel on (ico=id_platitela); -- nevrati nic - nikto nema rovnake ico a rodne cislo

select ico, rod_cislo, id_platitela
    from p_platitel
        full join p_osoba on (id_platitela=rod_cislo)
        full join p_zamestnavatel on (ico=id_platitela)
        order by rod_cislo nulls first; -- vrati vela vela - mam platitela vzdy + bud osobu alebo zamestnavatela
```

Dalsi priklad - kontakty - mozu byt bud na osoby alebo na firmu  
Dalsi priklad - ked mame nepovinne clenstvo

```sql
select * from personal_data full join contact using (personal_id);  -- osoby ku ktorym mam kontakty, ku ktorym nemam kontakty + kontakty ku ktorym nemam osobu
```

### Agregacne funkcie

Doteraz `min()`, `max()` `avg()`, `sum()`, `count()`, `mod()`

```sql
select count(*) from os_udaje; -- kolko zaznamov mame v tabulke os_udaje
-- namiesto * moze byt hocico
```

Agregacne funkcie ignoruju NULL hodnoty

```sql
select count(*), count(ukoncenie), count('ahoj') from student;
```

```sql
select min(rocnik), max(rocnik) from student;
select min(to_number(rocnik)), max(to_number(rocnik)) from student;

select count(*), count(cis_predmetu), count(disctinct cis_predmetu) from zap_predmety;
```

#### Group by

**Group by** - vsetko co je v selecte (okrem agregacnej funkcie) musi ist do group by, moze byt aj nieco naviac

```sql
select os_cislo, count(*) from zap_predmety group by os_cislo;

select meno, priezvisko, count(rod_cislo) from os_udaje
    join student using (rod_cislo)
    group by meno, priezvisko;  -- problem ak sa 2 osoby volaju rovnako

-- fix
select meno, priezvisko, count(rod_cislo) from os_udaje
    join student using (rod_cislo)
    group by meno, priezvisko, rod_cislo;
```

Pre kazdu osobu, kolko mala zapisanych predmetov

```sql
select meno, priezvisko, count(*)
    from os_udaje join student using(rod_cislo)
    join zap_predmety using (os_cislo)
    group by meno, priezvisko, rod_cislo;

-- iba unikatne predmety?
select meno, priezvisko, count(disctinct cis_predmetu)
    from os_udaje join student using(rod_cislo)
    join zap_predmety using (os_cislo)
    group by meno, priezvisko, rod_cislo;

-- niekto by mohol byt viac krat
select meno, priezvisko, count(disctinct cis_predmetu)
    from os_udaje join student using(rod_cislo)
    join zap_predmety using (os_cislo)
    group by meno, priezvisko, os_cislo;

-- nieco uplne ine??
select distinct meno, priezvisko, count(disctinct cis_predmetu)
    from os_udaje join student using(rod_cislo)
    join zap_predmety using (os_cislo)
    group by meno, priezvisko, os_cislo;
```

Kazda osoba kolko krat bola studentom

```sql
select meno, priezvisko, count(os_cislo)  -- nie *, nie rod_cislo - treba dat PK z tabulky ktora moze nadobudnut null
    from os_udaje left join student using (rod_cislo)  -- vsetko z os_udaje
    group by meno, priezvisko, rod_cislo
    order by 3;  -- podla tretieho stlpca
```

---

Osoby ktore mali >5 zap predmetov

```sql
select meno, priezvisko, count(*)
    from os_udaje
        join student using (rod_cislo)
        join zap_predmety using (os_cislo)
    -- where count(*) > 5 -- TOTO DA NEDA
    group by meno, priezvisko, rod_cislo
    having count(*) > 5;  -- TUTO TO TREBA - vyhodnocuje sa az neskor
```

Student, ktory ma najviac zapisanych predmetov

V `having` sa nemozu vnarat agregacne funkcie, iba v `select`

```sql
select meno, priezvisko, count(*)
    from os_udaje
        join student using (rod_cislo)
        join zap_predmety using (os_cislo)
    group by meno, priezvisko, rod_cislo
    having count(*) =
        (select max(count(*))
            from zap_predmety
            join student using (rod_cislo)
            join zap_predmety using (os_cislo)
            group by (meno, priezvisko, rod_cislo));

-- da sa vyhadzat nieco co nepotrebujem, ale neviem co to je
```

V niektorych databazovych systemoch sa neda vnarat agregacne funkcie ani v selecte - treba urobit dvojfazovo - vnoreny select

---

Vypisat zaznamy kde predmet_bod.ects != zap_predmety.ects

```sql
select os_cislo, cis_predm, zp.ects realne_ects, pb.ects predpokladane_ects
    from zap_predmety zp
    join predmet using (cis_predm)
    join predm_bod pb using (cis_predm)
    where zp.skrok=pb.skrok
        and zp.exts <> pd.ects;

-- realne z tabulky predmet nic neberiem - viem 2 tabulky spojit pomocou cis_predm a skrok
select os_cislo, cis_predm, zp.ects realne_ects, pb.ects predpokladane_ects
    from zap_predmety zp
    join predm_bod pb using (cis_predm, skrok)
    where zp.ects <> pd.ects;
```

## PL/SQL

- Anonymny/nepomenovany blok prikazov
- Pomenovany blok prikazov
  - Procedura
  - Funkcia - `to_char()` - nemoze robit insert, update, commit, ...
- DML Trigger - vykona sa ked nastane nejaka situacia, napr. Update
  - Kedy
    - Before
    - After
  - Kolko krat - vykoname Update, zmeni sa 5 zaznamov
    - Moze sa vyvolat 1 krat (1 update)
    - Moze sa vyvolat 5 krat (5 zaznamov)

SQL nepozna boolean, iba PL/SQL, dal by sa namodelovat `char(1) check (... in ('T', 'F'))`

```sql
declare  -- nepovinne, tu sa deklaruju premenne

begin
    null;  -- nic sa nevykona
end;
/
```

Premenne

- `premenna` `typ` `;`
- `premenna` `typ` `:=` `init_hodnota` `;`
- `premenna` `os_udaje.meon%type` `;`
- `premenna` `os_udaje%rowtype` `;`

If, Else, Elsif

```sql
if podmienka then
    prikazy;
elsif podmienka2 then
    prikazy;
[else
    prikazy;]
endif

```

Case

```sql
case premenna
    when hodnota1 then prikazy1;
    when hodnota2 then prikazy2;  -- neda sa skontrolovat ci je null, pretoze sa porovnava cez =
    else prikazy
end case;

case
    when podmienka1 then prikazy1;
    when podmienka2 then prikazy2;  -- neda sa skontrolovat ci je null, pretoze sa porovnava cez =
    else prikazy
end case;
```

Loop - nekonecny cyklus

```sql
loop
    prikazy
end loop;

loop
    if podmienka then
        exit;
    end if;
end loop

loop
    exit when podmienka
end loop;
```

While

```sql
while podmienka loop
    prikazy
end loop
```

For

```sql
for premenna in min..max loop  -- premenna (i) sa zadeklaruje automaticky (netreba manualne deklarovat), ma platnost iba vo vnutri for
    prikazy;
end loop;

for premenna in reverse min..max loop
    prikazy;
end loop;
```

Nepomenovany blok prikazov  
Jednorazovo vykonane

```sql
declare
    [premenna typ[:=init_hodnota];]
begin
    prikazy;
    [
        exception
            when typ_vynimky then
                prikazy;
            [when...]
    ]
end;
/
```

Procedura  
Nezadavat velkost typu - `varchar2(11)` nie, `varchar2` ano

```sql
create [or replace] procedure nazov_procedury
    [( nazov1 [mode1] typ1,
       nazov2 [mode2] typ2 )]
is | as
    [nazov_premennej typ [:= init_hodnota]; ]
begin
    prikazy;
    [exceptions]
end [nazov_procedury];
/

```

Funkcia  
Ako Procedura, ale ma navratovu hodnotu

```sql
create [or replace] function nazov_funkcie
    ...
return datatype
is | as
    ...
begin
    ...
    return vyraz;
end
/
```

Typ argumentu

- IN
- OUT
- IN OUT

Priklad

```sql
create or replace procedure query_stud
    (v_oc in student.os_cislo%type,
    v_meno out varchar2,
    v_skupina out student.st_skupina%type)
is
begin
    select meno || ' ' || priezvisko, st_skupina
    into v_meno, v_skupina  -- vlozi do premennych
    from ...
    where student.os_cislo = v_oc
end query_stud
/
```

---

```sql
select zoznam_stlpcov
into zoznam_premennych
from zoznam_tabuliek
```

```sql
declare
    p_meno os_udaje.meno%type;
    p_priezv os_daje.priezvisko%type;
    pocet integer;
begin
select meno, priezvisko, count(*)
into p_meno, p_priezv, pocet
from os_udaje ou
join student st using (rod_cislo)
left join zap_predmety using (os_cislo)
where os_cislo = 550807
group by meno, priezvisko;

dbms_output.put_line('Pocet predmeetov studenta - ' || p_meno || ' ' || p_priezv || ' je ' || pocet );
end;
/
```

Vynimky

| Exception | Oracle Error | Error Code |
| --------- | ------------ | ---------- |
|           |              |            |
|           |              |            |

Vlastne vynimky

```sql
raise_application_error(cislo_chyby, text_chyby);
```

`cislo_chyby` je cislo z intervalu `< -20999, -20000>`

---

Triggre

```sql
create [or replace] nazov_triggra
before | after
...
```
