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

- **Дрон COEX Clover** (физический или симулятор Gazebo)
- **ROS** + пакет `clover` (см. [официальную документацию](https://clover.coex.tech/))
- **Python 3.8+** с установленными библиотеками:
  - `ultralytics` (YOLOv8)
  - `opencv-python`
  - `cv_bridge`, `tf2_geometry_msgs` (доступны в ROS-окружении)
- **Инструмент разметки**: [LabelImg](https://github.com/HumanSignal/labelImg) или [Roboflow](https://roboflow.com/)

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
