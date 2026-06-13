# bai2mobai.tieuluan
# BÀI TẬP 2 - MÔN PHÁT TRIỂN ỨNG DỤNG TRÊN THIẾT BỊ DI ĐỘNG
#  Họ và Tên:Trương Văn Quyến 
#  Mssv: K225480106083
#  Lớp:KTP-K01
Sản phẩm triển khai: Ứng dụng học từ vựng tiếng Anh chuyên ngành Máy tính & Hệ thống (Offline) có tên IT FLASHCARD APP
Môi trường phát triển: Android Studio
Ngôn ngữ sử dụng: Java
#  CHƯƠNG 1: KIẾN THỨC NỀN TẢNG ANDROID CORE
1. Quản trị hệ thống qua Tập tin AndroidManifest.xml
Bản chất kỹ thuật: Tập tin AndroidManifest.xml đóng vai trò như bản khai sinh và cấu trúc tổng thể của ứng dụng, cung cấp các thông tin nền tảng bắt buộc cho Hệ điều hành (OS) Android trước khi tiến hành cài đặt.
Các thành phần mô tả chính: Định danh gói (package), các cấu hình hệ thống (allowBackup, các quy tắc trích xuất dữ liệu dataExtractionRules), định nghĩa giao diện chủ đạo (theme), và khai báo toàn bộ các màn hình hiển thị (Activity). Trong đó, bộ lọc intent-filter chứa hai thuộc tính MAIN và LAUNCHER nhằm định vị MainActivity là điểm khởi chạy đầu tiên của phần mềm.
##  Cơ chế Khai báo và Mục đích của Quyền hạn (Permissions): Để ứng dụng có thể thực thi các tác vụ can thiệp vào tài nguyên hệ thống, lập trình viên bắt buộc phải khai báo bằng thẻ <uses-permission> đặt bên trong thẻ gốc <manifest>.
Cú pháp cấu hình:

<img width="731" height="473" alt="image" src="https://github.com/user-attachments/assets/2532afe5-967f-49fc-a01f-11c9ad45bdf1" />

Mục đích: Đảm bảo tính minh bạch, thông báo cho OS và người dùng hệ thống biết phạm vi truy cập tài nguyên của ứng dụng để hệ điều hành cấp quyền thực thi một cách an toàn.
Code:

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
2. Chu trình Vòng đời Ứng dụng (Activity Lifecycle)
image
##  <img width="1000" height="707" alt="image" src="https://github.com/user-attachments/assets/e177015f-1241-4b28-84c9-c330184766d3" />
##  Khái niệm: Là một tập hợp các trạng thái tuần tự mà một màn hình (Activity) trải qua kể từ khi được khởi tạo, hiển thị, tương tác cho đến khi bị đóng hoặc giải phóng hoàn toàn khỏi bộ nhớ RAM bởi người dùng hoặc hệ thống (onCreate() ➔ onStart() ➔ onResume() ➔ onPause() ➔ onStop() ➔ onDestroy()).
##  Bản chất của hàm onCreate() tự sinh: Hàm onCreate() là điểm khởi đầu bắt buộc (Entry Point) của một Activity. Khi người dùng bấm mở app, OS Android sẽ thiết lập tiến trình và gọi hàm này đầu tiên. Hệ thống tự động sinh ra hàm này vì đây là nơi duy nhất phù hợp để thực hiện các thiết lập nền tảng không thể thiếu:
##  Nạp sơ đồ cấu trúc giao diện đồ họa từ tệp thiết kế XML lên màn hình thông qua hàm setContentView.
##  Ánh xạ, kết nối các đối tượng giao diện từ XML sang mã nguồn Java bằng hàm findViewById.
##  Khởi tạo bộ nhớ đệm và các biến dữ liệu logic ban đầu cho Activity.
## 3. Ngôn ngữ Java & Cơ chế Kiểm tra Quyền Động (Runtime Permission)
Bản chất kỹ thuật: Từ phiên bản Android 6.0 (API 23) trở lên, đối với các quyền thuộc nhóm nguy hiểm (Dangerous Permissions), việc khai báo trong file Manifest là chưa đủ. Ứng dụng bắt buộc phải kiểm tra quyền động tại thời điểm chạy (Runtime) để bảo vệ quyền riêng tư của người dùng.
Mã nguồn thực thi kiểm tra quyền mẫu:
if (ContextCompat.checkSelfPermission(this, Manifest.permission.READ_EXTERNAL_STORAGE) 
        != PackageManager.PERMISSION_GRANTED) {
    // Nếu chưa được cấp quyền, tiến hành gọi hộp thoại hệ thống yêu cầu phê duyệt
    ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.READ_EXTERNAL_STORAGE}, 101);
}
##  Ý nghĩa: Đảm bảo ứng dụng chạy an toàn, tôn trọng quyền bảo mật. Nếu người dùng từ chối, ứng dụng sẽ tắt tính năng liên quan thay vì làm sập (crash) hệ thống.

#  CHƯƠNG 2: KIẾN THẾ THIẾT KẾ LAYOUT & TƯƠNG TÁC SỰ KIỆN
##  1. Kiến trúc Giao diện XML (res/layout) & Cơ chế Tham chiếu Tài nguyên
Quy tắc khử Hardcode: Tuyệt đối không viết trực tiếp văn bản thuần (Hardcode) vào thuộc tính android:text trong file XML Layout. Mọi chuỗi ký tự hiển thị được đưa tập trung vào tệp res/values/strings.xml.
###  Cú pháp tham chiếu:
Trong XML Layout: Sử dụng tiền tố @string/tên_id_biến (Ví dụ: android:text="@string/btn_known").
Trong mã nguồn Java: Tham chiếu thông qua lớp định danh hệ thống R.string.tên_id_biến hoặc qua hàm getString(R.string.tên_id_biến).
###  Ưu điểm vượt trội:
##  Quản lý tập trung (Centralized Management): Giúp chỉnh sửa, bảo trì toàn bộ chuỗi văn bản, màu sắc của ứng dụng tại một nơi duy nhất mà không cần can thiệp hay lội vào cấu trúc layout.
Hỗ trợ tự động phân giải tài nguyên (Auto Resource Partitioning): Hệ điều hành Android tích hợp sẵn cơ chế phân giải tài nguyên thông minh. Khi thiết bị thay đổi Vùng miền (Location), Ngôn ngữ (Language), hoặc Chế độ giao diện (Theme), OS sẽ tự động tìm kiếm và nạp các giá trị tương ứng trong các thư mục đặc biệt cấu hình riêng (Ví dụ: values-zh/ cho tiếng Trung, hoặc values-night/ cho Dark Theme).
Lợi ích thực tế: Giúp ứng dụng đạt khả năng đa ngôn ngữ và đa giao diện tự động mượt mà mà lập trình viên không cần viết bất kỳ dòng mã logic if-else nào trong code Java để kiểm tra trạng thái máy.
image
code:
<img width="956" height="150" alt="image" src="https://github.com/user-attachments/assets/a3e30340-bb40-4e7d-b1f5-7d45edd38b8a" />

##  2. Quy luật Bố cục Tuyến tính (LinearLayout)
Bản chất cấu trúc: LinearLayout gộp các thành phần giao diện con lại và ép chúng tuân theo một quy luật sắp xếp kề khít nối đuôi nhau theo một hướng cố định thông qua thuộc tính android:orientation:
vertical: Sắp xếp các view con liên tiếp theo chiều dọc từ trên xuống dưới.
horizontal: Sắp xếp các view con liên tiếp theo chiều ngang từ trái sang phải.
Thuộc tính gravity: Sử dụng thuộc tính android:gravity="center" để căn chỉnh toàn bộ nội dung (như chữ, biểu tượng) tập trung vào chính giữa vật thể chứa, tối ưu hóa trải nghiệm thị giác cho người dùng.
image
##  3. Tương tác Logic giữa Mã nguồn và Layout
Mong muốn đồng bộ Text theo thiết lập của người dùng: Tránh tuyệt đối hardcode chữ trong code Java. Khi cần hiển thị text thay đổi động trên layout, mã nguồn Java tương tác thông qua hàm tham chiếu tài nguyên:
txtCardSide.setText(getString(R.string.label_front_side));
Lúc này, hệ điều hành tự động bốc đúng chuỗi chữ phù hợp với cài đặt ngôn ngữ máy của người dùng để hiển thị. image

##  4. Xử lý Sự kiện Tương tác (Event Handling)
Khi người dùng thực hiện hành động (Click vào button, text...), để layout phát tín hiệu cho code Java thực thi, View trong Layout cần được định danh một ID bằng thuộc tính: android:id="@+id/tên_id".

Dự án nghiên cứu và triển khai 2 cách viết mã nguồn xử lý sự kiện:

###  Cách 1: Sử dụng lớp nặc danh (Anonymous Inner Class) trực tiếp trong Java (Cách dự án đang dùng):

Java
btnKnown.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        // Mã nguồn xử lý nghiệp vụ đặt tại đây
        moveToNextWord();
    }
});
image
Ý nghĩa: Giúp tách biệt hoàn toàn vai trò (XML chỉ làm giao diện, Java làm logic), code sạch sẽ, dễ bảo trì và tránh lỗi đặt sai tên hàm từ XML làm sập ứng dụng.

###  Cách 2: Khai báo qua thuộc tính android:onClick trong XML:

Trong file XML: Thêm thuộc tính android:onClick="xuLySuKienClick" cho View.

Trong file Java: Khởi tạo hàm công khai bắt buộc tương ứng:

public void xuLySuKienClick(View view) {
    // Xử lý logic tại đây
}
#  CHƯƠNG 3: GIẢI PHÁP SỬ DỤNG DỮ LIỆU ĐỆM ASSETS VÀ THỰC NGHIỆM
##  1. Bản chất Kỹ thuật của thư mục Assets
Khi sử dụng Windows Explorer sao chép các tệp dữ liệu vào thư mục assets (app/src/main/assets), trong quá trình đóng gói ứng dụng (Compile), toàn bộ các tệp tin thô này đều đi theo và nằm trực tiếp bên trong tệp tin cài đặt ứng dụng APK.
Mã nguồn Java có thể truy cập trực tiếp vào các tệp tin này thông qua cú pháp luồng đọc byte:
InputStream is = getAssets().open("tuvung.json");
Lợi ích tối cao của cơ chế dữ liệu chuẩn bị trước (Offline Data Support):

Ứng dụng vận hành hoàn toàn Offline (Không cần kết nối mạng Internet), hỗ trợ học tập mọi lúc mọi nơi.
Tốc độ phản hồi nạp dữ liệu cục bộ diễn ra tức thì, không phát sinh độ trễ hay phụ thuộc vào tín hiệu mạng.
Tiết kiệm chi phí xây dựng, vận hành máy chủ Server API và bảo toàn băng thông cho thiết bị.
image
##  2. Giải quyết Bài toán Thực tế: Ứng dụng "IT Flashcard App"
Đặt vấn đề (SV Tự đặt ra vấn đề): Sinh viên ngành Máy tính gặp rất nhiều rào cản khi tiếp cận tài liệu kỹ thuật quốc tế do lượng từ vựng chuyên ngành rất nặng, trừu tượng và khó nhớ. Học theo phương pháp truyền thống kém hiệu quả.

Giải quyết vấn đề: Xây dựng ứng dụng Flashcard thông minh giúp phân loại từ vựng theo từng chuyên ngành hẹp, hỗ trợ học qua thẻ lật vật lý trực quan, và có thanh tiến trình hiển thị tỷ lệ % để người dùng dễ dàng theo dõi mức độ thuộc bài.

Đặc thù dữ liệu JSON (tuvung.json): Dữ liệu được định dạng cấu trúc mảng đối tượng JSON phân cấp rõ ràng, lưu trữ cục bộ trong Assets bao gồm các thuộc tính định danh: id, tu_tieng_anh, phien_am, loai_tu, nghia_tieng_viet, vi_du, chu_de (dùng để phân loại danh mục) và trang_thai (0 là chưa thuộc, 1 là đã thuộc).

Giải pháp Thuật toán xử lý dữ liệu trong Java:

Thuật toán Phân tích cú pháp (JSON Parsing): Đọc luồng dữ liệu thô từ Assets thành chuỗi String cấu hình UTF-8, bóc tách mảng JSONArray nạp vào danh sách động List allWords trên RAM.

Thuật toán Lọc động (Dynamic Filtering): Sử dụng vòng lặp duyệt tuyến tính (Linear Scan) để lọc dữ liệu dựa trên điều kiện kép: Trùng khớp chủ đề chuyên ngành được chọn trên Spinner và chỉ lấy các từ có trạng thái chưa thuộc (w.getTrangThai() == 0).

Thuật toán Lưu trữ bền vững (Persistence State): Áp dụng bộ lưu trữ Key-Value SharedPreferences của hệ thống Android để ghi vĩnh viễn trạng thái học tập (trang_thai = 1) theo ID từ vựng xuống bộ nhớ ROM. Khi khởi động lại ứng dụng, tiến độ được đồng bộ khôi phục 100%.

Đối tượng Đồ họa hiển thị dữ liệu:

Spinner: Custom lại giao diện qua tệp tin XML (bg_spinner.xml), bo góc 12dp để hiển thị danh mục phân tách các chuyên ngành CNTT.
CardView: Thiết kế bo góc lớn 24dp, đổ bóng đa tầng tạo hiệu ứng thẻ vật lý lật mặt linh hoạt khi nhận sự kiện chạm nhẹ.
ProgressBar: Thanh tiến trình hình học kết hợp Text hiển thị, thực thi công thức tính toán:$$\text{Tiến độ} = \frac{\text{Từ đã thuộc}}{\text{Tổng số từ trong chuyên ngành}} \times 100$$để hiển thị trực quan tiến độ thực tế lên màn hình (Ví dụ: 5/20 (25%)).
CHƯƠNG IV: CÁC NỘI DUNG SÁNG TẠO ĐÃ LÀM THÊM (NÂNG CAO)
Vượt trên các yêu cầu cơ bản của đề tài, ứng dụng đã nghiên cứu sâu và hiện thực hóa thành công các giải pháp công nghệ cao cấp của Hệ điều hành Android nhằm tối ưu hóa sản phẩm:

1. Tích hợp Icon Custom độc quyền trong Hệ thống Tài nguyên
Ứng dụng không sử dụng icon robot mặc định của Android Studio. Dự án đã tự thiết kế tệp tin biểu tượng icon.png riêng biệt.

Giải pháp triển khai: Di chuyển tệp tin hình ảnh từ thư mục thô assets vào đúng phân vùng quản lý tài nguyên app/src/main/res/drawable/ để hệ thống định danh.
Cấu hình Manifest thực tế:
<application
    android:icon="@drawable/iconapp"
    android:roundIcon="@drawable/iconapp"
    ... >
Kết quả: Ứng dụng hiển thị icon custom riêng biệt, vuông vức, chuyên nghiệp và đồng bộ trên màn hình chính của mọi thiết bị di động.

2. Hiện thực hóa Tính năng Đa ngôn ngữ (Locale - Tiếng Trung Quốc Giản thể)
Tận dụng triệt để cơ chế tự động phân giải tài nguyên của OS, dự án đã triển khai ngôn ngữ thứ 2 cho ứng dụng là tiếng Trung Quốc.
Giải pháp triển khai: Xây dựng thêm một tệp tin cấu hình ngôn ngữ đặt tại đường dẫn đặc biệt: app/src/main/res/values-zh/strings.xml.
Kịch bản thực nghiệm: Khi người dùng vào Cài đặt máy ➔ Đổi ngôn ngữ hệ thống sang 简体中文 (Chinese Simplified) ➔ Mở lại ứng dụng, toàn bộ tiêu đề nền, nhãn thanh tiến trình và văn bản trên các nút bấm tự động chuyển sang chữ tượng hình Trung Quốc (专业领域, 学习进度, 未掌握, 已掌握) một cách đồng bộ mà không cần can thiệp bằng bất kỳ dòng code Java nào.
3. Hiện thực hóa Tính năng Đa giao diện (Dark Mode / Chế độ ban đêm)
Để bảo vệ thị giác người dùng khi học tập trong điều kiện thiếu sáng, ứng dụng đã kích hoạt thành công tính năng Dark Mode tự động.
Giải pháp triển khai: Thiết lập tệp tin quản lý màu sắc ban đêm đặt tại phân vùng: app/src/main/res/values-night/colors.xml. Hệ thống định nghĩa lại các mã màu HEX chuẩn 8 ký tự quy định độ trong suốt Alpha (#AARRGGBB) để ép CardView chuyển sang tone xám tối (#1E293B) và chữ đổi sang màu trắng sáng.
Kịch bản thực nghiệm: Khi thiết bị kích hoạt Chế độ tối (Dark Mode) trên thanh thông báo, giao diện ứng dụng tự động đảo sắc hoàn toàn một cách mượt mà và trực quan.
Giao diện trên điện thoại
Icon app như đã cấu hình, spiner cho phép thả xuống 1 list danh sách chuyên ngành từ vựng.

Screenshot_2026-06-07-10-27-38-000_com miui home Screenshot_2026-06-07-10-26-59-393_com example itflashcardapp
Giao diện sáng tối

Screenshot_2026-06-07-10-26-06-651_com example itflashcardapp Screenshot_2026-06-07-10-26-20-250_com example itflashcardapp
Lật thẻ, mặt trước là tiếng anh, mặt sau giải nghĩa tiếng việt

Screenshot_2026-06-07-10-26-45-646_com example itflashcardapp Screenshot_2026-06-07-10-26-51-915_com example itflashcardapp
#  CHƯƠNG 4: CÁC NỘI DUNG SÁNG TẠO ĐÃ LÀM THÊM (NÂNG CAO)
Vượt trên các yêu cầu cơ bản của đề tài, ứng dụng đã nghiên cứu sâu và hiện thực hóa thành công các giải pháp công nghệ cao cấp của Hệ điều hành Android nhằm tối ưu hóa sản phẩm:

1. Tích hợp Icon Custom độc quyền trong Hệ thống Tài nguyên
Ứng dụng không sử dụng icon robot mặc định của Android Studio. Dự án đã tự thiết kế tệp tin biểu tượng icon.png riêng biệt.

Giải pháp triển khai: Di chuyển tệp tin hình ảnh từ thư mục thô assets vào đúng phân vùng quản lý tài nguyên app/src/main/res/drawable/ để hệ thống định danh.
