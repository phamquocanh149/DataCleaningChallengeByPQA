# 5-Day Data Cleaning Challenge - Tổng Hợp Toàn Bộ Kiến Thức & Thực Hành

> **Tài liệu tóm tắt toàn diện chuỗi thử thách 5 ngày làm sạch dữ liệu (5-Day Data Cleaning Challenge)**  
> Kho lưu trữ: `DataCleanChallenge` | Ngôn ngữ: `Python 3`

---

## 📌 Mục lục
1. [Tổng quan thử thách](#-tổng-quan-thử-thách)
2. [Ngày 1: Xử lý dữ liệu bị thiếu (Handling Missing Values)](#-ngày-1-xử-lý-dữ-liệu-bị-thiếu-handling-missing-values)
3. [Ngày 2: Thay đổi thang đo và Chuẩn hóa dữ liệu (Scaling & Normalization)](#-ngày-2-thay-đổi-thang-đo-và-chuẩn-hóa-dữ-liệu-scaling--normalization)
4. [Ngày 3: Phân tích cú pháp ngày tháng (Parsing Dates)](#-ngày-3-phân-tích-cú-pháp-ngày-tháng-parsing-dates)
5. [Ngày 4: Mã hóa ký tự (Character Encodings)](#-ngày-4-mã-hóa-ký-tự-character-encodings)
6. [Ngày 5: Xử lý dữ liệu nhập không đồng nhất (Inconsistent Data Entry)](#-ngày-5-xử-lý-dữ-liệu-nhập-không-đồng-nhất-inconsistent-data-entry)
7. [Bảng tổng hợp hàm & Thư viện theo ngày](#-bảng-tổng-hợp-hàm--thư-viện-theo-ngày)

---

## 📖 Tổng quan thử thách

Trong quy trình Khoa học dữ liệu (Data Science) và Học máy (Machine Learning), bước **Làm sạch dữ liệu (Data Cleaning / Data Munging)** thường chiếm tới **70% - 80%** thời gian làm việc. Dữ liệu thô thu thập từ thế giới thực luôn chứa đựng sai sót: ô trống (missing values), thang đo lệch pha, định dạng ngày tháng sai lệch, lỗi font chữ (mojibake), và lỗi chính tả do con người nhập liệu.

Chuỗi thử thách 5 ngày trang bị các kỹ thuật chuẩn mực bằng Python để giải quyết triệt để 5 bài toán cốt lõi này.

---

## 📅 Ngày 1: Xử lý dữ liệu bị thiếu (Handling Missing Values)

### 1. Nội dung học
* Nhận diện và thống kê mức độ khuyết thiếu dữ liệu trong DataFrame.
* Hình thành **"Trực giác dữ liệu" (Data Intuition)**: Phân biệt bản chất nguyên nhân gây thiếu dữ liệu.
* Đánh giá rủi ro khi xóa dữ liệu (`dropna`) và áp dụng các chiến lược quy gán/điền dữ liệu khuyết (`fillna`).

### 2. Thư viện sử dụng
* `pandas`: Thao tác cấu trúc dữ liệu DataFrame/Series và các phương thức xử lý `NaN`/`None`.
* `numpy`: Hỗ trợ tính toán ma trận và sinh mảng số ngẫu nhiên (`np.product`, `np.random.seed`).

### 3. Lý thuyết trọng tâm
* **Bản chất của giá trị bị thiếu**:
  * **Không tồn tại (*Doesn't exist*)**: Giá trị thực sự không có trong thực tế. Ví dụ: Cột `Street Number Suffix` (hậu tố số nhà như 123A, 123B) bị trống vì căn nhà chỉ mang số 123 đơn thuần; hoặc cột `PenalizedTeam` (đội bị phạt) bị trống vì pha bóng đó không có lỗi. **Giải pháp:** Giữ nguyên `NaN` hoặc điền nhãn đại diện (như `"None"`, chuỗi rỗng `""`). Tuyệt đối không đoán hay lấy giá trị trung bình để điền vào.
  * **Không được ghi nhận (*Wasn't recorded*)**: Giá trị có tồn tại trong thực tế nhưng bị sót trong quá trình thu thập/nhập liệu. Ví dụ: `Zipcode` (mã bưu chính của một công trình tại thành phố) hoặc `TimeSecs` (thời gian thi đấu còn lại). **Giải pháp:** Có thể dự đoán hoặc quy gán (imputation) dựa trên các cột liên quan khác hoặc dùng giá trị hàng xóm / thống kê.
* **Hậu quả của việc xóa bỏ bừa bãi**:
  * Xóa hàng (`dropna()`): Nếu hầu hết các hàng đều có ít nhất 1 cột bị khuyết, lệnh này sẽ **xóa sạch toàn bộ bảng dữ liệu** (DataFrame rỗng).
  * Xóa cột (`dropna(axis=1)`): Mất đi rất nhiều đặc trưng (features) quan trọng cần thiết cho phân tích/mô hình.

### 4. Các hàm mới, chức năng & Ví dụ

| Hàm / Thuộc tính | Cú pháp & Chức năng | Ví dụ minh họa |
| :--- | :--- | :--- |
| `df.isnull()` / `df.isna()` | Kiểm tra từng phần tử có bị khuyết thiếu hay không, trả về DataFrame boolean (`True`/`False`). | `null_mask = df.isnull()` |
| `df.isnull().sum()` | Đếm tổng số lượng ô bị khuyết trên từng cột. | `missing_counts = nfl_data.isnull().sum()` |
| `np.product(df.shape)` | Tính tổng số lượng ô (cells) trong toàn bộ DataFrame. | `total_cells = np.product(df.shape)` |
| `df.dropna(axis=0)` | Xóa các hàng chứa ít nhất một giá trị `NaN` (mặc định `axis=0`). | `clean_rows = df.dropna()` |
| `df.dropna(axis=1)` | Xóa các cột chứa ít nhất một giá trị `NaN`. | `clean_cols = df.dropna(axis=1)` |
| `df.fillna(value)` | Điền toàn bộ các giá trị `NaN` bằng một giá trị cố định (ví dụ số 0 hoặc chuỗi). | `df_filled = df.fillna(0)` |
| `df.fillna(method='bfill')` / `df.bfill()` | Điền lùi (Backward Fill): Lấy giá trị của ô ngay phía sau trong cùng cột để đắp vào ô `NaN`. Thích hợp cho dữ liệu có thứ tự thời gian/logic. | `df.bfill().fillna(0)` |
| `df.fillna(method='ffill')` / `df.ffill()` | Điền tiến (Forward Fill): Lấy giá trị của ô ngay phía trước trong cùng cột để đắp vào ô `NaN`. | `df.ffill().fillna(0)` |
| `df.sample(n)` | Lấy ngẫu nhiên $n$ hàng dữ liệu để quan sát tổng quan. | `df.sample(5)` |

**Ví dụ code tổng hợp:**
```python
import numpy as np
import pandas as pd

# 1. Thống kê tỷ lệ dữ liệu bị thiếu
missing_val_count = sf_permits.isnull().sum()
total_cells = np.product(sf_permits.shape)
total_missing = missing_val_count.sum()
percent_missing = (total_missing / total_cells) * 100
print(f"Dữ liệu bị thiếu: {percent_missing:.2f}%")

# 2. Điền lùi và đắp số 0 cho các giá trị còn sót ở cuối cột
sf_permits_imputed = sf_permits.bfill().fillna(0)
```

### 5. Một số lỗi thường gặp
* **`DtypeWarning: Columns (...) have mixed types`**:
  * *Nguyên nhân:* Xảy ra khi dùng `pd.read_csv()` với tập dữ liệu lớn. Pandas đọc file theo từng khối (chunk) và tự suy luận kiểu dữ liệu, nếu một cột ở chunk đầu là số nhưng chunk sau có chuỗi, cảnh báo này sẽ xuất hiện.
  * *Khắc phục:* Khai báo tham số `low_memory=False` hoặc chỉ định kiểu dữ liệu tường minh qua `dtype={'col_name': str}`.
* **Xóa xong mất sạch dữ liệu (`Empty DataFrame`)**:
  * *Nguyên nhân:* Gọi `df.dropna()` mặc định khi tất cả các hàng đều khuyết ít nhất 1 thuộc tính.
  * *Khắc phục:* Kiểm tra tỷ lệ khuyết trước, ưu tiên chọn lọc `df.dropna(subset=['col1', 'col2'])` hoặc dùng kỹ thuật điền `fillna()`.
* **Cảnh báo `FutureWarning: method='bfill' is deprecated`**:
  * *Nguyên nhân:* Trong các phiên bản Pandas mới (>= 2.1), `df.fillna(method='bfill')` bị thay thế.
  * *Khắc phục:* Sử dụng trực tiếp `df.bfill()` hoặc `df.ffill()`.
* **Ô cuối cùng vẫn là `NaN` sau khi `bfill`**:
  * *Nguyên nhân:* Nếu ô cuối cùng của cột bị `NaN`, phía sau nó không còn giá trị nào để điền lùi.
  * *Khắc phục:* Luôn chuỗi thêm `.fillna(0)` hoặc giá trị mặc định ở cuối: `df.bfill().fillna(0)`.

---

## 📅 Ngày 2: Thay đổi thang đo và Chuẩn hóa dữ liệu (Scaling & Normalization)

### 1. Nội dung học
* Hiểu rõ sự khác biệt bản chất giữa **Scaling** (Đổi quy mô / Thang đo) và **Normalization** (Chuẩn hóa phân phối).
* Biết trường hợp nào cần dùng kỹ thuật nào dựa trên đặc điểm dữ liệu và mô hình thuật toán.
* Thực hành kỹ thuật Min-Max Scaling và Phép biến đổi Box-Cox (Box-Cox Transformation).

### 2. Thư viện sử dụng
* `scipy.stats`: Thư viện tính toán thống kê, dùng hàm `boxcox` để đưa dữ liệu về phân phối chuẩn Gaussian.
* `mlxtend.preprocessing`: Thư viện mở rộng học máy, dùng hàm `minmax_scaling` để scale dữ liệu.
* `seaborn` & `matplotlib.pyplot`: Trực quan hóa phân phối trước và sau biến đổi (`sns.distplot` / `sns.histplot`).

### 3. Lý thuyết trọng tâm
* **So sánh Scaling vs. Normalization**:
  | Tiêu chí | Scaling (Đổi quy mô / Thang đo) | Normalization (Chuẩn hóa) |
  | :--- | :--- | :--- |
  | **Bản chất biến đổi** | Thay đổi **khoảng giá trị (range)**, giữ nguyên **hình dạng phân phối (shape of distribution)**. | Thay đổi **hình dạng phân phối**, biến phân phối lệch (skewed/exponential) thành **phân phối chuẩn (Gaussian / Bell Curve)**. |
  | **Khoảng giá trị đầu ra** | Thường nằm gọn trong $[0, 1]$ hoặc $[-1, 1]$. | Giá trị biến đổi theo tham số $\lambda$ sao cho đối xứng quanh kỳ vọng ($\mu$). |
  | **Khi nào sử dụng?** | Khi thuật toán học máy dựa trên **khoảng cách hình học** giữa các điểm dữ liệu (KNN, SVM, K-Means, PCA, Gradient Descent). Nếu không scale, biến có giá trị lớn (ví dụ: Yên Nhật, tiền lương) sẽ lấn át hoàn toàn biến có giá trị nhỏ (USD, số năm kinh nghiệm). | Khi thuật toán học máy hoặc kiểm định thống kê **giả định dữ liệu tuân theo phân phối chuẩn** (Linear Regression, Logistic Regression, LDA, Gaussian Naive Bayes, t-test, ANOVA). |
  | **Phương pháp tiêu biểu** | Min-Max Scaling, Robust Scaling, StandardScaler. | Box-Cox Transformation, Yeo-Johnson, Log Transform. |

* **Ví dụ thực tế**:
  * *Hồi quy tuyến tính dự đoán điểm số dựa trên số giờ học (dữ liệu lệch nặng)*: 👉 **Dùng Normalization** vì mô hình Linear Regression giả định phân phối chuẩn.
  * *So sánh số lần Jumping Jacks (TB: 40 cái/phút) và Push-ups (TB: 10 cái/phút)*: 👉 **Dùng Scaling** để đưa 2 bài tập về cùng thang đo so sánh công bằng mà không làm méo mó phân phối tự nhiên của bài tập.

### 4. Các hàm mới, chức năng & Ví dụ

| Hàm / Phương thức | Thư viện | Cú pháp & Chức năng | Ví dụ minh họa |
| :--- | :--- | :--- | :--- |
| `minmax_scaling(data, columns)` | `mlxtend` | Biến đổi các cột dữ liệu số về khoảng $[0, 1]$ theo công thức: $X_{scaled} = \frac{X - X_{min}}{X_{max} - X_{min}}$. | `scaled = minmax_scaling(df.goal, columns=[0])` |
| `stats.boxcox(data)` | `scipy.stats` | Thực hiện phép biến đổi Box-Cox lũy thừa để đưa phân phối dương về phân phối chuẩn Gaussian. Trả về `(transformed_array, lambda)`. | `norm_data, lmbda = stats.boxcox(positive_data)` |
| `np.random.exponential(size)` | `numpy` | Tạo mảng mẫu số ngẫu nhiên theo phân phối mũ (dạng lệch chữ L). | `data = np.random.exponential(size=1000)` |
| `sns.distplot(data, ax=ax)` | `seaborn` | Vẽ biểu đồ tần suất kèm đường cong ước lượng mật độ kernel (KDE). | `sns.distplot(scaled_data, ax=ax[1])` |

**Ví dụ code tổng hợp:**
```python
import matplotlib.pyplot as plt
from mlxtend.preprocessing import minmax_scaling
import numpy as np
from scipy import stats
import seaborn as sns

# 1. Min-Max Scaling cho cột mục tiêu kêu gọi vốn
usd_goal = kickstarters_2017['usd_goal_real']
scaled_goal = minmax_scaling(usd_goal, columns=[0])

# 2. Box-Cox Normalization (chỉ áp dụng cho dữ liệu > 0)
positive_pledges = kickstarters_2017.loc[
    kickstarters_2017['usd_pledged_real'] > 0, 'usd_pledged_real'
]
normalized_pledges, _ = stats.boxcox(positive_pledges)

# 3. Trực quan hóa đối chiếu
fig, ax = plt.subplots(1, 2, figsize=(10, 4))
sns.distplot(positive_pledges, ax=ax[0])
ax[0].set_title('Dữ liệu gốc (Lệch)')
sns.distplot(normalized_pledges, ax=ax[1])
ax[1].set_title('Sau khi Box-Cox (Phân phối chuẩn)')
plt.show()
```

### 5. Một số lỗi thường gặp
* **`ValueError: Data must be positive` khi gọi `stats.boxcox()`**:
  * *Nguyên nhân:* Phép biến đổi Box-Cox chỉ xác định toán học đối với các giá trị **dương nghiêm ngặt** ($x > 0$). Nếu cột có chứa giá trị bằng 0 hoặc số âm (ví dụ chiến dịch không nhận được đồng nào, `pledged == 0`), hàm sẽ văng lỗi.
  * *Khắc phục:* Lọc trước các giá trị $> 0$: `data[data > 0]` hoặc chuyển sang dùng phép biến đổi **Yeo-Johnson** (`stats.yeojohnson()`) có hỗ trợ giá trị 0 và số âm.
* **Vẽ biểu đồ Box-Cox hiển thị sai**:
  * *Nguyên nhân:* `stats.boxcox` trả về một tuple gồm 2 phần tử: `(mảng_dữ_liệu_sau_biến_đổi, giá_trị_lambda)`. Nếu truyền cả tuple vào seaborn sẽ gặp lỗi.
  * *Khắc phục:* Lấy phần tử đầu tiên `stats.boxcox(data)[0]`.

---

## 📅 Ngày 3: Phân tích cú pháp ngày tháng (Parsing Dates)

### 1. Nội dung học
* Nhận biết sự khác nhau giữa kiểu chuỗi văn bản (`object`) và kiểu thời gian (`datetime64[ns]`).
* Sử dụng bảng mã chỉ thị `strftime` để phân tích (parse) chuỗi ngày tháng sang datetime.
* Trích xuất các thuộc tính thời gian (ngày, tháng, năm, thứ trong tuần) bằng accessor `.dt`.
* Phương pháp vẽ đồ thị histogram ngày trong tháng để kiểm tra tính chính xác của việc parse ngày.

### 2. Thư viện sử dụng
* `pandas`: Chuyển đổi định dạng (`pd.to_datetime`) và xử lý dữ liệu thời gian.
* `datetime`: Module chuẩn của Python thao tác với thời gian.
* `seaborn` & `matplotlib`: Vẽ biểu đồ phân bố ngày trong tháng.

### 3. Lý thuyết trọng tâm
* **Tại sao phải parse dates?** Khi đọc file CSV, máy tính chỉ thấy ngày tháng là chuỗi ký tự thông thường (`object`/`string`). Khi đó, bạn không thể thực hiện các phép tính thời gian như: tính khoảng cách giữa 2 sự kiện, lọc dữ liệu theo mùa/năm, hay trích xuất ngày trong tháng.
* **Quy chuẩn `strftime` directives**:
  * `%d`: Ngày trong tháng (2 chữ số, từ 01 đến 31).
  * `%m`: Tháng trong năm (2 chữ số, từ 01 đến 12).
  * `%y`: Năm viết tắt 2 chữ số (ví dụ: `07` đại diện cho `2007`).
  * `%Y`: Năm đầy đủ 4 chữ số (ví dụ: `2007`).
  * `%H`, `%M`, `%S`: Giờ, phút, giây.
* **Tham số `infer_datetime_format=True`**:
  * *Ưu điểm:* Cho phép Pandas tự suy luận định dạng ngày mà không cần truyền chuỗi format thủ công.
  * *Nhược điểm:* Chạy chậm hơn đáng kể và có thể suy luận sai nếu dữ liệu có định dạng mơ hồ (ví dụ: `01/02/2020` có thể hiểu là 1 tháng 2 hoặc 2 tháng 1). Do đó, khi đã biết rõ format, **luôn nên chỉ định format tường minh**.
* **Phương pháp kiểm chứng (Sanity Check)**: Sau khi trích xuất ngày trong tháng (`.dt.day`), vẽ biểu đồ cột (histogram 31 bins). Nếu phân phối trải đều từ ngày 1 đến ngày 31 (giảm nhẹ ở ngày 31), việc parse ngày là chính xác. Nếu các cột chỉ dồn từ ngày 1 đến 12 và hoàn toàn vắng bóng các ngày 13-31, chứng tỏ bạn đã bị **ngược giữa Ngày và Tháng** (`%m/%d` bị nhầm thành `%d/%m`).

### 4. Các hàm mới, chức năng & Ví dụ

| Hàm / Thuộc tính | Cú pháp & Chức năng | Ví dụ minh họa |
| :--- | :--- | :--- |
| `series.dtype` | Kiểm tra kiểu dữ liệu của cột. Trả về `dtype('O')` nếu là chuỗi (object) hoặc `dtype('<M8[ns]')` nếu là datetime. | `landslides['date'].dtype` |
| `pd.to_datetime(series, format=...)` | Chuyển đổi cột chuỗi thành kiểu `datetime64[ns]` dựa theo chuỗi định dạng chỉ thị strftime. | `pd.to_datetime(df['date'], format="%m/%d/%y")` |
| `series.dt.day` | Accessor `.dt` cho phép trích xuất ngày trong tháng (từ 1 đến 31). | `df['parsed_date'].dt.day` |
| `series.dt.month` / `year` | Trích xuất tháng / năm từ cột datetime. | `df['parsed_date'].dt.year` |
| `series.str.extract(r'regex')` | Trích xuất nhóm chuỗi con theo biểu thức chính quy (Regex). | `df['raw'].str.extract(r'(\d+)\s*(CE\|BCE)')` |
| `pd.to_numeric(series, errors='coerce')` | Chuyển đổi chuỗi sang dạng số, nếu gặp lỗi không ép kiểu được sẽ tự động biến thành `NaN`. | `pd.to_numeric(df['year'], errors='coerce')` |

**Ví dụ code tổng hợp:**
```python
import pandas as pd
import seaborn as sns

# 1. Parse cột ngày tháng với format cụ thể
landslides['date_parsed'] = pd.to_datetime(
    landslides['date'], format='%m/%d/%y'
)

# 2. Trích xuất ngày trong tháng
day_of_month = landslides['date_parsed'].dt.day

# 3. Kiểm tra tính hợp lệ bằng biểu đồ phân phối 31 bins
day_of_month_clean = day_of_month.dropna()
sns.distplot(day_of_month_clean, kde=False, bins=31)
```

### 5. Một số lỗi thường gặp
* **`AttributeError: Can only use .dt accessor with datetimelike values`**:
  * *Nguyên nhân:* Cố gắng gọi `.dt.day` hoặc `.dt.month` trực tiếp trên cột có kiểu `object` (chưa qua `pd.to_datetime`).
  * *Khắc phục:* Phải chuyển đổi cột sang datetime trước khi dùng thuộc tính `.dt`.
* **`ValueError: time data '...' does not match format '...'`**:
  * *Nguyên nhân:* Một số dòng trong file có format khác hoặc chứa text rác (ví dụ: vừa có `2015-01-02` vừa có `02/01/2015` hoặc `1975-02-23T02:58:41.000Z`).
  * *Khắc phục:* Dùng `pd.to_datetime(..., infer_datetime_format=True, errors='coerce')` để các dòng sai định dạng tự chuyển thành `NaT` (Not a Time) thay vì làm dừng chương trình.

---

## 📅 Ngày 4: Mã hóa ký tự (Character Encodings)

### 1. Nội dung học
* Hiểu cơ chế mã hóa ký tự: Từ byte nhị phân sang ký tự đọc được.
* Phân biệt giữa kiểu `str` và kiểu `bytes` trong Python 3.
* Nhận diện các lỗi mã hóa: Ký tự rác (**Mojibake** `æ–‡å—åŒ–ã??`) và ký tự không xác định (Unknown `` hoặc `?`).
* Ứng dụng thư viện `chardet` để tự động phát hiện bảng mã gốc của file dữ liệu.
* Đọc và lưu trữ dữ liệu chuẩn hóa về định dạng UTF-8.

### 2. Thư viện sử dụng
* `chardet`: Thư viện phát hiện tự động bảng mã ký tự của chuỗi byte nhị phân.
* `pandas`: Đọc file CSV với tham số `encoding` tùy biến và ghi file chuẩn hóa UTF-8.

### 3. Lý thuyết trọng tâm
* **Mã hóa ký tự là gì?** Máy tính chỉ hiểu các byte nhị phân (`0` và `1`). Mã hóa ký tự (Character Encoding) là một bảng quy tắc ánh xạ dãy byte nhị phân thành ký tự mà con người đọc được.
* **Chuẩn UTF-8 vs ASCII vs Windows-1252**:
  * **ASCII**: Bảng mã cổ điển của Mỹ, chỉ dùng 7-bit đại diện cho 128 ký tự tiếng Anh cơ bản, không hỗ trợ dấu thanh tiếng Việt hay biểu tượng quốc tế (€, ¥, chữ Hán...).
  * **Windows-1252 / Latin-1 (ISO-8859-1)**: Bảng mã mở rộng của Microsoft cho các ngôn ngữ Tây Âu, dùng 8-bit. Rất nhiều file CSV xuất từ Excel chạy trên Windows mặc định lưu theo bảng mã này.
  * **UTF-8**: Chuẩn mã hóa phổ biến và toàn diện nhất hiện nay, tương thích với hầu hết mọi ngôn ngữ trên thế giới và là chuẩn mặc định của Python 3.
* **Mojibake vs. Unknown Replacement**:
  * *Mojibake:* Xảy ra khi đọc chuỗi byte bằng một bảng mã khác nhưng vẫn giải mã được ra ký tự khác sai lệch (ví dụ: chữ tiếng Nhật/Việt bị biến thành các ký tự lộn xộn tiếng Pháp/Nga).
  * *Unknown Replacement:* Xảy ra khi byte không tồn tại trong bảng mã mục tiêu, hệ thống buộc phải thay bằng dấu `?` hoặc `\ufffd` (``). Nếu đã bị thay thế thành `?`, dữ liệu gốc sẽ **mất vĩnh viễn** và không thể cứu vãn.
* **Nguyên tắc vàng:** Luôn đọc dữ liệu bằng đúng bảng mã nguồn của nó, và **ngay lập tức lưu lại thành UTF-8** để tái sử dụng xuyên suốt dự án.

### 4. Các hàm mới, chức năng & Ví dụ

| Hàm / Phương thức | Thư viện | Cú pháp & Chức năng | Ví dụ minh họa |
| :--- | :--- | :--- | :--- |
| `str.encode(encoding, errors)` | Python built-in | Chuyển đổi chuỗi (`str`) thành mảng byte (`bytes`). Tham số `errors` có thể chọn: `'strict'`, `'replace'`, `'ignore'`, `'backslashreplace'`. | `bytes_data = text.encode('utf-8')` |
| `bytes.decode(encoding)` | Python built-in | Giải mã mảng byte (`bytes`) ngược lại thành chuỗi (`str`). | `text = bytes_data.decode('utf-8')` |
| `open(path, 'rb')` | Python built-in | Mở file ở chế độ đọc nhị phân (Read Binary) để lấy chuỗi byte thô phục vụ việc đoán mã hóa. | `with open(file, 'rb') as f:` |
| `chardet.detect(raw_bytes)` | `chardet` | Dự đoán bảng mã của chuỗi byte. Trả về dict: `{'encoding': '...', 'confidence': ..., 'language': '...'}`. | `result = chardet.detect(raw_bytes)` |
| `pd.read_csv(..., encoding=...)` | `pandas` | Đọc file CSV bằng bảng mã chỉ định thay vì dùng UTF-8 mặc định. | `pd.read_csv(file, encoding='Windows-1252')` |
| `df.to_csv(..., encoding='utf-8')` | `pandas` | Lưu DataFrame ra file CSV chuẩn UTF-8 (mặc định trong Pandas). | `df.to_csv("clean_data.csv")` |

**Ví dụ code tổng hợp:**
```python
import chardet
import pandas as pd

file_path = "PoliceKillingsUS.csv"

# 1. Đọc thử 100,000 byte đầu tiên ở chế độ binary
with open(file_path, "rb") as rawdata:
  result = chardet.detect(rawdata.read(100000))

print(
    f"Bảng mã dự đoán: {result['encoding']} (Độ tin cậy: {result['confidence']})"
)

# 2. Đọc file CSV bằng đúng encoding tìm được
df = pd.read_csv(file_path, encoding=result["encoding"])

# 3. Lưu lại ngay lập tức sang định dạng UTF-8 chuẩn
df.to_csv("PoliceKillingsUS_utf8.csv", index=False)
```

### 5. Một số lỗi thường gặp
* **`UnicodeDecodeError: 'utf-8' codec can't decode byte ...`**:
  * *Nguyên nhân:* Cố đọc file CSV không phải UTF-8 (ví dụ Windows-1252 hoặc Latin-1) bằng lệnh `pd.read_csv()` mặc định.
  * *Khắc phục:* Dùng `chardet.detect()` để tìm ra bảng mã và truyền vào tham số `encoding=...` khi đọc.
* **`UnicodeEncodeError: 'ascii' codec can't encode character ...`**:
  * *Nguyên nhân:* Cố ép chuỗi chứa ký tự đặc biệt/tiếng Việt sang chuẩn ASCII.
  * *Khắc phục:* Sử dụng `utf-8` hoặc cấu hình tham số `errors='backslashreplace'`.
* **`chardet` đoán sai encoding**:
  * *Nguyên nhân:* Đọc quá ít byte mẫu ở đầu file (ví dụ 10,000 byte đầu chỉ chứa tên cột tiếng Anh thuần ASCII), dẫn tới `chardet` đoán là ASCII, nhưng tới dòng thứ 500 lại có ký tự lạ khiến `read_csv` văng lỗi.
  * *Khắc phục:* Tăng dung lượng đọc mẫu lên `50,000` đến `100,000` byte: `rawdata.read(100000)`.

---

## 📅 Ngày 5: Xử lý dữ liệu nhập không đồng nhất (Inconsistent Data Entry)

### 1. Nội dung học
* Phát hiện các lỗi nhập liệu thiếu nhất quán trong dữ liệu dạng văn bản phân loại (Categorical Text).
* Thực hiện tiền xử lý văn bản sơ bộ (Text Pre-processing): Viết thường (`lower`) và xóa khoảng trắng thừa (`strip`).
* Áp dụng thuật toán **Khớp mờ (Fuzzy Matching)** dựa trên khoảng cách chỉnh sửa Levenshtein để tự động nhận diện và gộp các biến thể sai chính tả.
* Xây dựng hàm chuẩn hóa dữ liệu tự động tái sử dụng.

### 2. Thư viện sử dụng
* `fuzzywuzzy`: Thư viện tính toán độ tương đồng giữa các chuỗi ký tự theo thuật toán Levenshtein Distance.
* `fuzzywuzzy.process`: Module tìm kiếm các chuỗi gần nhất trong danh sách.
* `pandas` & `numpy`: Thao tác lọc và cập nhật dữ liệu.

### 3. Lý thuyết trọng tâm
* **Nguyên nhân dữ liệu thiếu nhất quán**: Khi dữ liệu được nhập tay bởi nhiều người dùng khác nhau (ví dụ tên thành phố, tên trường đại học, địa chỉ), các biến thể không đồng nhất sẽ phát sinh:
  * Viết hoa lộn xộn: `'Lahore'`, `'lahore'`, `'LAHORE'`.
  * Khoảng trắng thừa: `'Karachi '` vs `'Karachi'`.
  * Lỗi gõ phím / viết tắt: `'d. i khan'`, `'d.i khan'`, `'kuram agency'`, `'kurram agency'`.
* **Hiệu quả của Tiền xử lý sơ bộ**:
  * Chỉ cần áp dụng `str.lower()` kết hợp `str.strip()`, bạn đã có thể loại bỏ được tới **80%** sự không nhất quán trong văn bản mà không làm mất thông tin!
* **Nguyên lý Khớp mờ (Fuzzy Matching)**:
  * Dựa trên **Khoảng cách Levenshtein (Levenshtein Edit Distance)**: Đếm số bước tối thiểu (thêm, xóa, thay thế 1 ký tự) để biến chuỗi $A$ thành chuỗi $B$.
  * Hàm tính trả về **tỷ lệ tương đồng (ratio)** theo thang điểm từ `0` đến `100`. Điểm càng gần 100 nghĩa là 2 chuỗi càng giống nhau.
  * Sử dụng Scorer `token_sort_ratio`: Tự động tách các từ (tokens), đưa về chữ thường, sắp xếp lại theo thứ tự chữ cái rồi mới tính điểm. Giúp các chuỗi như `"New York City"` và `"City New York"` đạt điểm tương đồng 100%.
* **Cảnh báo về Ngưỡng tương đồng (Threshold)**:
  * Phải cẩn trọng khi chọn ngưỡng `min_ratio`.
  * *Ví dụ trong bài học:* `'d.i khan'` và `'d.g khan'` là 2 thành phố hoàn toàn khác nhau tại Pakistan nhưng điểm tương đồng lên tới `88%`. Nếu đặt ngưỡng là 80%, bạn sẽ gộp nhầm 2 thành phố này làm một và làm sai lệch nghiêm trọng dữ liệu địa lý! Do đó, trong trường hợp này, ngưỡng an toàn được chọn là `>= 90%`.

### 4. Các hàm mới, chức năng & Ví dụ

| Hàm / Phương thức | Thư viện | Cú pháp & Chức năng | Ví dụ minh họa |
| :--- | :--- | :--- | :--- |
| `series.str.lower()` | `pandas` | Chuyển đổi toàn bộ chuỗi trong Series về dạng chữ thường. | `df['City'] = df['City'].str.lower()` |
| `series.str.strip()` | `pandas` | Cắt bỏ toàn bộ khoảng trắng thừa ở đầu và cuối chuỗi (leading & trailing spaces). | `df['City'] = df['City'].str.strip()` |
| `series.unique()` | `pandas` | Lấy ra một mảng các giá trị duy nhất trong cột. | `cities = df['City'].unique()` |
| `array.sort()` | `numpy` | Sắp xếp mảng theo thứ tự bảng chữ cái alphabet để dễ dàng rà soát lỗi. | `cities.sort()` |
| `fuzzywuzzy.process.extract(...)` | `fuzzywuzzy` | Tìm top $k$ chuỗi có độ tương đồng cao nhất với chuỗi mục tiêu từ một danh sách. | `matches = process.extract("d.i khan", cities, limit=10)` |
| `fuzzywuzzy.fuzz.token_sort_ratio` | `fuzzywuzzy` | Phương thức tính điểm tương đồng bỏ qua thứ tự từ trong chuỗi. | `scorer = fuzz.token_sort_ratio` |
| `series.isin(list)` | `pandas` | Trả về Series boolean kiểm tra phần tử có nằm trong danh sách hay không. | `mask = df['City'].isin(close_matches)` |

**Ví dụ code tổng hợp xây dựng hàm gộp dữ liệu:**
```python
from fuzzywuzzy import fuzz, process
import pandas as pd


def replace_matches_in_column(df, column, string_to_match, min_ratio=90):
  # 1. Lấy danh sách các chuỗi duy nhất trong cột
  strings = df[column].unique()

  # 2. Tìm top 10 chuỗi gần nhất
  matches = process.extract(
      string_to_match, strings, limit=10, scorer=fuzz.token_sort_ratio
  )

  # 3. Lọc ra các chuỗi có điểm tương đồng >= min_ratio
  close_matches = [m[0] for m in matches if m[1] >= min_ratio]

  # 4. Tìm các dòng tương ứng và gán về chuỗi chuẩn
  rows_with_matches = df[column].isin(close_matches)
  df.loc[rows_with_matches, column] = string_to_match
  print(f"Đã chuẩn hóa các biến thể: {close_matches} -> '{string_to_match}'")


# Thực thi trên dữ liệu thực tế:
suicide_attacks['City'] = suicide_attacks['City'].str.lower().str.strip()
replace_matches_in_column(
    df=suicide_attacks,
    column='City',
    string_to_match='kuram agency',
    min_ratio=90,
)
```

### 5. Một số lỗi thường gặp
* **Hiệu năng chậm chạp khi chạy fuzzy matching**:
  * *Nguyên nhân:* Thuật toán tính Levenshtein Distance có độ phức tạp cao. Nếu so khớp từng hàng một trên toàn bộ DataFrame hàng triệu bản ghi, quá trình sẽ mất hàng giờ.
  * *Khắc phục:* **Chỉ chạy so khớp trên tập giá trị duy nhất** (`df[col].unique()`), sau đó dùng `isin` hoặc dictionary mapping để cập nhật ngược lại DataFrame.
* **Gộp nhầm thực thể khác nhau (False Positives)**:
  * *Nguyên nhân:* Chọn ngưỡng `min_ratio` quá thấp (ví dụ 70-80).
  * *Khắc phục:* Luôn in danh sách top matches ra xem trước điểm số của từng từ trước khi quyết định ngưỡng lọc `min_ratio`.

---

## 📊 Bảng tổng hợp hàm & Thư viện theo ngày

| Ngày | Chủ đề chính | Thư viện chính | Các hàm / Phương thức quan trọng nhất |
| :---: | :--- | :--- | :--- |
| **1** | Xử lý dữ liệu bị thiếu | `pandas`, `numpy` | `isnull()`, `sum()`, `dropna(axis=0/1)`, `fillna()`, `bfill()`, `ffill()`, `sample()` |
| **2** | Scaling & Normalization | `scipy.stats`, `mlxtend`, `seaborn` | `minmax_scaling()`, `stats.boxcox()`, `np.random.exponential()`, `sns.distplot()` |
| **3** | Phân tích ngày tháng | `pandas`, `datetime`, `seaborn` | `pd.to_datetime(format=...)`, `series.dt.day`, `series.dt.year`, `str.extract()` |
| **4** | Mã hóa ký tự | `chardet`, `pandas` | `open('rb')`, `chardet.detect()`, `str.encode()`, `bytes.decode()`, `read_csv(encoding=...)` |
| **5** | Sửa dữ liệu không nhất quán | `fuzzywuzzy`, `pandas` | `str.lower()`, `str.strip()`, `unique()`, `process.extract()`, `fuzz.token_sort_ratio`, `isin()` |

---

## 💡 Đúc kết kinh nghiệm & Best Practices
1. **Hiểu bản chất trước khi can thiệp:** Luôn tự đặt câu hỏi *Tại sao dữ liệu bị như vậy?* (Thiếu do không tồn tại hay do sót? Lệch do bản chất dữ liệu hay do nhiễu?).
2. **Luôn kiểm tra kiểu dữ liệu:** Sử dụng `df.info()` và `df.dtypes` ngay sau khi nạp file để đảm bảo các cột ngày tháng, số học không bị hiểu nhầm thành chuỗi (`object`).
3. **Ưu tiên chuẩn UTF-8:** Bất kỳ file nào đọc vào bằng bảng mã khác đều nên chuyển đổi và lưu lại bằng UTF-8 để bảo đảm tính thống nhất cho toàn bộ pipeline dữ liệu.
4. **Giữ lại dữ liệu thô gốc:** Không bao giờ ghi đè trực tiếp lên tập tin gốc. Luôn tạo các cột mới (ví dụ `date_parsed`, `City_clean`) hoặc lưu ra tệp riêng biệt sau khi làm sạch.
