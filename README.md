# Haarcascade-to-detect-swastika

1.#Clone this repository with : git clone https://github.com/mrnugget/opencv-haar-classifier-training  OR download from
http://sourceforge.net/projects/opencvlibrary/files/?source=navbar

2. Download 20-40 good positive images(use fakuum batch download)

3. Crop images using processing J3 
Download processing script from repo - https://github.com/thiagohersan/memememe/tree/master/Processing/PositiveCollectionTagger. 
In the repo folder go to data/positive_clean and paste positive images 
run PositiveCollectionTagger.pde with J3 processing.Cropped postive images would be generated 

4. Acquire negative images and apply processing script(create your own or download at https://code.google.com/p/tutorial-haartraining/) Paste negative images about 600-800 in negative images folder

5. Go to the root of the haarcascade repository in terminal [ cd ../opencv-haar-classifier-training]
Paste  the command 
find ./positive_images -iname "*.jpg" > positives.txt
next paste 
find ./negative_images -iname "*.jpg" > negatives.txt

6. Generate samples out of the positive and negative images
Go to the root of the haarcascade repository in terminal [ cd ../opencv-haar-classifier-training]
Paste  the command 
perl bin/createsamples.pl positives.txt negatives.txt samples 1500\
  "opencv_createsamples -bgcolor 0 -bgthresh 0 -maxxangle 1.1\
  -maxyangle 1.1 maxzangle 0.5 -maxidev 40 -w 80 -h 40"

This will combine positive with random negative images to form 1500 samples. You should see multiple .vec files populate your samples folder

7. In the root of folder issue the command 
find ./samples -name '*.vec' > samples.txt   
this would generate a description file for your .vec files in your samples folder

8. Merge .vec files into a single sample.vec folder
Go to the tools folder
cd ./tools and issue the command 
python mergevec.py -v /Users/skelix/Desktop/opencv-haar-classifier-training-master/samples/ -o samples.vec

after the samples.vec is generated make sure to copy and paste it is inside the root directory

9. Start Training 
Go to the root of the haarcascade repository in terminal [ cd ../opencv-haar-classifier-training]
Paste  the command 
opencv_traincascade -data classifier -vec samples.vec -bg negatives.txt\
  -numStages 20 -minHitRate 0.999 -maxFalseAlarmRate 0.5 -numPos 1000\
  -numNeg 600 -w 80 -h 40 -mode ALL -precalcValBufSize 1024\
  -precalcIdxBufSize 1024
