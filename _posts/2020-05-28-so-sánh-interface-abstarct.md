---
layout: post
title:  "So sánh interface và abstract trong java"
tag: [Java]
fig-caption: Interface-Abstract.png # Add figcaption (optional)
img: Interface-Abstract.png # Add image post (optional)
---

Interface và abstract là hai khái niệm cơ bản trong lập trình OOP. Đa số các lập trình viên mới bắt đầu với OOP đều mơ hồ về hai khái niệm này. Bây giờ chúng ta cùng nhau tìm hiểu về interface và abstract.
<h1>I. Interface là gì?</h1>
  Interface (Giao diện) là một hình thức, giống như một hợp đồng, nó không thể tự làm bất cứ điều gì. Nhưng khi có một class ký kết hợp     đồng (implement Interface) này, thì class đó phải tuân theo hợp đồng này.<br>
  Trong interface định nghĩa các hành vi của một class sẽ thực hiện. Nghĩa là interface là thể hiện của một chức năng gì đó của class.       Interface có thể có nhiều phương thức, và các phương thức này thể hiện cùng 1 chức năng.<br>
  Trong java một class có thể kế thừa nhiều interface (như kiểu một đối tượng có nhiều chức năng v)
  <h2>Ví dụ đơn giản về interface</h2>
    <code>
    interface printable {  
        void print();  
    } 
   
    class A6 implements printable {  
        public void print() {
            System.out.println("Hello");
        }  
   
        public static void main(String args[]){  
            A6 obj = new A6();  
            obj.print();  
        }
    }   
    </code>
<h1>II.Abstract là gì?</h1>
  Abstract là một lớp trừu tượng, đúng như cái tên, nó biểu diễn cho một cái gì đó không hoàn toàn cụ thể -> abstract class chỉ là một cấu   trúc hoặc hướng dẫn được tạo ra cho một class cụ thể khác.<br>
  !Java không hỗ trợ kế thừa nhiều abstract class.
<h1>III. So sánh interface và abstract class</h1>
  1. **Methods**: Class abstract có các phương thức abstract và non-abstract. Trong khi Interface chỉ có phương thức abstract, từ Java 8, thì Interface có thêm 2 loại phương thức là default và static.<br>
  2. **Variable**: Class abstract có thể có các biến final, non-final, static và non-static. Trong khi Interface chỉ có các biến static và final.<br>
  3. **Implementation**: Class abstract có thể implement các Interface. Trong khi Interface thì không thể implement class abstract.<br>
  4. **Inheritance**: Class abstract có thể kế thừa được một class khác. Trong khi Interface có thể kế thừa được nhiều Interface khác.
  5. **Accessibility**: các thành viên trong Interface kiếu mặc định là public. Trong khi class abstract thì lại có thể là private, protected,…
<h1>Tham khảo</h1>
https://viblo.asia/p/so-sanh-interface-va-abstract-trong-lap-trinh-huong-doi-tuong-63vKjpk6l2R<br>
https://topdev.vn/blog/interface-va-abstract-la-gi/
