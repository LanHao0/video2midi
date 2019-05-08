# video2midi
youtube synthesia video to midi, just for fun )

Воссоздание midi с видео роликов synthesia и ей подобным..
![Alt text](docs/mainwindow.png?raw=true "main window")

# dependency / зависимости

- python-opencv
- python-pygame
- python-midiutil

# install dependency / установка зависимостей.

```bash
sudo apt install python-opencv python-pygame python-midiutil
```

# usage / использование

##### RU:
 Скачиваем видео с понравившейся мелодией ( рекомендую 720p ) , запускаем программу, в ней регулируем клавиши жмём Q и после завершения работы будет создан midi файл.
 
##### EN:
 Download the video with your favorite melody (I recommend 720p), launch the program, adjust the keys in it, press Q and after the completion of the work a midi file will be created.

  ```
  ./v2m.py ./test_video/Not\ Evil\ -\ The\ LEGO\ Movie\ 2\ -\ The\ Second\ Part\ _\ Piano\ Tutorial\ \(Synthesia\)-9mv_hKwCzUo.mkv
  ./v2m.py ./test_video/Gotham\ City\ Guys\ -\ The\ LEGO\ Movie\ 2\ -\ The\ Second\ Part\ _\ Piano\ Tutorial\ \(Synthesia\)-y_aBUQrp5vY.webm
  ```
  RU:
  Управление:
  * **h** - показать/спрятать помощь
  * **q** - приступить к воссозданию midi
  * **s** - Установить начальный кадр обработки (модификатор: shift, сброс на начальный кадр видео)
  * **e** - Установить конечный кадр обработки (модификатор: shift, сброс на конечный кадр видео)
  * **Mouse wheel** - подстройка клавиш
  * **Левая кнопка мыши** - перетаскивание выбранной клавиши / выбор цвета из карты цветов.
  * **CTRL + Левая кнопка мыши** - обновить выбранный цвет к карте цветов.
  * **Правая кнопка мыши** - перетаскивание всех клавиш, если клавиша выбрана, перенос осуществляется относительно неё 
  * **CTRL + 0** - Выключить выбранный цвет.
  * **Стрелки** - подстройка клавиш  (модификатор: shift)
  * **PageUp/PageDown** - прокрутка видео (модификатор: shift, шаг по кадру)
  * **Home/End** - переход в начало или конец видео
  * **ESCAPE** - выход / quit
  * **SPACE** - прервать воссоздание и записать midi файл на диск / abort re-creation and save midi file to disk
  
  EN:
  Control:
  * **h** - show/hide this help
  * **q** - begin to recreate midi
  * **s** - set start frame, (mods : shift, set processing start frame to the beginning)
  * **e** - set end frame, (mods : shift, set processing end frame to the ending)
  * **Mouse wheel** - keys adjustment
  * **Left mouse button** - dragging the selected key / select color from the color map
  * **CTRL + Left mouse button** - update selected color in the color map
  * **CTRL + 0** - disable selected color in the color map
  * **Right mouse button** - dragging all keys, if the key is selected, the transfer is carried out relative to it.
  * **Arrows** - keys adjustment (mods : shift)
  * **PageUp/PageDown** - scrolling video (mods : shift)
  * **Home/End** - go to the beginning or end of the video
  * **Escape** - quit
  * **Space** - abort re-creation and save midi file to disk
  
# how it works / как это работает

##### RU:
Кадр за кадром сканируется видео поток отслеживая изменения виртуальной клавиатуры после всё что зафиксировано дампим на винт.

##### EN:
Frame by frame we scan the virtual keyboard and write the keys to the midi file...

![Alt text](docs/frame47.jpg?raw=true "input from image")

# Troubleshooting / Устранение проблем 

##### RU:
Если клавиши не считываются, в файле v2m.py нужно поправить переменную keyp_colors, в ней записаны цвета активных клавиш (можно изменить либо добавить/удалить ) [ R,G,B ] , ... 

Ещё так же может быть полезным изменить дельту срабатывания ( переменная keyp_delta )

##### EN:
If the keys are not readable, in the v2m.py file you need to modify the variable keyp_colors, it contains the colors of the active keys (you can change or append/remove ) [R, G, B], ...

It may also be useful to change the response delta (keyp_delta variable)

```python
keyp_colors = [
#L.GREEN         D.GREEN
[166,250,103], [ 58,146,  0],
#L.BLUE          D.BLUE
[102,185,207], [  8,113,174],
#L.YELLOW        D.YELLOW
[255,255,85 ], [254,210,  0],
#L.ORANGE        D.ORANGE
[255,212,85 ], [255,138,  0],
#L.RED           D.RED
[253,125,114], [255, 37,  9],
#EMPTY
[0  ,  0,  0], [  0,  0,  0]
# .....
];

keyp_delta = 90; # sensitivity
```

# Дополнительные возможности / Additional features

##### RU:
Вы можете настроить разбиение на каналы в зависимости от цвета клавиши. Для этого в файле v2m.py нужно поправить соответствие цвета каналу midi трека. 
По умолчанию каждый цвет активирующий клавишу будет записан в собственный канал, таким образом если хотите объединить каналы просто укажите для разных цветов одинаковые номера.

##### EN:
You can also customize the separation into channels depending on the color of the key. To do this, in the v2m.py file, you need to modify the color matching to the midi channel of the track.
By default, each color key will be recorded in its own channel, so if you want to combine the channels, simply specify the same numbers for different colors.

```python
keyp_colors_channel = [ 0,0, 1,1, 2,2, 3,3, 4,4, 5,5 ]; # MIDI channel per color
```

![Alt text](docs/multichannel.png?raw=true "main window")

##### RU:
Вы можете настроить соотнесение канала к MIDI инструменту. Для этого в файле v2m.py нужно поправить соответствие канала midi инструменту. 
По умолчанию канал равен 0 midi инструменту

##### EN:
You can customize the channel mapping to a MIDI instrument. To do this, in the v2m.py file, you need to modify the correspondence of the midi channel to the instrument.
The default all channels is 0 midi instrument.

```python
keyp_colors_channel_prog = [ 0,0, 0,0, 0,0, 0,0, 0,0, 0,0 ]; # MIDI program ID per channel

```

##### RU:
Если есть необходимть обработать лижь какой то кусок файла, Вы можете указать начальный и конечные кадры для реконструкции. Только в указанных интервалах будет выполняться обработка.
Для этого в файле v2m.py необходимо изменить переменные "startframe" и "endframe".

##### EN:
If there is a need to process any piece of the file, You can specify the starting and ending frames for the reconstruction. Only at specified intervals will processing be performed.
To do this, in the v2m.py file, you need to modify variables "startframe" and "endframe"

```python
startframe = 0;
endframe = length;
```

##### RU:
Вы так же можете указать минимальную длительность нот, ноты длительность которых будет меньше указанной будут автоматически приравнены минимальной длительности.
Для этого в файле v2m.py необходимо изменить переменные "minimal_duration".

##### EN:
You can also specify the minimum duration of the notes, the notes whose duration will be less than that specified will be automatically equated to the minimum duration.
To do this, in the v2m.py file, you need to modify variables "minimal_duration".

```python
minimal_duration = 0.6;
```

##### RU:
Вы так же можете указать базовую ноту которая добавляется при записи нот в midi файл. По умолчанию basenote = 36 (3 октавы).
Для этого в файле v2m.py необходимо изменить переменные "basenote".

##### EN:
You can also specify the base note that is added when writing notes to the midi file. By default, basenote = 36 (3 octaves).
To do this, in the v2m.py file you need to change the variables "basenote".

```python
basenote = 36;
```

# Экспериментальные возможности / Experimental features:

##### RU:
Добавил возможность распознавать перекрытие клавиш друг другом. В данном случае в момент перекрытия будет создано окончание одной ноты и начало другой. По умолчанию данная функция выключена, в файле v2m.py можно включить её, изменив состояние переменной experimental в 1

##### EN:
Added the ability to recognize the overlap of keys with each other. In this case, at the moment of overlapping, the end of one note and the beginning of another will be created. By default, this function is disabled; in the v2m.py file, you can enable it by changing the state of the experimental variable "experimental" to 1

```python
experimental = 0;
```
