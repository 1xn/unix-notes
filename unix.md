# UNIX MEGA TIPSHEET
By dan@1xn.org. 

---

My little collection of useful terminal shortcuts and tips.


### Github 

Initialize new github repository

`git init`

Clone repository

`git clone /path/to/repository`

Clone repository (remote server)

`git clone username@host:/path/to/repository`

Add files to repository

`git add <filename>`

All files

`git add .`

Commit changes to repository

`git commit -m "Commit message"`

Push changes to repo

`git push origin master`

Connect to remote repo
`git remote add origin <server>`

Create new branch and switch to it
`git checkout -b feature_x`

Return to master branch
`git checkout master`

Delete new branch

`git branch -d feature_x`

Una rama nueva no estará disponible para los demás a menos que subas (push) la rama a tu repositorio remoto
git push origin <branch>


Check which remote repos you have connected with your local repos:

`git remote -v`

Remove the remote repo (say, origin)

`git remote rm origin`

### Timetracking
Using `timetrap` (https://github.com/samg/timetrap)


**Show timesheets**

`t list`

**View current timesheet**

`t display`

**Make a new timesheet**

`t sheet SHEETNAME`

**Check in**

`t in`

**Check out of timesheet**

`t out`

**check in 5 minutes ago with a note**

`t in --at '5 minutes ago' doing some stuff`

### Miscellaneous

In Unix `|` is a pipe and `>` is a output direct

Download Youtube video
`youtube-dl videourl`

Dictionary definitions Ex. Linux
`curl dict://dict.org/d:linux`

Copy 2 folders
`ditto -V /old/work/ /new/work/`

Transform XML
`xsltproc xsltfile input output`

Calculate SHA-256 checksum:
`shasum -a 256 ~/Downloads/file.iso`

Calculate Pi 3000
`echo "scale=3000; 4*a(1)" | bc -l`

Starwars ASCII demo
`telnet towel.blinkenlights.nl`

Start quick server in any folder
`python -m SimpleHTTPServer 8000`

Repeat last command with sudo 
`sudo !!`

Prevent going to sleep for 10 min
`caffeinate -u -t 600`

Make a 1Gb test file
`mkfile 1g test.abc`

Show System log last messages
`tail -f /var/log/system.log`

Get internal IP address
`ipconfig getifaddr en0`

Get external IP address
`curl ipecho.net/plain; echo`

Ping address 
`ping -c 10 www.apple.com`

Display WhoIs info
`whois 1xn.org`

Get useful tips about command
`curl cheat.sh/command`

Quick web server
`python -m SimpleHTTPServer 7777`

Convert Clipboard to Plain Text

`pbpaste | textutil -convert txt -stdin -stdout -encoding 30 | pbcopy`

### Batch Image processing

You can do loops inside bash easily (requires imagemagick)

```bash
for i in *.jpg; do
mogrify -quality 80% -resize 50% -units pixelsperinch -density 72x72 -verbose *.jpg
done
```

For removing white backgrounds from images, following is a bash script using imagemagick :


```bash
#!/bin/bash

# pass the image path, image name and threshold(used as a fuzz factor) to the bash script
IMGPATH=$1
IMGNAME=$2
THRESHOLD=$3

# start real
convert ${IMGPATH}${IMGNAME} \( +clone -fx 'p{0,0}' \)  -compose Difference  -composite   -modulate 100,0  +matte  ${IMGPATH}${IMGNAME}_difference.png

# remove the black, replace with transparency
convert ${IMGPATH}${IMGNAME}_difference.png -bordercolor white -border 1x1 -matte -fill none -fuzz 7% -draw 'matte 1,1 floodfill' -shave 1x1 ${IMGPATH}${IMGNAME}_removed_black.png
composite  -compose Dst_Over -tile pattern:checkerboard ${IMGPATH}${IMGNAME}_removed_black.png ${IMGPATH}${IMGNAME}_removed_black_check.png

# create the matte 
convert ${IMGPATH}${IMGNAME}_removed_black.png -channel matte -separate  +matte ${IMGPATH}${IMGNAME}_matte.png

# negate the colors
convert ${IMGPATH}${IMGNAME}_matte.png -negate -blur 0x1 ${IMGPATH}${IMGNAME}_matte-negated.png

# eroding matte(to remove remaining white border pixels from clipped foreground)
convert ${IMGPATH}${IMGNAME}_matte.png -morphology Erode Diamond ${IMGPATH}${IMGNAME}_erode_matte.png

# you are going for: white interior, black exterior
composite -compose CopyOpacity ${IMGPATH}${IMGNAME}_erode_matte.png ${IMGPATH}${IMGNAME} ${IMGPATH}${IMGNAME}_finished.png

#remove white border pixels
convert ${IMGPATH}${IMGNAME}_finished.png -bordercolor white -border 1x1 -matte -fill none -fuzz  ${THRESHOLD}% -draw 'matte 1,1 floodfill' -shave 1x1 ${IMGPATH}${IMGNAME}_final.png

#deleting extra files
rm ${IMGPATH}${IMGNAME}_difference.png
rm ${IMGPATH}${IMGNAME}_removed_black.png
rm ${IMGPATH}${IMGNAME}_removed_black_check.png
rm ${IMGPATH}${IMGNAME}_matte.png
rm ${IMGPATH}${IMGNAME}_matte-negated.png
rm ${IMGPATH}${IMGNAME}_finished.png

```



### Cool terminal tools

Here is a list of tools that dont **suck** 

`ranger` file browser

`calcurse` great calendar

`timew` time warrior

`task` task warrior



##### Text processing

`pandoc` mega text converter

`xsltproc`xsl processor

`groff` troff frontend



##### Fun 

`lolcat` Awesome colours for terminal

`figlet` Awesome banners for terminal 

`banner` big banners 

`fortune` fortune cookies

`say` talk from STdin

`sl` steam locomotive

`ddate` fun date 


#### Basic tools


`awk` Find text pattern and do something

`sed`

`grep` filter stuff from input

`cat` show contents of file

`rm` delete file or folder

`mkdir` make new folder

`ls` list directory

`cal` calendar

`clear` clear window

`ditto` a better copy

`yes` 100 cpu usage stress test

`uptime` tells you how long system is up

`curl` downlod file from internetz

`tail` show last line of document

`ipconfig` 

`history` shows last commands entered

`whatis` shows info about command

`wget` download file 

`rsync` backup sync tool







---
##Terminal Hacks

Notes function to add notes 

```bash
notes() {
  if [ ! -z "$1" ]; then
    # Using the "$@" here will take all parameters passed into
    # this function so we can place everything into our file.
    echo "$@" >> "$HOME/notes.md"
  else
    # If no arguments were passed we will take stdin and place
    # it into our notes instead.
    cat - >> "$HOME/notes.md"
  fi
}
```

#### Fish Shell

```
brew install fish

==> Caveats
You will need to add:
  /usr/local/bin/fish
to /etc/shells.

Then run:
  chsh -s /usr/local/bin/fish
to make fish your default shell.

```

#### OSX Terminal with colors
Open Terminal and type `nano .bash_profile`

Paste in the following lines:

```bash
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad
alias ls='ls -GFh'
```
Hit `Control+O` to save, then `Control+X` to exit out of 

#### Colored man pages

```bash
man() {
    env \
    LESS_TERMCAP_mb=$(printf "\e[1;31m") \
    LESS_TERMCAP_md=$(printf "\e[1;31m") \
    LESS_TERMCAP_me=$(printf "\e[0m") \
    LESS_TERMCAP_se=$(printf "\e[0m") \
    LESS_TERMCAP_so=$(printf "\e[1;44;33m") \
    LESS_TERMCAP_ue=$(printf "\e[0m") \
    LESS_TERMCAP_us=$(printf "\e[1;32m") \
    man "$@"
}
```

Source [+] (https://www.cyberciti.biz/faq/linux-unix-colored-man-pages-with-less-command/)

##### OSX SPECIFIC

###### Fix Spotlight
`sudo mdutil -E /Volumes/DriveName`

######See disk activity
`sudo fs_usage`

#######Screenshot format to PDF 
```
defaults write com.apple.screencapture type PDF
killall SystemUIServer
```

######Install OS X Software Updates

Despite Software Updates moving to the App Store in Mountain Lion, we’re able to use the command line to install system udates without having to launch it. To see available software updates for your Mac:

```
$ sudo softwareupdate -l
```
After a few minutes, you’ll be given a list of available updates.

If you’d like to install all available updates, enter:

```
$ sudo softwareupdate -ia
```

