## **1. Mục tiêu**
- Phát triển một mô hình RL có khả năng:
  - Đánh giá đặc điểm của từng nhóm lớp nhỏ (subgroups of layers).
  - Tự động điều chỉnh tỷ lệ cắt tỉa cho từng nhóm lớp dựa trên thông tin cục bộ và toàn cục.
  - Giảm thiểu suy giảm hiệu suất và tối ưu hóa tài nguyên tính toán.

---

## **2. Phương pháp**

### **2.1. Chia mô hình thành các nhóm lớp nhỏ**
- **Nguyên tắc chia nhóm:**
  - Chia mô hình thành các nhóm lớp nhỏ (ví dụ: nhóm các lớp liên tiếp hoặc nhóm theo chức năng, như attention blocks và Feed-Forward Networks - FFNs).
  - Mỗi nhóm lớp sẽ được coi là một "đơn vị" để áp dụng RL.

- **Lợi ích:**
  - Giảm độ phức tạp của bài toán RL bằng cách tập trung vào từng nhóm lớp nhỏ thay vì toàn bộ mô hình.
  - Cho phép tối ưu hóa cục bộ (local optimization) mà vẫn duy trì được hiệu suất tổng thể của mô hình.

---

### **2.2. Xây dựng môi trường RL (Environment)**
- **Trạng thái (State):**
  - Trạng thái của môi trường bao gồm các thông tin về:
    - Đặc điểm của từng nhóm lớp (ví dụ: kích thước ma trận trọng số, gradient, độ quan trọng của các tham số).
    - Tổng số lượng tham số đã bị cắt tỉa so với mục tiêu tổng thể.
    - Hiệu suất hiện tại của mô hình (ví dụ: Perplexity hoặc độ chính xác trên tập dữ liệu hiệu chuẩn).

- **Hành động (Action):**
  - Hành động là tỷ lệ cắt tỉa cho từng nhóm lớp tại mỗi bước thời gian \(t\).
  - Ví dụ: Nếu có \(k\) nhóm lớp, hành động \(A_t\) sẽ là một vector \(k\)-chiều, trong đó mỗi phần tử đại diện cho tỷ lệ cắt tỉa của một nhóm lớp cụ thể.

- **Phần thưởng (Reward):**
  - Phần thưởng được thiết kế để khuyến khích RL tối ưu hóa hai mục tiêu:
    1. **Giảm thiểu suy giảm hiệu suất:** Phần thưởng dương nếu hiệu suất của mô hình không bị suy giảm đáng kể sau khi cắt tỉa.
    2. **Tối ưu hóa tỷ lệ cắt tỉa:** Phần thưởng dương nếu tỷ lệ cắt tỉa đạt gần với mục tiêu đề ra mà không làm giảm hiệu suất.

---

### **2.3. Thiết kế tác nhân RL (Agent)**
- **Kiến trúc mạng nơ-ron:**
  - Sử dụng một mạng nơ-ron sâu (Deep Neural Network - DNN) để biểu diễn hàm chính sách (\( \pi \)) của tác nhân RL.
  - Đầu vào của mạng là trạng thái \(S_t\), đầu ra là hành động \(A_t\) (tỷ lệ cắt tỉa cho từng nhóm lớp).

- **Thuật toán RL:**
  - **Proximal Policy Optimization (PPO)** 
  - **Group Relative Policy Optimization (GRPO)** 
  - 
---

### **2.4. Quy trình huấn luyện RL**
1. **Khởi tạo:**
   - Khởi tạo mô hình RL với các tham số ngẫu nhiên.
   - Đặt tỷ lệ cắt tỉa ban đầu cho tất cả các nhóm lớp là 0% (không cắt tỉa).

2. **Lặp lại các bước sau:**
   - **Bước 1: Quan sát trạng thái \(S_t\).**
     - Thu thập thông tin về đặc điểm của từng nhóm lớp và hiệu suất hiện tại của mô hình.
   - **Bước 2: Chọn hành động \(A_t\).**
     - Tác nhân RL chọn tỷ lệ cắt tỉa cho từng nhóm lớp dựa trên chính sách hiện tại.
   - **Bước 3: Thực hiện hành động.**
     - Áp dụng tỷ lệ cắt tỉa \(A_t\) cho mô hình và đánh giá hiệu suất mới.
   - **Bước 4: Nhận phần thưởng \(R_t\).**
     - Tính toán phần thưởng dựa trên hiệu suất và tỷ lệ cắt tỉa.
   - **Bước 5: Cập nhật mô hình RL.**
     - Sử dụng thuật toán PPO để cập nhật chính sách của tác nhân.

3. **Kết thúc huấn luyện:**
   - Khi mô hình RL hội tụ, thu được một chính sách tối ưu để điều chỉnh tỷ lệ cắt tỉa cho từng nhóm lớp.

---

## **3. Kết hợp RL với SlimGPT**

### **3.1. Tích hợp RL vào SlimGPT**
- Thay thế chiến lược **Incremental Pruning Ratio** trong SlimGPT bằng mô hình RL.
- Sử dụng RL để tự động điều chỉnh tỷ lệ cắt tỉa cho từng nhóm lớp thay vì sử dụng hàm logarit cố định.

### **3.2. Ưu điểm của RL so với Incremental Pruning Ratio**
- **Tính linh hoạt:** RL có thể điều chỉnh tỷ lệ cắt tỉa dựa trên đặc điểm của từng nhóm lớp, thay vì áp dụng một công thức cố định.
- **Tối ưu hóa toàn cục:** RL có thể tận dụng thông tin toàn cục để đưa ra quyết định cắt tỉa tối ưu hơn.
- **Khả năng mở rộng:** RL có thể được áp dụng cho các mô hình và nhiệm vụ khác nhau mà không cần thay đổi nhiều.

---

## **4. Thực nghiệm**

### **4.1. Bộ dữ liệu và mô hình thử nghiệm**
- **Mô hình:** LLaMA-7B, LLaMA-13B, hoặc các mô hình ngôn ngữ lớn khác.
- **Bộ dữ liệu hiệu chuẩn:** C4, Alpaca, hoặc GPT4-Alpaca.
- **Nhiệm vụ đánh giá:** WikiText2, Commonsense Reasoning, MMLU.

### **4.2. So sánh hiệu suất**
- So sánh hiệu suất của RL với chiến lược **Incremental Pruning Ratio** hiện tại của SlimGPT.
- Đánh giá các chỉ số:
  - **Perplexity (PPL):** Độ phức tạp ngôn ngữ.
  - **Zero-shot performance:** Hiệu suất trên các nhiệm vụ suy luận không cần huấn luyện thêm.
  - **Thời gian và tài nguyên:** Thời gian huấn luyện RL và tài nguyên GPU cần thiết.

---

## **5. Kết quả mong đợi**
- Một mô hình RL có khả năng tự động điều chỉnh tỷ lệ cắt tỉa cho từng nhóm lớp, giảm thiểu suy giảm hiệu suất và tối ưu hóa tỷ lệ cắt tỉa.
- Hiệu suất của mô hình sau khi cắt tỉa tương đương hoặc vượt trội so với SlimGPT hiện tại.
- Khả năng mở rộng và áp dụng cho các mô hình và nhiệm vụ khác nhau.

---

## **6. Thách thức và hướng giải quyết**
- **Thách thức 1:** Huấn luyện RL có thể tốn nhiều tài nguyên và thời gian.
  - **Giải pháp:** Sử dụng các kỹ thuật tối ưu hóa như **Experience Replay** và **Parallel Training** để giảm thời gian huấn luyện.
- **Thách thức 2:** RL có thể gặp khó khăn khi xử lý các mô hình rất lớn.
  - **Giải pháp:** Áp dụng RL cho từng nhóm lớp nhỏ thay vì toàn bộ mô hình.

---
# SlimGPT 
Là một phương pháp pruning có cấu trúc cho các mô hình ngôn ngữ lớn (LLMs) dựa trên framework Optimal Brain Surgeon (OBS). Phương pháp này bao gồm các điểm chính sau:

1. Batched Greedy Pruning:
- Sử dụng phân tích Cholesky nhóm để chọn các head tối ưu cho pruning trong các khối attention.
- Áp dụng Dynamic Group Size cho FFN để đạt được kết quả pruning gần tối ưu và hiệu quả.

2. Incremental Pruning Ratio:
- Đề xuất chiến lược tỉ lệ pruning tăng dần theo logarit để giảm thiểu sự tích lũy lỗi trong quá trình pruning layer-wise.
- Công thức: $r_i = r_0 + (r_n−1 − r_0) * log(i + 1) / log(n)$

3. Quy trình thực hiện:
- Chỉ cần một tập dữ liệu hiệu chuẩn ngẫu nhiên từ kho dữ liệu tiền huấn luyện.
- Có thể nén mô hình chỉ với một GPU, vài trăm dữ liệu hiệu chuẩn và khoảng một giờ.
- Phù hợp với tất cả các mô hình lớn dựa trên kiến trúc Transformer thông thường.

4. Ưu điểm:
- Giữ lại hầu hết hiệu suất của mô hình sau khi pruning.
- Chi phí thấp, sử dụng tài nguyên ít và thời gian nén ngắn.
- Có tính chuyển đổi tốt và áp dụng được cho nhiều mô hình khác nhau.

5. Kết quả:
- Vượt trội hơn các phương pháp hiện có trên các tác vụ đánh giá như language modeling và commonsense reasoning.
- Hiệu quả đặc biệt rõ ràng khi tỉ lệ pruning cao (ví dụ: 50%).
