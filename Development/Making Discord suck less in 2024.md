
First locate the settings.json file in %appdata on windows or Application Support on MacOS, then enable Dev Options by inserting this in the JSON:

```
"DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING": true
```
As you can see from the it is Dangerous, don't be stupid and paste things in the console..

Now you have accesses to the dev tools and you are free to "display: none" anything you wish in CSS snippets. 

If you want me to do it for you install [Better Discord](https://betterdiscord.app/) and then download the theme from [Igor](https://github.com/bdsqqq/better-discord-vesper-theme).

Now open the theme css with an editor of choice and add the following to remove most of what makes discord special:

```
/* Remoevs the multiple Discords functionality */
.wrapper_a7e7a8 {
  display: none;
}
/* Removes the search bar */
.searchBar_e4ea2a {
  display: none;
}
/* Fixes the placing of whats left of the buttons */
.scroller__4b984 {
  margin-top: 20px;
}
/* I dont remember */
.container__11d72 {
  display: none;
}
/* Removes the now playing columbn on the right side of the screen */
.nowPlayingColumn_f5023f {
  display: none;
}
/* Removes the profile of the person you are chatting with */
.profilePanel__12596 {
  display: none;
}
/* No clue, maybe the green telephoen icon in chat after a call */
.iconContainer_dfc268 {
  display: none;
}
/* Removes the "gift" option in chat?? */
.buttons_ce5b56 {
  display: none;
}
/* Turns the ugly green "available" button orange, as Igor intended 
Also turns the sircle into a square sometimes but you cant win em all */
rect.pointerEvents__33f6a {
  fill: #ffc799;
}
```

One day I will remove the Friends, Nitro, Shop buttons, right now they have no IDs or custom classes, but i can take the first 3 children from a list 

Found it:

```
/* Removes the Friends, Nitro and Shop buttons*/
ul.content__23cab li:nth-child(-n + 4) {
display: none;
}
```