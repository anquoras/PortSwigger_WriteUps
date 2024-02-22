# Access control vulnerabilities - Privilege escalation

**Objective**:

* This lab aims to assess knowledge of security best practices and measures that can be adopted to mitigate the risk of privilege escalation.

In this lab, students need to:

* Answer the following questions:
  * In the context of privilege escalation, what is the difference between vertical and horizontal privilege escalation, and can you give an example of each?
  * What are some effective strategies or practices that can be implemented to prevent privilege escalation vulnerabilities in a system?
* Perform challenge:
  * [User ID controlled by request parameter](https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter)
*   This lab has a horizontal privilege escalation vulnerability on the user account page.

    To solve the lab, obtain the API key for the user `carlos` and submit it as the solution.

    You can log in to your own account using the following credentials: `wiener:peter`

![](<.gitbook/assets/0 (4).png>)

Ta sẽ truy cập vào my account thông qua tài khoản được cấp bởi author là wiener:peter

![](<.gitbook/assets/1 (4).png>)

Ta sẽ mở burpsuite để bắt request và response, tại đây ta sẽ gửi GET /my-account?id=wiener về burp repeater.

![](<.gitbook/assets/2 (4).png>)

Tại repeater, ta sẽ thay đổi id thành carlos, tài khoản mà author yêu cầu tấn công. Và bùm!!!!, ta đã lấy được api key của tk carlos : MUxXs5S2zGLb2xPd44dKpwuonMagDUQx

![](<.gitbook/assets/3 (4).png>)
