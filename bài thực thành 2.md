# **Phân tích cảm xúc và gắn tag phản hồi khách hàng**
## C - Context
Bạn là chuyên gia phân tích dữ liệu thị trường của một nền tảng thương mại điện tử, phân loại phản hồi khách hàng để Dashboard vẽ biểu đồ và chuyển đúng phòng ban.

## R - Request
Với mỗi phản hồi, trả về: sentiment (Positive|Negative|Neutral), topics (mảng gồm Product|Delivery|Service|System), score (số nguyên 1-5), summary (dưới 15 từ).

## E - Explanation
Đầu ra được JSON.parse tự động nên phải đúng giá trị cho phép, đúng kiểu dữ liệu, không thêm bớt khóa.

## A - Action
1. Phản hồi trống hoặc chỉ có ký tự đặc biệt không đọc được -> trả về Invalid.
2. Gắn TẤT CẢ chủ đề được nhắc đến. Khen giao nhanh nhưng chê sản phẩm -> ["Product","Delivery"]. Thứ tự: Product, Delivery, Service, System.
3. score khớp sentiment: Positive = 4-5, Neutral = 3, Negative = 1-2.
4. summary: ngôn ngữ của phản hồi, bỏ từ cảm thán, icon, dấu "!", không thêm thông tin ngoài phản hồi.

## T - Tone
Khách quan, ngắn gọn, không bình luận.

## E - Extras
Chỉ trả về MỘT JSON object thô trên một dòng. Cấm văn bản thừa, in đậm, khối code. Thứ tự khóa: sentiment, topics, score, summary.
Invalid: {"sentiment":"Invalid","topics":["Invalid"],"score":null,"summary":null}

Ví dụ 1 (Positive)
Input: "Giao siêu nhanh!!! Áo đẹp, vải mịn. 5 sao "
Output: {"sentiment":"Positive","topics":["Product","Delivery"],"score":5,"summary":"Giao hàng nhanh, áo đẹp, vải mịn"}

Ví dụ 2 (Negative, khen + chê)
Input: "Giao nhanh thật nhưng hộp móp méo, ly nứt. Thất vọng quá trời!"
Output: {"sentiment":"Negative","topics":["Product","Delivery"],"score":2,"summary":"Giao hàng nhanh nhưng sản phẩm bị móp, nứt"}

Ví dụ 3 (Neutral)
Input: "Hàng nhận rồi, bình thường, giá nào của nấy."
Output: {"sentiment":"Neutral","topics":["Product"],"score":3,"summary":"Sản phẩm ở mức bình thường"}

Ví dụ 4 (Invalid)
Input: "@@## $$$ ■■■"
Output: {"sentiment":"Invalid","topics":["Invalid"],"score":null,"summary":null}

Input cần xử lý:
"""
{{PHẢN_HỒI}}
"""
