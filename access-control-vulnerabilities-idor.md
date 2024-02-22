# Access control vulnerabilities - IDOR

**Objective**:

* We will explore the concept of Insecure Direct Object References (IDOR) and understand how it poses a significant security threat to web applications.

In this lab, students need to:

*   Answer the following questions:

    * What is an Insecure Direct Object Reference (IDOR), and how does it present a security risk in web applications?

    &#x20;Insecure Direct Object Reference (IDOR) là một lỗ hổng trong ứng dụng web xảy ra khi ứng dụng tiết lộ thông tin hoặc chức năng nhạy cảm bằng cách tham chiếu trực tiếp các đối tượng, chẳng hạn như tệp hoặc bản ghi cơ sở dữ liệu mà không có kiểm tra ủy quyền thích hợp. Nói cách khác, nó xảy ra khi kẻ tấn công có thể thao túng các tham số trong yêu cầu web để truy cập các tài nguyên mà họ không được phép truy cập.

    * How can attackers exploit IDOR vulnerabilities in a website, and what are some common techniques used in such attacks?



    Kẻ tấn công có thể tận dụng các lỗ hổng IDOR trong một trang web bằng cách thực hiện các cuộc tấn công như sau:

    1. Truy cập trực tiếp vào các đối tượng không được phép: Kẻ tấn công có thể thay đổi các tham số trong yêu cầu HTTP để truy cập vào các đối tượng không được phép, chẳng hạn như thông tin cá nhân của người dùng khác, tài liệu không công khai hoặc các tính năng quản trị.
    2. Thay đổi dữ liệu của các đối tượng: Khi đã truy cập được vào các đối tượng, kẻ tấn công có thể thực hiện các thay đổi không được ủy quyền trong dữ liệu của các đối tượng, chẳng hạn như thay đổi thông tin cá nhân, sửa đổi đơn hàng hoặc xóa dữ liệu.

    Một số kỹ thuật phổ biến được sử dụng trong các cuộc tấn công IDOR bao gồm:

    * Thay đổi giá trị của các tham số trong yêu cầu HTTP: Kẻ tấn công có thể thay đổi các giá trị của các tham số trong URL hoặc dữ liệu biểu mẫu để truy cập hoặc thay đổi các đối tượng không được phép.
    * Sử dụng các công cụ tự động hoặc các skript: Các công cụ tự động như Burp Suite hoặc OWASP ZAP có thể được sử dụng để tự động hóa quá trình thử nghiệm các lỗ hổng IDOR bằng cách thay đổi các tham số và kiểm tra các phản hồi.
    * Dò tìm và khai thác lỗ hổng: Kẻ tấn công có thể sử dụng các kỹ thuật dò tìm tự động hoặc thủ công để tìm lỗ hổng IDOR trong ứng dụng web, sau đó tận dụng chúng để thực hiện các cuộc tấn công không ủy quyền.
    * Sử dụng cơ sở dữ liệu lỗi: Kẻ tấn công có thể sử dụng các cơ sở dữ liệu lỗi như ngữ khí SQL để thực hiện các cuộc tấn công IDOR bằng cách thay đổi các truy vấn hoặc truy cập vào dữ liệu không được phép.
    * What types of functionality or data in a website can be affected as a result of an IDOR vulnerability being exploited?

    &#x20;      Database, dữ liệu nhạy cảm như căn cước công dân, các file..
* Perform challenge:
  * [Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)
* Explain and capture all steps (full windows screen capture).
* **Challenge:** [Insecure direct object references](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)

This lab stores user chat logs directly on the server's file system, and retrieves them using static URLs.

Solve the lab by finding the password for the user `carlos`, and logging into their account.

ở bài lab này, ta sẽ tiến hành cuộc tấn idor hay còn gọi là đi cửa sau vào website. Theo như mô tả đầu bài lab thì logs của các lần chat sẽ được lưu ngay tại server’file system và có thế get qua các static url

![](<.gitbook/assets/0 (2).png>)

ở bài lab lần này, ta không được cung cấp tài khoản để đăng nhập. tuy nhiên, website sẽ có 1 chức năng khác là live chat. Vậy thì ta sẽ thử tính năng này của web xem sao. Ta sẽ chat vài câu vs bot của server

![](<.gitbook/assets/1 (2).png>)

ở dưới cuộc hội thoại, ta có thể view transcript qua file tải về. ở đây file có tên là 2.txt. lúc đầu mình cũng không nghi ngở gì tên file, cho đến khi mình tiếp tục tải các transcript tiếp theo về có tên lần lượt là 3.txt,4.txt và 5.txt. vậy là file được sắp xếp theo thứ tự tang dần, tuy nhiên ta lại k có file 1.txt. bắt đầu nảy sinh nghi ngờ

![](<.gitbook/assets/2 (2).png>) vào burpsuite quan sát request, ta sẽ lấy request download transcript gửi về repeater. Tại repeater mình sẽ sửa get 2.txt thành 1.txt

![](<.gitbook/assets/3 (2).png>)

Và cuộc hội thoại bí ẩn đã hiện ra, ta biết được mật khẩu là z507blvs07qeiuqbb0rw. Đọc trên đề bài yêu cầu ta tìm password của carlos. Vậy ta có thể đoán password này là của carlos

![](<.gitbook/assets/4 (2).png>)

Nhập username carlos và password : z507blvs07qeiuqbb0rw. Ta đã thành công đăng nhập vào website
