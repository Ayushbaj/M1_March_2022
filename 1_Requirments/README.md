#Mini Project
*Individual
            
                        FACE MASK DETECTION
                        
                        
      This python program will detect of person with face mask and
      without face mask in an image which id feeded as input which is passed through
      command line argument.For detection of faces with and without mask in a video 
      stream or webcam program which is given below is used.Here the video stream
      is captured by mean of using open Cv command and processed frame by frame.



import numpy as np
import argparse
import time 
import blynklib
import cv2
import os

import RPI.GPIO as GPIO # Import Raspberry Pi GPIO library
from time import sleep # Import the sleep function from the time modul
GPIO.setwarnings(False) # Ignore warning for now
GPIO.setmode(GPIO.BCM) # Use physical pin numbering 
GPIO.setup(23, GPIO.OUT, initial GPIO.LOW)
GPIO.setup(24, GPIO.OUT, initial-GPIO.LOW)
GPIO.setup(17, GPIO.OUT, initial-GPIO.LOW)
GPIO.setup(25, GPIO.IN, pull_up_down GPIO.PUD UP)
flag=0
BLYNK AUTH='m-elBHw6vDu47oED-QS7ozyXV2tetdSu
TARGET EMAIL= 'ab7289@srmist.edu.in'
blynk- blynklib.Blynk(BLYNK AUTH)
EMAIL PRINT_MSG="Email and Mobile Notification was sent"

ap-argparse.ArgumentParser()
ap.add_argument("-i", "--image", required-True. help-"path to input image")
ap.add_argument("-o", "--output".help="path to output image") 
ap.add argument("-y", "--yolo", required-True, help-"base path to YOLO directory") 
ap.add_argument("-c", "--confidence", type-float. default-0.45.help="minimum
probability to filter weak detections")
ap.add_argument("-t", "--threshold", type-float, default-0.3.help="threshold when applying non-max suppression") 
args=vars(ap.parse_args())

labelsPath = os.path.sep.join([args["yolo"], "obj.names"])
LABELS = open(labelsPath).read().strip().split("\n")

COLORS = [[0,0,255],[0,255,0]]

weightsPath = os.path.sep.join([args["yolo"], "yolov3_face_mask.weights"]) 
configPath = os.path.sep.join([args["yolo"], "yolov3.cfg"])

print("[INFO] loading YOLO from disk...") 
net = cv2.dnn.readNetFromDarknet(configPath, weightsPath) 
image- cv2.imread(args["image"])
(H. W)-image.shape[:2]
In-net.getlayerNames()
In = [Infi[0]-1] for i in net.getUnconnectedOutLayers(]
blob-cv2.dnn.blobFromImage(image. 1/255.0, (832, 832), swapRB True, crop-False)
net.setInput(blob) 
start=time.time()
layerOutputs = net.forward(In) #list of 3 arrays, for each output layer. 
end = time.time()
print("[INFO] YOLO took (:.61) seconds".format(end-start))

boxes = [] 
confidences =[]
classIDs = []

for output in layerOutputs:
     for detection in output:
             scores = detection[5:] #last 2 values in vector 
             classID= np.argmax(scores)
             confidence scores[classID]
             if confidence> args["confidence"]:
             box = detection[0:4]*np.array([W, H, W. H])
             (centerX,centerY,width,height)=box.astype("int")
             x= int(centerX - (width / 2))
             y= int(centerY - (height/2)) 
             boxes.append([x, y, int(width), int(height)])
             confidences.append(float(confidence))
             classIDs.append(classID)

idxs= cv2.dnn.NMSBoxes(boxes, confidences, args["confidence"],args["threshold"])
border size=100
border_text_color=[255,255,255]
image = cv2.copyMakeBorder(image, border_size,0,0,0, cv2.BORDER_CONSTANT)
filtered_classids=np.take(classIDs,idxs)
mask_count=(filtered_classids=1).sum() 
nomask_count (filtered_classids==0).sum()
text="NoMaskCount: MaskCount: {}".format(nomask_count, mask_count)
cv2.putText(image,text, (0, int(border_size-50)), 
cv2.FONT HERSHEY SIMPLEX,0.8,border_text_color, 2)

text="Status:"
cv2.putText(image,text, (W-300, int(border_size-50)),
cv2.FONT_HERSHEY_SIMPLEX.0.8.border_text_color,2)
ratio nomask count/(mask_count+nomask_count)

if ratio>=0.1 and
   nomask_count>-3:text=
   "Danger!"
   cv2.putText(image,text. (W-200, int(border_size-50)).
cv2.FONT HERSHEY SIMPLEX,0.8.[26,13,247], 2)
   mss"!!!!!!!!withmask-() withoutmask
   ={}".format(text.mask_count,nomask_count)flag-2
   @blynk.handle_event("connect")def
   connect_handler():
   print('Sleeping 2 sec before sending
   email...")time.sleep(2) 
   blynk.email(TARGET_EMAIL, FACE MASK DETECTION UNIT, mss)
   blynk.notify(mss)
   print(EMAIL_PRINT
   MSG)blynk.run()
 elif ratio!-0 and
   np.isnan(ratio)!=True:text=
   "Warning !" 
   cv2.putText(image,text, (W-200, int(border_size-50)), 
 cv2.FONT_HERSHEY_SIMPLEX.0.8,[0,255,255], 2)
   mss="{!!!!!!!!! With Mask - Without Mask
   ={}".format(text,mask_count,nomask
   _count)flag=1
   @blynk.handle_event("connect") def
   connect_handler():
   print(Sleeping 2 sec before sending
   email...')time.sleep(2)
   blynk.email(TARGET_EMAIL,'FACE MASK DETECTION UNIT'. mss)
   blynk.notify(mss)
   print(EMAIL PRINT)
   _MSG)blynk.run()
else:
   text="Safe"
   cv2.putText(image,text,(W-200,int(border_size-50)),
cv2.FONT_HERSHEY_SIMPLEX,0.8,[0,255,0],2)
  flag=0

if len(idxs)>0:

        for i in idxs.flatten():
        (x, y) = (boxes[i][0], boxes[i][1]+border_size)
        (w, h) = (boxes[i][2], boxes[i][3])
        color = [int(c) for c in COLORS [classIDs[i]]]
        cv2.rectangle(image, (x, y), (x+w, y + h), color, 1)
        text="{}: {:.4f}".format(LABELS[classIDs[i]], confidences[i])
        cv2.putText(image, text, (x, y-5),
        cv2.FONT HERSHEY SIMPLEX,0.5,
color, 1)
if args["output"]:





      Above python program is for detection of person with face mask and
      without face mask in an image which id feeded as input which is passed through
      command line argument.For detection of faces with and without mask in a video 
      stream or webcam program which is given below is used.Here the video stream
      is captured by mean of using open Cv command and processed frame by frame.
