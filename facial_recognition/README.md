# Facial Recog
Make sure to have python, dlib, and OpenFace installed!

```
cd ~/path/to/OpenFace/
mkdir training-images
```

```
mkdir ./training-images/NAME
```
Copy all your images of each person into the correct sub-folders. Make sure only one face appears in each image. There's no need to crop the image around the face. OpenFace will do that automatically.

Run the openface scripts from inside the openface root directory:

First, do pose detection and alignment:

```
./util/align-dlib.py ./training-images/ align outerEyesAndNose ./aligned-images/ --size 96
```

This will create a new ```./aligned-images/``` subfolder with a cropped and aligned version of each of your test images.

Second, generate the representations from the aligned images:

```
./batch-represent/main.lua -outDir ./generated-embeddings/ -data ./aligned-images/
```

After you run this, the ```./generated-embeddings/``` sub-folder will contain a csv file with the embeddings for each image.

Third, train your face detection model:
```
./demos/classifier.py train ./generated-embeddings/
```
This will generate a new file called ```./generated-embeddings/classifier.pkl```. This file has the SVM model you'll use to recognize new faces.

At this point, you should have a working face recognizer!

Get a new picture with an unknown face. Pass it to the classifier script like this:

```
./demos/classifier.py infer ./generated-embeddings/classifier.pkl your_test_image.jpg
```
You should get a prediction that looks like this:

```
=== /test-images/your_test_image.jpg ===
Predict Jay-Z with 0.73 confidence.
```
