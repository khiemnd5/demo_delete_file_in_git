# demo_delete_file_in_git
Demo xoá lịch sử tận gốc rễ 1 file nếu đã lỡ push ở nhiều commit, nhiều branch


Chào các bạn,

Blog chắc bị mốc meo mất rồi. Mình lười viết quá ý, hôm nay rảnh thứ 7 nên ngó qua blog xem tình hình mọi người vào blog có nhiều không 😀

Đều đều 100 views/ngày là vui rồi =)). Lại thấy chủ đề Git là mọi người xem nhiều nhất nên tự dưng lại nghĩ ra một vấn đề mà hôm trước đồng nghiệp cũ hỏi là **làm thế nào để xoá tận gốc file mà nó đã từng commit và push lên github/gitlab/… file đó lại ở nhiều branchs (nhánh)** nữa.

Thấy đồng nghiệp hỏi câu này cũng hay nên bắt đầu mò tìm, thực ra thì cũng có người trong cộng đồng họ cũng gặp phải vấn đề này rồi. Mình thì cũng đọc lại của họ rồi giờ chia sẻ lại với mọi người đây.

## Tình huống là mình có 1 project demo “Hello World” như hình:

<!-- wp:image {"id":1536,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://nguyendangkhiemit.files.wordpress.com/2020/10/image-3.png?w=984" alt="" class="wp-image-1536"/><figcaption>Project demo delete file in git</figcaption></figure>
<!-- /wp:image -->

Project demo delete file in git
3 branch: master, dev1, dev2. Cả 3 branchs này đều đang chứa 1 file ActivationTool.zip. File này vô tình mình đã tải lên github và giờ muốn xoá tận gốc (xoá hết ở các commit, các branch) file này.

<!-- wp:image {"id":1529,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://nguyendangkhiemit.files.wordpress.com/2020/10/image.png?w=1024" alt="" class="wp-image-1529"/><figcaption>File ActivationTool.zip lỡ tay push lên cả 3 branch.</figcaption></figure>
<!-- /wp:image -->

File ActivationTool.zip lỡ tay push lên cả 3 branch.
Đầu tiên mình nghĩ là click chuột phải rồi xoá file đó đi rồi push lên github thêm 1 commit nữa là được (2 năm trước sẽ nghĩ vậy =))) ).
<!-- wp:image {"id":1531,"width":580,"height":158,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://nguyendangkhiemit.files.wordpress.com/2020/10/image-1.png?w=1024" alt="" class="wp-image-1531" width="580" height="158"/><figcaption>Xoá file và push thêm 1 commit nữa ở master</figcaption></figure>
<!-- /wp:image -->

Xoá file và push thêm 1 commit nữa ở master
Ơ, nào đâu có dễ cưng. Git nó tạo commit là nó đã lưu lại rồi. Ông nào muốn lấy file này chỉ cần reset lùi commit là lấy lại được file thôi, còn chưa nói tới các branchs khác cũng đang có file này nữa. Không làm thế này được. Vậy làm thế nào để trị trường hợp này.

Đây đây, các bạn sử dụng đoạn code sau:

## Bước 1: Xoá file ở local

``` git filter-branch --force --index-filter "git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA" --prune-empty --tag-name-filter cat -- --all ```

PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA: đường dẫn của file cần xoá tận gốc

Áp dụng:

``` git filter-branch --force --index-filter "git rm --cached --ignore-unmatch ActivationTool.zip" --prune-empty --tag-name-filter cat -- --all ```

<!-- wp:image {"id":1538,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://nguyendangkhiemit.files.wordpress.com/2020/10/image-5.png?w=1024" alt="" class="wp-image-1538"/></figure>
<!-- /wp:image -->
Sau khi thực hiện câu lệnh trên thì git nó sẽ xoá toàn bộ các file ở local trong các branch và các commit rồi.

## Bước 2: Xoá ở remote (hay hiểu là trên github, gitlab,…)

Để xoá được trên remote thì cần push force. Force là ép remote theo cái mới mất mà client gửi lên. **Khi dùng cờ force để push này các bạn phải quan sát, kiểm tra lại xem có đúng file các bạn muốn xoá không nhé. Không là toang đấy. =))) Sao lại toang hả? Nó force trên github thì nó xoá hết file ở các commit, branchs. Nhầm sang file quan trọng thì sẽ ra sao? =)))**

Lệnh push lên remote:

```git push origin --force --all```

<!-- wp:image {"id":1540,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://nguyendangkhiemit.files.wordpress.com/2020/10/image-6.png?w=826" alt="" class="wp-image-1540"/></figure>
<!-- /wp:image -->
2. Đối với các phiên bản mà đã release thì cần force cho các tags ở các file phiên bản nữa nhé.


```git push origin --force --tags```

<!-- wp:image {"id":1541,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://nguyendangkhiemit.files.wordpress.com/2020/10/image-7.png?w=754" alt="" class="wp-image-1541"/></figure>
<!-- /wp:image -->
Vậy là ngon rồi nhé :3

https://nguyendangkhiemit.wordpress.com/2020/10/03/git-xoa-tan-goc-re-mot-file-da-tung-push-len-github/

Github: https://github.com/khiemnd5/demo_delete_file_in_git

Tham khảo:

https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/removing-sensitive-data-from-a-repository

