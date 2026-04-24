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
- 130 строчке
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

## 📹 Сбор данных

Для обучения модели необходимо записать видео с бортовой камеры дрона, на котором присутствуют целевые объекты.  
Пример кода для захвата видео во время полёта можно найти в статье на Habr:  
[«Распознавание объектов с помощью YOLO на дроне Clover»](https://habr.com/ru/articles/821971/) (раздел «Запись видео»).  

Базовый фрагмент для инициализации видеозаписи:

```python
import cv2

def start_video_writer(output_path='flight_video.avi', fps=15.0, frame_size=(320, 240)):
    fourcc = cv2.VideoWriter_fourcc(*'MJPG')
    return cv2.VideoWriter(output_path, fourcc, fps, frame_size)
