# Golang depends for FvwmPrompt

This is a list of the depends needed to build FvwmPrompt using golang.
It also states if the package is in Debian or not. At this time Debian
is missing three depends. **Note**: golang-github-abiosoft-readline-dev
is basically a duplicate of golang-github-chzyer-readline-dev, which
is in Debian, but golang-github-abisoft-ishell-dev depends on aboisoft's
library.

+ golang-golang-x-sys-dev (yes)
+ golang-github-abiosoft-ishell-dev (no)
+ golang-github-abiosoft-readline-dev (no)
+ golang-github-faith-color-dev (no)
+ golang-github-flynn-archive-go-shlex-dev (yes)
+ golang-github-konsorten-go-windows-terminal-sequences-dev (yes)
+ golang-github-mattn-go-colorable-dev (yes)
+ golang-github-mattn-go-isatty-dev (yes)
+ golang-github-sirupsen-logrus-dev (yes)
