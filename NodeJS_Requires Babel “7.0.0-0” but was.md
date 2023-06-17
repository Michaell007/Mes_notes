# Requires Babel “7.0.0-0” but was loaded with “6.26.3”

Best Solution https://itecnote.com/tecnote/requires-babel-7-0-0-0-but-was-loaded-with-6-26-3/
Test which version you are running with cmd
```
babel -V
```
If it is not verion 7 or higher
```
npm uninstall babel-cli -g
npm uninstall babel-core -g
```
And
```
npm install @babel/cli -g
npm install @babel/core -g
```

## For me the issue was solved by installing 'babel-node' globally by running this command:

```javascript
npm install @babel/node -g
```