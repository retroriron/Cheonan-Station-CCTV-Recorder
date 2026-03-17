# Cheonan Station CCTV Recorder
##### OpenCV를 이용한 CCTV 영상 스트리밍 및 녹화 프로그램
---
## 1. 프로그램 설명
   이 프로그램은 OpenCV를 이용하여 천안역에 설치된 CCTV의 RTSP URL을 통해 실시간으로 영상을 받아 화면에 출력하는 프로그램이다.
   
   실행 중 키보드 입력을 통해 영상 녹화를 시작하거나 중지할 수 있다.

## 2. 주요 기능
   - 천안역 CCTV 영상 출력 (실시간)
   - 영상 녹화 기능
   - 녹화 중일 때 빨간 점과 "REC" 표시

## 3. 조작 방법
   - Space 키 : 녹화 시작 / 중지
   - ESC 키 : 프로그램 종료

## 4. 녹화 파일
   녹화된 영상은 output.avi 파일로 저장된다.

## 5. 코드 (VSCode, python)
```
import cv2
from datetime import datetime

# 천안역 CCTV URL
url = "rtsp://210.99.70.120:1935/live/cctv007.stream"
cap = cv2.VideoCapture(url)

# 코덱 설정
fourcc = cv2.VideoWriter_fourcc(*'XVID')
out = cv2.VideoWriter("output.avi", fourcc, 20.0, (640,480))

record = False

while True:
    ret, frame = cap.read()
    
    if not ret:
        break
    
    # 현재 시각 표시하기
    now = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    cv2.putText(frame, now, (10, 470), cv2.FONT_HERSHEY_COMPLEX, 0.5, (255,255,255), 1)
    
    if record:
        out.write(frame)
        cv2.circle(frame, (30,30), 10, (0,0,255), -1)
        cv2.putText(frame, "REC", (50,37), cv2.FONT_HERSHEY_COMPLEX, 0.8, (0,0,255), 2)
        
    cv2.imshow("Chenan Station CCTV", frame)
    
    key = cv2.waitKey(1)
    
    # ESC 키로 종료
    if key == 27:
        break
    
    # Space 키로 녹화 시작/중지
    elif key == 32:
        record = not record
    
cap.release()
out.release()
cv2.destroyAllWindows()

```
## 6. 작동 스크린샷
   <img width="642" height="512" alt="image" src="https://github.com/user-attachments/assets/c6afa3f2-69b1-4204-b2dc-fc595d4ef2d9" />
   <img width="642" height="512" alt="image" src="https://github.com/user-attachments/assets/29ba9ede-d007-423d-b821-4c50368c4ee5" />

## 7. 기타
   스트리밍 프로토콜 주소 출처 : [공공데이터 포털](https://www.data.go.kr/data/15063717/fileData.do)
