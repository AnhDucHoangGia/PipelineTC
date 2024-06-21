# Kiểm thử

## Dữ liệu kiểm thử

Gồm 2 Dataset:

- Chưa Agg: 10 sample, mỗi sample gồm 5 file NetCDF tương ứng 5 step. Minh họa:
    - Input1/
        - sample_1/
            - 0.nc
            - 1.nc
            ...
            - 4.nc
        - sample_2/
            ...
        ...
        - sample_10/
            ...

- Đã Agg: 10 sample, mỗi sample là 1 file NetCDF. Minh họa:
    - Input2/
        - sample_1.nc
        - sample_2.nc
        ...
        - sample_10.nc

## Kịch bản kiểm thử
Với từng Dataset trong 2 Dataset trên, điều chỉnh các tham số trong file ```config.json``` tương thích với Dataset. Sau đó thực thi dòng lệnh:
```
bash main.sh
```

## Kết quả kiểm thử
Kết quả thời gian chạy sẽ gồm thời gian chuẩn bị (khởi tạo các class/def, import các thư viện) và thời gian chạy mô hình đưa ra dự đoán.

- Dataset chưa Agg:
    + Thời gian chuẩn bị: 6.44s
    + Thời gian dự đoán: 2.36s / 10 samples

- Dataset đã Agg:
    + Thời gian chuẩn bị: 6.53s
    + Thời gian dự đoán: 2.26s / 10 samples
