# KPI สำหรับร้านกาแฟ (ครบทุกมิติ)

## ตาราง KPI ที่ครอบคลุม

| ลำดับ | มิติ | ชื่อ KPI | อธิบาย KPI (สำหรับ CEO) | การแปลผล (สำหรับ CEO) | วิธีการคำนวน | ข้อมูลที่ใช้ |
|:---:|:---|:---|:---|:---|:---|:---|
| 1 | ยอดขาย | Total Revenue | รายได้รวมจากการขายสินค้าทั้งหมด | ยอดขายรวม 209,950 บาท จากการขาย 1,500 ออร์เดอร์ | SUM(net_sales_thb) | net_sales_thb |
| 2 | ยอดขาย | Average Order Value (AOV) | มูลค่าเฉลี่ยต่อออร์เดอร์ | ลูกค้าเฉลี่ยซื้อ 140 บาทต่อออร์เดอร์ | SUM(net_sales_thb) / COUNT(DISTINCT order_id) | net_sales_thb, order_id |
| 3 | ยอดขาย | Average Transaction Value per Item | ราคาเฉลี่ยต่อชิ้นสินค้า | สินค้าแต่ละชิ้นเฉลี่ย 82.6 บาท | SUM(net_sales_thb) / SUM(qty) | net_sales_thb, qty |
| 4 | ยอดขาย | Total Items Sold | จำนวนชิ้นสินค้าที่ขายทั้งหมด | ขายสินค้าได้ 2,542 ชิ้น | SUM(qty) | qty |
| 5 | ยอดขาย | Total Discount Amount | มูลค่าส่วนลดรวมที่ให้ลูกค้า | ให้ส่วนลดรวม 1,300 บาท | SUM(discount_thb) | discount_thb |
| 6 | ยอดขาย | Discount Rate | อัตราส่วนลดต่อยอดขาย | ส่วนลดคิดเป็น 0.62% ของยอดขาย | SUM(discount_thb) / SUM(unit_price_thb * qty) * 100 | discount_thb, unit_price_thb, qty |
| 7 | สินค้า | Category Revenue Mix | สัดส่วนยอดขายตามหมวดหมู่ | Coffee 54%, Tea 26%, Bakery 10%, Non-Coffee 6%, Soda 4% | SUM(net_sales_thb) GROUP BY category | category, net_sales_thb |
| 8 | สินค้า | Top Selling Product | สินค้าขายดีที่สุด | Thai Tea ขายดีสุด 22,155 บาท | SUM(net_sales_thb) GROUP BY menu_item | menu_item, net_sales_thb |
| 9 | สินค้า | Product Profitability | ความสามารถในการทำกำไรต่อหน่วย | วัดจากราคาต่อหน่วยเฉลี่ยต่อหมวดหมู่ | AVG(unit_price_thb) BY category | category, unit_price_thb |
| 10 | สินค้า | Hot vs Iced Ratio | สัดส่วนร้อนเย็น | ลูกค้านิยมสินค้าเย็นมากกว่าร้อน | COUNT BY hot_iced / total | hot_iced, qty |
| 11 | สินค้า | Size Preference | ความนิยมขนาด | ขนาด M เป็นที่นิยมที่สุด | COUNT BY size | size, qty |
| 12 | สินค้า | Add-on Attachment Rate | อัตราการซื้อสินค้าเพิ่มเติม | สินค้าเพิ่มเติม (เช่น Oat Milk, Whipped Cream) มียอดขาย | COUNT WHERE add_on != 'None' | add_on |
| 13 | กิจกรรม | Order Count | จำนวนคำสั่งซื้อทั้งหมด | มีคำสั่งซื้อทั้งหมด 1,500 รายการ | COUNT(DISTINCT order_id) | order_id |
| 14 | กิจกรรม | Orders per Day | จำนวนคำสั่งซื้อต่อวัน | เฉลี่ยวันละ 250 ออร์เดอร์ | COUNT(order_id) / COUNT(DISTINCT order_date) | order_id, order_date |
| 15 | กิจกรรม | Items per Order | จำนวนชิ้นต่อออร์เดอร์ | ลูกค้าซื้อเฉลี่ย 1.7 ชิ้นต่อออร์เดอร์ | SUM(qty) / COUNT(DISTINCT order_id) | qty, order_id |
| 16 | กิจกรรม | Peak Hour Performance | ประสิทธิภาพช่วงพีค | Morning Peak มียอดขายสูงสุด 95,580 บาท | SUM(net_sales_thb) BY time_slot | time_slot, net_sales_thb |
| 17 | กิจกรรม | Promo Utilization Rate | อัตราการใช้โปรโมชัน | มีการใช้โปรโมชัน 5.13% ของออร์เดอร์ทั้งหมด | SUM(promo_flag) / COUNT(order_id) * 100 | promo_flag |
| 18 | ลูกค้า | New vs Returning Customer Ratio | สัดส่วนลูกค้าใหม่และลูกค้าประจำ | ลูกค้าประจำ 59.5%, ลูกค้าใหม่ 40.5% | COUNT BY customer_type | customer_type |
| 19 | ลูกค้า | Customer Acquisition Rate | อัตราการได้ลูกค้าใหม่ | มีลูกค้าใหม่ 608 ราย คิดเป็น 40.5% | COUNT WHERE customer_type = 'New' / total | customer_type |
| 20 | ลูกค้า | Customer Retention Rate | อัตราการรักษาลูกค้า | ลูกค้าประจำ 892 ราย คิดเป็น 59.5% | COUNT WHERE customer_type = 'Returning' / total | customer_type |
| 21 | ลูกค้า | Returning Customer Revenue | รายได้จากลูกค้าประจำ | ลูกค้าประจำสร้างรายได้มากกว่าลูกค้าใหม่ | SUM(net_sales_thb) BY customer_type | customer_type, net_sales_thb |
| 22 | ลูกค้า | New Customer Revenue | รายได้จากลูกค้าใหม่ | ลูกค้าใหม่สร้างรายได้น้อยกว่า | SUM(net_sales_thb) BY customer_type | customer_type, net_sales_thb |
| 23 | ช่องทาง | Channel Revenue Mix | สัดส่วนยอดขายตามช่องทาง | Walk-in 73%, Grab 16%, Line Man 11% | SUM(net_sales_thb) BY channel | channel, net_sales_thb |
| 24 | ช่องทาง | Walk-in Revenue | รายได้จากลูกค้าเดินน้ำมา | Walk-in สร้างรายได้สูงสุด 152,755 บาท | SUM(net_sales_thb) WHERE channel = 'Walk-in' | channel, net_sales_thb |
| 25 | ช่องทาง | Delivery Channel Revenue | รายได้จากช่องทาง Delivery | Delivery รวม 57,195 บาท (Grab + Line Man) | SUM(net_sales_thb) WHERE channel LIKE '%Delivery%' | channel, net_sales_thb |
| 26 | ช่องทาง | Payment Method Mix | สัดส่วนวิธีการชำระเงิน | QR/PromptPay เป็นที่นิยมที่สุด | COUNT BY payment_method | payment_method |
| 27 | เวลา | Daily Revenue Trend | แนวโน้มยอดขายรายวัน | วันเสาร์มียอดขายสูงสุด 47,585 บาท | SUM(net_sales_thb) BY day_of_week | day_of_week, net_sales_thb |
| 28 | เวลา | Time Slot Performance | ประสิทธิภาพตามช่วงเวลา | Morning Peak ดีที่สุด ตามด้วย Midday | SUM(net_sales_thb) BY time_slot | time_slot, net_sales_thb |
| 29 | เวลา | Hourly Revenue Pattern | รูปแบบยอดขายรายชั่วโมง | ชั่วโมง 9-10 น. มียอดขายสูงสุด | SUM(net_sales_thb) BY hour | hour, net_sales_thb |
| 30 | เวลา | Weekend vs Weekday | เปรียบเทียบวันธรรมดาและวันหยุด | วันหยุด (เสาร์-อาทิตย์) มียอดขายสูงกว่า | SUM(net_sales_thb) GROUP BY weekend/weekday | day_of_week, net_sales_thb |

---

## สรุปมิติและ KPI ย่อย

### 1. มิติยอดขาย (Sales Performance)
- Total Revenue
- Average Order Value (AOV)
- Average Transaction Value per Item
- Total Items Sold
- Total Discount Amount
- Discount Rate

### 2. มิติสินค้า (Product Performance)
- Category Revenue Mix
- Top Selling Product
- Product Profitability
- Hot vs Iced Ratio
- Size Preference
- Add-on Attachment Rate

### 3. มิติกิจกรรม/การดำเนินงาน (Activity/Operation)
- Order Count
- Orders per Day
- Items per Order
- Peak Hour Performance
- Promo Utilization Rate

### 4. มิติลูกค้า (Customer)
- New vs Returning Customer Ratio
- Customer Acquisition Rate
- Customer Retention Rate
- Returning Customer Revenue
- New Customer Revenue

### 5. มิติช่องทาง (Channel)
- Channel Revenue Mix
- Walk-in Revenue
- Delivery Channel Revenue
- Payment Method Mix

### 6. มิติเวลา (Time)
- Daily Revenue Trend
- Time Slot Performance
- Hourly Revenue Pattern
- Weekend vs Weekday
