---
layout: home
title: "Hướng dẫn đọc mã QR Code bằng OpenCV Java"
---


<p class="sub-title" style="font-size: 22px; color: blueviolet;">OpenCV là gì?:</p>
OpenCV là một thư viện mã nguồn mở hàng đầu cho thị giác máy tính (computer vision), xử lý ảnh và máy học, và các tính năng tăng tốc GPU trong hoạt động thời gian thực. OpenCV được phát hành theo giấy phép BSD, do đó nó hoàn toàn miễn phí cho cả học thuật và thương mại. Nó có các interface C++, C, Python, Java và hỗ trợ Windows, Linux, Mac OS, iOS và Android. OpenCV được thiết kế để tính toán hiệu quả và với sự tập trung nhiều vào các ứng dụng thời gian thực. Được viết bằng tối ưu hóa C/C++, thư viện có thể tận dụng lợi thế của xử lý đa lõi. Được sử dụng trên khắp thế giới, OpenCV có cộng đồng hơn 47 nghìn người dùng và số lượng download vượt quá 6 triệu lần. Phạm vi sử dụng từ nghệ thuật tương tác, cho đến lĩnh vực khai thác mỏ, bản đồ trên web hoặc công nghệ robot.


Tải và cấu hình OpenCV:

1. Tải và cài đặt OpenCV [Downloads OpenCV](https://opencv.org/releases/)
    ![]()
    ![1.png](/img/1.png)

2. Tiến hành chạy file vừa tải xuống bạn sẽ nhận được folder opencv
    ![]()
    ![2.png](/img/2.png)

3. Tiến hành cấu hình OpenCV vào Apache NetBeans   
    ![]()
    Tạo project mới trong Apache NetBeans, click chuột phải vào Libraries -> Add JAR/Folder
    Tìm đến thư mục opencv -> build -> java -> chọn opencv-460.jar(Số hiệu phiên bản tùy vào bản cài đặt trước đó)

4. Cấu hình VMOptions:
    ![]()
    Tạo đường dẫn đến opencv: click chuột phải vào project chọn properties -> chọn run -> Chỏ đường dẫn đến opencv (Ví dụ của mình là: -Djava.library.path="C:\Users\ppolo\Downloads\opencv\build\java\x64")
    ![3.png](/img/3.png)


<p class="sub-title" style="font-size: 22px; color: blueviolet;">Tải và cấu hình ZXing</p>
Để tạo/đọc QR Code trong java, chúng ta cần sử dụng một thư viện thứ 3 có tên là <strong>ZXing(Zebra Crossing)</strong>. Bạn có thể tải các libraries gốc [Tại đây](https://jar-download.com/?search_box=%20zxing) Hoặc:

- [zxing core-3.3.0.jar](https://repo1.maven.org/maven2/com/google/zxing/core/3.3.0/core-3.3.0.jar)
    
- [zxing javase-3.3.0.jar](https://repo1.maven.org/maven2/com/google/zxing/javase/3.3.0/javase-3.3.0.jar)

Sau khi tải 2 file trên tiến hành thêm thư viện vào project.
    ![]()
    ![4.png](/img/4.png)

<p class="sub-title" style="font-size: 22px; color: blueviolet;">Demo đọc QR Code java swing</p>
B1: Tạo một project mới thêm thư viện OpenCV, ZXing và cấu hình như các bước trên
B2: Tạo mới một JFrame (DemoReadQRCode) có giao diện như hình:
    ![]()
    ![5.png](/img/5.png)

B3: Tạo một biến toàn cục VideoCapture, 

    import com.google.zxing.BinaryBitmap;
    import com.google.zxing.MultiFormatReader;
    import com.google.zxing.NotFoundException;
    import com.google.zxing.Result;
    import com.google.zxing.client.j2se.BufferedImageLuminanceSource;
    import com.google.zxing.common.HybridBinarizer;
    import java.awt.image.BufferedImage;
    import java.io.ByteArrayInputStream;
    import java.io.IOException;
    import java.io.InputStream;
    import javax.imageio.ImageIO;
    import javax.swing.ImageIcon;
    import org.opencv.core.Core;
    import org.opencv.core.Mat;
    import org.opencv.core.MatOfByte;
    import org.opencv.imgcodecs.Imgcodecs;
    import org.opencv.videoio.VideoCapture;

    /**
    *
    * @author ppolo
    */
    public class DemoReadQRCode extends javax.swing.JFrame {

        private VideoCapture capture;//Camera
        private Thread th;


Vì JFrame đang chiếm giữ luồng chạy chính nên chúng ta cần tạo thêm một luồng mớ để sử dụng camera

Tạo hàm mới private Result deCode(byte[] imageData) nhận vào là một mảng byte chứa hình ảnh, và trả về Result(là kết quả khi giải mã đoạn QR Code)


    private Result deCode(byte[] imageData) {
        try {
            InputStream is = new ByteArrayInputStream(imageData);
            BufferedImage bf = ImageIO.read(is);

            BinaryBitmap binaryBitmap
                    = new BinaryBitmap(new HybridBinarizer(new BufferedImageLuminanceSource(bf)));

            Result result = new MultiFormatReader().decode(binaryBitmap);

            if (result != null) {
                capture.release();
                this.dispose();               
                
                return result;

            }
        } catch (IOException ex) {
            ex.printStackTrace();
        } catch (NotFoundException ex) {
    //        System.out.println("Không chứa code");
    //        ex.printStackTrace();
        }
        return null;
    }
    

B4: Tạo hàm private void readQRCode() để mở camera và giải mã QR Code

    private void readQRCode() {
        //Load thư viện
        System.loadLibrary(Core.NATIVE_LIBRARY_NAME);
        capture = new VideoCapture(0);//gán VideoCapture là camera gốc của máy
        Mat image = new Mat();
        byte[] imageData;//Tạo mảng byte lưu trữ hình ảnh

        while (true) {
            capture.read(image);
            MatOfByte buf = new MatOfByte();
            Imgcodecs.imencode(".jpg", image, buf);
            imageData = buf.toArray();
            ImageIcon icon = new ImageIcon(imageData);
            this.lblScreen.setIcon(icon);//load ảnh vào lblScreen

            Result result = deCode(imageData);
            if (result != null) {
                System.out.println(result);
                th.stop();
                }
            }
        }

B5: Set action performed button Open Camera

    private void btnOpenActionPerformed(java.awt.event.ActionEvent evt) {                                        
        th = new Thread(new Runnable() {
            @Override
            public void run() {
                readQRCode();
            }
        });
        th.start();
    }    


B6: Set action performed button Close Camera

    private void btnCloseActionPerformed(java.awt.event.ActionEvent evt) {                                         
        if (capture != null) {
            if(th != null){
                th.stop();
            }
            lblScreen.setIcon(null);
            capture.release();
        }
    }     

.
    ![]()
    ![6.png](/img/6.png)
