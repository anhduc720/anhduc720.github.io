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
