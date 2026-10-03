
### package.json script to ensure using yarn
```json
{
    "scripts": {
        "preinstall": "node -e \"if(process.env.npm_execpath.indexOf('yarn') === -1) throw new Error('You must use Yarn to install, not NPM')\"",
    }
}
```

### Install package with an alias
```sh
yarn add pouchdb-md5@npm:react-native-pouchdb-md5 react-native-quick-md5
```

will result in package.json dependencies property:
```json
"pouchdb-md5": "npm:react-native-pouchdb-md5",
```