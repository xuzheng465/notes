# Keng - Github



## Why is github keeping asking for username/password after adding ssh key?





Don't use HTTP use SSH instead

change

```
https://github.com/WEMP/project-slideshow.git 
```

to

```
git@github.com:WEMP/project-slideshow.git
```

you can do it in `.git/config` file



[Ref](https://stackoverflow.com/questions/10909221/why-is-github-asking-for-username-password-when-following-the-instructions-on-sc#:~:text=If%20Git%20prompts%20you%20for,through%20strict%20firewalls%20and%20proxies.)

