# SQL injection

### Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

ở bài lab này, ta sẽ tập trung vào tìm kiếm lỗ hổng trong database của 1 website.

<figure><img src=".gitbook/assets/image (4) (1) (1).png" alt=""><figcaption><p>giao diện website</p></figcaption></figure>

nhìn qua webstie này ta biết nó trưng bày các mặt hàng. vậy ta sẽ thử click vào một mặt hàng bất kì xem sao.

<figure><img src=".gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

khi ta chọn accessories, thì trên thanh url của website sẽ hiện category=Accessories. ở đây mình suy nghĩ là "sẽ ra sao nếu chúng ta chèn 1 kí tư lạ vào category"

<figure><img src=".gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption><p>website sau khi chèn kí tự lạ</p></figcaption></figure>

ở đây mình sử dụng "category=' ". và trang web đã hiện ra lỗi hệ thống thay vì trả về kết quả thông thường như "không tìm thấy " hay hiện ra list đồ. từ đó, ta biết được website đã có lỗi trong database. để khai thác vào lỗi này mình sủ dụng category=%27+OR+1=1--

<figure><img src=".gitbook/assets/image (3) (1) (1) (1).png" alt=""><figcaption><p>kết quả cuộc tấn công</p></figcaption></figure>

## Lab: [SQL injection](https://portswigger.net/web-security/sql-injection) attack, querying the database type and version on Oracle

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string

&#x20;**Hint**

On Oracle databases, every `SELECT` statement must specify a table to select `FROM`. If your `UNION SELECT` attack does not query from a table, you will still need to include the `FROM` keyword followed by a valid table name.

There is a built-in table on Oracle called `dual` which you can use for this purpose. For example: `UNION SELECT 'abc' FROM dual`

For more information, see our [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet).

<figure><img src=".gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption><p>giao diện bài lab</p></figcaption></figure>

ở bài lab này, tác giả yêu cầu ta khai thác version của trang web này vả tiết lộ rằng database của trang web này sử dụng phần mềm cùa oracle. đầu tiên ta sẽ thao tác cơ bản với web và bắt gói tin tại burp-suite. tại http history. ta sẽ gửi GET /filter?category=Gifts đến repeater.

<figure><img src=".gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

tại đây ta sẽ thử tìm lỗi sql thông qua câu lệnh cơ bản '+SELECT+'abc'+FROM+dual-- và trang web trả về lỗi server. câu truy vấn này lỗi bởi vì mình thiếu UNION để thực thi 2 câu truy vấn cùng lúc và cùng với đó mình nhận ra rằng câu truy vấn trả vè một bảng bị lỗi bới vì qua quan sát, mình nhìn thấy nội dung trả về của trang web sẽ là title + script. vì vậy, ta sẽ thay đổi câu truy vấn của mình 1 chút.

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt=""><figcaption><p>sau khi thay đổi câu truy vấn</p></figcaption></figure>

mình sẽ thay đổi thành '+UNION+SELECT+'abc','def'+FROM+dual-- . trang web đã in ra abc def như ta yêu cầu. vì vậy, chúng ta đã tìm được cách ép server in ra mà ta mong muốn. việc còn lại là tìm ra câu lệnh in ra phiên bản của database như yêu cầu.

<figure><img src=".gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

ta đọc lại hint của đề bài, tác giả đã cho ta 1 cheatseet về SQL Injecttion. và ta đã tìm được câu lệnh trả về version. ở đây loại database mà chúng ta thao tác là của Oracle. &#x20;

SELECT banner FROM v$version. vì vậy câu truy vấn cuối cung của ta sẽ là

'UNION+SELECT+banner,'def'+from+v$version--

<figure><img src=".gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

vậy ta đã in ra thành công version của website và giải quyết challenge này.

## Lab: [SQL injection](https://portswigger.net/web-security/sql-injection) attack, querying the database type and version on MySQL and Microsoft

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

<details>

<summary><strong>Hint</strong></summary>

You can find some useful payloads on our [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet).

</details>

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

ở bài lab này, tác giả yêu cầu chúng ta khai thác version của database của MySQL cũng như Microsoft bằng cách in ra '8.0.36-0ubuntu0.20.04.1' giao diện của website. ta bắt các gói tin trong burpsuite. tại đây, sau khi xem qua các request, ta sẽ gửi request GET /filter?category=Tech+gifts vào repeater.

<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption><p>request in repeater.</p></figcaption></figure>

tại đây, mình sẽ xem cheat sheet mà tác giả đã cho, và mình biết được câu truy vấn trả về version trong MySQL là SELECT @@version. và để câu command thành công mình sẽ sử dụng UNION và URL ecoding.&#x20;

final command: 'UNION+SELECT+@@version,+null#

ở đây mình +null ở sau version là bởi vì database trả về 2 bảng là title và content.  [\
](https://portswigger.net/academy/labs/launch/13e48d1949abb8793e11da34ed06a5811ea3a9b2be0501a5e9f0deb255d37406?referrer=%2fweb-security%2fsql-injection%2fexamining-the-database%2flab-querying-database-version-mysql-microsoft)

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption><p>result</p></figcaption></figure>

sau khi chạy command, mình đã in ra thành công version của database trong MySQL.

## Lab: [SQL injection](https://portswigger.net/web-security/sql-injection) attack, listing the database contents on non-Oracle databases

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the `administrator` user.

<details>

<summary> <strong>Hint</strong></summary>

You can find some useful payloads on our [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet).

</details>

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption><p>xác định số cột được in ra</p></figcaption></figure>

Ở bài lab này, ta phải khai thác thông tin từ non-oracle database. ở ảnh trên, mĩnh xác định xem database sẽ trả về bao nhiêu cột, ở đây là 2.

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

tiếp theo, ta sẽ dùng cheatsheet của đề bài cung cấp. ở đây, ta nhìn thấy việc truy xuất dữ liệu từ database không thuộc oracle đều được lấy từ information\_schema.tables & information\_schema.columns.

vì vậy, ta sẽ in ra tên bảng của database thông qua command sau: **'UNION+select+table\_name,null+from+information\_schema.tables--**&#x20;

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption><p>các bảng được in ra qua câu command</p></figcaption></figure>

Sau khi lướt xem toàn bộ bảng, mình chú ý đến 2 bảng là pg\_user, users\_legzqo. Tuy nhiên, sau khi test thử thì bảng đầu tiên không cho mình thông tin gì cho việc khai thác. Trong khi đó bảng số 2 lại trả về những thông tin có ích.

Câu command mình dùng để in ra số cột của bảng:

**'UNION+SELECT+column\_name,NULL+FROM+information\_schema.columns+WHERE+table\_name='users\_legzqo'--**

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption><p>cột của bảng users_legzqo</p></figcaption></figure>

sau khi trích xuất cột từ bảng users\_legzqo, mình để ý tới 2 cột là username\_xshbws, và password\_stbolo. Khi đã biết 2 cột cần khai thác, mình chỉ cần trích xuất thông tin từ 2 cột thông qua câu command:

'UNION+SELECT+username\_xshbws,password\_stbolo+FROM+users\_legzqo--

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>username và mật khẩu được in ra</p></figcaption></figure>

administrator: vmsguwzh126iicoib1n5

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

## Lab: [SQL injection](https://portswigger.net/web-security/sql-injection) attack, listing the database contents on Oracle

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the `administrator` user.

<details>

<summary> <strong>Hint</strong></summary>



</details>

Ở bài lab này, ta sẽ khai thác database contents của oracle. trước tiên, ta vẫn sẽ truy cập trang web và bắt request ở burpsuite.

<figure><img src=".gitbook/assets/image (7) (1).png" alt=""><figcaption><p>giao diện trang web</p></figcaption></figure>

thông qua quan sát các thông tin được trả về, mình đưa ra nhận định rằng : trang web sẽ truy vấn 2 cột và đưa thông tin ra dưới format: title và content. vì vậy câu truy vấn và ta đưa vào phải lấy thông tin 2 cột. bên cạnh đó, ta đã biết được phiên bản của database là thuộc oracle.&#x20;

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption><p>cheatsheet</p></figcaption></figure>

qua cheatsheet ta viết được câu truy vấn in ra các bảng của database 'UNION+SELECT+table\_name,null+from+all\_tables--

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption><p>kết quả trả về sau câu truy vấn</p></figcaption></figure>

sau khi quan sát respnse của server, mình phát hiện bảng USERS\_TWRRPY khá đáng ngờ. Sau đó mình thông qua câu truy vấn xem tên cột của bảng này.

'UNION+SELECT+column\_name,null+from+all\_tab\_columns+WHERE+table\_name+%3d+'USERS\_TWRRPY'--

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption><p>kết quả được gửi lên browser</p></figcaption></figure>

biết được tên của các cột, mình tiến hành khai thác username và password.

'UNION+SELECT+USERNAME\_BKHDTO,PASSWORD\_TTSWTR+from+USERS\_TWRRPY--

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption><p>kết quả của câu truy vấn</p></figcaption></figure>

administrator: ol1iy37tllx2fo1h9mli

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption><p>thông báo hoàn thành bài lab</p></figcaption></figure>
