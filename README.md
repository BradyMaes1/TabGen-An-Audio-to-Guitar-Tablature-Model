# TabGen-An-Audio-to-Guitar-Tablature-Model

Researcher: Maes, Brady
Mentor: Uddin, Md Yusuf Sarwar,
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

