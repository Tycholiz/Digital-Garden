
### Pass in grep results to a command
Imagine we wanted to run a command each file in the stdout of `eslint`:
```
/Users/kyletycholiz/programming/projects/project-zero/modules/client-ui-components/src/atoms/Radio/FontFaceRadio.js
  62:50  warning  'onClick' is missing in props validation  react/prop-types

/Users/kyletycholiz/programming/projects/project-zero/modules/client-ui-components/src/atoms/Radio/TextAlignRadio.js
  38:36  warning  'children' is missing in props validation  react/prop-types
```
We could use grep to get just the filenames, then run a command like `yarn lint | grep /Users/kyletycholiz | xargs npx react-proptypes-generate fix` to pipe those filenames into a command that takes an arbitrary number of args
- this would be like running: `npx react-proptypes-generate fix FontFaceRadio.js TextAlignRadio.js`