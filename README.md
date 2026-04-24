# Нюансы при работе с Clover/Техник 🚁

## 📚 Содержание

- [Виртуалка](#-виртуалка)
- [Сбор данных](#-сбор-данных)
- [Разметка датасета](#-разметка-датасета)
- [Обучение YOLOv8](#-обучение-yolov8)
- [Тестирование модели](#-тестирование-модели)
- [Интеграция с Clover](#-интеграция-с-clover)
- [Полезные ссылки](#-полезные-ссылки)

---

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
```bash
pip uninstall torch torchvision torchaudio -y
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
```
Базовый фрагмент для инициализации видеозаписи:

```python
import cv2

def start_video_writer(output_path='flight_video.avi', fps=15.0, frame_size=(320, 240)):
    fourcc = cv2.VideoWriter_fourcc(*'MJPG')
    return cv2.VideoWriter(output_path, fourcc, fps, frame_size)
