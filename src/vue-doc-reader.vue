<template>
  <div class="vue-doc-reader"> 
     <label for="file" v-if="label">Spreadsheet</label>
     <input type="file" class="form-control" id="file" :accept="SheetJSFT" @change.prevent="readFile($event)" />
     <div> {{fileSize}}</div>
    
  </div>
</template>
<script lang="ts">
  import Vue from 'vue';
  import Component from 'vue-class-component';
  import {Prop} from "vue-property-decorator";

  import * as XLSX from 'xlsx';

  @Component({})
  export default class VueDocReaderComponent extends Vue {

  @Prop({default: false}) visible!: boolean
  @Prop({default: null}) label!: string;


  private data:Array<any> = new Array<any>()
  private headers:Array<string> = new Array<string>()
  public fileSize:string | undefined = '';


  SheetJSFT = [
    "xlsx", "xlsb", "xlsm", "xls", "xml", "csv", "txt", "ods", "fods", "uos", "sylk", "dif", "dbf", "prn", "qpw", "123", "wb*", "wq*", "html", "htm"
  ].map(function(x) { return "." + x; }).join(",");

  make_cols = (refstr:Array<string>) => refstr.map((item:any)=> {return {text:item,value:item}})

  get mimeTypes(){
    return ["text/csv", "text/x-csv", "application/vnd.ms-excel", "text/plain",
      "application/zip",
      "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"]
  }

readFile(event: any) {
    const files = event.target?.files;

    if (files && files[0]) this.processFile(files[0]);

  }

  processFile(file: File) {
    let reader = new FileReader()
    this.fileSize = this.clcFileSize(file.size)
    
    reader.onload = (e: any) => {
      let data = e.target.result;
      let workbook = XLSX.read(data, {
        type: 'binary'
      });

      workbook.SheetNames.forEach((sheetName) => {
        var XL_row_object = XLSX.utils.sheet_to_json(workbook.Sheets[sheetName],);
        var cols = this.get_header_row(workbook.Sheets[sheetName])
        this.data = XL_row_object;
        this.headers = cols;
      })

      reader.onloadend = () => {
        this.$emit('onLoad', {data: this.data, headers: this.headers})
      }

    };

    reader.onerror = function (ex) {
      console.log(ex);
    };

    reader.readAsBinaryString(file);
  }

  get_header_row(sheet:any) {
    var headers = [];
    var range = XLSX.utils.decode_range(sheet['!ref']);
    var C, R = range.s.r; /* start in the first row */
    /* walk every column in the range */
    for(C = range.s.c; C <= range.e.c; ++C) {
      var cell = sheet[XLSX.utils.encode_cell({c:C, r:R})] /* find the cell in the first row */

      var hdr = "UNKNOWN " + C;
      if(cell && cell.t) hdr = XLSX.utils.format_cell(cell);

      headers.push(hdr);
    }
    return headers;
  }

  clcFileSize(number:number) {
  if(number < 1024) {
    return number + 'bytes';
  } else if(number >= 1024 && number < 1048576) {
    return (number/1024).toFixed(1) + 'KB';
  } else if(number >= 1048576) {
    return (number/1048576).toFixed(1) + 'MB';
  }
}

    
  }

</script>

<style scoped lang="scss">
  .vue-doc-reader {

  }
</style>
