# mac hacks

### Show and Hide Desktop Icons

To hide desktop icons, copy and paste each of these into Terminal

 ```bash
> defaults write com.apple.finder CreateDesktop false
> killall Finder
 ```

To show desktop icons again,

 ```bash
> defaults write com.apple.finder CreateDesktop true
> killall Finder
 ```



### Show and Hide Hidden Files in Finder

To show hidden files in Finder, you can use the keyboard shortcut **Command-Shift-.** in macOS Sierra.

To show hidden files in Finder via the Terminal, copy and paste each of these into Terminal

 ```bash
> defaults write com.apple.finder AppleShowAllFiles YES
> killall Finder
 ```

To hide hidden files in Finder via the Terminal,

 ```bash
> defaults write com.apple.finder AppleShowAllFiles NO
> killall Finder
 ```



### Remove App from App-Switcher



### Take a screenshot of just the window

This one’s pretty well known but still my favourite. Hit **Command-Shift-4** then **Space** then select the window you want to get a screenshot of. This can be combined with the tip above about disabling the shadow for window screenshots.



### Open a file or app in Finder from the Dock

From the Dock, you can reveal a file or application in Finder by holding **Command** and clicking the icon.



###Remove a single prediction from the omnibox in chrome

To remove a single prediction from the omnibox, arrow down with your keyboard to highlight the URL, `Shift-Fn-Delete`.



### Force Page Break in html 

https://stackoverflow.com/questions/1664049/can-i-force-a-page-break-in-html-printing