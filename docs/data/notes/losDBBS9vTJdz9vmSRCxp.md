
# Path
## Path.join
- concatenate each argument to get a full URL
	- therefore, it can be relative or absolute (using `__dirname`) depending on what args we pass 
- depending on if the OS is Unix-based or Windows, different delimiters will be used, abstracting this away from us. 

## Path.resolve
- attempts to resolve a sequence of paths from RTL, with each subsequence path prepended until an absolute directory is formed. 
	- if an absolute directory is not able to be formed, then the args will be put on the end of the current working directory. 
- this method will treat the first argument as the root directory 
- when called without arguments, it will return the working directory (which may happen to be equivalent to `__dirname`)
- it will always result in an absolute URL 
