## **Phase 1: Chuẩn bị dữ liệu và kiến trúc tổng thể**

### **1.1. Thu thập và tiền xử lý dữ liệu pháp luật**
- **Mục tiêu**: Xây dựng một bộ dữ liệu pháp luật chất lượng cao để huấn luyện và đánh giá hệ thống.
- **Công việc cụ thể**:
  - **Thu thập dữ liệu**:
    - Văn bản pháp luật chính thống (Bộ luật Dân sự, Bộ luật Hình sự, Luật Lao động, v.v.).
    - Án lệ thực tế từ Tòa án Nhân dân Tối cao hoặc các cơ quan pháp luật.
    - Đề thi pháp luật (đại học, học phần pháp luật).
    - Tài liệu tham khảo chuyên ngành (giáo trình, sách pháp luật).
  - **Tiền xử lý dữ liệu**:
    - Loại bỏ ký tự đặc biệt, chuẩn hóa chữ viết hoa/thường, tách từ (tokenization).
    - Trích xuất thực thể (entities) và quan hệ (relations) từ văn bản pháp luật.
    - Biểu diễn dữ liệu dưới dạng Knowledge Graph (KG).

- **Kết quả mong đợi**:
  - Một bộ dữ liệu pháp luật đã được cấu trúc hóa thành Knowledge Graph.
  - Các câu hỏi pháp luật kèm theo đáp án chuẩn (ground truth).

---

### **1.2. Thiết kế kiến trúc tổng thể**
- **Mục tiêu**: Xây dựng một hệ thống tích hợp giữa GNN và LLM để tăng cường khả năng suy luận pháp luật.
- **Kiến trúc tổng thể**:
  1. **Module trích xuất Knowledge Graph**:
     - Chuyển đổi câu hỏi của người dùng thành Knowledge Graph.
  2. **Module GNN**:
     - Xử lý Knowledge Graph và tạo ra graph embedding.
  3. **Module LLM**:
     - Kết hợp graph embedding với các block của LLM để sinh ra câu trả lời.

- **Kết quả mong đợi**:
  - Kiến trúc tổng thể rõ ràng, dễ triển khai và mở rộng.

---

## **Phase 2: Xây dựng Module Trích Xuất Knowledge Graph**

### **2.1. Trích xuất thực thể và quan hệ từ câu hỏi**
- **Mục tiêu**: Chuyển đổi câu hỏi của người dùng thành Knowledge Graph.
- **Công việc cụ thể**:
  - Sử dụng Named Entity Recognition (NER) để nhận diện các thực thể pháp luật (ví dụ: điều khoản, văn bản, chủ thể, hành vi, hậu quả).
  - Sử dụng Relation Extraction (RE) để xác định mối quan hệ giữa các thực thể (ví dụ: "điều khoản A thuộc văn bản B").
  - Công cụ đề xuất: SpaCy, OpenIE, hoặc Stanford NLP.

- **Kết quả mong đợi**:
  - Một Knowledge Graph được trích xuất từ câu hỏi của người dùng.

---

### **2.2. Xây dựng Knowledge Graph**
- **Mục tiêu**: Biểu diễn Knowledge Graph dưới dạng đồ thị có hướng.
- **Công việc cụ thể**:
  - **Node**: Các thực thể pháp luật.
  - **Edge**: Mối quan hệ giữa các thực thể.
  - Công cụ đề xuất: NetworkX, PyTorch Geometric, hoặc Neo4j.

- **Kết quả mong đợi**:
  - Knowledge Graph hoàn chỉnh, sẵn sàng để xử lý bởi GNN.

---

## **Phase 3: Xây dựng Module GNN**

### **3.1. Chọn mô hình GNN phù hợp**
- **Mục tiêu**: Chọn và huấn luyện một mô hình GNN để xử lý Knowledge Graph.
- **Công việc cụ thể**:
  - Sử dụng các mô hình GNN phổ biến như:
    - **Graph Convolutional Network (GCN)**: Phù hợp với Knowledge Graph đơn giản.
    - **Graph Attention Network (GAT)**: Phù hợp với Knowledge Graph phức tạp.
    - **GraphSAGE**: Phù hợp với Knowledge Graph lớn.
  - Huấn luyện GNN trên dataset pháp luật đã chuẩn bị.

- **Kết quả mong đợi**:
  - Một mô hình GNN đã được huấn luyện, có khả năng tạo ra graph embedding từ Knowledge Graph.

---

### **3.2. Tạo graph embedding**
- **Mục tiêu**: Chuyển đổi Knowledge Graph trích xuất từ câu hỏi của ngừoi dùng thành vector đại diện (graph embedding).
- **Công việc cụ thể**:
  - Từ Knowledge Graph của câu hỏi, sử dụng GNN đã huấn luyện để tạo ra graph embedding.
  - Đảm bảo graph embedding phản ánh ngữ nghĩa pháp luật của câu hỏi.

- **Kết quả mong đợi**:
  - Graph embedding sẵn sàng để kết hợp với LLM.

---

## **Phase 4: Kết hợp GNN với LLM**

### **4.1. Kết hợp graph embedding với LLM**
- **Mục tiêu**: Tích hợp graph embedding vào kiến trúc của LLM.
- **Công việc cụ thể**:
  - **Cách 1: Nhập graph embedding vào input của LLM**:
    - Concatenate graph embedding với embedding của câu hỏi (từ tokenizer của LLM).
    - Đưa vào LLM để sinh ra câu trả lời.
  - **Cách 2: Điều chỉnh attention mechanism của LLM**:
    - Thêm một attention head phụ thuộc vào graph embedding để điều chỉnh trọng số attention trong các layer transformer của LLM.

- **Kết quả mong đợi**:
  - Một kiến trúc hybrid tích hợp GNN và LLM.

---

### **4.2. Fine-tuning LLM**
#### 4.2.1. Chuẩn bị dataset pháp luật
Nguồn dữ liệu :
- Văn bản pháp luật chính thống (Bộ luật Dân sự, Bộ luật Hình sự, Luật Lao động, v.v.).
- Án lệ thực tế từ Tòa án Nhân dân Tối cao hoặc các cơ quan pháp luật.
- Đề thi pháp luật (đại học, học phần pháp luật).
- Tài liệu tham khảo chuyên ngành (giáo trình, sách pháp luật).
  
Tiền xử lý dữ liệu :
- Loại bỏ ký tự đặc biệt, chuẩn hóa chữ viết hoa/thường, tách từ (tokenization).
- Chuyển đổi văn bản thành dạng phù hợp với LLM (ví dụ: câu hỏi - đáp án, đoạn văn bản pháp luật).
  
Kết quả mong đợi :
- Một bộ dataset pháp luật đã được cấu trúc hóa, sẵn sàng để fine-tune.
#### 4.2.2. Fine-tuning LLM trên dataset pháp luật
Mục tiêu : Huấn luyện LLM để nắm vững kiến thức pháp luật.
Cách thực hiện :
- Sử dụng kỹ thuật Masked Language Modeling (MLM) hoặc Next Sentence Prediction (NSP) để fine-tune LLM.
  
Ví dụ:
- Input: "Theo Điều 38 Bộ luật Lao động 2019, người lao động có quyền [MASK] hợp đồng lao động."
- Output: "đơn phương chấm dứt."
  
Chiến lược fine-tuning :
- Supervised Fine-tuning : Sử dụng dataset pháp luật đã gán nhãn (câu hỏi - đáp án) để fine-tune LLM.
- Contrastive Learning : Tăng cường khả năng liên kết giữa câu hỏi và đáp án bằng cách sử dụng cặp câu hỏi - đáp án tương ứng.
  
Hyperparameters :
- Batch size: 16–32.
- Learning rate: 5e-5 đến 1e-4.
- Epochs: 3–5 epochs.
  
Kết quả mong đợi :
- Một LLM đã được fine-tune, có khả năng hiểu và trả lời các câu hỏi pháp luật cơ bản
#### 4.2.3. Fine-tuning LLM trên task QA chọn đáp án đúng
Mục tiêu : Huấn luyện LLM để chọn đáp án đúng từ các lựa chọn.
Cách thực hiện :
- Input : Câu hỏi + các lựa chọn.
- Output : Dự đoán đáp án đúng.
- Sử dụng Cross-Entropy Loss để tối ưu hóa việc chọn đáp án đúng.
  
Chiến lược fine-tuning :
- Multi-task Learning : Kết hợp task QA chọn đáp án đúng với các task khác (ví dụ: giải thích điều khoản, phân loại văn bản pháp luật).
- Graph-aware Attention : Kết hợp graph embedding từ GNN vào quá trình attention của LLM để tăng cường khả năng suy luận.
  
Hyperparameters :
- Batch size: 16–32.
- Learning rate: 5e-5 đến 1e-4.
- Epochs: 3–5 epochs.
  
Kết quả mong đợi :
- Một LLM đã được fine-tune, có khả năng chọn đáp án đúng từ các lựa chọn với độ chính xác cao.
---

## **Phase 5: Triển khai và tối ưu hóa**

### **5.1. Triển khai hệ thống**
- **Mục tiêu**: Xây dựng API để tích hợp hệ thống vào ứng dụng thực tế.
- **Công việc cụ thể**:
  - Xây dựng API để nhận câu hỏi từ người dùng, xử lý bằng GNN và LLM, và trả về câu trả lời.
  - Tối ưu hóa tốc độ và hiệu suất của hệ thống.

- **Kết quả mong đợi**:
  - Một hệ thống hoàn chỉnh, sẵn sàng để sử dụng.

---

### **5.2. Tối ưu hóa mô hình**
- **Mục tiêu**: Giảm kích thước và tài nguyên cần thiết để chạy mô hình.
- **Công việc cụ thể**:
  - Áp dụng các kỹ thuật như pruning, quantization để giảm kích thước mô hình.
  - Tối ưu hóa quá trình trích xuất Knowledge Graph và tính toán graph embedding.

- **Kết quả mong đợi**:
  - Một mô hình nhẹ hơn, hiệu quả hơn.

---

### **5.3. Đánh giá cuối cùng**
- **Mục tiêu**: Đánh giá hiệu suất của hệ thống trên dataset chuẩn.
- **Công việc cụ thể**:
  - So sánh hiệu suất của hệ thống khi có và không có sự hỗ trợ từ GNN.
  - Đánh giá theo các chỉ số: Độ chính xác, độ liên quan, và khả năng giải thích.

- **Kết quả mong đợi**:
  - Một báo cáo đánh giá chi tiết, chứng minh tính hiệu quả của hệ thống.
