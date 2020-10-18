# Problem Set 5: Template Matching

In this problem set, you'll be implementing dynamic time warping to perform temmplate matching on speech files. Your goal is to determine whether each of the ``mystery`` files is *yes* or *no*. Add, push, and commit (1) a pdf with answers to the questions in part 2; and (2) your Python 3.7 or higher code called `dtw.py` to your repository by 11:59pm on Monday, October 26.

In the ``data`` directory you'll find the following files:

```
yes_template.wav
no_template.wav
yes_template.txt
no_template.txt
yes_validate.wav
no_validate.wav
yes_validate.txt
no_validate.txt
mystery1.txt
mystery2.txt
```

The `.wav` files are recordings of me saying the word indicated in the file name. The `.txt` files are the acoustic features (MFCCs) of those `.wav` files. I did not include `.wav` files for the mystery files, of course, since you will use temnplate matching (rather than your own ears) to decide whether each mystery file is *yes* or *no*.

Each line of a `.txt` file contains the features for one frame of the corresponding `.wav` file. There are 13 features per line, separated by space: the first 12 MFCCs and the overall energy for that frame. These are real MFCCs, which I extracted using [this code](https://github.com/jameslyons/python_speech_features), which is [described in detail here](http://practicalcryptography.com/miscellaneous/machine-learning/guide-mel-frequency-cepstral-coefficients-mfccs/). Some of you used this code in the hackathon, so it might be familiar.

## Part 1: Implementing DTW
**Using Python 3.7 or higher**, implement the dynamic time warping algorithm described in the lecture slides using  Euclidean distance to calculate the difference between the feature vectors in the `.txt` files. (You can calculate this yourself or use [scipy]( https://docs.scipy.org/doc/scipy-0.14.0/reference/generated/scipy.spatial.distance.euclidean.html).)

Use the `validate.txt` files to make sure you're getting the right answers: `yes_validate.txt` should result in a lower final distance value when compared with `yes_template.txt` than with `no_template.txt`; `no_validate.txt` should result in a lower final distance value when compared with `no_template.txt` than with `yes_template.txt`. 

### Specs (Important!)
* Name your program `dtw.py`.
* The program should take one of the `validate` or `mystery` files as a command line argument. It should read in the template files without having to supply them as arguments.
* It should print out (1) the normalized distances for *yes* and *no* (i.e., the distance divided by the number of franes in the template); and (2) which word the file is predicted to be using these distances (i.e., the template that gave the smaller nornalized distance).

Here is an example run:

```
python3 dtw.py mystery5.txt

The normalized distance between mystery5.txt and "no" is 64.253
The normalized distance between mystery5.txt and "yes" is 57.092
mystery5.txt is "no"
```

## Part 2: Testing for speaker independence
Record someone saying "yes" and "no". Convert the two wav files to MFCC feature vectors using the python code linked to above. Just download the code from github, and run example.py with the following modifications: (1) point to your wav files instead of file.wav; and (2) print out each sublist in mfcc_feat rather than the first two sublists of fbank_feat.

### Answer the following questions in a pdf

**Q1: For each recording, which template gave the lowest distance? Were these results correct?***

**Q9: Were the distances larger than the ones generated using the files I provided, which were made using the same speaker? Why do you think this might be?**


## Part 3: Bonus (!)
When developing a speaker-independent template-matching system, it might be helpful to have multiple templates and to combine them into a single template that's a sort of average of all of the templates for a particular word. To do this, you would need to keep track, for each frame in a given template, of the frame in the other template that was the best match. In other words, when you picked the matrix cell with the minimum distance in your min function, which cell did you pick? 

An easy way to implement this is to create a second matrix that you fill in as you fill in your distance matrix. Each cell will simply store the coordinates (the i and j value) of the cell that contained the minimum distance leading to that cell. These pointers back to the preceding cell are called back pointers.  

When your back pointer matrix is full, you can simply follow it, cell by cell, to the (0,0) cell of the matrix. This backtrace is the path that provides the best alignment of the two inputs.

Take your code for DTW from part 3, make a copy of it called `dtw-backpointer.py`, and add in functionality to store the back pointers and to print out the final backtrace. Then, for mystery1.txt, print out the full backtrace you get when you try to match the template for "no".  Submit the code as a .py file in the DropBox and the backtrace for mystery1.txt as a txt file.
 
Note: I am not asking you to combine multiple templates! You just need to implement the backtrace.



