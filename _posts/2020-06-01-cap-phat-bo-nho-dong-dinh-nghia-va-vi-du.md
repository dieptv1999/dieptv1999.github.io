---

layout: post
title: Cấp phát bộ nhớ động trong ngôn ngữ lập trình C
date: 2020-06-01 18:30:20 +0700
description: # Add post description (optional)
img:  /dynamic-memory-c/caption.png # Add image post (optional)
fig-caption: /dynamic-memory-c/caption.png # Add figcaption (optional)
tags: [C]
published: true

---

<h1> Cấp phát bộ nhớ động: Định nghĩa và ví dụ</h1>
Trong bài viết này, chúng ta sẽ tìm hiểu về bộ nhớ động thông qua các định nghĩa và ví dụ. Chúng ta cũng sẽ tìm hiểu về cấp phát bộ nhớ động bằng ngôn ngữ lập trình C cùng với các ví dụ chương trình C.

<h1> Vậy thế nào là bộ nhớ động?</h1>
Phân bổ bộ nhớ là một phần rất quan trọng trong phát triển phần mềm, nhất là với những phần mềm tốc độ đáp ứng cao, phần mềm nhúng. Khi chương trình được tải vào bộ nhớ hệ thống, vùng bộ nhớ được phân bổ cho chương trình được chia thành 3 phân vùng lớn: stack, heap, code.<br>

![memory](/assets/img/dynamic-memory-c/memoryLayoutC.jpg)

**Stack**<br>
Vùng ngăn xếp chứa ngăn xếp của chương trình, cấu trúc LIFO (last in first out), thường nằm ở phần cao hơn của bộ nhớ (phát triển địa chỉ từ địa chỉ cao đến địa chỉ thấp). Trên máy tính PC x86, nó phát triển theo địa chỉ 0, trên một số kiến trúc khác, nó phát triển theo hướng ngược lại. Một 'con trỏ ngăn xếp' (stack pointer) được cấp phát trên đỉnh của stack. <br>
Vùng ngăn xếp được sử dụng để lưu trữ các biến cục bộ của chương trình khi chúng được khai báo. Ngoài ra, các biến và mảng được khai báo khi bắt đầu một hàm, bao gồm cả hàm main, được phân bổ trong không gian ngăn xếp.<br>
**Heap**<br>
Heap thường dành riêng cho phân bổ bộ nhớ động. Không giống như ngăn xếp, heap phát triển từ địa chỉ thấp đến địa chỉ cao. Vùng heap được quản lí bởi hàm malloc(), realloc(), free(). Có thể sử dụng các lệnh gọi hệ thống *brk()* và *sbrk()* để điều chỉnh kích thước của heap. Heap được chia sẻ bởi tất cả các thư viện dùng chung (shared libraries) và các module được tải động trong một tiến trình (proccess). <br>
**Code**<br>
Vùng **code** được chia ra làm ba phần:<br>

- *BSS*: các biến tĩnh chưa được khởi tạo.
- *Data*: Lưu trữ các biến tĩnh được khởi tạo.
- *Text*: chứa các hướng dẫn thực thi của chương trình (program's executable instructions).
<h2> Cấp phát bộ nhớ động</h2>
Cấp phát bộ nhớ động đề cập đến việc quản lí bộ nhớ hệ thống trong thời gian chạy. Quản lí bộ nhớ động trong ngôn ngữ lập trình C được thực hiện thông qua một nhóm 4 chức năng : malloc(), calloc(), realloc(), free(). Cấp phát bộ nhớ động sử dụng không gian heap của hệ thống. Bài viết này chỉ viết về hai chức năng malloc() và calloc().
<h3> malloc()</h3>
malloc() được sử dụng thể phân bổ một khối bộ nhớ trên heap. Hàm malloc() cấp phát cho người dùng một số byte được chỉ định nhưng không khởi tạo. Sau khi được cấp phát, chương trình truy cập vào khối bộ nhớ này thông qua một con trỏ mà malloc() trả về. Con trỏ trả về bởi malloc() mặc định có kiểu void nhưng có thể được chuyển thành con trỏ của bất kì loại dữ liệu nào khác. Tuy nhiên, nếu không đủ dung lượng cho bộ nhớ được yêu cầu bởi malloc(), thì việc cấp phát không thành công và con trỏ NULL được trả về.<br>
Ví dụ: cấp phát bộ nhớ cho một mảng int có kích thước *SIZE_USER_NEEDS*<br>

    int * buffer = (int *) malloc (SIZE_USER_NEEDS * sizeof (int));
  
 <h3>calloc() </h3>
 calloc() cũng tương tự như malloc(). Hàm calloc() cấp phát số byte do người dùng chỉ định và khởi tạo chúng bằng 0. Hàm có 2 đối số: số lượng bộ nhớ được cấp phát và kích thước của mỗi khối bộ nhớ.<br>
 Ví dụ: cấp phát bộ nhớ cho mảng int có kích thước *SIZE_USER_NEEDS* và khởi tạo giá trị ban đầu bằng 0.<br>

     int * buffer = (int *) calloc (NUMBER_OF_ELEMENTS, sizeof (int));

> Khi kết thúc hàm hoặc chương trình chúng ta nên gọi hàm free() để giải phóng vùng nhớ đã cấp phát tránh lãng phí bộ nhớ.

<h3> Một ví dụ về hàm malloc() </h3>

    // Chương trình tính tổng n số mà người dùng nhập vào
    
    #include <stdio.h>
    #include <stdlib.h>
    
    int main()
    {
        int n, i, *ptr, sum = 0;
    
        printf("Enter number of elements: ");
        scanf("%d", &n);
    
        ptr = (int*) malloc(n * sizeof(int));
     
        // bộ nhớ không được cấp phát
        if(ptr == NULL)                     
        {
            printf("Error! memory not allocated.");
            exit(0);
        }
    
        printf("Enter elements: ");
        for(i = 0; i < n; ++i)
        {
            scanf("%d", ptr + i);
            sum += *(ptr + i);
        }
    
        printf("Sum = %d", sum);
      
        // giải phóng bộ nhớ
        free(ptr);
    
        return 0;
    }
