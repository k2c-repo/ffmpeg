## Pipe Example
python3 frame_server.py | ffmpeg -fflags nobuffer -flags low_delay -f rawvideo -pix_fmt bgr24 -s 640x480 -r 30 -i - -an -f mpegts udp://10.40.111.48:8182
python3 frame_server.py | ffmpeg -f rawvideo -pixel_format rgb24 -video_size 640x480 -framerate 30 -i - http://localhost:8090/feed1.ffm
python frame_server.py | /usr/bin/ffmpeg -y -re -r 30 -f rawvideo -pixel_format bgr24 -s 640x480 -i pipe: -r 30 http://127.0.0.1:8090/feed1.ffm


ffmpeg -i sapce.mp4 -v 0 -vcodec mpeg4 -f mpegts -s 640x480 udp://127.0.0.1:23000
ffmpeg -r 30 -f rawvideo -pixel_format bgr24 -s 640x480 -i - -f mpegts -c:v mpeg1video -s 640x480 -b:v 2500k -bf 0 http://localhost:8081/supersecret
ffmpeg -re -r 30 -s 640x480 -f video4linux2 -i /dev/video0 -b:v 2500k -f flv rtmp://127.0.0.1:1935/stream/test
ffmpeg -re -f video4linux2 -i /dev/video0 -vcodec libx264 -profile:v main -preset:v medium -r 30 -g 60 -b:v 2500k -maxrate 2500k -f flv rtmp://127.0.0.1:1935/stream/test

<br>

## CAM to RTSP
ffmpeg -r 30 -s 640x480 -f video4linux2 -i /dev/video0 -preset ultrafast -f rtsp -rtsp_transport udp rtsp://localhost:8554/play
ffmpeg -r 30 -s 640x480 -f video4linux2 -i /dev/video0 -c:v libx264 -preset ultrafast -b:v 500k -max_muxing_queue_size 1024 -g 30 -f rtsp -rtsp_transport udp rtsp://localhost:8554/play/compressed

<br>

## CAM to HLS
ffmpeg -r 30 -s 640x480 -f video4linux2 -i /dev/video0 -c:v libx264 -preset ultrafast -g 30 -f rtsp -rtsp_transport udp rtsp://localhost:8554/play

<br>

## Resize
ffmpeg -i 11.mkv -vf scale=1920:1080 -c:V libx264 -c:a aac 11.mp4
ffmpeg -i 11.mkv -vf scale=1920:1080 -codec copy 11.mp4

<br>

## Encode with subtitle
ffmpeg -i "input.mp4" -vf "subtitles=11.srt:force_style='FontName=맑은고딕,Fontsize=18'" -c:V libx264 -c:a aac "output.mp4"
ffmpeg -ss 00:00:36 -i "input.mp4" -to 00:01:00 -c:V libx264 -c:a aac "output.mp4"
