---
title: "Open Java Runtime Environment instalācija"
author: "MLiberts"
date: "2019-09-05 15:18:37"
output:
  html_document: 
    keep_md: yes
    toc: yes
---



# Java

## Kāpēc ir vajadzīga Java?

Java ir nepieciešama vairāku programmu darbībai, piemēram:

- [JDemetra+](https://github.com/jdemetra/jdemetra-app/releases)
- [RJdemetra](https://jdemetra.github.io/rjdemetra/)
- [μ-ARGUS](http://research.cbs.nl/casc/mu.htm)
- [τ-ARGUS](http://research.cbs.nl/casc/tau.htm)
- [rJava](http://rforge.net/rJava/)


## JRE un JDK

JRE ir *Java Runtime Environment*. JDK ir *Java Development Kit*. Java izstrādātājiem ir nepieciešams JDK, Java lietotājiem pietiek ar JRE. MND Java programmatūru neizstrādā, attiecīgi mums pietiek ar JRE.


## Oracle Java

Līdz šim MND izmantojām [Oracle Java JRE](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html). 2019. gada sākumā Oracle mainīja licencēšanas politiku un šī politika ir neskaidra. Joprojām nav skaidrs vai mēs CSP varam lietot Oracle Java JRE 8 bez maksas. Ir dažādi viedokļi.

Lai izkļūtu no šīs neskaidrās situācijas, ir pieņemts lēmums turpmāk vairs nelietot Oracle Java un lietot kādu citu Open Java distribūciju.


## Open Java

Java pirmkods ir brīvi lietojams, bet praksē ir nepieciešama kompilēta Java. Ir vairāki Java provaideri, kā piemēram:

- [Zulu JDK](https://www.azul.com/downloads/zulu/)
- [AdoptOpenJDK](https://adoptopenjdk.net/)
- [Amazon Corretto](https://aws.amazon.com/corretto/)

Teorētiski ir vienalga, kura Open Java versija tiek lietota, jo tās visas ir kompilētas no vienu un tā paša pirmkoda. Kaut gan praksē nelielas atšķirības ir iespējamas. Tāpēc būtu labi, ja darbinieki, kas strādā pie viena projekta izvēlētos vienu un to pašu Open Java relīzi.


# Instalācija

## Funkcija `install.open.jre`

Open JRE instalāciju var veikt lietotājs. Nav vajadzīga administratora palīdzība. Lai atvieglotu Open JRE instalāciju, ir sagatavota R funkcija `install.open.jre`. Skatīt failu `install_OpenJRE.R`. Funkcija veic sekojošus instalācijas soļus:

- No izvēlētā provaidera mājas lapas tiek lejupielādēta pēdējā Java 8 JRE portable versija (zip fails).
- Tiek vaikta faila pārbaude ar pārbaudes summu (sha256). Tikai AdoptJDK gadījumā.
- Tiek atarhivēts zip faila saturs.
- Pēc noklusējuma tiek definēts vides mainīgais (*environment variable*) `JAVA_HOME`. Šo var atslēgt ar funkcijas argumentu `set.env.variable = FALSE`.

Pēc noklusējuma faili tiek saglabāti mapē `C:\Users\[user]\Documents\OpenJRE`. Failu saglabāšanas mapi var mainīt ar funkcijas argumentu `path.jre`. Esošie faili tiek pārrakstīti, bet katrai JRE versijai ir sava mape.

Java provaideri var izvēlēties ar funkcijas parametru `provider`. Iespējamās vērtības ir:
- `amazon`: instalēs Amazon Corretto,
- `adopt`: instalēs AdoptOpenJDK,
- `zulu`: instalēs Zulu JDK.
Noklusētā vērtība ir `amazon`.



## Vides mainīgais `JAVA_HOME`

Liela daļa programmu, piemēram, R un R pakotnes, lietotāja instalētu Java atrod pēc vides mainīgā (*environment variable*) `JAVA_HOME`. Šādā gadījumā vairs nekādas papildus darbības nav nepieciešamas. Atliek pārstartēt R vai RStudio un installētā Java būs pieejama.


## Pārbaude

OpenJRE instalāciju var pārbaudīt no komandrindas (*Command Prompt* vai *Windows PowerShell*) ar sekojošām komandām:

`echo %JAVA_HOME%` parāda OpenJRE instalācijas mapi.

```
C:\Users\MLiberts>echo %JAVA_HOME%
C:\Users\MLiberts\Documents\OpenJRE\jre8
```

`%JAVA_HOME%\bin\java -version` parāda instalēto Java versiju.

```
C:\Users\MLiberts>%JAVA_HOME%\bin\java -version
openjdk version "1.8.0_222"
OpenJDK Runtime Environment Corretto-8.222.10.3 (build 1.8.0_222-b10)
OpenJDK 64-Bit Server VM Corretto-8.222.10.3 (build 25.222-b10, mixed mode)
```


# JDemetra+

Ne visas programmas izmanto vides mainīgo `JAVA_HOME`, piemēram, JDemetra+. Lai JDemetra+ zinātu, kur meklēt lietotāja instalēto Java, ir jāizveido īsceļš (*shortcut*) uz failu `..\nbdemetra\bin\nbdemetra64.exe`. Īsceļā ir jānorāda papildus arguments `--jdkhome %JAVA_HOME%`. Attiecīgi īsceļam ir jābūt noformētam līdzīgi `..\nbdemetra\bin\nbdemetra64.exe --jdkhome %JAVA_HOME%`. Skatīt attēlu.

![](JDemetra-target.png)
