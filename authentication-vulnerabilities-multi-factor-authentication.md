# Authentication Vulnerabilities (multi-factor authentication)

**Objective**:

* In this section, we'll look at some of the vulnerabilities that can occur in multi-factor authentication mechanisms. We've also provided several interactive labs to demonstrate how you can exploit these vulnerabilities in multi-factor authentication.

In this lab, students need to:

* Perform two challenges:
  * [2FA simple bypass](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass)
  * [2FA broken logic](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-broken-logic)
* Explain and capture all steps (full windows screen capture).
* &#x20;                                               **Challenge 1:** [2FA simple bypass](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass)

This lab's two-factor authentication can be bypassed. You have already obtained a valid username and password, but do not have access to the user's 2FA verification code. To solve the lab, access Carlos's account page.

* Your credentials: `wiener:peter`
* Victim's credentials `carlos:montoya`

![](<.gitbook/assets/0 (3).png>)

ở bài lab này, ta sẽ bypass qua hệ thống xác thực hai lớp của website. Đầu tiên, ta sẽ đăng nhập vs user account hợp lệ mà tác giả đưa.

![](<.gitbook/assets/1 (3).png>)

Ta sẽ nhận được 1 email verify user của server gửi về. điền security code ta sẽ đăng nhập dc vào website.

Tại url, ta quan sát thấy header my-account?id=peter. Ta lưu lại để thử sử dụng vs các user khác.

![](<.gitbook/assets/2 (3).png>)

Lần này ta sẽ thử vs account của nạn nhân carlos:Montoya.vs account này, cta sẽ k nhận đc email verification code. Tuy nhiên ta sẽ thử thay đổi url website. Ta sẽ thay my-account?id =carlos.

![](<.gitbook/assets/3 (3).png>)

Kết quả là thành công !!

&#x20;                                               Challenge 2: [2FA broken logic](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-broken-logic)

This lab's two-factor authentication is vulnerable due to its flawed logic. To solve the lab, access Carlos's account page.

* Your credentials: `wiener:peter`
* Victim's username: `carlos`

![](<.gitbook/assets/4 (3).png>)

Như challenge trc, ta vẫn sẽ đăng nhập vs tk wiener:peter mà đề bài cho.

Tuy nhiên, ở bài lab này ta sẽ phải đăng nhập mà chỉ biết username là : carlos

![](<.gitbook/assets/5 (2).png>)

Sang burpsuite, tại đây sẽ gửi get /login2 về repeater

![](<.gitbook/assets/6 (2).png>)

Tại đây, ta sẽ thay username thành carlos và gửi gói tin đi. Vậy, ta đã thành công xác thực username carlos mà k cần password

![](<.gitbook/assets/7 (2).png>)

Để check lại, ta sẽ vào trang đăng nhập vs tk peter:wiener. Tuy nhiên khi nhập code thì sẽ hiện message incorrect bởi vì ta đang đăng nhập dưới tư cách carlos

![](<.gitbook/assets/8 (2).png>)

Việc tiếp theo sẽ là bruteforce mã code xác thực. ta sẽ gửi request post /login2 về intruder

![](<.gitbook/assets/9 (2).png>)

Thay verify = carlos và gán biến cho mfa-code

![](<.gitbook/assets/10 (2).png>)

ở payload, chọn brute forcer và set character thành 0-9

![](<.gitbook/assets/11 (2).png>)

Sau khi bruteforce, ta thấy payload 1089 trả về kết quả 302. Ta sẽ xem payload và mở bằng browser

![](<.gitbook/assets/12 (2).png>)

Và ta đã thành công.
