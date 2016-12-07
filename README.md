This sample project demonstrates a bug with setting the node path in gradle-node-plugin for the 'yarnSetup' task.  How to reproduce this issue:

Steps to reproduce:
- On Windows 10, start git bash (note on git bash settings: I am using the option "Use Windows' default console window", not MinTTY.  This does not work on MinTTY either, but the issue could be slightly different)
- Clone this repository
- Ensure that your yarn is clean (if yarnSetup is UP-TO-DATE, you will not see an error).  Run in the gradle-node-plugin-git-bash-issue folder:
```
rm -rf .gradle/yarn
```
- Run command
```
./gradlew yarnSetup
```

On Windows 10 in cmd.exe, gradle-node-plugin correctly sets the path of node when running 'yarnSetup'.  However, when run in git bash, the 'yarnSetup' task fails when trying to run the `node postinstall` command with the following error:

```
:yarnSetup
C:\Users\user\repo\gradle-node-plugin-git-bash-issue\.gradle\yarn\yarnpkg -> C:\Users\user\repo\gradle-node-plugin-git-bash-issue\.gradle\yarn\node_modules\yarn\bin\yarn.js
                                                                                                                       C:\Users\user\repo\gradle-node-plugin-git-bash-issue\.gradle\yarn\yarn -> C:\Users\user\repo\gradle-node-plugin-git-bash-issue\.gradle\yarn\node_modules\yarn\bin\yarn.js

> spawn-sync@1.0.15 postinstall C:\Users\user\repo\gradle-node-plugin-git-bash-issue\.gradle\yarn\node_modules\yarn\node_modules\spawn-sync
> node postinstall

'node' is not recognized as an internal or external command,
operable program or batch file.
C:\Users\user\repo\gradle-node-plugin-git-bash-issue\.gradle\yarn
`-- (empty)

                                                                                  npm ERR! Windows_NT 10.0.14393
npm ERR! argv "C:\\Users\\user\\repo\\gradle-node-plugin-git-bash-issue\\.gradle\\nodejs\\node-v6.9.1-win-x64\\node.exe" "C:\\Users\\user\\repo\\gradle-node-plugin-git-bash-issue\\.gradle\\nodejs\\node-v6.9.1-win-x64\\node_modules\\npm\\bin\\npm-cli.js" "install" "--prefix" "C:\\Users\\user\\repo\\gradle-node-plugin-git-bash-issue\\.gradle\\yarn" "yarn@0.17.10"
npm ERR! node v6.9.1
npm ERR! npm  v3.10.8
npm ERR! code ELIFECYCLE

npm ERR! spawn-sync@1.0.15 postinstall: `node postinstall`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the spawn-sync@1.0.15 postinstall script 'node postinstall'.
npm ERR! Make sure you have the latest version of node.js and npm installed.
npm ERR! If you do, this is most likely a problem with the spawn-sync package,
npm ERR! not with npm itself.
npm ERR! Tell the author that this fails on your system:
npm ERR!     node postinstall
npm ERR! You can get information on how to open an issue for this project with:
npm ERR!     npm bugs spawn-sync
npm ERR! Or if that isn't available, you can get their info via:
npm ERR!     npm owner ls spawn-sync
npm ERR! There is likely additional logging output above.

npm ERR! Please include the following file with any support request:
npm ERR!     C:\Users\user\repo\gradle-node-plugin-git-bash-issue\npm-debug.log
npm ERR! code 1
:yarnSetup FAILED
```

This command should use the node that is auto-downloaded by gradle-node-plugin.  But the path is not correctly set in git bash.  If you don't have node globally installed, you will see the error.

If you do have node globally installed, it will use the wrong version (not the version required by gradle-node-plugin).  This can lead to errors as the wrong version of node can be used to create node_modules, and then the user will get errors when running commands that require node-specific versions of packages.

The 'npmInstall' task works fine, it is only yarn that has this error.


