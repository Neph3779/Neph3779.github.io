---
layout: post
title: Strategy to Access Excel data
image:
categories: "Excel-Data-Driven"
tags:
  - ExcelData
  - Java
---



## Strategy to Access Excel data

1. Create object for XSSFWorkbook class
2. Get access to sheet
3. Get access to all raws of sheet
4. Access to specific row from all rows
5. Get access to all cell of Row
6. Access the data from excel into arrray

- - - -
오브젝트를 생성하기전에, `FileInputStream` 객체를 생성하여 읽어들일 준비를 하자
```java
//fileInputStream argument
FileInputStream fis = new FileInputStream("../../../testing_excelData.xlsx");
XSSFWorkbook workbook = new XSSFWorkbook(fis);

```

- - - -
어떤 sheet를 참조할 것인지 지정해줘야함.

```java
for (int I = 0; I<sheetsCount; I++) {
    if(workbook.getSheetName(i).equals(“testData”)) {
        XSSFSheet sheet = workbook.getSheetAt(i);
    }

}
```

위 코드에서는 `testData` 라는 sheet를 찾아주었다.

- - - -
일단 첫번째 행을 다 스캔해서 데이터가 어디까지 있는지를 확인하는 것이 좋다.

```xml
<properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>

```

혹시나 버전에 대한 에러가 난다면 위 프로퍼티 내용을 넣어주는게좋다.

- - - -
![](/assets/images/posts/ExcelDataDriven/image-20200505210221710.png)

헤더행의 데이터를 읽어들였다

```java
for (int I = 0; I<sheetsCount; I++) {
    if(workbook.getSheetName(i).equals(“testData”)) {
        XSSFSheet sheet = workbook.getSheetAt(i);

        Iterator<Row> rows = sheet.iterator(); //sheet is collection of rows
        Row firstRow = rows.next(); //access first row?

        Iterator<Cell> cells = firstRow.cellIterator(); //row is collection of cells
        int k = 0;
        int column;
        while(cells.hasNext()) {
            Cell value = cells.next();
            if(value.getStringCellValue().equals("TestCases")) {
                System.out.println("TestCases");
                System.out.println(value.getColumnIndex());
            }
            System.out.println(value.getColumnIndex());
        }
        //read column.
    }
}

```

- - - -
```java
for (int I = 0; I<sheetsCount; I++) {
    if(workbook.getSheetName(i).equals(“testData”)) {
        XSSFSheet sheet = workbook.getSheetAt(i);

        Iterator<Row> rows = sheet.iterator(); //sheet is collection of rows
        Row firstRow = rows.next(); //access first row?

        Iterator<Cell> cells = firstRow.cellIterator(); //row is collection of cells
        int column=0;
        while(cells.hasNext()) {
            Cell value = cells.next();
            if(value.getStringCellValue().equals("TestCases")) {
                System.out.println("TestCases");
                System.out.println(value.getColumnIndex());
                column = value.getColumnIndex();
            }
            System.out.println(value.getColumnIndex());
        }

        while(rows.hasNext()) {//read column.
            Row row = rows.next();
            if(row.getCell(column).getStringCellValue().equals(“Purchase”)) {
                Iterator<Cell> dataCell = row.cellIterator();
                while(dataCell.hasNext()) {
                    System.out.println( dataCell.next().getStringCellValue());

                }
            }
        }

    }

}

```
특정 헤더의 위치를 확인한 다음, 해당 컬럼의 row를 바꾸는 것으로 결과를 얻을 수 있음.



