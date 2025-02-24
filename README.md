# audiosignals
Hello, my dear friends. This is an example of implementing an algorithm for working with audio signals and classification in PyTorch.

What sequential steps was the task divided into:

1. Generating a synthetic audio signal. Given: the sum of three sinusoids with random frequencies from 100 Hz to 4000 Hz, duration 1 second, sampling frequency 16 kHz. Then add Gaussian noise with a variance of 0.01. To generate sinusoids, you need to create a time array t for each frequency, then multiply by 2πf and take the sine. Then add all three waves and add noise via numpy.random.normal.

2. Bandpass filter. We will use scipy.signal.butter to create a Butterworth filter. It is necessary to correctly set the filter order and cutoff frequencies. Then apply filtfilt for zero delay. It is necessary to check whether the filter passes the desired range correctly.

3. Feature extraction: spectral centroid and spectral width. To do this, calculate the FFT of the signal, get the frequency amplitudes, then the centroid as a weighted average of the frequencies, and the width as the square root of the standard deviation around the centroid. It is important to use numpy for these calculations, without librosa.

4. Creating a fully connected neural network on PyTorch with two features: centroid and width. This means the input layer is 2 in size. Hidden layers can be made, for example, with 64 and 32 neurons, the output layer with one neuron and a sigmoid for binary classification. You need to define the forward method and initialize the layers.

5. Dataset generation. You need to create a set of examples where in half of the cases the signal contains frequencies from a given range [f_low, f_high], and in the other half it does not. To do this, when generating a signal, you can choose whether to add a frequency in the target range. Or, perhaps, check after generation if the signal contains frequencies from the range. But how can we determine their presence? Maybe, if at least one of the three frequencies falls within the range, then the label is 1, otherwise 0. Then, when generating data, we need to randomly select frequencies and labels accordingly.

6. Training the model. We need to prepare the DataLoader, split the data into training and test sets. Use binary cross-entropy as a loss function and the Adam optimizer. It is important to monitor overfitting, perhaps add a validation set.

7. The process_audio_signal function should return the filtered signal and features. We need to make sure that all steps (generation, filtering, extraction) are correctly linked.

8. In the quality report, we need to display accuracy, precision, recall, and possibly an error matrix. Draw conclusions about how well the model determines the presence of a frequency range, and which features are the most informative.

Potential problems: are the features extracted correctly, are two features enough for classification, is the data too noisy. You may need to experiment with the network parameters or add more layers.
