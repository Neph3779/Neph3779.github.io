---
layout: post
title: Excel Driven Testing
image:
categories: "Excel-Data-Driven"
tags:
  - ExcelData
  - Java
---



```java
        while(rows.hasNext()) {//read column.
            Row row = rows.next();
            if(row.getCell(column).getStringCellValue().equals(testcase)) {
                Iterator<Cell> dataCell = row.cellIterator();
                while(dataCell.hasNext()) {
                    arraylist.add(dataCell.next().getStringCellValue());

                }
            }
        }

    }

}

return arraylist;

```
return을 하여서 필요한 테스트케이스에서 데이터를 사용할 수 있도록 하는 것이 좋음.

- - - -
현재는 String ArrayList로 하였으나, 셀 안에 숫자, 정수가 포함되어있으면...??

if문으로 데이터값을 실행하여 실행되는 코드의 분기를 만들어주면 됨.
```java
while(dataCell.hasNext()) {
    Cell value = dataCell.next();
    if(value.getCellType() == CellType.STRING) {
        arraylist.add(value.getStringCellValue());
    } else {
        arraylist.add(arraylist.add(NumberToTextConverter.toText(value.getNumericCellValue()));
    }
}

```

`NumberTotext()` 를 이용하여 숫자값을 텍스트형태로 변환해주는게 무난하다.