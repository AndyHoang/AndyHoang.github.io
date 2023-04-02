---
title: "Bạn đã luôn 'xài' stream processing"
time: "2023-04-01 13:15"
author: Andy Hoang
description: ""
---

## Stream processing ~~101~~

Không quá khó để nhờ ChatGPT hay Notion AI, hay bất kì cái gì để có một định nghĩa từ hàn lâm đến dễ hiểu cho nó, đơn cử

> Xử lý luồng là một cách tuyệt vời để xử lý dữ liệu trong thời gian thực ngay khi nó được tạo ra. Điều này có nghĩa là dữ liệu được xử lý và gửi đến người dùng cuối ngay lập tức. Điều này khác với xử lý ~~theo lô~~ bulk processing, trong đó dữ liệu được xử lý theo các ~~lô~~ bulk tại các khoảng thời gian được ~~lên lịch~~ schedule.

Tuy nhiên để "làm việc" với môn này thường các sếp sẽ bàn giao cho bạn 1 lô sách "how to" từ cookbook cho tới chuyên sâu để có thể làm "đúng" 1 chương trình streaming processing.
Một vài tựa sách mà các đàn anh thường hay giới thiệu có lẽ là
* [Stream Processing with Apache Flink](https://www.oreilly.com/library/view/stream-processing-with/9781491974285/)
* [Designing Data-Intensive Applications](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)
* Giới thiệu mình với...

## Một chút context về functional programming

Thường thì chúng ta nhắc đến `map`, `flatmap`, `filter`, `reduce` ở một vài ngôn ngữ như js, python, java, scala như là mốt sự "hiện hữu" của functional programming trong mindset của developer. Chẳng qua chưa có hội, điều kiện "all in" mà thôi.

Nó có phần đúng và cũng có phần sai, nhưng mà khoan, tại sao chúng ta lại nhắc về FP nhỉ?

Thường những "phép biến đổi stateless" của FP được dùng khá nhiều trong các "operator" của stream procesing có lẽ là mối liên quan duy nhất mà người viết nhìn thấy được, ta tạm để việc này lại ở đây đã.

## Pipe (|) như là một streaming cơ bản

(|) được dùng từ khá lâu, nhất là các bạn choi choi thời 8x-9x mới đầu chơi emoji bậy mà k cần yahoo support vậy. Tuy nhiên lịch sử của nó thì khá xa so với thời kì này, ai muốn search thêm thì cứ search về lịch sử của nó. Bài viết này tập trung vào mối quan hệ của (|) và Title của chúng ta

`ls | xargs rm` là một hành động thường có kết quả không tốt cho HĐH của bạn tuy nhiên bạn vừa mới hoàn thành xong một chương trình khá xịn sò, đó là
* list tất cả các file folder trong directory
* xoá nó với `rm` và sử dụng thêm 1 từ hơi lạ `xargs`

Chương trình ta vừa viết cũng không hẵn lúc nào cũng chạy đúng cả, vì đơn giản phần sau (|) sẽ "chừa" lại folder, vì rờ mờ mà không rờ ép thì không xoá được folder.

Ta cũng để ý 


