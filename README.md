# automatic_indian_number_plate-recognition
the aim of the project is to detect number plate in an image using yolov5s and crop the detected part and extract the text in the cropped image using ocr technique.

step1:

firstly, clone the yolov5 offical repo

git clone 'https://github.com/ultralytics/yolov5.git'

step2:
change the directory to yolov5
%cd  /content/yolov5/


step3:
install the required libraries using requirements.txt file 
pip install -qr 'requirements.txt' 

step4:
In the yolov5 folder yolov5
                        |
                        data
                        |
                        images
                        |--test(upload test images)
                        |--train(upload train images)
step5:                        
Create the vehicle_plate.yaml file inside the data folder(give path of train and test images in the file and also define number of classes along with their names in the list.)
        
step6: 
Pick the model of your choice yolov5s,yolov5l,yolov5m and start training by executing following command(i used yolov5s model) 
python train.py --batch 16 --epochs 50 --data vehicle_plate.yaml  --cfg models/yolov5s.yaml --name lplatedetection

__________________________________________________________________________________________________________________________________________________
To crop the detected part and extract the text from detected part of the image few changes were made in the detect.py file.
after the line 114 paste the following code.
for k in range(len(det)):
  x,y,w,h=int(xyxy[0]), int(xyxy[1]), int(xyxy[2] - xyxy[0]), int(xyxy[3] - xyxy[1])                   
  img_ = im0.astype(np.uint8)
  crop_img=img_[y:y+ h, x:x + w]                          
                                
                            #!!rescale image !!!
  filename=label+ '{:}.jpg'.format(+1)
  filepath="/content/yolov5"+filename
  cv2.imwrite(filepath, crop_img)
  reader = easyocr.Reader(['en']) # need to run only once to load model into memory
  result = reader.readtext(filepath,detail=0)
  print(result) 


step7:
Now, that the model is trained to test execute the following cmd
python detect.py --weight weights/best.pt --conf 0.7 --source test.jpg --save-txt



