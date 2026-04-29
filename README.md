# Нюансы при работе с Clover/Техник в работе с нейросетью YOLO🚁
**Подборка команд для развёртывании симулятора Gazebo и обучении YOLO прямо на виртуальной машине Clover.**
---

## ⚙️ Расширение дискового пространства под Gazebo и YOLO

При установке образа виртуальной машины корневой раздел часто занимает не всё доступное место. Gazebo и библиотеки машинного обучения потребуют гигабайты, поэтому первым делом расширяем диск.
*   **Шаг 1.** Установите утилиту `growpart`, если её ещё нет:
  ```sudo apt install cloud-guest-utils```
*   **Шаг 2.** Проверьте, сколько места сейчас занято:
  ```df -h /```
*   **Шаг 3.** Расширьте второй раздел диска Затем расширьте сам логический раздел sda5: 
```
sudo growpart /dev/sda 2
sudo growpart /dev/sda 5
```
*   **Шаг 4.** Обновите таблицу разделов и увеличьте файловую систему до нового размера:
```
sudo partprobe /dev/sda
sudo resize2fs /dev/sda5
```
---

## 📹 Yolo

## 🏷️ Разметка датасета

1. **Создайте структуру папок** в терминале Linux:
   ```bash
   mkdir -p clover_yolo/{train/{images,labels},val/{images,labels}}
**Эта команда разом создаст всё дерево:**
```
  clover_yolo/
├── train/
│   ├── images/   # сюда копируем ~80% кадров (.jpg)
│   └── labels/   # сюда LabelImg сохранит аннотации (.txt)
└── val/
    ├── images/   # оставшиеся ~20% кадров
    └── labels/   # их разметка
 ```

2. **Файл конфигурации dataset.yaml**

* **В корне clover_yolo создайте текстовый файл и пропишите пути:**
   ```bash
   nano dataset.yaml
   ```
   **Содержимое:**
   ```yaml
   # dataset.yaml
    train: /home/clover/clover_yolo/train/images
    val: /home/clover/clover_yolo/val/images

    nc: 3
    names: ['name1', 'name2', 'name3']
   ```
   **Не забудьте заменить name1, name2, name3 на реальные названия объектов.**
  **Инструмент для разметки — LabelImg**

Установите и запустите графический разметчик:
  ```bash
    pip install labelImg
    labelImg
  ```
**В программе:**

*    Откройте папку clover_yolo/train/images.

*    Укажите папку для сохранения clover_yolo/train/labels.

*    В левой панели переключите формат на YOLO.

*    Обводите объекты рамками и вводите имена классов.

*    Повторите ту же процедуру для папки val/images → val/labels.
## Переустановка PyTorch под CPU

**Если обучение планируется вести на процессоре (без видеокарты), лучше поставить специальную сборку PyTorch, оптимизированную под CPU. Это ускорит вычисления по сравнению с универсальной версией.**
  ```bash
    pip uninstall torch torchvision torchaudio -y
    pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
  ```
## Обучение
**Перейдите в домашнюю директорию (или туда, где лежит папка clover_yolo) и запустите обучение:**
  ```bash
    yolo detect train model=yolov8n.pt data=/home/clover/clover_yolo/dataset.yaml epochs=100 imgsz=320 batch=8 name=clover_objects
  ```
**Здесь:**

*    model=yolov8n.pt – предобученная nano-модель (самая лёгкая).

*    data=… – путь к вашему dataset.yaml.

*    imgsz=320 – размер кадра (камера Clover обычно 320×240).

*    name=clover_objects – папка с результатами внутри runs/detect/.
**После окончания обучения лучший чекпоинт скопируйте в удобное место:**
  ```bash
    cp runs/detect/clover_detector/weights/best.pt /home/clover/
  ```
---
**Как пользоваться YOLO можно найти здесь:
https://habr.com/ru/articles/821971/**
