
## Component design principles
- set your touch target (ie. buttons) size to 9 mm square or greater (48×48 pixels on a 135 PPI display at a 1.0x scaling).
- Keep the number of character per line within 60-80 (30–40 for mobile)

* * *

## Atomic components
### Navbars
- when the page the user is on is where an action is accomplished, a navbar is not needed. Presumably, the user came to this part of the app with intention and thus does not need to navigate freely to other parts of the app so easily. In lieu of the missing navbar, there should be a back button available at the top of the screen so the navbar is realistically only 1 navbar click away.

### Icons
- when picking which icon to represent which idea, think more in terms of what the outcome of the icon helps you accomplish, rather than the method it uses to get there
    - Google Slides uses the icon of a highlighter to symbolize the act of highlighting text. Makes sense why they would do that, but it's misguided. While you are using a piece of software you and want to accomplish a goal like highlighting text, your eyes naturally start looking for a symbol that shows the result. In other words, they start looking for an icon perhaps with text and a colored background. Instead, they should have been looking for a symbol that is easily mistaken with a pen. This creates friction, and understandably a lot of friction. 

### Lists
anything that can be a bulleted list probably should be.
