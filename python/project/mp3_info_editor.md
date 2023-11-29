# Get Started  
---
This are using mutagen, time, tkinter module.  
You need to download mutagen module before run the code.
---
-`editor.py`
```python
from mutagen.id3 import ID3, USLT, TPE1, TPE2, TALB

def add_lyrics_to_mp3(file_path, lyrics, artist, performer, album):
    # ID3 태그 로드 (load ID3 tag)
    audio = ID3(file_path)
    
    #가사 변경 (edit lyrics)
    audio.add(USLT(encoding=3, lang='kor', desc='', text=lyrics))
    
    # 앨범 아티스트 변경 (edit artist)
    audio["TPE2"] = TPE2(encoding=3, text=artist)

    # 참여 아티스트 변경 (edit performer)
    audio["TPE1"] = TPE1(encoding=3, text=performer)

    # 앨범 변경 (edit album)
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
from tkinter import Tk, Text, Entry, Button, LabelFrame, DoubleVar, END
from tkinter.filedialog import askopenfilename
from tkinter.ttk import Progressbar
from tkinter.messagebox import showerror, showinfo
from mutagen.id3 import ID3, USLT, TPE1, TPE2, TALB
import time

root = Tk()
root.title("lyrics editor")
root.geometry("640x480")
root.resizable(0,0)
file_path = None

def info_editor(file_path, lyrics, artist, performer, album):
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


def open_file():
    global file_path
    file_path = askopenfilename(filetypes=[("MP3 Files", "*.mp3")]) # 파일 선택 대화 상자 열기
    if file_path:  # 파일이 선택된 경우에만 실행
        file_name = file_path.split("/")[-1]
        e.delete(0,END)
        e.insert(0, file_name)

def edit():
    if file_path is None:
        showerror("ERROR", "Choose the music file")

    else:
        info_editor(file_path, lyrics.get("1.0", END), artist.get(), performer.get(), album.get())
        for i in range(1,101):
            time.sleep(0.0001)
            p_var.set(i)
            progressbar.update()
        showinfo("info","FINISH")

def dele():
    lyrics.delete("1.0", END)
    performer.delete("1", END)
    artist.delete("1", END)

# 1. 파일 열기 프레임 설정
file_frame = LabelFrame(root, text="open file")
file_frame.pack()

e = Entry(file_frame, width=82)
e.pack(side="left", expand="true", ipady=4)

button = Button(file_frame, text="Open File", command=open_file)
button.pack(side="right")

# 2.설정 프레임 설정
setting_frame = LabelFrame(root, text="edit")
setting_frame.pack()

#2-1.가사 프레임 설정
lyrics_frame = LabelFrame(setting_frame, text="lyrics")
lyrics_frame.pack()

lyrics = Text(lyrics_frame, width=89, height=20)
lyrics.pack()

#2-2. 아티스트 프레임 설정
artist_frame = LabelFrame(setting_frame, text="artist")
artist_frame.pack(side="left")

artist = Entry(artist_frame, width=29)
artist.pack()

#2-3. 참여 아티스트 프레임 설정
performer_frame = LabelFrame(setting_frame, text="performer")
performer_frame.pack(side="left")


performer = Entry(performer_frame, width=29)
performer.pack() 

#2-4. 참여 아티스트 프레임 설정
album_frame = LabelFrame(setting_frame, text="album")
album_frame.pack(side="left")

album = Entry(album_frame, width=29)
album.pack()

#3.진행상황
progress_frame = LabelFrame(root, text="progress")
progress_frame.pack()

p_var = DoubleVar()
progressbar = Progressbar(progress_frame, maximum=100, length="640", variable=p_var)
progressbar.pack()

#4.버튼
Button(root, text="Delete the lyrics", command=dele).pack(side="right")

Button(root, text="Insert the lyrics", command=edit).pack(side="right")

root.mainloop()
```
