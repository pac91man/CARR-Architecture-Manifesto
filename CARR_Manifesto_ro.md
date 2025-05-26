# Manifestul Arhitecturii CARR
## *Compartmentalized Architecture for Resources and Repositories*

Copyright (c) 2025 Munteanu Vasile-Alexandru. 
Acest Manifest este licențiat sub Creative Commons Attribution 4.0 International License (CC BY 4.0). 
Pentru a vizualiza o copie a acestei licențe, vizitați: https://creativecommons.org/licenses/by/4.0/


### Rezumat Executiv
CARR reprezintă o abordare alternativă în organizarea sistemelor Linux, bazată pe principii de modularitate, izolare și intuitivitate. Prin compartimentarea resurselor sistemului, CARR oferă o structură care poate reduce conflictele între pachete, îmbunătăți securitatea și facilita rularea simultană a mai multor ecosisteme Linux.

## Introducere

Arhitectura CARR (Compartmentalized Architecture for Resources and Repositories) propune o abordare alternativă în modul de gestionare a resurselor și pachetelor într-un sistem de operare Linux. Inspirată de concepte precum o casă în stil vagon, un șantier organizat și o macara rulantă care mută resursele acolo unde este nevoie, CARR oferă o structură clară pentru sistemele Linux.

## Comparație cu organizarea actuală

Sistemele Linux actuale utilizează în general standardul Filesystem Hierarchy Standard (FHS), care are următoarele caracteristici:

* Fișierele unei aplicații sunt distribuite în multiple locații (`/usr/bin`, `/etc`, `/var`, `/opt`)
* Bibliotecile sunt partajate între multiple aplicații în locații comune
* Configurațiile sunt centralizate în directoare precum `/etc`
* Actualizarea unei biblioteci afectează toate aplicațiile care o utilizează

CARR propune o abordare diferită bazată pe izolare și modularitate, care poate oferi anumite avantaje în scenarii specifice.

## Beneficii concrete pentru utilizatori

## Caracteristici principale ale CARR

### Pentru utilizatorii obișnuiți:
* Posibilitatea de a rula aplicații din diferite ecosisteme Linux simultan
* Mecanism de rollback pentru revenirea la configurații funcționale anterioare
* Organizare intuitivă a datelor personale
* Izolare care poate reduce impactul problemelor unei aplicații asupra întregului sistem

### Pentru profesioniști IT:
* Securitate îmbunătățită prin separarea componentelor sistemului
* Medii izolate pentru testarea software-ului
* Management unificat pentru diferite formate de pachete
* Structură clară pentru deployment-ul aplicațiilor

## Principiile Fundamentale ale CARR

### Izolare Totală (Modularitate)

Sistemul CARR se bazează pe module independente (foldere, containere), fiecare cu un rol bine definit, având capacitatea de a funcționa izolat pentru a îmbunătăți securitatea și performanța.

### Organizare Clară și Logică

Structura sistemului este inspirată de o "casă în stil vagon", unde fiecare folder principal are o funcție clară:

```
/CARR
├── /core                   # Componenta esențială a sistemului de operare
│   ├── /kernel             # Nucleul propriu-zis (Linux, microkernel etc.)
│   ├── /bootloader         # Bootloader și fișierele de boot
│   ├── /drivers            # Drivere pentru hardware
│   ├── /core-libs          # Librării critice, partajate, esențiale pentru funcționarea de bază a sistemului (ex: libc, libm, libdl etc.)
│   ├── /init               # Init system: systemd, init.d, runit etc.
│   ├── /config             # Configurări sistemice (fstab, grub.cfg, hostname, network etc.)
│
├── /apps                # Aplicații organizate per format de pachet
│   ├── /deb             # Aplicații din pachete .deb
│   ├── /rpm             # Aplicații din pachete .rpm
│   ├── /zypper          # Aplicații openSUSE (.rpm + metadata)
│   ├── /eopkg           # Aplicații Solus
│   ├── /appimage        # Aplicații portabile AppImage
│   ├── /flatpak         # Aplicații sandboxate Flatpak
│
├── /repos               # Surse de aplicații (repozitorii) per format
│
├── /games               # Jocuri izolate, organizate după tehnologie (vulkan, openGL, proprietare nVidia/AMD)
│
├── /user                # Datele personale ale utilizatorului
│   ├── /documents       # Documente personale
│   ├── /pictures        # Fotografii
│   ├── /music           # Muzică
│   ├── /videos          # Clipuri video
│   ├── /personal-data   # Date sensibile (opțional criptate)
│   ├── /share           # Date partajabile (cu alte containere, rețea etc.)
│
├── /data                # Cache aplicații, baze de date locale, sesiuni
│
├── /rollback               # Stocare temporară pentru rollback de sistem, creată în timpul update-urilor/upgrade-urilor majore ale sistemului de operare.
│   * Versiuni anterioare ale fișierelor critice
│   * Util pentru revenire după update/upgrade eșuat
│
├── /tmp                 # Fișiere temporare (șterse la boot)
├── /logs                # Jurnale de sistem (syslog, journalctl etc.)
├── /security            # Chei GPG, reguli firewall, SELinux/AppArmor
├── /virtualization      # Mașini virtuale și containere (QEMU, Docker, etc.)
├── /docs                # Manuale, tutoriale, README-uri, PDF-uri tehnice
```

### Compatibilitate cu Tehnologii Moderne

CARR este nativ compatibil cu aplicații moderne și tehnologii de virtualizare, incluzând:
* Containere: Docker, Kubernetes, LXC, OpenVZ, systemd-nspawn, etc.
* Aplicații portabile și sandboxate: Flatpak, AppImage.
* Virtualizare: Tehnologii care permit rularea aplicațiilor în medii controlate și izolate.

### Securitate și Stabilitate

Prin separarea clară a componentelor și utilizarea unui mecanism de izolare și permisiuni stricte, CARR reduce suprafața de atac și riscul de exploatări. În plus, rollback-ul nativ permite utilizatorilor să restaureze sistemul în caz de upgrade-uri eșuate sau să revină la versiuni anterioare ale aplicațiilor când este necesar.

### Actualizare și Întreținere Ușoară

Sistemul permite actualizări și întrețineri simple, în care pachetele pot fi actualizate sau înlocuite fără a afecta alte componente critice datorită compartimentării folderelor.

## Mecanismul de Legătură: Cum funcționează CARR

### FolderBridge - "Creierul" Sistemului

FolderBridge este mecanismul central de legătură controlată între spațiile compartimentate ale sistemului CARR, permițând transferul de date sau accesul temporar între foldere izolate, cu politici clare de securitate și filtrare. 

Funcțiile principale:
* **Controlul de bază al sistemului**: Gestionarea pornirii, opririi și repornirii sistemului
* **Instalarea de aplicații**: Un manager de pachete unificat care înregistrează aplicațiile în compartimentul lor propriu
* **Update/upgrade inteligent**: Actualizări separate pentru sistemul de operare și aplicații, cu detectarea conflictelor între versiuni
* **Gestionarea resurselor**: Monitorizează și optimizează utilizarea CPU, RAM și stocare
* **Compatibilitate universală**: Integrare cu systemd/init și alte sisteme standard Linux

### System_Control - "Brațele" Sistemului

Modulele system_control sunt extensiile specializate ale FolderBridge, fiecare având o funcție specifică în gestionarea eficientă a resurselor. Aceste module acționează coordonat pentru a asigura funcționarea armonioasă a întregului sistem.

### Scripetele - "Cablurile" Sistemului

Scripturile externe (scripetele) se ocupă de sarcini speciale, aducând flexibilitate și automatizare în gestionarea sistemului, similar cu scripetele unei macarale care facilitează mișcarea resurselor acolo unde este nevoie.

## Scenarii ilustrative

### Scenariul 1: Utilizare multi-distribuție
Un utilizator care are nevoie de aplicații specifice din diferite distribuții poate:

1. Instala aplicații .deb în `/apps/deb`
2. Instala aplicații .rpm în `/apps/rpm`
3. Instala aplicații Arch în `/apps/pacman`
4. Rula aceste aplicații fără a se preocupa de posibilele conflicte între bibliotecile dependente

### Scenariul 2: Rollback sistem
În cazul unei actualizări problematice:

1. Sistemul păstrează o copie a fișierelor critice în `/rollback`
2. La întâlnirea unei probleme, utilizatorul poate restaura versiunea funcțională anterior
3. Procesul poate fi automatizat pentru situații specifice

### Scenariul 3: Izolarea aplicațiilor
Pentru aplicații cu cerințe de securitate diferite:

1. Aplicațiile sunt instalate în compartimente separate
2. Datele sensibile pot fi stocate izolat în `/user/personal-data`
3. Accesul între compartimente este controlat prin mecanisme de securitate

## Considerații pentru implementare

Tranziția către CARR ar necesita o abordare în două etape:

### Etapa de tranziție:
* Dezvoltarea unor adaptoare și instrumente care să permită funcționarea aplicațiilor existente (create pentru FHS) în arhitectura CARR
* Crearea unor mecanisme de compatibilitate care să simuleze structura FHS din perspectiva aplicațiilor
* Implementarea unui sistem de "punți" între compartimente pentru comunicarea controlată
* Această etapă va permite testarea și validarea beneficiilor arhitecturii CARR cu software existent create pentru FHS.

### Etapa de adopție nativă:
* Adaptarea aplicațiilor și programelor din ecosistemul Linux pentru a funcționa nativ cu CARR
* Dezvoltarea de instrumente și biblioteci optimizate specific pentru această arhitectură
* Rescrierea componentelor critice pentru a exploata la maximum avantajele structurii compartimentate

Ca în analogia roților de la căruțe versus roți moderne pentru automobile, aplicațiile care rulează în modul de compatibilitate nu vor putea exploata complet beneficiile CARR. Însă odată rescrise pentru arhitectura nativă, aplicațiile vor putea beneficia de toate avantajele structurii moderne și optimizate pe care CARR o oferă.

## Concluzie

Arhitectura CARR oferă o perspectivă alternativă pentru organizarea sistemelor Linux, cu accent pe modularitate și izolare. Utilizând principii de compartimentare clară, această arhitectură ar putea rezolva unele provocări actuale și ar putea oferi noi oportunități pentru evoluția ecosistemului Linux.

Prin prezentarea acestei viziuni, invităm comunitatea Linux să exploreze conceptele CARR și să evalueze potențialele beneficii în contextul nevoilor actuale și viitoare ale utilizatorilor și dezvoltatorilor.
