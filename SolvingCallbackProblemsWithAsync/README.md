## callback basic example
```
var fs  = require('fs');
fs.readFile(
  'text.txt',      
  'utf8',                 
  function(err,text) {   
    console.log('Error:',err);    
    console.log('Text:',text);    //the contents of the file
  }
);
console.log('This logged before the contents of the text file');
```
You want to display the contents of two files in a particular order.
```
var fs  = require('fs');
fs.readFile(
  'file1.txt',      //file1
  'utf8',                 
  function(err,text) {    
    if (err) {
      console.error(err);           
    } else {
      console.log('First text file:',text);    
      fs.readFile(
        'file2.txt',  //file2
        'utf8',                   /
        function(err,text) {      
          if (err) {
            console.error(err);                      
          } else {
            console.log('Second text file:',text);    
          }
        }
      );
    }
  }
);
```
##async basic example
using async:  
###async.series
```
var async = require('async'), fs = require('fs');
 
async.series(                     //execute the functions in the first argument one after another
  [                               //The first argument is an array of functions
    function(cb) {                
      fs.readFile(
        'file1.txt', 
        'utf8',
        cb
      );
    },
    function(cb) {
      fs.readFile(
        'file2.txt', 
        'utf8',
        cb
      );
    }
  ],
  function(err,values) {          //Note the param name `values`
    if (err) {                    
      console.error(err);
    } else {                      //values[0] and values[1]
      console.log('First text file:',values[0]);
      console.log('Second text file:',values[1]);
    }
  }
);
```
With async.js, the error handling is simplified because if it encounters an error,
it returns the error to the argument of the final callback and will not execute any further asynchronous functions.  

###async.parallel
```
var async = require('async'), fs = require('fs');
 
async.parallel(                     //execute the functions in the first argument, but don't wait for the first function to finish to start the second
  [                               //The first argument is an array of functions
    function(cb) {                
      fs.readFile(
        'file1.txt', 
        'utf8',
        cb
      );
    },
    function(cb) {
      fs.readFile(
        'file2.txt', 
        'utf8',
        cb
      );
    }
  ],
  function(err,values) {          //Note the param name `values`
    if (err) {                    
      console.error(err);
    } else {                      //values[0] and values[1]
      console.log('First text file:',values[0]);
      console.log('Second text file:',values[1]);
    }
  }
);
```
