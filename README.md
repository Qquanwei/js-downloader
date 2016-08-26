#JS-DOWNLOADER

Features:
* download
* pause
* resume
* progress

## Install

* npm install js-downloader --save


## Usage

```
"use strict"
let fs = require('fs');
let download = require('js-downloader');

/*
 * defaultoption ={
 *  resume: true,  // resume switch
 *  output:{
 *    path: '/tmp',
 *    filename: $urlfilename,
 *  },
 *  headers:{}
 * }
  * */

download('http://localhost:8000/setup.exe',{}).then(function(req){
  /*
   * req = {
   *   request,
   *   path,   //download fullpath,will be write'
   *   resume, //if download server support resume and option.resume=true, resule is true,otherwise false
   * }
    * */

  let stream = fs.createWriteStream(req.path,{flags:req.resume?'a':'w'});

  req.request
    .on('end',()=>console.log('done'))
    .on('error',console.log)
    .on('progress',console.log)
    .pipe(stream);

  setTimeout(function(){
    stream.close(); // will be pause
  },10000);

}).catch(function(e){
  if(e.message === 'file is done'){
    //ignore , local file is already download
  }else
    console.log(e);
});
```
