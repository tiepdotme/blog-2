---
layout: post
title: "Top 10 VS Code Settings"
image: "/assets/social/code-settings.png"
date: 2020-07-11 12:00:00 -0500
---

I should probably rename this from Top 10 VS Code settings to Top 10 lesser-known settings! This blog post brings up settings buried in Visual Studio Code that increase productivity and are the settings that are not extremely popular or mainstream. I cheated a bit, and instead of having precisely ten settings, I have ten sections!

> Remember you can get to your user settings using Ctrl+, on Windows and Cmd+, on Mac

You might like some, and you might hate a few, use it to your liking 😅 , and as always I appreciate your feedback.

### Breadcrumbs

VS Code has a navigation bar above its contents called Breadcrumbs. It shows the current location and allows you to navigate between symbols and files quickly. Enable it with the View > Show Breadcrumbs command or:

```json
"breadcrumbs.enabled": true
```

If you have very long paths or are only interested in either file paths or symbols paths, you can use the `breadcrumbs.filePath` and `breadcrumbs.symbolPath` settings.

### CSS Important

The CSS feedback rule helps you to make sure you are not using `!important` in your stylesheets. It works with SCSS and CSS. Using `!important` is an indication that the specificity of the CSS should be refactored.

```json
"css.lint.important": "warning",
"scss.lint.important": "warning",
```

### Debug Toolbar

Moving the debug toolbar around because it is in your way is a thing of the past! You can have it `docked` or `floating` now. I prefer docked. If you use `docked` mode, the debug actions appear at the top of the Debug view when a debug session starts.

```json
"debug.toolBarLocation": "docked"
```

### Editor

These settings increase the visual appeal of the editor. Code provides a ton of ways you can customize your cursor (smooth, expand, solid or phase). Phasing in and out with a line cursor is my choice. Line height makes the code more readable. I know that not everyone likes font ligatures because, for some, it does reduce the readability of code. I get it. It takes time getting used to it.

```json
"editor.cursorBlinking": "phase",
"editor.cursorStyle": "line",
"editor.fontLigatures": true, // Please don't hate me.
"editor.formatOnSave": false,
"editor.lineHeight": 22
```

### Explorer

You might want to see the file you currently have open in your file explorer. `autoReveal` helps you with that. It automatically scrolls the file explorer to the file you are editing now. As a fair warning, some folks do not like the "jumpiness" it introduces in the file explorer. Try it out and see if you like it.

```json
"explorer.autoReveal": true
```

### File

Code has multiple options to autosave. I have set mine to `onFocusChange`. You might not like it if you continuously have a live server running. Your server might throw errors if you were in the middle of writing a code block, it is not complete, and you switch your tab.

Inserting a new line is helpful to maintain POSIX conformity.

```json
"files.autoSave": "onFocusChange",
"files.insertFinalNewline": true,
"files.trimTrailingWhitespace": true,
```

### HTML

Enable your HTML formatter! And end with a new line like I mentioned in the section above.

```json
"html.format.enable": true,
"html.format.endWithNewline": false,
```

### JavaScript

Most of these settings are formatting options that make my code more readable. Make sure folks on your team use similar settings otherwise understanding diffs in pull requests might be difficult (if your pull request method does not support "Ignore Whitespace").

```json
"javascript.format.enable": true,
"javascript.format.insertSpaceAfterConstructor": true,
"javascript.format.insertSpaceAfterFunctionKeywordForAnonymousFunctions": true,
"javascript.format.insertSpaceAfterOpeningAndBeforeClosingNonemptyBraces": true,
"javascript.format.insertSpaceAfterOpeningAndBeforeClosingNonemptyBrackets": false,
"javascript.format.insertSpaceAfterOpeningAndBeforeClosingNonemptyParenthesis": false,
"javascript.updateImportsOnFileMove.enabled": "always",
"typescript.updateImportsOnFileMove.enabled": "always",
```

The last two settings are a lifesaver when you have to re-organize your project and not worry about changing import statements everywhere. There is in-built support for JavaScript and Typescript.

### Markdown

I spend a lot of time writing Markdown. Due to a widescreen monitor, I have the preview and editor window open side by side. The following settings help me keep the editor and preview in sync when I scroll.

```json
"markdown.preview.scrollEditorWithPreview": true,
"markdown.preview.scrollPreviewWithEditor": true,
```

### Terminal

If you are on Mac and use iTerm2 like me or you are on Windows and prefer Cmder, you might want to tweak VS Code to use the same terminal. I also prefer `zsh` due to `oh-my-zsh` and the customizations offered. It is also the default shell on Mac going forward (Catalina and above).

```json
"terminal.explorerKind": "external",
"terminal.external.osxExec": "iTerm.app",
"terminal.integrated.fontFamily": "Inconsolata for Powerline", // Install Powerline fonts for this to work
"terminal.integrated.fontSize": 12,
"terminal.integrated.shell.osx": "zsh",
```

### Workbench

Workbench changes impact everything around your editor. I have tweaked my tabs a bit and switched my settings preview to JSON instead of the GUI provided. I also moved the sidebar to the right. It is one of my favorite settings. You don't realize how nice it is until you do it. Having the sidebar on the left means your code jumps when you show or hide it. Not anymore if it is on the right.

```json
"workbench.editor.enablePreview": false,
"workbench.editor.tabCloseButton": "right",
"workbench.editor.tabSizing": "shrink",
"workbench.panel.defaultLocation": "right",
"workbench.settings.editor": "json",
"workbench.sideBar.location": "right",
```
