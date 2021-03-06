
# viminfo file
The viminfo is like a cache, to store cut buffers persistently
- If you exit Vim and later start it again, you would normally lose a lot of
information.  The viminfo file can be used to remember that information, which
enables you to continue where you left off.
- Vim writes this file for us.

The viminfo file is used to store:
- The command line history.
- The search string history.
- The input-line history.
- Contents of non-empty registers.
- Marks for several files.
- File marks, pointing to locations in files.
- Last search/substitute pattern (for 'n' and '&').
- The buffer list.
- Global variables.
