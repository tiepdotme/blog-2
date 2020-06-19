---
layout: post
title:  "Code Editor: Visual Studio Code"
date:   2020-06-17 12:00:00 -0500
---

In the past 30ish blog posts, I have gone over some basic and some advanced topics of JavaScript. Most of my code samples could be executed in a browser console or an online JavaScript playground. This is not a long term solution, and sooner or later, you will need a code editor.

Today I am going to write about my code editor of choice: Visual Studio Code.

### Background

I started coding professionally around 2008. Coding for personal projects began in early 2004. So it's been a 16-year journey and counting. Here are the code editors I have gone through in the past 16 years:

1. [Eclipse](https://www.eclipse.org/downloads/): Give me what is free!
2. [Notepad++](https://notepad-plus-plus.org/downloads/) : The minimalist phase.
3. [Vim](https://www.vim.org/download.php): Not doing it for productivity but rather to look cool i.e., Hackerman
4. [Sublime Text](https://www.sublimetext.com/3): To date, one of the best editors. Fastest indexing, and it can load large projects without breaking a sweat.
5. [IntelliJ IDEA or WebStorm](https://www.jetbrains.com/idea/download/): Work paid for it, and it is excellent for Java Development I couldn't complain.
6. [Visual Studio Code](https://code.visualstudio.com): The best editor, especially if you are coding with MEAN or MERN stack.

I've been with VS Code for the last 3 years. I'm absolutely in love ❤️ with it because:

* Extremely lightweight and fast (yes, even though it is based on Electron)
* IntelliSense
* Command Palette (exactly like Sublime Text or Chrome Console)
* Top-notch debugger for JavaScript or TypeScript
* Multi-pane editing
* Markdown preview
* Built-in terminal and version control (Git)
* Smart refactoring & Code management
* Cross-platform support
* Extensions, extensions, and extensions
* Free 💰

### Download

You can get Visual Studio Code [here](https://code.visualstudio.com/download). It is available for Windows, Mac & Linux.

If you would like an online workspace that supports VS Code, you can check out [codespaces](https://visualstudio.microsoft.com/services/visual-studio-codespaces/).

Or, if you would like to use an online playground that utilizes VSCode and supports Angular, React, Ionic, and Svelte, check out [Stackblitz](https://stackblitz.com).

### My Setup

Now comes the important part, my setup of VSCode. I am going to share my extensions and setting for the Visual Studio Code. You can use them; as you feel like. Install all, replace all settings or tweak them a bit.

#### Extensions: Copy and paste this in terminal once you have VS Code installed

```bash
code --install-extension Angular.ng-template
code --install-extension CoenraadS.bracket-pair-colorizer
code --install-extension IBM.output-colorizer
code --install-extension Tyriar.sort-lines
code --install-extension WallabyJs.quokka-vscode
code --install-extension adamwalzer.string-converter
code --install-extension alexkrechik.cucumberautocomplete
code --install-extension Arjun.swagger-viewer
code --install-extension atlassian.atlascode
code --install-extension auchenberg.vscode-browser-preview
code --install-extension ban.spellright
code --install-extension ChakrounAnas.turbo-console-log
code --install-extension christian-kohler.npm-intellisense
code --install-extension christian-kohler.path-intellisense
code --install-extension dbaeumer.vscode-eslint
code --install-extension dzannotti.vscode-babel-coloring
code --install-extension eamodio.gitlens
code --install-extension eg2.vscode-npm-script
code --install-extension EliverLara.andromeda
code --install-extension formulahendry.auto-close-tag
code --install-extension formulahendry.auto-rename-tag
code --install-extension hediet.vscode-drawio
code --install-extension iciclesoft.workspacesort
code --install-extension jspolancor.presentationmode
code --install-extension kiteco.kite
code --install-extension lehni.vscode-titlebar-less-macos
code --install-extension mdickin.markdown-shortcuts
code --install-extension mechatroner.rainbow-csv
code --install-extension miclo.sort-typescript-imports
code --install-extension mikestead.dotenv
code --install-extension mohsen1.prettify-json
code --install-extension ms-python.python
code --install-extension naumovs.color-highlight
code --install-extension naumovs.node-modules-resolve
code --install-extension oderwat.indent-rainbow
code --install-extension pnp.polacode
code --install-extension qcz.text-power-tools
code --install-extension redhat.vscode-yaml
code --install-extension richie5um2.vscode-sort-json
code --install-extension robinbentley.sass-indented
code --install-extension sasa.vscode-sass-format
code --install-extension Shan.code-settings-sync
code --install-extension shyykoserhiy.vscode-spotify
code --install-extension spoonscen.es6-mocha-snippets
code --install-extension steoates.autoimport
code --install-extension techer.open-in-browser
code --install-extension VisualStudioExptTeam.vscodeintellicode
code --install-extension vscjava.vscode-java-debug
code --install-extension wayou.vscode-todo-highlight
code --install-extension yzhang.markdown-all-in-one
code --install-extension ziishaned.snippetmaker
```

#### Settings: Hit `Command + ,` on Mac or `Ctrl + ,` on Windows and paste
```json
{
    "autoimport.doubleQuotes": true,
    "breadcrumbs.enabled": true,
    "css.lint.important": "warning",
    "debug.node.autoAttach": "on",
    "debug.toolBarLocation": "docked",
    "editor.codeLens": false,
    "editor.colorDecorators": true,
    "editor.cursorBlinking": "phase",
    "editor.cursorStyle": "line",
    "editor.fontFamily": "Dank Mono, SF Mono Powerline, Operator Mono, Victor Mono, Fira Code, ZeitungMonoPro, Menlo",
    "editor.fontLigatures": true,
    "editor.fontSize": 14,
    "editor.formatOnPaste": true,
    "editor.formatOnSave": false,
    "editor.insertSpaces": true,
    "editor.lineHeight": 22,
    "editor.minimap.enabled": false,
    "editor.multiCursorModifier": "ctrlCmd",
    "editor.rulers": [
        120
    ],
    "editor.snippetSuggestions": "top",
    "editor.suggestSelection": "first",
    "editor.tabSize": 4,
    "editor.tokenColorCustomizations": {
        "comments": "#746f68"
    },
    "editor.wordWrap": "on",
    "explorer.autoReveal": true,
    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,
    "extensions.ignoreRecommendations": false,
    "files.associations": {
        "*.irul": "iRule"
    },
    "files.autoSave": "onFocusChange",
    "files.exclude": {
        "**/*.js": {
            "when": "$(basename).ts"
        },
        "**/*.js.map": true,
        "**/.DS_Store": true,
        "**/._*": true,
        "**/.git": true,
        "**/.hg": true,
        "**/.svn": true,
        "**/node_modules": true,
        "**/node_modules/**": false,
        "**/target": true,
        ".idea": true,
        ".metadata": true,
        ".vscode": true,
        "build": false,
        "node_modules": true
    },
    "files.insertFinalNewline": true,
    "files.trimTrailingWhitespace": true,
    "git.autofetch": true,
    "git.ignoreMissingGitWarning": true,
    "gitlens.views.compare.location": "gitlens",
    "gitlens.views.fileHistory.location": "gitlens",
    "gitlens.views.repositories.location": "gitlens",
    "gitlens.views.search.location": "gitlens",
    "html.format.enable": true,
    "html.format.endWithNewline": false,
    "html.suggest.html5": true,
    "javascript.format.enable": true,
    "javascript.format.insertSpaceAfterConstructor": true,
    "javascript.format.insertSpaceAfterFunctionKeywordForAnonymousFunctions": true,
    "javascript.format.insertSpaceAfterOpeningAndBeforeClosingNonemptyBraces": true,
    "javascript.format.insertSpaceAfterOpeningAndBeforeClosingNonemptyBrackets": false,
    "javascript.format.insertSpaceAfterOpeningAndBeforeClosingNonemptyParenthesis": false,
    "javascript.updateImportsOnFileMove.enabled": "always",
    "json.format.enable": true,
    "json.schemas": [
        {
            "$schema": "~/scripts/tsconfig.json"
        }
    ],
    "markdown.extension.preview.autoShowPreviewToSide": false,
    "markdown.preview.scrollEditorWithPreview": true,
    "markdown.preview.scrollPreviewWithEditor": true,
    "material-icon-theme.folders.color": "#26a69a",
    "material-icon-theme.folders.theme": "classic",
    "material-icon-theme.hidesExplorerArrows": true,
    "material-icon-theme.showUpdateMessage": false,
    "mocha-snippets.function-type": "arrow",
    "mocha-snippets.quote-type": "double",
    "npm-intellisense.importQuotes": "\"",
    "npm.enableScriptExplorer": true,
    "peacock.favoriteColors": [
        {
            "name": "Angular Red",
            "value": "#b52e31"
        },
        {
            "name": "Auth0 Orange",
            "value": "#eb5424"
        },
        {
            "name": "Azure Blue",
            "value": "#007fff"
        },
        {
            "name": "C# Purple",
            "value": "#68217A"
        },
        {
            "name": "Gatsby Purple",
            "value": "#639"
        },
        {
            "name": "Go Cyan",
            "value": "#5dc9e2"
        },
        {
            "name": "Java Blue-Gray",
            "value": "#557c9b"
        },
        {
            "name": "JavaScript Yellow",
            "value": "#f9e64f"
        },
        {
            "name": "Mandalorian Blue",
            "value": "#1857a4"
        },
        {
            "name": "Node Green",
            "value": "#215732"
        },
        {
            "name": "React Blue",
            "value": "#00b3e6"
        },
        {
            "name": "Something Different",
            "value": "#832561"
        },
        {
            "name": "Vue Green",
            "value": "#42b883"
        }
    ],
    "presentationMode.zoomLevel": 3,
    "problems.showCurrentInStatus": true,
    "python.jediEnabled": false,
    "scss.lint.important": "warning",
    "search.exclude": {
        "**/.git": true,
        "**/bower_components": true,
        "**/node_modules": false,
        "**/tmp": true,
        "build/**": true,
        "node_modules/**": true
    },
    "spellright.documentTypes": [
        "latex",
        "plaintext"
    ],
    "spellright.language": [
        "en"
    ],
    "sync.askGistName": false,
    "sync.autoDownload": false,
    "sync.autoUpload": false,
    "sync.forceDownload": false,
    "sync.forceUpload": true,
    "sync.quietSync": false,
    "sync.removeExtensions": true,
    "sync.syncExtensions": true,
    "terminal.explorerKind": "external",
    "terminal.external.osxExec": "iTerm.app",
    "terminal.integrated.fontFamily": "Inconsolata for Powerline",
    "terminal.integrated.fontSize": 12,
    "terminal.integrated.rendererType": "dom",
    "terminal.integrated.shell.osx": "zsh",
    "terminal.integrated.shell.windows": "C:\\Windows\\sysnative\\cmd.exe",
    "terminal.integrated.shellArgs.osx": [
        "-l"
    ],
    "todohighlight.keywords": [
        "// TODO:",
        "// TODO",
        "//TODO:",
        "//TODO"
    ],
    "tslint.jsEnable": true,
    "typescript.extension.sortImports.enableJavascript": true,
    "typescript.extension.sortImports.quoteStyle": "double",
    "typescript.extension.sortImports.sortOnSave": false,
    "typescript.referencesCodeLens.enabled": true,
    "typescript.updateImportsOnFileMove.enabled": "always",
    "vsintellicode.modify.editor.suggestSelection": "automaticallyOverrodeDefaultValue",
    "window.nativeTabs": false,
    "window.openFoldersInNewWindow": "on",
    "window.restoreWindows": "all",
    "window.title": "${activeEditorLong}",
    "window.titleBarStyle": "custom",
    "window.zoomLevel": 0,
    "workbench.colorCustomizations": {},
    "workbench.colorTheme": "Andromeda",
    "workbench.editor.enablePreview": false,
    "workbench.editor.tabCloseButton": "right",
    "workbench.editor.tabSizing": "shrink",
    "workbench.panel.defaultLocation": "right",
    "workbench.settings.editor": "json",
    "workbench.sideBar.location": "right",
    "workbench.startupEditor": "newUntitledFile",
    "workbench.tree.renderIndentGuides": "always",
    "zenMode.centerLayout": true,
    "editor.renameOnType": true
}
```

You can also find the latest list of extensions [here](https://gist.github.com/bhagatparwinder/8f4fade30c473e326426caebe7463f04).

And the user settings [here](https://gist.github.com/bhagatparwinder/13af4a593ff62a3c0796206c30436fdf)

I keep it up to date. I would most likely end up making my Settings Sync Gists public.

Happy Coding 👨🏼‍💻
