### 1️⃣ Originali raiška, originalus FPS, 128 kbps stereo

```cmd
ffmpeg -i "Black Widow.2021.Action Adventure Sci-Fi.mp4" -vf "pad=iw:ih:(iw-iw)/2:(ih-ih)/2:black" -c:v h264_nvenc -preset p4 -pix_fmt yuv420p -c:a aac -b:a 128k -ac 2 -ar 48000 -movflags +faststart "Black_Widow_ORIG_NV.mp4"
```

---

### 2️⃣ 1920×1080, originalus FPS, 128 kbps stereo

```cmd
ffmpeg -i "Black Widow.2021.Action Adventure Sci-Fi.mp4" -vf "scale=1920:1080:flags=lanczos,pad=1920:1080:(1920-iw)/2:(1080-ih)/2:black" -c:v h264_nvenc -preset p4 -pix_fmt yuv420p -c:a aac -b:a 128k -ac 2 -ar 48000 -movflags +faststart "Black_Widow_1080p_NV.mp4"
```

---

### 3️⃣ 1280×720, originalus FPS, 128 kbps stereo

```cmd
ffmpeg -i "Black Widow.2021.Action Adventure Sci-Fi.mp4" -vf "scale=1280:720:flags=lanczos,pad=1280:720:(1280-iw)/2:(720-ih)/2:black" -c:v h264_nvenc -preset p4 -pix_fmt yuv420p -c:a aac -b:a 128k -ac 2 -ar 48000 -movflags +faststart "Black_Widow_720p_NV.mp4"
```

---

💡 Šios komandos:

* Naudoja **NVENC** GPU H.264 kodavimui
* Audio: 128 kbps, 48 kHz, stereo
* Vaizdas automatiškai centruojamas, juostos paruoštos, kad tilptų į 16:9 arba 1920×1080 / 1280×720 raiškas.

Jeigu nori, galiu parašyti **.bat failą**, kuris vienu paleidimu sukurs visus tris variantus. Tai labai patogu daugeliui filmų.
