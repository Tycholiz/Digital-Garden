
Vim remembers the locations you have recently visited (where you jumped from). Each position (file name, column number, line number) is recorded in a jump list, and each window has a separate jump list that records the last 100 positions where a jump occurred.
- The commands that are regarded as "jumps" include searching, substitute and marks. Scrolling through a file is not regarded as jumping.

Like a web browser, you can go back, then forward:
- Press Ctrl-O to jump back to the previous (older) location.
- Press Ctrl-I (same as Tab) to jump forward to the next (newer) location.

use `:jump` to see the jumplist
