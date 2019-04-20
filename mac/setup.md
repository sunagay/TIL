インストール

- Google 日本語入力 (https://www.google.co.jp/ime/)
- Chrome (https://www.google.com/intl/ja_ALL/chrome/)
- Slack (https://slack.com/intl/ja-jp/)
- Office 365 (https://www.office.com/?omkt=ja-jp)
- iTerm2 (https://www.iterm2.com/)
- Homebrew (https://brew.sh/index_ja)
- Git ()
```bash
$ brew install git
```
- Visual Studio Code (https://code.visualstudio.com/)
- Ricty Diminished (https://github.com/edihbrandon/RictyDiminished)
```bash
$ brew tap caskroom/fonts
$ brew cask install font-ricty-diminished
```
- Google Cloud SDK (https://cloud.google.com/sdk/downloads?hl=JA)
```bash
$ cd ~/Downloads/google-cloud-sdk/
$ ./install.sh

$ gcloud components install app-engine-go
```
- goenv (https://github.com/syndbg/goenv)
```bash
$ git clone https://github.com/syndbg/goenv.git ~/.goenv
$ echo 'export GOENV_ROOT="$HOME/.goenv"' >> ~/.bash_profile
$ echo 'export PATH="$GOENV_ROOT/bin:$PATH"' >> ~/.bash_profile

$ goenv install 1.12.4
$ goenv global 1.12.4
$ go version
go version go1.12.4 darwin/amd64
```
- PlantUML (http://plantuml.com/ja/)
```bash
$ brew install graphviz
$ brew cask install java
$ brew install plantuml
```

