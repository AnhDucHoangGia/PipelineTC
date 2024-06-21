# Mô tả module

## Mô tả chung

Module nhận đầu vào là thư mục các file NetCDF (đã Agg / chưa Agg), cho ra kết quả một file output.csv gồm kết quả dự báo tương ứng với dữ liệu đầu vào (label: 1/0; score: [0; 1])

## Mô tả chi tiết

### Src/Model/dataset.py

Gồm 2 class:
- ```BaseDataset```: Khởi tạo Dataset cho dữ liệu đầu vào là thư mục các file NetCDF chưa Agg
- ```AggDataset```: Khởi tạo Dataset cho dữ liệu đầu vào là thư mục các file NetCDF đã 

Các class dataset này sẽ khởi tạo bảng kết quả và xuất kết quả ra file (.csv)

### Src/Model/model.py
Gồm 2 class của 2 kiến trúc mạng ```Resnet``` và ```CNN2D```

### Src/Model/predict.py
Chứa class luồng dự đoán ```PipelinePredcit```. Class này được khởi tạo với 2 tham số chính gồm:
- ```model_architecture```: định nghĩa loại mô hình (Resnet/CNN2D)
- ```model_path```: đường dẫn đến file pt/pth lưu mô hình đã huấn luyện

### Src/Model/config.json (Điều chỉnh)
Chứa các biến đầu vào cần thiết cho Pipeline. Các tham số này cần được điền chính xác trước khi thực thi Pipeline:
- ```inp_path``` (str): Đường dẫn thư mục đầu vào
- ```isAgg``` (bool): Dữ liệu đầu vào đã được Agg chưa ?
- ```out_path``` (str): Đường dẫn thư mục đầu ra
- ```model_type`` (str): Loại mô hình sử dụng (CNN2D/Resnet)
- ```pth_path``` (str): Đường dẫn đến file lưu mô hình đã huấn luyện

### Src/pipeline.py
Luồng tổng hợp các module/class con ở trên, gồm các bước:
- Khởi tạo ```Dataset``` từ thư mục dữ liệu đầu vào
- Khởi tạo ```Model``` và tải lên các tham số của ```Model``` đã huấn luyện
- Truyền ```Dataset``` vào ```Model``` để đưa ra các dữ đoán (label: có/không bão; score: khả năng bão)
- Xuất kết quả dự đoán ra file (.csv)

### Src/main.sh
File bash script, thực thi sau khi chuẩn bị dữ liệu/config.

# Quy trình thực thi

## Quy trình cơ bản

### Chuẩn bị dữ liệu

Như trong mô tả ```Model/dataset.py```, có hai loại dữ liệu đầu vào. Mỗi loại dữ liệu này sẽ yêu cầu cách tổ chức dữ liệu đầu vào khác nhau:

- ```BaseDataset``` (Dữ liệu chưa Agg): Mỗi sample cho mô hình là một thư mục (tên bất kỳ) chứa 5 file NetCDF đánh số từ 0 đến 4 (0.nc, 1.nc, ... 4.nc), các sample được đặt trực tiếp trong thư mục đầu vào. File số 0 tương ứng với thời điểm đưa ra dự báo, các file 1 đến 4 tương ứng với step 1 đến 4 (6h đến 24h) trước thời điểm đưa ra dự báo. Ví dụ minh họa trong thư mục ```Input1```:
    - Input1/
        - sample_1/
            - 0.nc
            - 1.nc
            ...
            - 4.nc
        - sample_.../
            ...

- ```AggDataset``` (Dữ liệu đã Agg): Mỗi sample là một file NetCDF (tên bất kỳ), các sample được đặt trực tiếp trong thư mục đầu vào. Ví dụ minh họa trong thư mục ```Input2```:
    - Input2
        - sample_1.nc
        - sample_2.nc
        ...
        - sample_5.nc

### Cấu hình file config.json
Cấu trúc các tham số trong file config.json đã mô tả ở phần ```Src/Model/config.json```. Lưu ý là các tham số cần tương thích với nhau.

### Thực thi file main.sh
```
bash main.sh
```

### Kết quả đầu ra
Kết quả dự đoán của mô hình cho dữ liệu đầu vào sẽ được xuất ra file ```output.csv``` nằm trong thư mục đầu ra được cấu hình ở biến ```out_path``` trong file ```config.json```