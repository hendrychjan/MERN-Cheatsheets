# Node Package Manager
### Installing and uninstalling packages
- Install a package:
```npm i <package_name>```
example: ```npm i bootstrap```

- Install a specific version of a package:
```npm i <package_name>@<version>```
example: ```npm i bootstrap@3.4.1```

- Install a package as a development dependency:
```npm i <package_name> --save-dev```
example: ```npm i gh-pages --save-dev```

- Uninstall a package:
```npm un <package_name>```
example: ```npm un bootstrap```

### Managing packages
- List installed packages:
```npm list --depth=0```

- List outdated packages:
```npm outdated```

- Update packages:
```npm update```

- Look for vulnerable packages:
```npm audit```