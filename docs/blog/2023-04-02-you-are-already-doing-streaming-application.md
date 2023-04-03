---
title: "Bạn đã luôn 'xài' stream processing"
time: "2023-04-01 13:15"
author: Andy Hoang
description: "Lại là stream processing, nhưng lần này nó chỉ cần chạy vài cli cơ bản.."
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

Khoan đã, vậy chương trình mình vừa viết có phải là 1 streaming procesing application không?
> Không... hẵn


ChatGpt giúp mình tạo ra 1 server log dạng này

>`server.log`
```
[2021-09-22 11:09:10] INFO: Starting server...
[2021-09-22 11:09:28] INFO: Server successfully started and listening on port 8080
[2021-09-22 11:10:12] DEBUG: Received GET request from 127.0.0.1 with status code 200
[2021-09-22 11:12:56] DEBUG: Received POST request from 127.0.0.1 with status code 503
[2021-09-22 11:13:01] DEBUG: Received GET request from 127.0.0.1 with status code 404
[2021-09-22 11:15:38] DEBUG: Received GET request from 127.0.0.1 with status code 200
[2021-09-22 11:20:49] DEBUG: Received POST request from 127.0.0.1 with status code 200
[2021-09-22 11:25:12] DEBUG: Received GET request from 127.0.0.1 with status code 200
[2021-09-22 11:30:35] DEBUG: Received GET request from 127.0.0.1 with status code 404
[2021-09-22 11:35:47] DEBUG: Received POST request from 127.0.0.1 with status code 200
[2021-09-22 11:40:22] DEBUG: Received GET request from 127.0.0.1 with status code 200
[2021-09-22 11:43:59] DEBUG: Received GET request from 127.0.0.1 with status code 503
[2021-09-22 11:45:22] INFO: Stopping server for backups
```

Có bao nhiêu request trả về status_code 200?
```bash
cat server.log | grep 200 | wc -l
```

`cat, grep, wc` đều là những cli cơ bản mà hầu như lập trình viên nào cũng dùng qua, và nó có 1 đặc tính là hầu như stateless (à trừ `sudo rm -rf  --no-preserve-root /` ra). Khi bạn "compose" chúng lại bằng compatible input/output bạn sẽ được 1 câu "cli cool ngầu hơn" và phục vụ được nhiều mục đích hơn.

Đổi lại bài toán trên 1 chút, với 1 file server.log đang chạy  ở 1 con máy chủ nào đó, với nhiều tmux panel được mở:

```bash
tail -f server.log | grep 200 >> server_200.log

tail -f server.log | grep 500 >> server_500.log

watch "wc -l server_200.log"
watch "wc -l server_500.log"

```
Bạn có thể cơ bản được cập nhật "real-time" tổng số lỗi 200/500 mà con server đang gặp phải. Tất nhiên trường hợp này quá ư là thừa, khi mà toàn bộ log server ngày nay đã được đưa vào kibana, graphana để vẽ chart nhìn sướng con mắt.

Một số trường hợp dùng nâng cao hơn về CLI sử dụng "tail" để mở đầu, và kết thúc là 1 stdout cũng rất phổ biến trong quá trình debug nhanh hệ thống.

```
{"log_timestamp": "2021-09-22 13:15:27.000", "request": "/", "body_bytes_sent": 1024, "ip": "192.168.0.1", "thread": "T-1000"}
{"log_timestamp": "2021-09-22 13:15:32.000", "request": "/login", "body_bytes_sent": 1987, "ip": "192.168.0.2", "thread": "T-1001"}
{"log_timestamp": "2021-09-22 13:15:34.000", "request": "/home.html", "body_bytes_sent": 3056, "ip": "192.168.0.3", "thread": "T-1003"}
{"log_timestamp": "2021-09-22 13:16:01.000", "request": "/api/v1/products", "body_bytes_sent": 4096, "ip": "192.168.0.4", "thread": "T-1004"}
{"log_timestamp": "2021-09-22 13:16:10.000", "request": "/about.html", "body_bytes_sent": 1234, "ip": "192.168.0.5", "thread": "T-1005"}
```

```
tail -f server.log | grep --line-buffered  thread | sed -r 's/^.*log_timestamp\": \"([^,]*)\",.*request\": \"([^,]*)\".*body_bytes_sent\": ([^,]*),.*$/\1 \3 \2/'
```

```
2021-09-22 13:15:27.000 1024 /
2021-09-22 13:15:32.000 1987 /login
2021-09-22 13:15:34.000 3056 /home.html
2021-09-22 13:16:01.000 4096 /api/v1/products
2021-09-22 13:16:10.000 1234 /about.html
```

## Steam processing 101

Tóm lại, với những chương trình "nhỏ", "cơ bản" bạn có thể gộp nó lại thành 1 chương trình to hơn, hoành tráng hơn rât nhiều, và **stream processing** cũng thế, với những element cơ bản:

* source (tail? nc? kafka?)
* transformation (grep? uniq? sort? map? filter? aggregate? reduce?)
* sink (stdout? file? kafka? database connector?)

> **Nếu bạn đã và đang làm việc với (|) thì bạn xem như hoàn thành stream processing 101 rồi!**

Hãy sẵn với bài toán nhập môn của [Flink](https://nightlies.apache.org/flink/flink-docs-release-1.17/docs/dev/datastream/overview/#anatomy-of-a-flink-program) nhé!

## My thought

Cám ơn [Making Sense of Stream Processing - Chapter 4](https://www.oreilly.com/library/view/making-sense-of/9781492042563/) đã tạo cảm hứng cho mình viết bài này, phần lớn ý tưởng của bài blog được tạo ra ở đây
Phần còn lại là nhờ ChatGPT viết ví dụ.
Với mình thì đâu đó Functional Programming vẫn 1 cái gì đó đóng góp vào mindset đồ chơi lập trình real-time này, còn các bạn thì sao?