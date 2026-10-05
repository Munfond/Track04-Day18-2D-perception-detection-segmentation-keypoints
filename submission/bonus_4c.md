# Bonus 4C — Kiểm tra flip_idx trên val lật gương

Train hai model từ yolo26n-pose.pt, cùng seed=0, epochs=40, imgsz=640, fliplr mặc định; thiết bị cuda. Dùng best.pt của từng model. Model giải phẫu hoán đổi left/right; model đồng nhất giữ nguyên chỉ số. Val gương lật ảnh, x → 1 − x và hoán đổi nhãn keypoint theo FLIP_IDX giải phẫu.

| Model | mAP50 gốc | mAP50 gương | mAP50-95 gốc | mAP50-95 gương |
|---|---:|---:|---:|---:|
| flip_idx giải phẫu | 0.9950 | 0.9950 | 0.4573 | 0.4388 |
| flip_idx đồng nhất | 0.9950 | 0.8775 | 0.4169 | 0.2977 |

Trên lần chạy này, chênh lệch mAP50-95 (giải phẫu trừ đồng nhất) là +0.0404 trên val gốc và +0.1411 trên val gương. Model giải phẫu đổi -0.0185 và model đồng nhất đổi -0.1192 khi chuyển sang val gương. Đây là số liệu đo được, không giả định trước rằng model nào chắc chắn hơn.

Metric có thể che lỗi là Pose mAP trên val gốc: toàn bộ hổ cùng quay phải, nên metric đó không kiểm tra khả năng giữ đúng quy ước trái/phải khi gặp hướng quay trái. Pose mAP50 còn dùng ngưỡng OKS thấp hơn mAP50-95; với sigma đều 1/12 và các chân gần nhau, đảo nhãn có thể vẫn đạt ngưỡng ghép đúng, nên mAP cao chưa chứng minh đúng giải phẫu. Nếu cả hai model vẫn cho số liệu gần nhau trên val gương, thí nghiệm chưa làm lộ lỗi qua mAP; cần xem keypoint từng chân và đo lỗi hoán đổi, không kết luận quy ước đồng nhất là đúng.

Tập val tốt hơn cần ảnh thật của cả hai hướng, đủ tư thế/che khuất/kích thước, nhãn giải phẫu nhất quán và báo cáo riêng theo hướng cùng sai số từng cặp left/right. Val gương là phép kiểm tra có kiểm soát, không thay thế ảnh triển khai thật; một seed chưa đủ kết luận khác biệt có ý nghĩa thống kê.
