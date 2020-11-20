---
title: "GUI di Linux Seringkas Mungkin dengan LXQt, SDDM, Openbox"
date: 2020-11-11T23:45:00+08:00
publishDate: 2020-11-11T23:45:00+08:00
image: "/images/2020/gui_linux_seringkas_mungkin_lxqt_sddm_openbox.webp"
url: "/content/gui-linux-seringkas-mungkin-dengan-lxqt-sddm-openbox"
tags: ["Linux", "X11"]
draft: false

---

{{< figure src="/images/2020/gui_linux_seringkas_mungkin_lxqt_sddm_openbox.webp" title="GUI di Linux Seringkas Mungkin dengan LXQt, SDDM, Openbox" alt="GUI di Linux Seringkas Mungkin dengan LXQt, SDDM, Openbox" position="center">}}

## Sekilas Tentang Aplikasi yang Digunakan

LXQt [^1] sebagai Desktop Environment.

Sebetulnya sering pakai Desktop Environment LXDE (pendahulu dari LXQt) di Raspberry Pi. Kemudian menggunakan Lubuntu yang ternyata belakangan beralih dari LXDE ke LXQt [^2].

SDDM [^3] sebagai Desktop Manager.

Sering menggunakan SDDM ini karena komputer sehari-hari menggunakan Kubuntu [^4].

OpenBox [^5] sebagai Window Manager.

Awalnya tidak ingat kalo Desktop Manager masih butuh Window Manager untuk mengatur layout, jadi yang diinstal hanya sampai SDDM, ternyata saat login SDDM ditanya dan diminta memilih aplikasi Window Manager. Baru kemudian teringat Openbox.

## Instalasi Linux

Untuk Linux basisnya menggunakan Debian, karena installer yang dimiliki cukup kecil ukurannya, versi net install [^6], sekitar 370 MB.

Prosesnya dilakukan di VirtualBox agar memudahkan mengulang-ulang proses jika terjadi kesalahan.

Hasil instalasi minimalnya (tanpa memilih paket apa-apa, hanya basisnya) jika diekspor dari VirtualBox sekitar 677 MB.

## Instalasi LXQt

Versi yang lebih ringkas dari paket `lxqt` adalah paket `lxqt-core`.

Jika kita coba instalasi dengan `sudo apt install lqxt-core`, kita akan disajikan pilihan berikut:

> 591 newly installed.
> Need to get 248 MB of archives.
> After this operation, 1,060 MB of additional disk space will be used.

Perhatikan bahwa ukurannya akan bertambah 1 GB hanya melengkapi instalasi `lxqt-core`, lalu apa pilihan lainnya? Yakni dengan instalasi tanpa menyertakan paket rekomendasinya, dengan `sudo apt --no-install-recommends install lqxt-core` hasilnya:

> 162 newly installed.
> Need to get 54.2 MB of archives.
> After this operation, 337 MB of additional disk space will be used.

Penambahan ukuran dari penyimpanan hanya 337 MB.

## Instalasi SDDM

Berdasar dari pengalaman menggunakan argumen `--no-install-recommends` tersebut, saat akan instalasi SDDM, tentu akan mencoba meringkas instalasinya, tapi ternyata hal ini menyebabkan GUI tidak langsung muncul saat startup. (perbedaan paketnya aku tuliskan di bagian paling bawah artikel), padahal perbedaannya hanya sekitar 40 MB. Jadi kalo tidak mau repot, cukup instalasi dengan `sudo apt install sddm`.

Kalo memilih instalasi dengan `sudo apt --no-install-recommends install sddm`, menghasilkan:

> 22 newly installed. 
> Need to get 12.8 MB of archives. 
> After this operation, 29.7 MB of additional disk space will be used.

Setelahnya kita masih perlu menambahkan sejumlah paket lagi, dengan perintah: `sudo apt --no-install-recommends install desktop-base plymouth-label xauth xfonts-base`, dimana ini akan menambahkan:

> 10 newly installed. 
> Need to get 10.2 MB of archives. 
> After this operation, 21.0 MB of additional disk space will be used.

Dengan instalasi SDDM dan paket tambahan ini, jika kita melakukan reboot, menu `grub` sudah berubah jadi grafis, juga kita akan masuk ke mode grafis (GUI).

## Instalasi Openbox

Instalasi Openbox akan tetap dilakukan dengan argument `--no-install-recommends` dengan selisihnya sekitar 120 MB.

Menggunakan perintah `sudo apt install openbox` akan menghasilkan:

> 70 newly installed. 
> Need to get 40.5 MB of archives. 
> After this operation, 136 MB of additional disk space will be used.

Dengan menggunakan perintah `sudo apt --no-install-recommends openbox` akan menghasilkan:

> 23 newly installed. 
> Need to get 4,623 kB of archives. 
> After this operation, 16.3 MB of additional disk space will be used.

## Reboot

Setelah `lxqt-core`, `sddm`, `openbox` terinstall, kita reboot dengan perintah: `sudo systemctl reboot`.

Kita akan masuk ke mode grafis (GUI), dimana akan tampil user kita, menanyakan password untuk login.

## Fitur dari GUI

Kita dapat sejumlah fitur pada GUI yang sangat ringkas ini sudah sesuai dengan yang penulis butuhkan:

* File Manager: PCManFM-Qt File Manager
* Terminal untuk shell: QTerminal

Salah satu yang belum berhasil saya pecahkan adalah mengaktifkan fitur "Leave" misalnya untuk shutdown, dimana ditangani oleh paket `lxqt-leave` yang masih kena batasan akses.

> Debug: systemd: "CanPowerOff" = "Permission denied"

Tapi ya tidak apa-apa, proses shutdown bisa saya gunakan terminal saja: `sudo systemctl poweroff`.

## Ukuran Final Linux "Ringkas" dengan GUI LXQt, SDDM, Openbox

Dengan proses instalasi mininum di atas, jika diekspor kembali pada VirtualBox akan mendapatkan **ukuran total 720 MB**. Sehingga untuk eksplorasi dengan mode grafis di Linux sekarang cukup mengopi file sebesar itu saja.

#### Catatan: Perbedaan Instalasi SDDM

Menggunakan perintah `sudo apt install sddm` hasilnya:

> The following additional packages will be installed: 
> desktop-base fonts-quicksand haveged libcairo2 libcroco3 libdatrie1 libepoxy0 libfribidi0 libgdk-pixbuf2.0-0 libgdk-pixbuf2.0-bin libgdk-pixbuf2.0-common libhavege1 libjbig0 libpango-1.0-0 
> libpangocairo-1.0-0 libpangoft2-1.0-0 libpixman-1-0 libplymouth4 libqt5qml5 libqt5quick5 librsvg2-2 librsvg2-common libthai-data libthai0 libtiff5 libunwind8 libwebp6 libxatracker2 libxfont2 libxvmc1 
> plymouth plymouth-label qml-module-qtquick2 sddm-theme-debian-maui xauth xfonts-base xfonts-encodings xfonts-utils xserver-common xserver-xorg xserver-xorg-core xserver-xorg-input-all 
> xserver-xorg-input-libinput xserver-xorg-input-wacom xserver-xorg-legacy xserver-xorg-video-all xserver-xorg-video-amdgpu xserver-xorg-video-ati xserver-xorg-video-fbdev xserver-xorg-video-intel 
> xserver-xorg-video-nouveau xserver-xorg-video-qxl xserver-xorg-video-radeon xserver-xorg-video-vesa xserver-xorg-video-vmware 
>
> Suggested packages: 
> gnome | kde-standard | xfce4 | wmaker qt5-qmltooling-plugins librsvg2-bin plymouth-themes libpam-kwallet5 qtvirtualkeyboard-plugin xfonts-100dpi | xfonts-75dpi xfonts-scalable xinput firmware-amd-graphics 
> xserver-xorg-video-r128 xserver-xorg-video-mach64 firmware-misc-nonfree 
>
> The following NEW packages will be installed: 
> desktop-base fonts-quicksand haveged libcairo2 libcroco3 libdatrie1 libepoxy0 libfribidi0 libgdk-pixbuf2.0-0 libgdk-pixbuf2.0-bin libgdk-pixbuf2.0-common libhavege1 libjbig0 libpango-1.0-0 
> libpangocairo-1.0-0 libpangoft2-1.0-0 libpixman-1-0 libplymouth4 libqt5qml5 libqt5quick5 librsvg2-2 librsvg2-common libthai-data libthai0 libtiff5 libunwind8 libwebp6 libxatracker2 libxfont2 libxvmc1 
> plymouth plymouth-label qml-module-qtquick2 sddm sddm-theme-debian-maui xauth xfonts-base xfonts-encodings xfonts-utils xserver-common xserver-xorg xserver-xorg-core xserver-xorg-input-all 
> xserver-xorg-input-libinput xserver-xorg-input-wacom xserver-xorg-legacy xserver-xorg-video-all xserver-xorg-video-amdgpu xserver-xorg-video-ati xserver-xorg-video-fbdev xserver-xorg-video-intel 
> xserver-xorg-video-nouveau xserver-xorg-video-qxl xserver-xorg-video-radeon xserver-xorg-video-vesa xserver-xorg-video-vmware 
>
> 0 upgraded, 56 newly installed, 0 to remove and 0 not upgraded. 
> Need to get 31.3 MB of archives. 
> After this operation, 71.3 MB of additional disk space will be used.

Menggunakan perintah `sudo apt --no-install-recommends install sddm` hasilnya:

> The following additional packages will be installed: 
> libepoxy0 libpixman-1-0 libqt5qml5 libqt5quick5 libunwind8 libxatracker2 libxfont2 qml-module-qtquick2 xserver-common xserver-xorg xserver-xorg-core xserver-xorg-input-all xserver-xorg-input-libinput 
> xserver-xorg-video-all xserver-xorg-video-amdgpu xserver-xorg-video-ati xserver-xorg-video-fbdev xserver-xorg-video-nouveau xserver-xorg-video-radeon xserver-xorg-video-vesa xserver-xorg-video-vmware 
>
> Suggested packages: 
> qt5-qmltooling-plugins libpam-kwallet5 qtvirtualkeyboard-plugin xfonts-100dpi | xfonts-75dpi xfonts-scalable firmware-amd-graphics xserver-xorg-video-r128 xserver-xorg-video-mach64 firmware-misc-nonfree 
> Recommended packages: 
> haveged sddm-theme-debian-maui | sddm-theme xfonts-base xauth xserver-xorg-legacy xserver-xorg-input-wacom xserver-xorg-video-intel xserver-xorg-video-qxl 
>
> The following NEW packages will be installed: 
> libepoxy0 libpixman-1-0 libqt5qml5 libqt5quick5 libunwind8 libxatracker2 libxfont2 qml-module-qtquick2 sddm xserver-common xserver-xorg xserver-xorg-core xserver-xorg-input-all xserver-xorg-input-libinput 
> xserver-xorg-video-all xserver-xorg-video-amdgpu xserver-xorg-video-ati xserver-xorg-video-fbdev xserver-xorg-video-nouveau xserver-xorg-video-radeon xserver-xorg-video-vesa xserver-xorg-video-vmware 
>
> 0 upgraded, 22 newly installed, 0 to remove and 0 not upgraded. 
> Need to get 12.8 MB of archives. 
> After this operation, 29.7 MB of additional disk space will be used.

Foto oleh [SpaceX](https://unsplash.com/@spacex?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) di Unsplash.

[^1]: https://lxqt-project.org/
[^2]: https://lubuntu.me/tag/lxqt/
[^3]: https://github.com/sddm/sddm
[^4]: https://oo.or.id/content/add-new-resolution-xorg-kubuntu/
[^5]: http://openbox.org/
[^6]: https://www.debian.org/CD/netinst/

