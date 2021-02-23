# CSV2JSON
Convert CSV to automatically nested JSON in Golang.

This project transforms CSV to JSON without struct and allows recreating nested object using delimiters. <br/>
The Reason for this tool is I looked for golang examples but found most of it using struct, so I built this to be flexible enough for any file.

## Table of Contents
<!--ts-->
   + [Usage](#how-to-use-this-tool)
   + [Examples](#example)
      + [Basic](#basic-transformation)
      + [Nested](#nested-transformation)
        + [Object](#object)
        + [Array Object](#array-object)
        + [Multi-Level Object](#multi-level-object)
        + [Multi-level Array with Internal Object](#multi-level-array-with-internal-objects)
   + [License](#license)
<!--te-->

## How to use this tool:
* After Downloading the go file you can run
`go run main.go -path={path to csv file} > {path to output json file}` example `go run main.go -path=/tmp/data.csv > /tmp/output.json`
* you will have a new file in the output location as the csv one but with JSON extension.

## Example:

### Basic Transformation
If I have a csv file contains a test data `test.csv`:
```csv
Id,Name,Age
1,Aditya,26
2,Raj,22
```
and we need to convert it to json file:

`go run main.go -path=/tmp/test.csv > /tmp/output.json`

after writing this command you will get another file in the `tmp` directory called `output.json` with the data in the new format:
```json
[
  {
    "Age":"26",
    "Id":"1",
    "Name":"Aditya"
  },
  {
    "Age":"22",
    "Id":"2",
    "Name":"Raj"
  }
]
```
and that's it for basic transformation.

### Nested Transformation

#### Object
If I have a csv file contains a test data with employee information as internal object `test.csv`:
```csv
id,employee.name,employee.age
1,Aditya,26
2,Raj,22
```
and we need to convert it to json file:

`go run main.go -path=/tmp/test.csv > /tmp/output.json`

after writing this command you will get another file in the `tmp` directory called `output.json` with the data in the new format:
```json
[
   {
      "employee":{
         "age":"26",
         "name":"Aditya"
      },
      "id":"1"
   },
   {
      "employee":{
         "age":"22",
         "name":"Raj"
      },
      "id":"2"
   }
]
```

#### Array Object
If I have a csv file contains a test data with info as array `test.csv`:
```csv
id,employee.name,employee.age,employee.info[0],employee.info[1],employee.info[2]
1,Aditya,26,--AdditionalInfo1--,--AdditionalInfo2--,--AdditionalInfo3--
2,Raj,22,--Info1--,--Info2--,--Info3--
```
and we need to convert it to json file:

`go run main.go -path=/tmp/test.csv > /tmp/output.json`

after writing this command you will get another file in the `tmp` directory called `output.json` with the data in the new format:
```json
[
   {
      "employee":{
         "age":"26",
         "info":[
            "--AdditionalInfo1--",
            "--AdditionalInfo2--",
            "--AdditionalInfo3--"
         ],
         "name":"Aditya"
      },
      "id":"1"
   },
   {
      "employee":{
         "age":"22",
         "info":[
            "--Info1--",
            "--Info2--",
            "--Info3--"
         ],
         "name":"Raj"
      },
      "id":"2"
   }
]
```

#### Multi-level Object
If I have a csv file contains a test data with multi-level object `test.csv`:
```csv
id,employee.name.firstname,employee.name.lastname,employee.age,employee.info[0],employee.info[1],employee.info[2]
1,Aditya,Raj,26,--AdditionalInfo1--,--AdditionalInfo2--,--AdditionalInfo3--
2,Raj,Aditya,22,--Info1--,--Info2--,--Info3--
```
and we need to convert it to json file:

`go run main.go -path=/tmp/test.csv > /tmp/output.json`

after writing this command you will get another file in the `tmp` directory called `output.json` with the data in the new format:
```json
[
   {
      "employee":{
         "age":"26",
         "info":[
            "--AdditionalInfo1--",
            "--AdditionalInfo2--",
            "--AdditionalInfo3--"
         ],
         "name":{
            "firstname":"Aditya",
            "lastname":"Raj"
         }
      },
      "id":"1"
   },
   {
      "employee":{
         "age":"22",
         "info":[
            "--Info1--",
            "--Info2--",
            "--Info3--"
         ],
         "name":{
            "firstname":"Raj",
            "lastname":"Aditya"
         }
      },
      "id":"2"
   }
]
```

#### Multi-level Array with internal objects
If I have a csv file contains a test data with multi-level object with arrays `test.csv`:
```csv
id,employee.name.firstname,employee.name.lastname,employee.age,employee.info[0].title,employee.info[0].content,employee.info[1].title,employee.info[1].content
1,Aditya,Raj,26,--AdditionalInfoTitle1--,InfoContent1,--AdditionalInfo2--,InfoContent2
2,Raj,Aditya,22,--Info1--,--Content1--,--Info2--,--Content2--
```
and we need to convert it to json file:

`go run main.go -path=/tmp/test.csv > /tmp/output.json`

after writing this command you will get another file in the `tmp` directory called `output.json` with the data in the new format:
```json
[
   {
      "employee":{
         "age":"26",
         "info":[
            {
               "content":"InfoContent1",
               "title":"--AdditionalInfoTitle1--"
            },
            {
               "content":"InfoContent2",
               "title":"--AdditionalInfo2--"
            }
         ],
         "name":{
            "firstname":"Aditya",
            "lastname":"Raj"
         }
      },
      "id":"1"
   },
   {
      "employee":{
         "age":"22",
         "info":[
            {
               "content":"--Content1--",
               "title":"--Info1--"
            },
            {
               "content":"--Content2--",
               "title":"--Info2--"
            }
         ],
         "name":{
            "firstname":"Raj",
            "lastname":"Aditya"
         }
      },
      "id":"2"
   }
]
```


## License:
[The MIT License](https://github.com/Ahmad-Magdy/CSV-To-JSON-Converter/blob/master/LICENSE)