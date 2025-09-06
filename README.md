# Hühnerchat Parser

## Hintergrund

Im Quartiergarten halten wir Hühner. Jeden Morgen und jeden Abend geht jemand vorbei und schaut nach den Hühnern. Zum Füttern, Saubermachen, Eier Einsammeln.

In einem Signal Chat rapportieren wir, dass der Dienst geamcht wurde, wie es den Hühnern geht, wie viele Eier gelegt wurden und wie viele Eier wir an andere Mitglieder des Gartenvereins weitergegeben haben.

Diese Info hätten wir gern in strukturierter Form, also einer Tabelle. Das ist das Ziel des Skripts. Mit Regex werden die Anzalh Eier extrahiert.

## Das Skript

Das Skript zählt immer die Wörter "Ei" und "Eier" wenn sie in einem bestimmten Kontext vorkommen:

* `zahl` `Ei/er` `an` oder `für` wird als abgegebenes Ei gezählt 
* `zahl` `Ei/er` `mitgenommen` wird gar nicht gezählt

* `zahl` `Ei/er` wird als gelegte Eier gezählt.

`zahl` kann eine Nummer oder das ausgeschriebene wort sein.

## Limitierungen

Das funktioniert natürlich nur wenn man nicht sonst noch über Eier schreibt. Eine Nachricht wie "Fritz wollte drei Eier aber ich habe ihm nur zwei gegeben" würde drei gelegte Eier ergeben anstatt zwei abgegebene. Man sollte die Nachrichten also so schreiben, wie es oben erklärt ist.

