# Нюансы при работе с Clover/Техник 🚁

## ⚙️ Gazebo & Yolo
  **Рашрирение памяти**
  ```bash
  sudo apt install cloud-guest-utils
  
  df -h /

  sudo growpart /dev/sda 2

  sudo growpart /dev/sda 5

  sudo partprobe /dev/sda

  sudo resize2fs /dev/sda5
```
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
    names: ['name1', 'name2', 'name3']
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
---
**Как пользоваться YOLO можно найти здесь:
https://habr.com/ru/articles/821971/**
