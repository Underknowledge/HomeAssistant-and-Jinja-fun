Keeps the size under 1.5mb for 5 secconds and I can send it via telegram - this is for the Android app IP Webcam

shell script:
```bash
#!/bin/bash
date
folder=/config/ffmpeg
id=$(date +"%y-%m-%d_%H-%M-%S")
rtsp_url=http://user:password@10.0.0.100:8080/video?profile=0
duration=5
bitrate=2048k
codec=libx264
filesize=2048k

echo "$id.mp4" >/config/ffmpeg/file.txt
ffmpeg -y -i $rtsp_url -t $duration -b:v $bitrate -vcodec $codec -fs $filesize $folder/$id.mp4
echo "1" >/config/ffmpeg/done.txt
find $folder -type f -name '*.mp4' -mtime +30 -exec rm {} \;
sleep 20s
echo "0" >/config/ffmpeg/done.txt
``` 

cam Package for the filename and wait template


```yaml
shell_command:
  ipcam_sh: bash -x /config/shell_scripts/cam.sh >> /share/ipcam2.log 2>&1
sensor:
  - platform: file
    name: cam_file
    file_path: /config/ffmpeg/file.txt
  - platform: file
    name: cam_done
    file_path: /config/ffmpeg/done.txt
```

Telegram action:
```yaml
    action:
    - service: shell_command.ipcam_sh
    - wait_template: "{{ is_state('sensor.cam_done', '1') }}"
      timeout: '00:00:40'
      continue_on_timeout: 'true'
    - service: telegram_bot.send_video
      data_template:
        file: /config/ffmpeg/{{ states('sensor.cam_file')}}
    - service: telegram_bot.send_message
      data_template:
        target: 'xxxxxxxx'
        message: " Stand: {{ states('sensor.cam_file')}} Es ist {{now().strftime('%H:%M:%S %Y-%m-%d')}} /vid um es noch mal zu senden."
``` 
you have to whitelist the folders you use - Otherwise you get an error
