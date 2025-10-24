# bai_tap_2_laptrinhweb
Bài tập 02: Lập trình web.
==============================
NGÀY GIAO: 19/10/2025
==============================
DEADLINE: 26/10/2025
==============================
1. Sử dụng github để ghi lại quá trình làm, tạo repo mới, để truy cập public, edit file `readme.md`:
   chụp ảnh màn hình (CTRL+Prtsc) lúc đang làm, paste vào file `readme.md`, thêm mô tả cho ảnh.
2. NỘI DUNG BÀI TẬP:
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
  + D:Apache24\conf\extra\httpd-vhosts.conf
  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)
- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  ví dụ sv tên là: `Đỗ Duy Cốp` thì tạo website với domain là fullname ko dấu, liền nhau: `doduycop.com`
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)
  + cài đặt vào thư mục `D:\nodejs`
- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`
  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`
  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*
  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
  + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`
2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name
2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880
- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus
- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php
- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`
  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây): 
  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem
  2. function : để tiền xử lý dữ liệu gửi đến
  3. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý
  4. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.
- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị
2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css
- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.
- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.
2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
==============================
TIÊU CHÍ CHẤM ĐIỂM:
1. y/c bắt buộc về thời gian: ko quá Deadline, quá: 0 điểm (ko có ngoại lệ)
2. cài đặt được apache và nodejs và nodered: 1đ
3. cài đặt được các thư viện của nodered: 1đ
4. nhập dữ liệu demo vào sql-server: 1đ
5. tạo được back-end api trên nodered, test qua url thành công: 1đ
6. tạo được front-end html css js, gọi được api, hiển thị kq: 1đ
7. trình bày độ hiểu về toàn bộ quá trình (mục 2.7): 5đ

# bài làm
cài apache
<img width="1920" height="889" alt="image" src="https://github.com/user-attachments/assets/9c336c6f-a29c-4552-bc42-ad6103d23762" />
<img width="1078" height="277" alt="image" src="https://github.com/user-attachments/assets/e0b99226-b7e7-4ec9-843b-729ff739db10" />
<img width="373" height="90" alt="image" src="https://github.com/user-attachments/assets/11272b97-fdce-41fb-b613-8ebf00bac21a" />
<img width="476" height="182" alt="image" src="https://github.com/user-attachments/assets/6a0b2c31-67b4-4c1c-b8bc-20cd4c9a36e7" />

cài node.js và nodered
<img width="1341" height="689" alt="image" src="https://github.com/user-attachments/assets/5e336f6a-4191-43cb-aac2-6af2f10be6df" />
<img width="410" height="288" alt="image" src="https://github.com/user-attachments/assets/c09104d3-a60c-4e34-975f-3fc747c6b17e" />
<img width="735" height="293" alt="image" src="https://github.com/user-attachments/assets/37171fb9-23f4-49b5-ba5c-6bc5083d3218" />
<img width="725" height="392" alt="image" src="https://github.com/user-attachments/assets/2f80fb23-0b55-4ffb-a644-42f63a5d0bc2" />
<img width="1919" height="318" alt="image" src="https://github.com/user-attachments/assets/a37718b2-4689-4dec-80ae-6feb0349690b" />
<img width="888" height="828" alt="image" src="https://github.com/user-attachments/assets/c2abb2df-11fe-4a44-81b0-0e8aaeaff906" />
database
<img width="826" height="326" alt="image" src="https://github.com/user-attachments/assets/40f34303-b9d7-4263-9bdd-298dcc782b94" />
cấu hình nodered
<img width="1296" height="526" alt="image" src="https://github.com/user-attachments/assets/d14016a7-38bf-427b-9639-0a73a38facb5" />

trang web http://luongquangha.com
<img width="1325" height="317" alt="image" src="https://github.com/user-attachments/assets/adc1886e-3717-42ad-8156-5591aac53863" />
khi ấn tìm kiếm 
<img width="1040" height="415" alt="image" src="https://github.com/user-attachments/assets/b125e5b8-c5ed-4ed1-b637-b45de8710693" />


# Nhận xét bài làm của mình

✅ Hiểu quá trình cài đặt phần mềm và thư viện:
Trong quá trình làm bài, em đã hiểu rõ các bước cài đặt môi trường làm việc, bao gồm Node.js, Node-RED, cũng như cách cài đặt và quản lý các thư viện cần thiết thông qua npm. Em nắm được vai trò của từng phần mềm và thư viện trong việc xây dựng hệ thống (Node.js làm nền tảng, Node-RED làm công cụ thiết kế luồng API, và các module hỗ trợ như http, mysql, mqtt, v.v.).

✅ Hiểu cách sử dụng Node-RED để tạo API back-end:
Em đã nắm được cách sử dụng Node-RED để thiết kế API back-end bằng cách kết hợp các node (HTTP In, Function, HTTP Response, Database, ...). Em hiểu được quy trình dữ liệu từ khi nhận request từ client → xử lý logic trong Node-RED → trả response lại cho người dùng. Việc cấu hình và triển khai luồng API bằng Node-RED trở nên dễ dàng và trực quan hơn.

✅ Hiểu cách front-end tương tác với back-end:
Em đã hiểu cách phần front-end (ví dụ: HTML/JavaScript hoặc ứng dụng web) gửi yêu cầu đến back-end thông qua các phương thức HTTP (GET, POST,...). Khi front-end gọi API do Node-RED tạo ra, dữ liệu được gửi đến server, xử lý, và phản hồi kết quả để hiển thị lại trên giao diện người dùng. Em đã nắm được mối liên kết giữa giao diện và hệ thống xử lý dữ liệu phía server.




