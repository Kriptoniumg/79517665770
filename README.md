# Нюансы при работе с Clover/Техник 🚁

## ⚙️ Виртуалка

- /home/rosbase/catkin_ws/src/clover/clover_simulation/src/clover_simulation/marker.py
  130 строчка
- Вместо
- ```python
  marker_image[1:marker_border_bits - 1, 1:marker_border_bits - 1] = cv2.aruco.drawMarker(
    aruco_dict, marker.id_, marker_outer_bits)
  ```
- На
- ```python
  marker_image[1:marker_border_bits - 1, 1:marker_border_bits - 1] = cv2.aruco.generateImageMarker(
    aruco_dict, marker.id_, marker_outer_bits, borderBits=1)
  ```
  **Рашрирение памяти**
  ```bash
  sudo apt install cloud-guest-utils


  sudo growpart /dev/sda 2


  sudo partprobe /dev/sda


  sudo partprobe /dev/sda 5


  sudo partprobe /dev/sda


  sudo resize2fs /dev/sda5

  df -h /

  sudo growpart /dev/sda 2

  sudo growpart /dev/sda 5

  sudo partprobe /dev/sda

  sudo resize2fs /dev/sda5
---

## 📹 Yolo

## 🏷️ Разметка датасета

1. **Создайте структуру папок** в терминале Linux:
   ```bash
   mkdir -p clover_yolo/{train/{images,labels},val/{images,labels}}
2. ** dataset.yaml**
   ```bash
   nano dataset.yaml
   ```
   ```yaml
   # dataset.yaml
    train: /home/clover/clover_yolo/train/images
    val: /home/clover/clover_yolo/val/images

    nc: 3
    names: ['grebnik', 'brakonier', 'tyrist']
  **labelImg**
  ```bash
    pip install labelImg
    labelImg
  ```
  **Фикс**
  ```bash
    pip uninstall torch torchvision torchaudio -y
    pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
  ```
## Обучение
  ```bash
    yolo detect train model=yolov8n.pt data=/home/clover/clover_yolo/dataset.yaml epochs=100 imgsz=320 batch=8 name=clover_objects
  ```
  ```bash
    cp runs/detect/clover_detector/weights/best.pt /home/clover/
  ```
  Проверка на отдельном изображении:
  ```bash
      yolo predict model=/home/clover/best.pt source=/home/clover/clover_yolo/val/images/imiapomenai.jpg imgsz=320
  ```
---
## Sudo apt update
  ```bash
  sudo nano /etc/apt/sources.list
```

поменять это: deb http://raspbian.raspberrypi.org/raspbian/ buster main contrib non-free rpi

на вот это: 
  ```bash
deb http://legacy.raspbian.org/raspbian/ buster main contrib non-free rpi
  ```
