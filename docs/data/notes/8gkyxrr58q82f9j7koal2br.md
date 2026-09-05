
There are multiple criteria to determine whether to store a file using Git LFS.
- The size of the file. IMO if a file is more than 10 MB, you should consider storing it in Git LFS
- How often the file is modified. A large file (based on the users intuition of a large file) that changes very often should be stored using Git LFS
- The type of the file. A non-text file that cannot be merged is elligible for Git LFS storage

Depending on how often the file changes, any size of file from say 1MB to 50MB and above can benefit. Consider the case where you do 100 commits editing the file every time. For a 20MB file that can be compressed say to 15 MB, the repository size would increase by approximately 1.5GB if the file is not stored using Git LFS.

Why not store all files in LFS? Because files in LFS cannot easily be diffed, basically breaking an important part of the version control system.
- hence, why Git LFS is ideal for binary files

## Setting up
1. Once it has been installed, in your repo run `git lfs track "*.jpg"` to start tracking a certain filetype with git lfs
    - see all tracked files in `.gitattributes` file
2. Add `.gitattributes` file to version control.
3. Operate as you normally would.

