---
title: "Reload Your Browser in Vim"
layout: single
summary: "Let's use AppleScript to make our Vimming more Vimmy"
---

I've been experimenting with external scripts to make my daily Vim more enjoyable. One of my pain points as a web developer is constantly refreshing the browser after editing a file. I finally got fed up with hitting `⌘+Tab` dozens of times a day and pulled out some AppleScript.

Here's all the code you need to tell Safari to reload the current page:

```applescript
tell application "Safari"
  set sameURL to URL of current tab of front window
  set URL of current tab of front window to sameURL
end tell
```

If you use Chrome you can use this instead:

```applescript
tell application "Google Chrome" to reload active tab of window 1
```

Here's how we hook up the keybinding in Vim:

```vimscript
function! ReloadBrowser()
  silent !osascript ~/.vim/refresh-safari.applescript
endfunction
nnoremap <silent> <leader>rs :call ReloadBrowser()<CR>
```

Using this script I was able to turn `⌘+Tab`, `⌘+r`, `⌘+Tab` into `<leader>+rs`. I've been using it for about a week and it feels great. LiveReload will afford even greater ease, but you don't always have the luxury of installing it and this will work for any project.