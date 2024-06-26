# การตรวจจับและติดตามบุคคลในวิดีโอโดยการใช้ Deep Learning 
วัตถุประสงค์ของโปรเจกต์

วัตถุประสงค์ของโปรเจกต์นี้คือการสร้างตัวตรวจจับคนเข้า - ออก ตรวจจับเพศและอายุที่ร้านสะดวกซื้อแห่งหนึ่งซึ่งโมเดลสามารถทำนายเพศและอายุของบุคคลจากภาพใบหน้าในรูปภาพเดียวหรือวิดีโอโดยใช้เทคโนโลยี Deep Learning ฝึกบนชุดข้อมูลของ Adience ซึ่งในโปรเจกต์นี้จะใช้โมเดลที่ฝึกสอนโดย Tal Hassner และ Gil Levi การทำนายเพศอาจเป็น 'ชาย' หรือ 'หญิง' และการทำนายอายุอาจเป็นหนึ่งในช่วงอายุต่อไปนี้ - (0 – 2), (4 – 6), (8 – 12), (15 – 20), (25 – 32), (38 – 43), (48 – 53), (60 – 100) (มี 8 โหนดในชั้น softmax สุดท้าย) การทำนายอายุแม่นยำในภาพเดียวมีความยากเนื่องจากปัจจัยเช่น การแต่งหน้า, แสงสว่าง, สิ่งกีดขวาง, และท่าทางใบหน้า เราจึงเลือกที่จะทำให้เป็นปัญหาการจัดประเภทแทนการทำให้เป็นปัญหาการคาดการณ์.

# ชุดข้อมูล
The Dataset ที่ใช้ฝึกโมเดล Age and Gender Classification using Deep Convolutional Neural Networks
ใช้ชุดข้อมูล Adience ซึ่งเป็นชุดข้อมูลที่มีอยู่ในโดเมนสาธารณะซึ่งประกอบไปด้วยภาพใบหน้าของบุคคลในเกณฑ์อายุแต่ละกลุ่มและเงื่อนไขการถ่ายภาพต่างๆ เช่น แสงสว่าง, ท่าทาง, และลักษณะเห็นได้ชัดแจ้ง สามารถดาวน์โหลดได้ที่นี่ https://www.kaggle.com/datasets/ttungl/adience-benchmark-gender-and-age-classification  ลิงก์ชุดข้อมูล Adience ชุดข้อมูลนี้เป็นเกณฑ์มาตรฐานสำหรับภาพถ่ายใบหน้าที่รวบรวมมาจากอุปกรณ์ที่หลากหลายซึ่งมาจากการถ่ายภาพในชีวิตจริง เช่น การมีแสงรบกวน เงา ท่าทาง และลักษณะทั่วไปของภาพ ภาพถ่ายได้รับการเก็บรวบรวมมาจากอัลบั้ม Flickr และเผยแพร่ภายใต้ใบอนุญาต Creative Commons (CC) โดยชุดข้อมูลนี้ประกอบด้วยภาพถ่ายทั้งหมด 26,580 รูปของบุคคลทั้งหมด 2,284 คนในแต่ละช่วงอายุที่กล่าวถึง (ตามที่กล่าวไว้ข้างต้น) และมีขนาดประมาณ 1GB

The Dataset หรือวิดีโอที่ใช้ทดสอบจริง
	วิดีโอที่ใช้ทดสอบจริงคือวิดีโอจากกล้อง CCTV ความละเอียดสูง 3MP และ 4MP จาก iProx CCTV HD Dome ที่ขายบนเว็บไซต์ hdcctvcameras.net ซึ่งได้ถูกติดตั้งไว้ในร้านค้าปลีกบนถนนใหญ่ที่ขายผลิตภัณฑ์ดูแลบ้าน การบันทึกวิดีโอนี้ทำผ่าน Hikvision 8Channel POE NVR หากต้องการดูวิดีโอและภาพถ่าย CCTV ความละเอียดสูงเพิ่มเติม สามารถเข้าชมได้ที่ hdcctvcameras.net

# การติดตั้งและการใช้งาน
ขั้นตอนการติดตั้งคุณจะต้องติดตั้ง OpenCV (cv2) และ argparse ซึ่งเป็นไลบรารีมาตรฐานของ Python ที่ใช้สำหรับการจัดการและพาร์สพารามิเตอร์
เพื่อให้สามารถรันโปรเจ็กต์นี้ได้ คุณสามารถทำได้ด้วย pip-

pip install opencv-python

pip install argparse

ดาวน์โหลดและแตกไฟล์ zip: ดาวน์โหลด zip ไฟล์ที่ประกอบด้วยไฟล์โค้ด-โมเดลและภาพวิดีโอตัวอย่าง แล้วแตกไฟล์ออกไปไว้ในโฟลเดอร์ที่ต้องการ ภายในโฟลเดอร์นี้จะมีไฟล์ดังนี้:

-opencv_face_detector.pbtxt

-opencv_face_detector_uint8.pb

-age_deploy.prototxt

-age_net.caffemodel

-gender_deploy.prototxt

-gender_net.caffemodel

-รูปภาพตัวอย่างบางส่วน

-HD CCTV.mp4

-tracker.py

-main.py

-detect.py

# ติดตั้งไลบรารีที่จำเป็น 
OpenCV: ใช้สำหรับการประมวลผลภาพและวิดีโอ

Math: ใช้สำหรับการคำนวณทางคณิตศาสตร์

Argparse: ใช้สำหรับการจัดการพารามิเตอร์ที่รับจากผู้ใช้

Tkinter: ใช้สำหรับสร้าง GUI

Pillow (PIL): ใช้สำหรับการจัดการและประมวลผลภาพ

Pandas: ใช้สำหรับการจัดการข้อมูลในรูปแบบตาราง

Numpy: ใช้สำหรับการคำนวณทางคณิตศาสตร์และการจัดการอาร์เรย์

Ultralytics YOLO: ใช้สำหรับการตรวจจับวัตถุด้วย YOLO model

Tracker: ใช้สำหรับการติดตามวัตถุ

JSON: ใช้สำหรับการอ่านและเขียนไฟล์ JSON

OS: ใช้สำหรับการจัดการระบบไฟล์และการทำงานที่เกี่ยวข้องกับระบบปฏิบัติการ

Datetime: ใช้สำหรับการจัดการวันที่และเวลา

# สร้างไฟล์ python สำหรับการรันโปรแกรมหรือเข้าไปที่ main.py เพื่อใช้งานและทดสอบวิดีโอ
เรียกใช้โปรแกรมจาก command prompt ด้วย python your_script.py --video path_to_your_video_file โดยที่ your_script.py คือชื่อไฟล์ python ที่คุณสร้างขึ้น และ path_to_your_video_file คือเส้นทางไปยังไฟล์วิดีโอที่คุณต้องการใช้ตรวจจับ
หรือ python main.py --video "HD CCTV.mp4" คือไฟล์ที่เราใช้งาน

# รายละเอียดการทำงานของโปรแกรม
โปรแกรมจะอ่านเฟรมจากไฟล์วิดีโอและใช้โมเดล DNN (Deep Neural Network) ในการตรวจจับใบหน้าในแต่ละเฟรม เพื่อตรวจจับเพศและอายุของบุคคลในเฟรมโดยใช้โมเดลที่ถูกฝึกสอน ใช้โมเดล YOLO ในการตรวจจับและติดตามบุคคลที่เข้าและออกจากพื้นที่ที่กำหนดโดยแสดงกรอบสีรอบ ๆ บุคคลที่ตรวจพบพร้อมกับข้อมูลเพศและอายุ เพื่อแสดงจำนวนบุคคลที่เข้า-ออกจากร้าน และจำนวนบุคคลที่อยู่ในร้านในแต่ละช่วงเวลาแสดงผลการตรวจจับบนหน้าจอและบันทึกผลลัพธ์ลงในไฟล์ JSON

# ข้อควรระวังและข้อจำกัด
การตรวจจับเพศและอายุอาจมีความคลาดเคลื่อนจากปัจจัยเช่น แสงสว่าง, สิ่งกีดขวาง, และท่าทางใบหน้า, ระยะห่างที่ไกลเกิน
และในส่วนการทำนายอายุมีความยากเนื่องจากปัจจัยหลายประการ ดังนั้นโปรแกรมนี้จึงเลือกที่จะทำให้เป็นปัญหาการจัดประเภทแทนการคาดการณ์

# สรุป
โปรเจกต์นี้มีการใช้เทคโนโลยี Deep Learning ในการตรวจจับและติดตามบุคคล รวมถึงการตรวจจับเพศและอายุจากวิดีโอ ทำให้สามารถนำไปประยุกต์ใช้ในสถานการณ์จริงได้ เช่น การตรวจสอบการเข้า-ออกของบุคคลในร้านค้าหรือสถานที่ต่าง ๆ
