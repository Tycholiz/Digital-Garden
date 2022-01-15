
# Buffer
- a part of vim's memory that can hold text
- This is normally in the form of:
    - an actual file
    - text stored temporarily (yank, paste etc)
- when opening an existing file, the text of that file is copied and put into a new buffer. When we save that text, the original file is replaced by writing the buffer to disk
- buffer list vs argument list
    - argument list is subset of buffer list
    - buffer list can be seen as more random access, while argument list could have more organization to it
        - if you are working with only a few files in your current micro session, you'd have just those ones in the arg list, while having many more in the buffer list
    - idea is that we might have lots of buffers open, and want to execute a macro across many files, but not all buffers.
        - one solution is to prune the buffer list, but the most practical solution is to populate the argument list with just the files we are interested in. 
