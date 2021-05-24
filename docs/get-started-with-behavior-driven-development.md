---
title: Get Started With Behavior Driven Development
date: 2021-05-24 00:42:00
slug: behavior-driven-develop-testing
tags: ["development"]
status: draft
desc: " In the world, the h "
---
## Behavior Driven Development - Phát triển phần mềm hướng hành vi là gì
Phát triển phần mềm hướng hành vi (BDD) là một phương pháp phát triển phần mềm thông qua việc giao tiếp dựa trên ví dụ liên tục giữa các developer, QA và BA.

Mục đích chính của BDD là cải thiện việc giao tiếp của tất cả các bên liên quan của dự án, để cho tất cả các tính năng đều được hiểu chính xác bởi các thành viên trong dự án, trước khi quá trình phát triển bắt đầu. Nó giúp xác định các kịch bản chính cho mỗi User Story, và làm cho Requirements rõ ràng hơn, tránh sự mơ hồ.

Trong BDD, các ví dụ được gọi là các Scenario (kịch bản). Các kịch bản được viết theo một format đặc biệt gọi là Gherkin. Các kịch bản này giải thích cách mà một tính năng nhất định sẽ hoạt động trong các tình huống khác nhau và với tham số đầu vào khác nhau. Chúng được gọi là Executable Specifications ( Thông số kỹ thuật thực thi) vì Gherkin được cấu trúc và chứa cả đặc tả kỹ thuật lẫn đầu vào cho các bài test.

BDD được phát triển bới **Dan North** để trả lời cho các vấn đề sau

```
•    Khi nào thì bắt đầu tiến trình?
•    Cái gì cần test và cái gì không?
•    Cái gì nên gọi test?
•    Chúng ta hiểu như thế nào khi 1 test case failed?
```

Trọng tâm của BDD là thay đổi cách tiếp cận tới UT và AT mà North đã đưa ra trong khi giải quyết những vấn đề này.

Ví dụ, anh ấy tìm ra rằng: tên của 1 UT case nên là cả câu và bắt đầu bằng từ "should" và phải được viết theo thứ tự của các giá trị trong nghiệp vụ. Các AT case nên được viết theo chuẩn agile framework của User Story

Các tiêu chí chấp nhận nên được viết dưới dạng các kịch bản và được thực hiện dưới dạng các lớp: 
```Gherkin
"As a [role] I want [feature] so that [benefit]".
Given [initial context],
When [event occurs],
Then [ensure some outcomes].
```
## Vậy, Ưu điểm của BDD là gì
Nó đưa các bên liên quan và nhóm phát triển với những góc nhìn khác nhau vào chung một vị trí và đảm bảo rằng tất cả sẽ có cùng một mong đợi.

Mục tiêu của BDD là một ngôn ngữ domain-specific và có thể đọc hiểu nghiệp vụ cho phép bạn mô tả hành vi của một hệ thống mà không cần phải giải thích hành vi đó được thực hiện ra sao.

Các Test viết dưới dạng văn bản thuần túy mô tả các tính năng và các tính huống, được viết trước tiên và được xác nhận bởi các bên phi kỹ thuật (QA, BA, Client...) Các Test này được viết dưới dạng ngôn ngữ tự nhiên. Vì vậy, không cần thiết phải có bất cứ kỹ năng lập trình nào để có thể viết hay hiểu được chúng. Các BA có thể tích cực tham gia vào quá trình xem xét các thử nghiệm tự động và đưa ra các phản hồi của họ để cải thiện chúng.

BDD giúp tập trung vào nhu cầu của người dùng và các hành vi mong đợi của hệ thống, thay vì việc quá tập trung vào kiểm tra việc implement tính năng.

Nó thu hút các team về sản phẩm, team QA, team BA và team phát triển vào cùng một chỗ, cung cấp nền tảng để giải quyết sự khác biệt về quan điểm. Do đó, nó đảm bảo rằng các bên liên quan có cùng kỳ vọng. Sự hợp tác này giữa các bên liên quan sẽ tạo ra một bộ tiêu chí tốt và rõ ràng.

## Tất nhiên, Nó sẽ có nhược điểm
Trong một số trường hợp, các test case được viết bởi Manual Tester, Khách Hàng, BA chưa đủ tốt để thực hiện tự động, Vì vậy có thể cần thiết một số effort để làm các case này thích hợp cho việc thực thi tự động

Khái niệm về BDD là tương đối mới, cộng đồng hỗ trợ hiện tại chưa đủ lớn nên có thể sẽ mất nhiều thời gian để xử lý các vấn đề kỹ thuật phát sinh

## Công cụ
* Cucumber
* Behat
* Ginko, Godog
