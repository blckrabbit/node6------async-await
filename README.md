# node6------async-await
async\await只有ES7对应的node6之后的版本支持，如果node6、ES6支持这种需要添加对应babel支持

解决nodejs不支持async和await关键字的问题

1、$ npm install --save-dev babel-cli 
2、$ npm install --save-dev babel-preset-es2015 babel-preset-es2017  
3、Create .babelrc in the project root folder with the following contents:
 {
 "presets": ["es2015","es2017"] 
 }

4、Run your script with babel-node

$ babel-node async.js


