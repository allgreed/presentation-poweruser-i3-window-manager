# Poweruser: i3 window manager
## by Olgierd &#34;Allgreed&#34; Kasprowicz

Note:
node bower_components/reveal.js/plugin/notes-server/
Na ekranie nr 8



# <span style="color: #b58900">Disclaimer</span>
<!-- .slide: data-background-color="black" -->


## <span style="color: #b58900">Cykl PowerUser</span>
<!-- .slide: data-background-color="black" -->


## <span style="color: #b58900">Idea OpenSource</span>
<!-- .slide: data-background-color="black" -->


## <span style="color: #b58900">Hacker:Space</span>
<!-- .slide: data-background-color="black" -->



# Podstawy linuxowych DE


![](./img/linux_basic_components_of_a_gui.png)

Note:
- To będzie mega ogólnie
- Kernel ma tylko teletype (tty)
- X dostarcza podstawę/framework do wyświetlnaia stuffu
- X jest w architekturze klient-serwer
- DE to wm + dodatki
- wm zarządza wyświetlaniem okien


## Demo time!

Note:
Na maszynie wirtualnej, odpalam TTY (ctrl+F1), przechodzę na pulpit, edytuję vimem keton.txt, odpalam "cal", wyszukuję lynxem na google.pl "keton", próbuję odpalić firefoxa, załączam X ```startx```, odpalam cosie, załączam Compiza ```compiz```



# Rodzaje menadżerów okien


## Stacking
![](./img/window_malfunction.jpg)


## Floating
![](./img/apple_desktop.jpg)

Note:
To samo, co stacking, tylko okna są buforowane, więc nie ma takich jaj ;)


## Tiling
![](./img/tiling.png)


## Tiling c.d.

### Manual
np. i3

### Dynamic
np. awesome

Note:
Man: User wskazuje położenie okna
Dyn: Okna wskakują "same", predefiniowane layouty


## Hybrid
![](./img/hybrid.jpg)

Note:
Jak coś jest do wszystkiego to jest do niczego, w praktyce z floatowania się nie korzysta



# Czemu i3?


![](img/terminal_friend.png)

Note:
Trzeba się zaprzyjaźnić z terminalem


alopex, Awesome, bspwm, catwm, dswm, dwm, echinus, euclid-wm, FrankenWM, herbstluftwm, <span style="color: #b58900">i3</span>, Ion3, monsterwm, Musca, Notion, qtile, Ratpoison, Snapwm, Spectrwm, Stumpwm, sway, subtle, Wingo, WMFS, wmii, xmonad

Note:
Któryś trzeba wybrać


![](./img/i3-pop.png)
![](./img/qtile-pop.png)
![](./img/catwm-pop.png)

Note:
Popularność


```ps -eo rss,pid,euser,args:100 --sort %mem | grep -i i3 | awk '{printf $1/1024; $1=""; print $1 }' | python -c "import sys; print(sum(float(l) for l in sys.stdin))"
```

|       wm       |   RAM   |
|:--------------:|:-------:|
|       i3       | ~ 25 MB |
| compiz (Unity) | ~ 85 MB |
|    cinnamon    | ~ 85 MB |

Note:
(Skrypt do zmierzenia ilości zużywanego RAMu przez i3). DE zużywają do 500mb RAMu. Nie ma to znaczenia dla RAMu, ale ma dla baterii w laptopach.


![](./img/open_source.png)

Note:
Otwarta licencja



# Demo time!

Note:
U mnie:
- Przeskakiwanie po workspace'ach
- Okna w 1 workspacie
- Bar



# Głęboki nur
## "Deep dive"


## <span style="color: #b58900">Disclaimer</span>
<!-- .slide: data-background-color="black" -->


## Instalacja i3

``` sh
    sudo apt install i3
```
![](img/logout.png)


![](img/choose_session.png)

![](img/initconf.png)



# Podstawy konfiguracji i3


## <span style="color: #b58900">Disclaimer</span>
<!-- .slide: data-background-color="black" -->

Note:
- pełna dowolność, nie ma jedynej słusznej drogi
- wiem, że nie ogarniacie jeszcze
- wytłumaczę ideę, a potem jak się poruszać


![](img/mod_key.png)

```
    vim .config/i3/config
```


![](img/basic_config.png)

Note:
Set -> to co wybraliśmy wcześniej przez GUI


## Exec

```
    exec[_always] [--no-startup-id] <command>
```

- [_always] - przy każdym reloadzie configu
- [--no-startup-id] - zablokuj notyfikacje startowe

Note:
Bezpośrednio do shella (korzystając z $PATH), więc nie ma aliasów z .bashrc
Niektóre aplikacje nie obsugują poprawnie notyfikacji startowych i wtedy kursor zmienia się w zegar na minutę
_always jest świetny np. do ustawiania tapety 


## Bindsym

```
    bindsym [--release] [<Modifiers>+]<keysym> command
```

- [--release] -> przydatne przy screenshotach kawałka ekranu
- Modifiers + keysym -> np. Shift+x

Note:
Bindsym to "bind symbol", command jest komendą i3, a nie shella


## Set

```
    set $<name> <value>
```

np.

```
    set $keton "relatywnie długi tekst"
```


## Przykład

```
    set $imgedit "gimp --no-splash"

    bindsym Shift+i exec $imgedit
```



# Defaultowy config

<!-- Tutaj robić rzeczy -->



# Żegnaj Unity


## Compiz
```sudo apt autoremove --purge compiz compiz-gnome compiz-plugins-default libcompizconfig0```


## Reszta Unity
```sudo apt autoremove --purge unity unity-common unity-services unity-lens-* unity-scope-* libunity-core-6* libunity-misc4 appmenu-gtk appmenu-gtk3 appmenu-qt* overlay-scrollbar* activity-log-manager-control-center firefox-globalmenu thunderbird-globalmenu```


## Zablokowanie desktopu
```gsettings set org.gnome.desktop.background show-desktop-icons false```

Note:
Likwiduje bug podczas odpalania nautiliusa


## Przestawienie lightdm na i3

```sh
    echo "exec i3" >> ~/.xinitrc
```

```sh
    sudo vim /etc/lightdm/lightdm.conf
```

```user-session=i3```

Note:
Jak jest problem z lightdm.conf (nie ma go, nie ma wpisu user-session) to można olać. Testowane na wirtualce.


# Jak znaleźć klawisze


## scancodes

![](img/scancodes.gif)

Note: 
kernel dostaje od klawiatury


## keycodes

```xev | awk -F'[ )]+' '/^KeyPress/ { a[NR+2] } NR in a { printf "%-3s %s\n", $5, $8 }'```

![](img/keycodes.png)

Note: 
kernel dostaje od klawiatury


## keysyms

```xev | sed -n 1~2p -u | awk '/keysym/ {print substr($7, 1, length($7)-2)}'```

![](img/keysyms.png)

Note: 
kernel dostaje od klawiatury
<!-- Podstawowa konfiguracja i3 status -->
<!-- Jak ustawić tapetę -->
<!-- Font awesome -->
<!-- - Bonus: jak połącyć się z wifi z konsoli ;) -->
<!-- Add notes to disclaimers -> all -->


Questions? :)

<!-- Ciekawe linki -->
- [1st demo inspiration](https://www.youtube.com/watch?v=4J5snV2wjtw)
- [Window managers comparison](https://wiki.archlinux.org/index.php/Comparison_of_tiling_window_managers)
- [Window managers memory consumption](https://askubuntu.com/questions/181370/how-heavy-on-resources-is-cinnamon-desktop-environment)
- [Scancodes, keycodes, keysyms theorys](http://www.tldp.org/HOWTO/BackspaceDelete/actions.html)
- [More on keycodes and keysyms](https://wiki.archlinux.org/index.php/extra_keyboard_keys)

Materiały:
http://walther.io/how-to-replace-unity-with-i3-window-manager-on-ubuntu-1204/
https://i3wm.org/screenshots/
https://fedoramagazine.org/getting-started-i3-window-manager/
https://i3wm.org/docs/userguide.html
