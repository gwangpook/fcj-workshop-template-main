---
title: "Blog 3"
date: 2026-04-17
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# XÂY DỰNG DATA LAKE VỚI AMAZON S3 VÀ AMAZON ATHENA

Kiến trúc Data Lake trên AWS cung cấp giải pháp lưu trữ và phân tích dữ liệu tập trung dành cho các hệ thống phát sinh khối lượng lớn dữ liệu như log ứng dụng, dữ liệu giao dịch, dữ liệu IoT hoặc dữ liệu người dùng. Thay vì lưu trữ toàn bộ dữ liệu trên cơ sở dữ liệu truyền thống, dữ liệu được lưu trực tiếp dưới dạng thô (Raw Data) trên Amazon S3, sau đó được quản lý bằng AWS Glue Data Catalog và truy vấn thông qua Amazon Athena. Giải pháp này giúp giảm chi phí lưu trữ, tăng khả năng mở rộng và cho phép khai thác dữ liệu hiệu quả mà không cần triển khai hoặc quản lý hạ tầng xử lý dữ liệu.

Các điểm chính cần nắm: 

* Lưu trữ dữ liệu trên Amazon S3: Dữ liệu từ các hệ thống ứng dụng, log server, thiết bị IoT hoặc các nguồn dữ liệu khác được lưu trữ trực tiếp trên Amazon S3 dưới nhiều định dạng như JSON, CSV, Apache Parquet hoặc các tệp Log. Amazon S3 cung cấp khả năng lưu trữ bền vững với độ sẵn sàng cao và không yêu cầu xác định cấu trúc dữ liệu trước khi lưu.
* Tối ưu chi phí lưu trữ: Amazon S3 hỗ trợ nhiều Storage Class như Standard, Standard-IA, Intelligent-Tiering và Glacier, cho phép tự động hoặc chủ động tối ưu chi phí lưu trữ dựa trên tần suất truy cập dữ liệu. Việc lưu trữ dữ liệu trên S3 giúp giảm đáng kể chi phí so với việc sử dụng cơ sở dữ liệu để lưu trữ dữ liệu lịch sử hoặc dữ liệu ít được truy cập.
* Quản lý Metadata bằng AWS Glue: AWS Glue Crawler tự động quét dữ liệu trong Amazon S3, nhận diện cấu trúc dữ liệu và tạo Metadata trong AWS Glue Data Catalog. Metadata này được sử dụng làm schema chung cho các dịch vụ phân tích dữ liệu như Amazon Athena mà không cần cấu hình thủ công.
* Truy vấn dữ liệu bằng Amazon Athena: Amazon Athena là dịch vụ phân tích dữ liệu theo mô hình Serverless, cho phép thực hiện truy vấn bằng SQL chuẩn ANSI trực tiếp trên dữ liệu lưu trữ trong Amazon S3 thông qua AWS Glue Data Catalog. Người dùng không cần triển khai hoặc quản lý máy chủ, đồng thời chỉ thanh toán theo dung lượng dữ liệu được quét trong mỗi truy vấn.
* Kiến trúc Serverless và khả năng mở rộng: Amazon S3, AWS Glue và Amazon Athena đều là các dịch vụ Serverless, tự động mở rộng theo khối lượng dữ liệu và số lượng truy vấn. Điều này giúp giảm chi phí vận hành, đơn giản hóa việc quản trị hạ tầng và đáp ứng hiệu quả nhu cầu xử lý dữ liệu ở quy mô lớn.
* Tích hợp với hệ sinh thái phân tích dữ liệu của AWS: Dữ liệu được quản lý trong Data Lake có thể tích hợp trực tiếp với Amazon QuickSight để xây dựng Dashboard và báo cáo trực quan hoặc được sử dụng làm nguồn dữ liệu cho các dịch vụ Machine Learning như Amazon SageMaker mà không cần sao chép hoặc di chuyển dữ liệu.

Kiến trúc Data Lake sử dụng Amazon S3, AWS Glue và Amazon Athena là giải pháp phù hợp cho các hệ thống yêu cầu lưu trữ và phân tích khối lượng lớn dữ liệu với chi phí tối ưu. Việc lưu trữ dữ liệu tập trung trên Amazon S3 kết hợp với khả năng quản lý Metadata của AWS Glue và truy vấn Serverless của Amazon Athena giúp đơn giản hóa quy trình khai thác dữ liệu, nâng cao khả năng mở rộng của hệ thống và tạo nền tảng cho các ứng dụng phân tích dữ liệu, báo cáo Business Intelligence cũng như Machine Learning trong tương lai.

## Hình ảnh minh họa

<p align="center">
  <img src="/images/3-BlogsPosted/Blog3/img3.png" width="700">
</p>

<p align="center">
  <em>Hình 1. Kiến trúc Data Lake trên AWS sử dụng Amazon S3 và Amazon Athena.</em>
</p>

## Link bài viết

[🔗 Xem bài viết trên Facebook](https://web.facebook.com/share/p/1Bn86KFzSx/?_rdc=1&_rdr)

