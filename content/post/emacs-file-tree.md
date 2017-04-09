+++
date = "2015-05-31T14:30:49+02:00"
title = "Emacs quick tip: No file tree pollution"
draft = false

+++
This is just a quick tip for Emacs beginners. I didn't find these informations in one place, so it might be useful for somebody: 

If not configured otherwise Emacs will create three different special files when editing a file:

- [Backup Files](http://emacswiki.org/emacs/BackupDirectory): The file name is appended with a tilde `~`.

- [Auto Save Files](http://emacswiki.org/emacs/AutoSave): The file name is prepended and appended with `#`.

- [Lock Files](http://www.gnu.org/software/emacs/manual/html_node/elisp/File-Locks.html): The file name is prepended with `.#`.

All files will are named similar to the files edited and placed in the same directory. I really dislike this default behaviour, because I don't want any "magic" files appearing in my projects.

So here is the emacs config to deactivate each feature or to save the files in a different place:

```
 (setq backup-directory-alist
          `((".*" . ,temporary-file-directory)))
 (setq auto-save-file-name-transforms
          `((".*" ,temporary-file-directory t)))         
 (setq create-lockfiles nil)
```
