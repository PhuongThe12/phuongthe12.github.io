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
B2: Tạo class DemoWriteFileExcel

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

            XSSFWorkbook workbook = new XSSFWorkbook();
            XSSFSheet sheet = workbook.createSheet("book");//Tạo Sheet tên là book

            String[][] bookData = {
                {"Name", "Author"},
                {"Head First Java", "Kathy Serria"},
                {"Effective Java", "Joshua Bloch"},
                {"Clean Code", "Robert martin"},
                {"Thinking in Java", "Bruce Eckel"}};

            int rowCount = 0;

            for (String[] aBook : bookData) {
                Row row = sheet.createRow(rowCount++);

                int columnCount = 0;

                for (String field : aBook) {
                    Cell cell = row.createCell(columnCount++);

                    cell.setCellValue(field);

                }

            }

            try ( FileOutputStream outputStream = new FileOutputStream("DemoWriteExcel.xlsx")) {// Ghi file
                workbook.write(outputStream);
                System.out.println("Ghi thành công");
            } catch (FileNotFoundException ex) {
                ex.printStackTrace();
            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }

    }

B3: Kết quả
    ![]()
    ![ExcelResult3.png](/img/ExcelResult3.png)
    [FileExcel](/excel/DemoWriteExcel.xlsx)
