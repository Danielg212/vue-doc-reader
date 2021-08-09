<h1 align="center">Welcome to vue-doc-reader 👋</h1>
<p>
  <img src="https://img.shields.io/npm/types/vue-doc-reader" />
  <img alt="Version" src="https://img.shields.io/badge/version-0.1.0-blue.svg?cacheSeconds=2592000" />
  <img src="https://img.shields.io/badge/node-%3E%3D10-blue.svg" />
  <a href="#" target="_blank">
    <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-yellow.svg" />
  </a>
</p>

> file input reader for docs

## Supported Formats

.xlsx
.xlsb
.xlsm
.xls
.xml
.csv 

## Prerequisites

- node >=10

## Install

```sh
npm i vue-doc-reader
```

## Usage

In main.ts
```ts
import VueDocReader from 'vue-doc-reader'
Vue.component('vue-doc-reader',VueDocReader)
```
In parent component template
```html
 <vue-doc-reader @onLoad="onLoad" />
```
### Props
* (optional) label:string - input label.
* (optional) includeRows:boolean - if ``true`` the table data will return with ``rowIndex`` property for each row.

### Callback
onLoad event callback return ``{data:Array<any>,headers:Array<string>}``
* ``data`` represents array of row objects ``[columnName:string]:value``
* ``headers`` represents array of columns name

```ts
  onLoad(results:any){
    this.data = results.data;
    this.headers = results.headers
  }
```



## Author

👤 **daniel212**


## Show your support

Give a ⭐️ if this project helped you!
