---
title: "Open Java Runtime Environment instalācija"
author: "MLiberts"
date: "2019-06-25 10:59:59"
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

Java pirmkods ir brīvi lietojams, bet praksē ir nepieciešama kompilēta Java. Ir vairāki Java devēji, kā piemēram:

- [Zulu JDK](https://www.azul.com/downloads/zulu/)
- [AdoptOpenJDK](https://adoptopenjdk.net/)
- [Amazon Corretto](https://aws.amazon.com/corretto/)

Teorētiski ir vienalga, kura Open Java versija tiek lietota, jo tās visas ir kompilētas no vienu un tā paša pirmkoda. Kaut gan praksē nelielas atšķirības ir iespējamas. Tāpēc būtu labi, ja darbinieki, kas strādā pie viena projekta izvēlētos vienu un to pašu Open Java relīzi.

LRA grupā esam izvēlējušies lietot [AdoptOpenJDK](https://adoptopenjdk.net/). Pamatā dēļ tā, ka visi kompilācijas kodi ir publicēti [github.com/AdoptOpenJDK](https://github.com/AdoptOpenJDK) un arī instalācijas failus var iegūt no [github.com](https://github.com/AdoptOpenJDK/openjdk8-binaries/releases). Caurspīdīgums vieš uzticamību.


# Instalācija

## Funkcija `install.open.jre`

Open JRE instalāciju var veikt lietotājs. Nav vajadzīga administratora palīdzība. Lai atvieglotu Open JRE instalāciju, ir sagatavota R funkcija `install.open.jre`. Skatīt failu `install_OpenJRE.R`. Funkcija veic sekojošus instalācijas soļus:

- No [github.com](https://github.com/AdoptOpenJDK/openjdk8-binaries/releases) tiek lejupielādēta pēdējā Java 8 JRE portable versija (zip fails) un attiecīgais pārbaudes summas fails (txt fails).
- Tiek vaikta faila pārbaude ar pārbaudes summu (sha256).
- Tiek atarhivēts zip faila saturs.
- Pēc noklusējuma tiek definēts vides mainīgais (*environment variable*) `JAVA_HOME`. Šo var atslēgt ar funkcijas argumentu `set.env.variable = FALSE`.

Pēc noklusējuma faili tiek saglabāti mapē `C:\Users\[user]\Documents\OpenJRE`. Failu saglabāšanas mapi var mainīt ar funkcijas argumentu `path.jre`. Esošie faili tiek pārrakstīti, bet katrai JRE versijai ir sava mape.



## Vides mainīgais `JAVA_HOME`

Liela daļa programmu, piemēram, R un R pakotnes, lietotāja instalētu Java atrod pēc vides mainīgā (*environment variable*) `JAVA_HOME`. Šādā gadījumā vairs nekādas papildus darbības nav nepieciešamas. Atliek pārstartēt R vai RStudio un installētā Java būs pieejama.


## Pārbaude

OpenJRE instalāciju var pārbaudīt no komandrindas (*Command Prompt* vai *Windows PowerShell*) ar sekojošām komandām:

`java -version` parāda instalēto Java versiju.

```
C:\Users\MLiberts>java -version
openjdk version "1.8.0_212"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_212-b03)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.212-b03, mixed mode)
```

`echo %JAVA_HOME%` parāda OpenJRE instalācijas mapi.

```
C:\Users\MLiberts>echo %JAVA_HOME%
C:\Users\MLiberts\Documents\OpenJRE\jdk8u212-b04-jre
```



# JDemetra+

Ne visas programmas izmanto vides mainīgo `JAVA_HOME`, piemēram, JDemetra+. Lai JDemetra+ zinātu, kur meklēt lietotāja instalēto Java, ir jāizveido īsceļš (*shortcut*) uz failu `..\nbdemetra\bin\nbdemetra64.exe`. Īsceļā ir jānorāda papildus arguments `--jdkhome %JAVA_HOME%`. Attiecīgi īsceļam ir jābūt noformētam līdzīgi `..\nbdemetra\bin\nbdemetra64.exe --jdkhome %JAVA_HOME%`. Skatīt attēlu.

![](JDemetra-target.png)