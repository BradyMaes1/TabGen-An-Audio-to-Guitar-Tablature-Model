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
      ***Mel-Spectrograms** are essentially spectrograms that consider human perception of sound​

**Encoding**- Converting data from one form or format into another. In this case, encoding serves the purpose of making text into a format that machine learning models can understand (integers that are then converted into binary).​

**Convolutional Neural Network (CNN**)- A  type of deep learning model specifically designed for processing structured grid data, such as images (very simplified).  ​

**Long Short-Term Memory (LSTM)**-A type of recurrent neural network (RNN)  designed to effectively learn and remember over long sequences of data. They're especially good for problems that involve sequential data with long-term dependencies. ​

**Transformer Model**- A model that relies entirely on self-attention to handle dependencies between input and output elements, eliminating the need for recurrent or convolutional layers, which were commonly used in previous architectures (like LSTM and CNN)

​
<h3>Preprocessing</h3>

The audio, annotation file pairs must be preprocessed to be useful and meaningful to our model. Machine learning models expect input and output data in the form of integer (sometimes float) arrays. This step is crucial because how we preprocess our files will determine how much information is lost versus how much is kept & fed into our model. Additionally, the format and representation of our data created by our preprocessing will influence our model's learning as well as how resource-intensive the training will be. For the sake of simplicity, most of the examples given in this presentation will use the first file named "05_SS3-98-C_solo." ​

The audio files from this dataset come in WAV format, which is ideal because of its lack of compression and therefore lack of data loss. However, the WAV files must be preprocessed into a more model-friendly format. Spectrograms are a popular visual representation of the spectrum of frequencies in a signal as it varies with time. They can be thought of as visual, graphical representations of sound where the x-axis represents time (from left to right) and the y-axis represents the frequency of a sound or sounds. Finally, there is a third parameter- the intensity or magnitude of each frequency in decibels. This intensity is represented by the color of the lines, similar to a heat map. Mel-spectrograms are an extension of spectrograms that have been filtered to more accurately represent human interpretation of sound. Spectrograms are highly compatible with models that excel at image processing & choosing Mel-spectrograms accounts for human perception. An example Mel-spectrogram from the preprocessing can be seen below in Figures 1 and 2. Note that the spectrogram contains empty yellow space on the right side because all spectrograms were padded to a length of 4000 (the largest audio file rounded up)  to standardize the input. The spectrograms are divided into 512 bins each. ​

​<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/audioFileExample.PNG" alt="Audio File Location" />
</p>

<h3 align="center">Converted To</h3>

<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/spectrogramExample.PNG" alt="Mel Spectrogram" />
</p>


The annotation files from this dataset come in JSON Annotated Music Specification (JAMS) format. These JAMS files contain information about their corresponding audio file and how it is played, such as Musical Instrument Digital Interface (MIDI) data which can be used to determine frequencies and notes at specific times.  From here, we can calculate what fret of what string was being played at what time based on MIDI note numbers.  A dictionary is then used to store guitar string letters (represented as numbers) as keys and their value contents as an array containing fret numbers. From here,  text files are made showing the fret numbers on their corresponding guitar strings with the strings indicated by their letter names (e, A, D,  G, B, E) followed by a "|" bar symbol for organizational reasons.  Next, dashes "-" are inserted into the array to show the passing of time (10 dashes/second) and inserted between touching fret numbers to act as a separator. It is also worth noting that a check is added to treat double-digit fret numbers as double digits by checking the length of the current line and if this character plus the next one forms a digit. If both criteria are true, then they're added together as one fret number and the next iteration of the loop is skipped.  Finally, padding of dashes is added to the end to make all files the same length (the length of the longest file). An example of some contents from an annotation file and its corresponding tab can be seen below (both partial). ​

Note: The data on the left is not an exact translation of the data on the right as their different file formats do not place their relevant information near one another in a way that could fit in a graphic like this. ​

​<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/JAMSpitchExample.PNG" alt="JAMS Annotation File" />
</p>

<h3 align="center">Converted To</h3>

<p align="center">
  <img src="https://github.com/BradyMaes1/TabGen-An-Audio-to-Guitar-Tablature-Model/blob/main/tabFormattedExample.PNG" alt="Tab Representation" />
</p>

The representation shown in Figure 4 is the closest to actual guitar tabs and is our desired final product when making predictions. However, this format is not compatible with models and needs to be encoded so that it is readable for our model.  The text files containing our tabs are one-hot encoded based on a mapping of integers  0-28 (inclusive).  This means we have 29 classes or categories.  Our arrays of encoded tabs are then converted into binary 2D arrays so that they can finally be used for training.  An example of what the first few characters from Figure 4 look like encoded can be seen below in Figure 5. ​

​

​

