# TabGen-An-Audio-to-Guitar-Tablature-Model

Researcher: Maes, Brady                                                                                                                                      
Mentor: Uddin, Md Yusuf Sarwar                                                                                                                                                  
Department: School of Science and Engineering, Computer Science                                                                                        
Funding: The Department of the Navy, Office of Naval Research under ONR award number N00014-21-1-2710.

This project aims to train a model to take in short audio files of a guitar being played and transform them into tablature descriptions as text files. 

Tablature is a simplified musical notation that aids a user in playing a particular sound or song. Guitar tabs are ideal for amateur guitar enthusiasts who cannot read and understand traditional sheet music. This model was trained on the GuitarSet dataset (Xi et al.) containing 360 samples of guitar being played as well as annotations. The audio files are in the form of WAV files and the annotations are in the form of JAMS files. The WAV files are preprocessed into Mel-spectrograms to standardize the inputs into the model and account for human perception of sounds. The JAMS files are preprocessed into text files mimicking what tabs look like to provide our desired outputs. This project is a work in progress and different model architectures are being experimented with. A hybrid model with convolutional layers at the beginning and Long Short-Term Memory (LSTM) could combine the strengths of Convolutional Neural Networks (CNN) and LSTM neural networks. Most notably, CNNs are excellent for processing images such as mel-spectrograms while LSTMs are useful for their ability to understand long-term sequential relationships (importance of the order of events in both audio files and tab representations).​



<h3>Vocabulary</h3>

Some vocabulary terms are important for understanding this project:​

**Guitar tablature**- A form of musical notation that indicates the positions on the guitar's fretboard where the player should place their fingers to play a song. The horizontal represents time, the vertical represents guitar strings, and the numbers and symbols placed on the x-axis indicate the fret (or special case) to be played. The frets on a guitar are the vertical strips that the user applies pressure to, shortening the length of the string and giving it a higher frequency. ​

**Spectrogram**- A visual representation of the spectrum of frequencies in a signal as it varies with time.  The horizontal represents time, the vertical represents frequencies, and the colors indicate the intensity of the frequencies. The  bins are the intervals or segments into which the frequency and time dimensions are divided into

**Mel-Spectrograms** Spectrograms using the Mel scale to imitate how humans perceive sound frequencies

​
<h3>Preprocessing</h3>

The audio, annotation file pairs must be preprocessed to be useful and meaningful to our model. Machine learning models expect input and output data in the form of integer (sometimes float) arrays. This step is crucial because how we preprocess our files will determine how much information is lost versus how much is kept & fed into our model. Additionally, the format and representation of our data created by our preprocessing will influence our model's learning as well as how resource-intensive the training will be. For the sake of simplicity, most of the examples given in this presentation will use the first file named "05_SS3-98-C_solo." ​

The audio files from this dataset come in WAV format, which is ideal because of its lack of compression and therefore lack of data loss. However, the WAV files must be preprocessed into a more model-friendly format. Spectrograms are a popular visual representation of the spectrum of frequencies in a signal as it varies with time. They can be thought of as visual, graphical representations of sound where the x-axis represents time (from left to right) and the y-axis represents the frequency of a sound or sounds. Finally, there is a third parameter- the intensity or magnitude of each frequency in decibels. This intensity is represented by the color of the lines, similar to a heat map. Mel-spectrograms are an extension of spectrograms that have been filtered to more accurately represent human interpretation of sound. Spectrograms are highly compatible with models that excel at image processing & choosing Mel-spectrograms accounts for human perception. An example Mel-spectrogram from the preprocessing can be seen below in Figures 1 and 2. Note that the spectrogram contains empty yellow space on the right side because all spectrograms were padded to a length of 4000 (the largest audio file rounded up)  to standardize the input. The spectrograms are divided into 512 bins each. ​

​<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/audioFileExample.PNG" alt="Audio File Location" />
</p>

<h3 align="center">Converts To</h3>

<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/spectrogramExampleFixed.PNG" alt="Mel Spectrogram" />
</p>


The annotation files from this dataset come in JSON Annotated Music Specification (JAMS) format. These JAMS files contain information about their corresponding audio file and how it is played, such as Musical Instrument Digital Interface (MIDI) data which can be used to determine frequencies and notes at specific times.  From here, we can calculate what fret of what string was being played at what time based on MIDI note numbers.  A dictionary is then used to store guitar string letters (represented as numbers) as keys and their value contents as an array containing fret numbers. From here,  text files are made showing the fret numbers on their corresponding guitar strings with the strings indicated by their letter names (e, A, D,  G, B, E) followed by a "|" bar symbol for organizational reasons.  Next, dashes "-" are inserted into the array to show the passing of time (10 dashes/second) and inserted between touching fret numbers to act as a separator. It is also worth noting that a check is added to treat double-digit fret numbers as double digits by checking the length of the current line and if this character plus the next one forms a digit. If both criteria are true, then they're added together as one fret number and the next iteration of the loop is skipped.  Finally, padding of dashes is added to the end to make all files the same length (the length of the longest file). An example of some contents from an annotation file and its corresponding tab can be seen below (both partial). ​

Note: The data on the left is not an exact translation of the data on the right as their different file formats do not place their relevant information near one another in a way that could fit in a graphic like this. ​

​<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/JAMSpitchExample.PNG" alt="JAMS Annotation File" />
</p>

<h3 align="center">Converts To</h3>

<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/tabFormattedExample.PNG" alt="Tab Representation" />
</p>

The representation shown in Figure 4 is the closest to actual guitar tabs and is our desired final product when making predictions. However, this format is not compatible with models and needs to be encoded so that it is readable for our model.  The text files containing our tabs are one-hot encoded based on a mapping of integers  0-28 (inclusive).  This means we have 29 classes or categories.  Our arrays of encoded tabs are then converted into binary 2D arrays so that they can finally be used for training.  An example of what the first few characters from Figure 4 look like encoded can be seen below in Figure 5. ​

<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/tabFormattedExample.PNG" alt="Tab Representation" />
</p>

<h3 align="center">Converts To</h3>

<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/onehotencodedBinaryExample.PNG" alt="One Hot Encoded Binary" />
</p>
​

<h3>Proposed Architectures</h3>

As previously stated, this project is a work in progress. Mainly, a model architecture needs to be decided on, thoroughly tested, and trained. After some basic testing, it is clear that the model architecture needs to understand sequences and their relationships and needs to use a loss function that accounts for dominant classes because of the majority of characters being "-" (the dash character representing empty space and acting as a separator). The model needs to treat the problem as a long sequence of classifications. Additionally, the model (or the preprocessing) needs to include class merging because we have combinations of the classes pertaining to guitar string letters (e, A, D, G, B, E = 0, 1, 2, 3, 4, 5) and the fret numbers (0-19 = 8-28). For example, fret number 8 playing on string D is NOT the same as fret number 8 playing on string B, so we must combine the fret number classes and string letter classes to correlate these relationships. A visual of this problem can be found below in Figures  6  and 7 where Figure 6 represents the case of fret 8 being played on string D and Figure 7 represents fret 8 being played on string B. As you can see, there is a difference in the tabs for these two cases, but without class merging, these cases may be treated as identical. ​

​<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/classMergingEx1.PNG" alt="Tab With 8th Fret on D" />
</p>

<h3 align="center">Does NOT Equal</h3>

<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/classMergingEx2.PNG" alt="Tab With 8th Fret on B" />
</p>

Another important detail is that our architecture must match our input/output data. Our input data has a dimesionality of (360, 512, 4000) and our output data of  (360, 6934). This means our input layer must have the same shape of (360, 512, 4000) and the same for the output layer of (360, 6934). For context, the 360 comes from our number of samples or recordings. The 512 from the input data comes from the number of bins in our mel-spectrograms,  the 4000 comes from our spectrograms' padded lengths, and the 6934 comes from our tabs' padded lengths. ​

​
**Proposal 1: Transformer Model​**
* A transformer model poses the benefit of processing the entire sequence at once, meaning it can form relationships between parts of the sequence without regard to length (global context).  ​
* Transformers are excellent at sort of "translating" a piece of data from one form to another, which is essentially what this project calls for​
* Does not strictly process from left to right (processes all simultaneously) meaning transformers can make decisions about what happens earlier in a matrix based on what happens later or vice versa​

**Proposal 2: CNN/LSTM Hybrid​**
* A CNN/LSTM hybrid model would use convolutional layers to effectively process the spectrograms and LSTM layers to build sequential relationships when trying to create the tabs.​
* Could potentially lead to the model only sequentially looking at the tabs and not the spectrograms too​
* The CNN/LSTM hybrid would likely be less complex and require less computational resources than the transformer approach​

​
Regardless of model architecture, the loss function needs to severely punish false positives for dashes to prevent the model from predicting everything as dashes because this would technically lead to a highly accurate result in terms of correct characters (but not a real solution). The loss function also must account for our data being categorical, so a loss function like Keras' CategoricalFocalCrossentropy could work. ​

​
<h3>Conclusion</h3>

The most challenging part of this project so far has been the collection and preprocessing of data into a usable form. The next step is designing and training on either a Transformer or CNN/LSTM hybrid architecture. However, due to the complexity of this problem, a dataset of 360 samples (about 30 seconds of audio each) is likely not enough to achieve highly accurate results. On the other hand, obtaining a larger dataset and training on it might also be unrealistic. A more grounded approach might be to find a pre-trained model that serves a similar purpose and use transfer learning to have a better training starting point. Additionally, there is likely software that can produce audio given either tabs or JAMS files. ​

This could be used to make a much larger dataset, especially if random shifting and artifacting are added in to give more variety to the artificial samples. ​

<h3>Source for Dataset and Sponsor</h3>

This project effort was sponsored by the Department of the Navy, Office of Naval Research under ONR award number N00014-21-1-2710. This project would not have been possible without their support.​

GuitarSet Dataset Author:​                              
Q. Xi, R. Bittner, J. Pauwels, X. Ye, and J. P. Bello, ["Guitarset: A Dataset for Guitar Transcription"](http://tomxi.weebly.com/uploads/1/2/1/6/121620128/xi_ismir_2018.pdf), in 19th International Society for Music Information Retrieval Conference, Paris, France, Sept. 2018.

​


​

​

​

​

​
​

