---
layout: home
title: "Hướng dẫn đọc file Excel bằng Apache POI"
date: 2022-08-03 16:11:30 +0700
---

Trong Java, JDK không cung cấp trực tiếp API để đọc hay ghi các file Microsoft Excel vì vậy chúng ta cần đến thư viện thứ 3 đó là Apache POI.

<p style="color: #610b38; font-size: 22px">Aoache POI là gì?</p>

<strong>Apache POI</strong>(Poor Obfuscation Implementation) là một Java API để đọc và ghi các file Microsoft Documents. Thư viện Apache POI cung cấp 2 cách triển khai để đọc và ghi file Excel:

- HSSF (Horrible SpreadSheet Format) Implementation: Sử dụng cho các file Excel 2003 hoặc những phiên bản thấp hơn.

- XSSF (XML SpreadSheet Format) Implementation: Sử dụng cho các file Excel 2007 hoặc những phiên bản cao hơn. 


<p style="color: #610b38; font-size: 22px">Các bước đọc file Excel</p>

### B1: Tạo một project mới.

### B2: Tải các thư viện 
- [commons-collections4-4.1.jar](https://bit.ly/2SG4r3Y)
- [poi-3.17.jar](https://bit.ly/2Y6HRY9)
- [poi-ooxml-3.17.jar](https://bit.ly/2LJ1leO)
- [poi-ooxml-schemas-3.17.jar](https://bit.ly/2LHjeL9)
- [xmlbeans-2.6.0.jar](https://bit.ly/2ybqxBR)

### B3: Thêm các thư viện vào project: click chuột phải vào project -> ADD JAR/Folder -> Chọn các file Jar mới tải
![]()
![ExcelLib.png](/img/ExcelLib.png)

### B4: Chuẩn bị File Excel
Đây là [file Excel mẫu](/excel/DemoExcel.xlsx)

### B5: Tạo class DemoReadFileExcel
    import java.io.File;
    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.IOException;
    import java.util.Iterator;
    import org.apache.poi.ss.usermodel.Cell;
    import org.apache.poi.ss.usermodel.Row;
    import org.apache.poi.ss.usermodel.Sheet;
    import org.apache.poi.ss.usermodel.Workbook;
    import org.apache.poi.xssf.usermodel.XSSFSheet;
    import org.apache.poi.xssf.usermodel.XSSFWorkbook;

    /**
    *
    * @author ppolo
    */
    public class DemoReadFileExcel {
        
    }

### B6: Tạo hàm readAll(File file) thực hiện việc đọc toàn bộ dữ liệu có trong file excel, hàm này nhận vào file excel bạn muốn đọc 

    public static void readAll(File file) {
        
    }

### Tạo ra đối tượng FileInputStream để đọc file
    FileInputStream fis = null;
        try {
            fis = new FileInputStream(file);
            
            }

                System.out.println("");

            }

            fis.close();
        } catch (FileNotFoundException ex) {
            ex.printStackTrace();
        } catch (IOException ex) {
            ex.printStackTrace();
        }

### Tạo ra đối tượng XSSFWorkbook và XSSFSheet 
    XSSFWorkbook wb = new XSSFWorkbook(fis);
    XSSFSheet sheet = wb.getSheetAt(0);//Lấy trang đầu tiên trong file excel
    Iterator<Row> itr = sheet.iterator();//Lấy ra các dòng

### Do trong file excel có các dòng và các cột thứ tự các dòng. Các cột sẽ có index từ 0 và tăng 1 và các dòng cũng tương tự như vậy

![]()
![ExcelIndex.png](/img/ExcelIndex.png)

### B7: Để thực hiện việc đọc file mình sẽ tạo ra một vòng lặp 

    //Vòng lặp đầu sẽ lấy dòng tiếp theo đến khi không còn dòng nào
    while (itr.hasNext()) {//Lặp đến khi hết các dòng trong excel
        Row row = itr.next();//Lấy dòng tiếp theo
        Iterator<Cell> cellItr = row.iterator(); // Lấy ra các ô trong dòng row
        
        //Vòng lặp này sẽ lấy ra các ô trong dòng cho đến khi không còn ô nào
        while (cellItr.hasNext()) {
            Cell cell = cellItr.next(); //Lấy ra ô(cell) tiếp theo
            switch (cell.getCellType()) {
                case Cell.CELL_TYPE_STRING:   //Nếu trong ô có kiểu dữ liệu là text
                    System.out.print(cell.getStringCellValue() + "\t\t\t");
                    break;
                case Cell.CELL_TYPE_NUMERIC:    //Nếu trong ô có kiểu dữ liệu là number
                    System.out.print(cell.getNumericCellValue() + "\t\t\t");
                    break;
                default:
            }
            
        }
        //Xuống dòng sau các dòng
        System.out.println("");

    }
    fis.close();//giải phóng fis


### B8:Tạo hàm readByCell(File file, int vRow, int vColumn) để đọc một ô bất kỳ trong file excel. Hàm này sẽ nhận vào file excel, thứ tự dòng và thứ tự cột và tiến hành in ra màn hình

    public static void readByCell(File file, int vRow, int vColumn) {
        Workbook wb = null;
        try {

            FileInputStream fis = new FileInputStream(file);

            wb = new XSSFWorkbook(fis);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e1) {
            e1.printStackTrace();
        }
        Sheet sheet = wb.getSheetAt(0);   //Lấy trang đầu tiên trong file excel
        Row row = sheet.getRow(vRow); //Lầy dòng thứ vRow
        Cell cell = row.getCell(vColumn); //Lấy ô thứ vColumn trong dòng thứ vRow

        System.out.println(cell.getStringCellValue());//In ra giá trị trong ô

    }


### B9: Tạo hàm main để tiến hành chạy chương trình

-   Đọc file excel với hàm readAll()

        public static void main(String[] args) {
            File file = new File("DemoExcel.xlsx");
            readAll(file);
        }

    ![]()
    ![ExcelResult1.png](/img//ExcelResult1.png)


- Đọc file excel với hàm readByCell(), Ví dụ mình đang muốn lấy thông tin của dòng 5 cột B thì index khi nhập vào hàm readByCell(file, 4, 1)
    ![]()
    ![ExcelRequest1.png](/img//ExcelRequest1.png)


        public static void main(String[] args) {
            File file = new File("DemoExcel.xlsx");
            readByCell(file, 4, 1);
        }

Và đây là kết quả
    ![]()
    ![ExcelResult2.png](/img/ExcelResult2.png)


[Source code ở đây](https://github.com/PhuongThe12/DemoReadFileExcel)

![]()
![]()
![]()
![]()


