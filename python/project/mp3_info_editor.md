# Get Started
This are using mutagen, time, tkinter module.  
You need to download mutagen module before run the code/
---
-`editor.py`
```python
from mutagen.id3 import ID3, USLT, TPE1, TPE2, TALB

def add_lyrics_to_mp3(file_path, lyrics, artist, performer, album):
    # ID3 태그 로드
    audio = ID3(file_path)
    
    #가사 변경
    audio.add(USLT(encoding=3, lang='kor', desc='', text=lyrics))
    
    # 앨범 아티스트 변경
    audio["TPE2"] = TPE2(encoding=3, text=artist)

    # 참여 아티스트 변경
    audio["TPE1"] = TPE1(encoding=3, text=performer)

    # 앨범 변경
    audio["TALB"] = TALB(encoding=3, text=album)  

    audio.save()

file_path = ''

lyrics = ''' '''

artist = ''

performer = ''

album = ''
```

-`editor_GUI.py`
```python

```
