But before you doom yourself, here are some things you should know:

1. Don't forget to run 'doom sync', then restart Emacs, after modifying
   ~/.doom.d/init.el or ~/.doom.d/packages.el.

   This command ensures needed packages are installed, orphaned packages are
   removed, and your autoloads/cache files are up to date. When in doubt, run
   'doom sync'!

2. If something goes wrong, run `doom doctor`. It diagnoses common issues with
   your environment and setup, and may offer clues about what is wrong.

3. Use 'doom upgrade' to update Doom. Doing it any other way will require
   additional steps. Run 'doom help upgrade' to understand those extra steps.

4. Access Doom's documentation from within Emacs via 'SPC h d h' or 'C-h d h'
   (or 'M-x doom/help')

Have fun!



- `doom sync` to synchronize your private config with Doom by installing missing packages, removing orphaned packages, and regenerating caches. Run this whenever you modify your private `init.el` or `packages.el`, or install/remove an Emacs package through your OS package manager (e.g. mu4e or agda).
- `doom upgrade` to update Doom to the latest release & all installed packages.
- `doom doctor` to diagnose common issues with your system and config.
- `doom env` to dump a snapshot of your shell environment to a file that Doom will load at startup. This allows Emacs to inherit your `PATH`, among other things.
- `doom build` to recompile all installed packages (use this if you up/downgrade Emacs).



https://stackoverflow.com/questions/61998389/full-ide-features-support-for-golang-in-doom-emacs



## Install on ArchLinux



```
sudo pacman -S emacs git ripgrep fd
```

```
git clone --depth 1 https://github.com/hlissner/doom-emacs ~/.emacs.d
```

```
~/.emacs.d/bin/doom install
```



recent file

```
spc f r
```

browse file

spc .





```bash
doom sync
```

In packages.el add

```lisp
(package! evil-tutor)
```

1





















