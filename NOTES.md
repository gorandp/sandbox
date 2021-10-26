### vim and ctags

Jump to tag under cursor: `CTRL-]`

Jump back: `CTRL-o` or `CTRL-t`

Show tag in split preview window, don't move cursor: `CTRL-w }`

Close preview window: `CTRL-w z`

Show tag in split preview window, move cursor to preview windows: `CTRL-w ]`

Close preview window: `CTRL-w c`

Go to definition of word under cursor in current function: `gd`

Go to definition in current file: `gD`

### Installing SystemC

```sh
wget http://accellera.org/images/downloads/standards/systemc/systemc-2.3.1a.tar.gz
tar xvfz systemc-2.3.1a.tar.gz
cd systemc-2.3.1a/
mkdir objdir
cd objdir/
../configure --prefix=/usr/local/systemc-2.3.1
make
make check
sudo make install
```

### Enable serial console on Raspberry Pi 3

```sh
echo enable_uart=1 >>/boot/config.txt
```

### Using Android Studio with IBus version < 1.5.11

[IDEA-78860: Keyboard input sometimes is blocked when IBus is active](https://youtrack.jetbrains.com/issue/IDEA-78860)

```sh
export IBUS_ENABLE_SYNC_MODE=1
```

### Orange Pi One

* [Allwinner A31 and Allwinner H3 SoC baremetal application](https://github.com/skristiansson/ar100-info)
* [Open firmware for Allwinner H3 SoC](https://github.com/megous/h3-firmware)
* <https://github.com/megous/h3-ar100-firmware-decompiler>
* <http://www.orangepi.org/orangepione/>
* [Installing Debian on Allwinner](https://wiki.debian.org/InstallingDebianOn/Allwinner)
* [armbian - Linux for ARM development boards](https://www.armbian.com/download/)

### Installing Coq on Ubuntu 14.04

```sh
wget https://coq.inria.fr/distrib/V8.6/files/coq-8.6.tar.gz
tar xzf coq-8.6.tar.gz && cd cd coq-8.6
sudo apt-get install ocaml ocaml-findlib camlp5 liblablgtk2-ocaml-dev liblablgtksourceview2-ocaml-dev
./configure && make world && sudo make install && sudo make clean-ide

coqtop # Coq toplevel
coqc   # Coq compiler
coqide # Coq IDE
```

### NetBSD on Raspberry Pi 2

* [NetBSD/evbarm on Raspberry Pi](http://wiki.netbsd.org/ports/evbarm/raspberry_pi/)
* [How to get NetBSD](http://www.netbsd.org/releases/)
* [The NetBSD Guide](http://netbsd.org/docs/guide/en/netbsd.html)
* [NetBSD Internals](http://netbsd.org/docs/internals/en/index.html)
* [NetBSD Documentation: Kernel](http://netbsd.org/docs/kernel/index.html)
* [NetBSD Documentation: Kernel Programming FAQ](http://netbsd.org/docs/kernel/programming.html)
* [NetBSD Device Driver Writing Guide](http://netbsd.org/docs/kernel/ddwg.html)
* [NetBSD Documentation: Writing a pseudo device](http://netbsd.org/docs/kernel/pseudo/index.html)

Crosscompiling NetBSD on Linux:
```sh
mkdir netbsd-src && cd netbsd-src
wget http://cdn.netbsd.org/pub/NetBSD/NetBSD-7.1/source/sets/gnusrc.tgz
wget http://cdn.netbsd.org/pub/NetBSD/NetBSD-7.1/source/sets/sharesrc.tgz
wget http://cdn.netbsd.org/pub/NetBSD/NetBSD-7.1/source/sets/src.tgz
wget http://cdn.netbsd.org/pub/NetBSD/NetBSD-7.1/source/sets/syssrc.tgz
wget http://cdn.netbsd.org/pub/NetBSD/NetBSD-7.1/source/sets/xsrc.tgz
for f in *.tgz; do tar xfz $f; done
cd /usr/src && ./build.sh -U -u -m evbarm -a earmv7hf release
sudo sh -c 'zcat obj/releasedir/evbarm/binary/gzimg/armv7.img.gz |dd bs=4M of=/dev/mmcblk0'
```

### Zybo

Zynq internal temperature sensor:
```sh
cat /sys/devices/amba.0/f8007100.ps7-xadc/temp
```

### stdint.h, inttypes.h

* [<stdint.h>](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/stdint.h.html): `intptr_t, uintptr_t`
* [<inttypes.h>](http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/inttypes.h.html)

### GitHub syntax highlighting

<https://github.com/github/linguist/blob/master/lib/linguist/languages.yml>

### Apache

* SSI modification time: ```<!--#flastmod virtual="index.shtml" -->```
* SSI remote IP: ```<!--#echo var="REMOTE_ADDR" -->```
* SSI execute CGI: ```<!--#exec cgi="count.cgi" -->```
* htaccess remove CGI handler: ```RemoveHandler .pl```

### RTL-SDR

Determining frequency offset:

```sh
git clone https://github.com/asdil12/kalibrate-rtl.git
cd kalibrate-rtl && ./bootstrap && ./configure && make
```

`src/kal -s GSM900`:

```
Found 1 device(s):
  0:  Generic RTL2832U

Using device 0: Generic RTL2832U
Found Rafael Micro R820T tuner
Exact sample rate is: 270833.002142 Hz
kal: Scanning for GSM-900 base stations.
GSM-900:
	chan: 2 (935.4MHz - 5.411kHz)	power: 83509.38
	chan: 41 (943.2MHz - 5.213kHz)	power: 37202.89
	chan: 57 (946.4MHz - 5.122kHz)	power: 31001.38
	chan: 85 (952.0MHz - 5.204kHz)	power: 40837.42
```

`src/kal -c 2`:

```
Found 1 device(s):
  0:  Generic RTL2832U

Using device 0: Generic RTL2832U
Found Rafael Micro R820T tuner
Exact sample rate is: 270833.002142 Hz
kal: Calculating clock frequency offset.
Using GSM-900 channel 2 (935.4MHz)
average		[min, max]	(range, stddev)
- 4.616kHz		[-4646, -4582]	(64, 16.380167)
overruns: 0
not found: 0
average absolute error: 4.935 ppm
```

#### POCSAG

```sh
git clone https://github.com/EliasOenal/multimon-ng.git
cd multimon-ng && mkdir build && cd build
cmake ..
make

gqrx & # Frequency: 466.075 MHz, Narrow FM, Bandwidth ~ 15 kHz, Stream Audio to UDP

nc -l -u -p 7355 |
    sox -t raw -esigned-integer -b 16 -r 48000 - -esigned-integer -b 16 -r 22050 -t raw - |
    ./multimon-ng -t raw -a POCSAG512 -a POCSAG1200 -a POCSAG2400  -f alpha -
```

#### ADS-B

```sh
sudo apt-get install librtlsdr-dev
git clone https://github.com/antirez/dump1090.git
cd dump1090 && make

./dump1090 --interactive --net

# Web interface (map):  http://localhost:8080
```

#### ACARS

Frequencies: <https://www.acarsd.org/ACARS_frequencies.html>

```sh
git clone https://github.com/TLeconte/acarsdec.git
cd acarsdec && make -f Makefile.rtl

./acarsdec -r 0 131.525 131.550 131.725 p 5 # 131.{525,550,725} MHz, frequency offset 5 ppm
```

ACARS message labels: <http://acarsonline.pbworks.com/w/page/1286730/Message%20Labels>

### LaTeX listings package: syntax highlighting for SPICE netlists

```latex
\usepackage{listings}

% Listings package language definition for SPICE netlists
\lstdefinelanguage{SPICE}
{
	% SPICE commands
	morekeywords={
		% Control statements
		.op,
		.dc,
		.ac,
		.tran,
		.ic,
		.if,
		.nodeset,
		.temp,
		.param,
		.step,
		% Output statements
		.print,
		.plot,
		.option,
		.options,
		.measure,
		.meas,
		% Misc
		.include,
		.end,
		.subckt,
		.ends,
		.global
	},
	alsoletter = {.},
	% Keywords are not case sensitive
	sensitive = false,
	% Comments
	morecomment = [f][\em\color{gray}][0]{*},
	morecomment = [l]{;} % LTSpice extension
}
\lstset{
	language     = SPICE,
	basicstyle   = \footnotesize\ttfamily, % Global font
	captionpos   = b,               % Place caption at the bottom
	tabsize      = 4,               % Tab equals four spaces
	columns      = fixed,           % Same width for all characters
	keepspaces   = true,            % Leave spaces as is
	breaklines   = true,            % Wrap long lines
	numbers      = left,            % Linenumbers on the left
	numberstyle  = \tiny\ttfamily,  % Font for numbers
	commentstyle = \em\color{gray}, % Font for comments
	keywordstyle = \bfseries,       % Font for keywords
}

```

### LaTeX: undertilde package (for matrix notation)

Installation:
```sh
wget http://mirrors.ctan.org/macros/latex/contrib/undertilde/undertilde.dtx
wget http://mirrors.ctan.org/macros/latex/contrib/undertilde/undertilde.ins
latex undertilde.ins # generate "undertilde.sty"
```

Usage:
```latex
\usepackage{undertilde}

...

\[
	\underline{\dot{x}}(t) = \utilde{A}\cdot\underline{x}(t) + \underline{b}\cdot u(t)
\]
```

### LaTeX
#### Size of brackets (parentheses)

Manually adjust size of parentheses: `\big`, `\Big`, `\bigg` and `\Bigg`

#### epstopdf error due to missing "helvetic" package

Error message:
```
$ epstopdf.exe XXX.eps
Error: /invalidfont in /findfont
Operand stack:
   Helvetica-Oblique
Execution stack:
   %interp_exit   .runexec2   --nostringval--   --nostringval--   --nostringval--   2   %stopped_push   --nostringval--   --nostringval--   --nostringval--   false   1   %stopped_push   2015   1   3   %oparray_pop   2014   1   3   %oparray_pop   --nostringval--   1998   1   3   %oparray_pop   1884   1   3   %oparray_pop   --nostringval--   %errorexec_pop   .runexec2   --nostringval--   --nostringval--   --nostringval--   2   %stopped_push   --nostringval--   1967   1   3   %oparray_pop
Dictionary stack:
   --dict:973/1684(ro)(G)--   --dict:0/20(G)--   --dict:123/200(L)--
Current allocation mode is local
Last OS error: No such file or directory
MiKTeX GPL Ghostscript 9.25: Unrecoverable error, exit code 1

Sorry, but "MiKTeX EPS-to-PDF Converter" did not succeed.

The log file hopefully contains the information to get MiKTeX going again:

  C:\Users\matth\AppData\Local\MiKTeX\2.9\miktex\log\epstopdf.log
```

Solution: install the "helvetic" package using the MiKTeX Console

#### Acronyms using the "glossary" package

Package:
```latex
\usepackage[acronym]{glossaries}
\makeglossaries
```

Defining new acronyms:
```latex
\newacronym{fft}{FFT}{Fast Fourier Transform}
```

Using the full version + abbreviation:
```latex
\acrfull{fft}
```

Short version:
```latex
\acrshort{fft}
```

Full version:
```latex
\acrlong{fft}
```

Printing the list of acronyms in the document:
```latex
\printglossary[type=\acronymtype]
```

Compiling the glossary in TeXworks:
* Install Strawberry Perl for Windows: <http://strawberryperl.com/>
* TeXworks Preferences -> Processing Tools -> makeglosseries
* Program: "makeglossaries", Arguments: "$basename"

### SSH

Unable to negotiate with 10.0.0.1 port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1,diffie-hellman-group14-sha1

`.ssh/config`:
```
KeepAlive yes
ServerAliveInterval 60

Host 10.0.0.1 10.1
    KexAlgorithms +diffie-hellman-group1-sha1
```

### Windows 10: delete all partitions on USB flash drive including EFI partitions
```
diskpart
list disk
select disk XX
clean
```
