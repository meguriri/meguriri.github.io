---
layout: post
title: Update on my .gitconfig
date: '2017-03-08T06:40:00 -05:00'
author: alberto
categories: development
tags:
- Git

---

Here is another update on my gitconfig:

{% highlight conf linenos %}
[user]
	name = Alberto A. Flores
	email = aaflores@gmail.com
[color]
	branch = auto
	diff = auto
	status = auto
	ui = true
[color "branch"]
	current = yellow reverse
	local = yellow
	remote = green
[color "diff"]
	meta = yellow bold
	frag = magenta bold
	old = red bold
	new = green bold
[color "status"]
	added = yellow
	changed = green
	untracked = cyan
[alias]
  lg= log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
  lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
  lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
  #quick look at all repo
  loggsa = log --color --date-order --graph --oneline --decorate --simplify-by-decoration --all
  #quick look at active branch (or refs pointed)
  loggs  = log --color --date-order --graph --oneline --decorate --simplify-by-decoration
  #extend look at all repo
  logga  = log --color --date-order --graph --oneline --decorate --all
  #extend look at active branch
  logg   = log --color --date-order --graph --oneline --decorate
  #Look with date
  logda  = log --color --date-order --date=local --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ad%Creset %C(auto)%d%Creset %s\" --all
  logd   = log --color --date-order --date=local --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ad%Creset %C(auto)%d%Creset %s\"
  #Look with relative date
  logdra = log --color --date-order --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ar%Creset %C(auto)%d%Creset %s\" --all
  logdr = log --color --date-order --graph --format=\"%C(auto)%h%Creset %C(blue bold)%ar%Creset %C(auto)%d%Creset %s\"
  loga   = log --graph --color --decorate --all

{% endhighlight %}