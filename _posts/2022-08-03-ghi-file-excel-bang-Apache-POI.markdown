---
layout: home
title: "Hướng dẫn ghi file Excel bằng Apache POI"
date: 2022-08-03 16:11:30 +0700
---

Nếu bạn chưa có thư viện Apache POI thì hãy xem qua bài viết này [Hướng dẫn đọc file excel bằng Apache POI](https://phuongthe12.github.io/2022/08/03/doc-file-excel-bang-Apache-POI.html)

<p style="color: #610b38; font-size: 22px">Các bước ghi file Excel</p>
B1: Tạo project và add các thư viện cần thiết

Ví dụ đây là dữ liệu mình muốn ghi vào file excel
    ![]()
    ![ExcelRequest2.png](/img/ExcelRequest2.png)

### B2: Tạo class DemoWriteFileExcel và hàm main

    import java.io.FileNotFoundException;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import org.apache.poi.ss.usermodel.Cell;
    import org.apache.poi.ss.usermodel.Row;
    import org.apache.poi.xssf.usermodel.XSSFSheet;
    import org.apache.poi.xssf.usermodel.XSSFWorkbook;

    /**
    *
    * @author ppolo
    */
    public class DemoWriteFileExcel {

        public static void main(String[] args) {
            
        }

    }

### B3: Tạo đối tượng XSSFWorkbook và  XSSFSheet 
    XSSFWorkbook workbook = new XSSFWorkbook();
    XSSFSheet sheet = workbook.createSheet("book");//Tạo Sheet tên là book

### B4: Ví dụ mình đang muốn ghi vào file excel gồm danh sách tên tác giả như hình bên dưới

![]()
![ExcelResult3.png](/img/ExcelResult3.png)

### Do toàn bộ dữ liệu trong file mình muốn ghi là String -> mình sẽ tạo một mảng 2 chiều để lưu thông tin
    String[][] bookData = {
            {"Name", "Author"},
            {"Head First Java", "Kathy Serria"},
            {"Effective Java", "Joshua Bloch"},
            {"Clean Code", "Robert martin"},
            {"Thinking in Java", "Bruce Eckel"}};

### B5: Tạo một biến để lưu lại index dòng hiện tại
    int rowCount = 0;

### Tiếp theo đó mình sẽ tạo ra một vòng lặp để thực hiện việc ghi vào sheet(trang) mà mình đã tạo ra trước đó
            for (String[] aBook : bookData) {
            Row row = sheet.createRow(rowCount++);//Tạo ra một dòng mới có index là rowCount sau đó tăng biến rowCount lên 1

            int columnCount = 0; // biến columnCount để lưu lại cột hiện tại

            for (String field : aBook) {
                Cell cell = row.createCell(columnCount++);
                //Tạo ra một ô(cell) để lưu thông tin muốn ghi
                cell.setCellValue(field);
                //Thêm dữ liệu vào ô đó
            }

        }

### B6: Hiện tại dữ liệu đã được lưu vào sheet nhưng chưa được ghi ra file vì vậy mình sẽ tạo đối tượng FileOutputStream để thực hiện việc lưu file(tên file: DemoWriteExcel.xlsx)
    FileOutputStream outputStream = new FileOutputStream("DemoWriteExcel.xlsx");

### Sử dụng hàm write để lưu dữ liệu trong sheet(trang) vào file excel
    workbook.write(outputStream);
    System.out.println("Ghi thành công");


Đây là file excel sau khi ghi [FileExcel](/excel/DemoWriteExcel.xlsx)

[Source code ở đây](https://github.com/PhuongThe12/DemoReadFileExcel)

![]()
![]()
![]()
![]()
