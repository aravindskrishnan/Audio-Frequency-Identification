# Audio Frequency Identification
This project aims to implement the functionality of a tuning fork into a software based domain.
This is done using the fast fourier transform algorithm and separating out the components of
frequency corresponding to the notes from the music file which the user points to and comparing it
to a lookup table which contains the notes along with their frequency range.
This information can then be used by the user to tune musical instruments as well as identify stray
noise frequency in a particular recording. The main scope of this project extends beyond the usual
tuning for alternatives to be used in specific audio recording and post processing cases where a
specific humming frequency ( like that of an engine or motor ) of a noise that has been captured by
the recording microphone during the process.



## Environment

The project is currently implemented in MATLAB, ports into other supported languages like python will be made available soon. Feel free to fork or clone this repository and create deviations of the same.  



## Scope 
This project finds use mainly in the recording and acousting tuning industry. By using a software
based implementation of tuning musical instruments rather than a tuning fork has several
advantages like the following

- Over time the resonant frequency of the tuning fork changes, causing the note of the
musical instrument to change with tuning as well. With a software based implementation
like this project the accuracy of the readings stay consistent throughout the
implementation.
- While tuning forks are available in resonance with standard note frequencies, this requires
the need for several tuning forks which are expensive and bulky. But with a unified
software based interface this reduces the need of multiple tuning forks and even
accommodates non standard note frequencies for unique application.
- While the concern of cost is a vital factor in tuning forks, which of good quality and
accuracy can cost a small fortune.This specific implementation using matlab is virtually
free given the online community behind such a platform.
- While tuning forks are able to resonate to a particular frequency, in most of the cases the
notes produced by musical instruments have more than one frequency component in them,
making tuning time consuming and expensive. Since this project uses the fourier transform
as its engine, it natively separates out all frequency components making tuning easy and
fast.
- The scope can also be extended to identifying and removing humming noise in recording
and post production cases. This can be done by identifying the frequency and suppressing it
and then taking the inverse fourier transform to get back the original result.

## Project Background
A fast Fourier transform ( FFT ) is an algorithm that computes the discrete Fourier transform
(DFT) of a sequence, or its inverse (IDFT). Fourier analysis converts a signal from its original
domain (often time or space) to a representation in the frequency domain and vice versa. The DFT
is obtained by decomposing a sequence of values into components of different frequencies. This
operation is useful in many fields, but computing it directly from the definition is often too slow to
be practical. An FFT rapidly computes such transformations by factorizing the DFT matrix into a
product of sparse (mostly zero) factors. As a result, it manages to reduce the complexity of
computing the DFT from O ( N2 ), which arises if one simply applies the definition of DFT, to O (
N log ⁡ N ) , where N is the data size. The difference in speed can be enormous, especially for long
data sets where N may be in the thousands or millions. In the presence of round-off error, many
FFT algorithms are much more accurate than evaluating the DFT definition directly or indirectly.
There are many different FFT algorithms based on a wide range of published theories, from simple complex-number arithmetic to group theory and number theory

Let Xo , …, Xn-1 be complex numbers. The DFT is defined by the formula:
![alt text](https://miro.medium.com/max/812/1*Hm88khvsCJE7e0kkUJ97wA.png)


## Solution Approach
- 1. The user initially inputs the required audio file to the code. Several other parameters and
constants are also initialised in this step. Like the sampling frequency of the audio file.
- 2. A lookup table containing the frequency and the names of notes are also initialised to act as
the reference in future steps.
- 3. The fast fourier transform of the input audio file is then calculated and then plotted on the
frequency vs amplitude scale.
- 4. The array containing the amplitude is then sorted using the unique function. This will help
in identifying the frequencies corresponding to the dominant amplitudes
- 5. The dominant frequency corresponding to the dominant amplitude is then found out and
this value is stored in a variable
- 6. The dominant amplitude along with the dominant frequency is then displayed to the user
- 7. The dominant frequency variable is then compared with the entries in the lookup table to
identify the name of the Note.
- 8. The name of the note is then displayed to the user.
- 9. The steps from 4 to 8 are then repeated for the 5 ( which can be easily expanded ) largest dominant amplitudes.

## Code Explanation
Importing the audio file and assigning the parameters
```Matlab
[audio_in,sample_frequency]=audioread('mixed.wav');
mm=[];
frequency_list=[];
sample_time=1/sample_frequency;
t=(1:6000)*sample_time;
N=length(t);
```
Assigning Note Lookup Table (optional)
```Matlab
note_names=['C0','G1','G2','D3','G3','B3','D4','F4','G4','A4','B4','B5','B8'];
note_frequency=[50 103 150 200 250 300 350 400 441 500 1000 1500 10003];
```
Fourier transform algorithm implementation
```Matlab
for(k=1:N)
    for(n=1:N)
        y1(n)=audio_in(n).*exp(-1i*2*pi*((-N/2)+n-1)*((-N/2)+k-1)/N);
    end
    mm=[mm sum(y1)];
end
```
Plotting the fourier spectrum of audio file
```Matlab
f=sample_frequency.*(-N/2:N/2-1)/N;
figure
grid on
y=2*abs(mm)/N;
plot(f,y,'r','lineWidth',2);
title('Musical Spectum')
xlabel('Frequency')
ylabel('Amplitude')
```
Find the dominant frequnecy and maximum amplitude of 1st tone
```Matlab
amplitude_array=2*abs(mm)/N;
maximum_amplitude=max(amplitude_array);
txt0=['1st Maximum Amplitude is : ',num2str(maximum_amplitude)];
disp(txt0)
dominant_frequency=round(f(y==max(amplitude_array)));
txt1=['1st Dominant Frequency is : ',num2str(dominant_frequency(1,2)),' Hz'];
disp(txt1)
frequency_list=[frequency_list dominant_frequency(1,2)];
```
Identification of Note 1
```Matlab
pos=find(note_frequency==dominant_frequency(1,2));
numm=note_names(pos*2);
alphabett=note_names(pos*2-1);
name=['Note Name = ',alphabett,numm];
disp(name)
```
#### Repeat the last two code blocks for the number of frequencies to be identified


## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.


## License
The [MIT](https://choosealicense.com/licenses/mit/) License

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sub license, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NON INFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
