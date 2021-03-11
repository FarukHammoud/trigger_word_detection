## Trigger Word Detection

The main objective of this tutorial of the coding weeks is to build a trigger word detector. Trigger word detection is the technology that allows devices like Amazon Alexa, Google Home, Apple Siri to wake up upon hearing a certain word. Our version will be quite limited (a weaker version of Alexa) and a single trigger word (`activate`) with everything else is ignored. So every time we detect the corresponding sound to the word "activate," we need to detect it, and output a "bell" sound.

To build such a system, we'll use a Deep Neural Network (A recursive one to be exact) to detect the trigger words within a [spectrogram](https://en.wikipedia.org/wiki/Spectrogram) (the FFT of a given audio clip).

Given that audio clips are of temporal nature, we will use Recurrent Neural Networks, where the new state at time `t` can be dependent on all the previous steps `[0, t-1]` (given enough computational resources), for a brief introduction to these types of models, please refer to [A short intro to RNNs](../notes/intro_to_rnn.pdf).

This lab will be divided into three main parts: 
- Synthesizing and processing the raw audio recordings to create train/test datasets.
- Defining our model based on the provided architecture.
- Training and testing our model.

*Next tutorial: Use it in production*

## Packages

All the packages we might need are already installed, the only packages missing are `pydub`, that we will use to manipulate our audio recordings and create a train and test (in this case we use test set and validation set interchangeably) sets, `scipy` to load the `wav` audio-files and `jupyter-notebook`. Please install them in the virtualenv:

```bash
pip install pydub
pip install scipy
pip install jupyter
```

### Jupyter Notebooks

In data sience (unlike software engineering), prototyping is very important, and jupyter notebook helps a lot. So for this tutorial, we'll work within a jupyter notebook. if you've never used a [jupyter notebook](https://jupyter.org/). Please take a few minutes and see a [quick intro to notebooks](https://www.youtube.com/watch?v=jZ952vChhuI), and go through the [basics](https://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/Notebook%20Basics.html).

Here are some tricks that may help you during (and after) the tutorial:

#### Jupyter tricks

let's see how is jupyter good for prototyping. For example, given a function `display`:
1. type `display` in a cell and press shift+enter (to execute a cell), it will tell us from where was the function was imported, in this case `<function IPython.core.display.display>`
2. type `?display` in a cell and press shift+enter, it will show us the documentation
3. type `??display` in a cell and press shift+enter, it will show us the source code. This is particularly useful for some Deep Learning libraries because most of the functions are easy to read and comprehend.

Here are some nice ones too:

- `shift + tab` in Jupyter Notebook will bring up the inspection of the parameters of a function, if we hit `shift + tab` twice it'll tell us even more about the function parameters (part of the docs)
- If we put `%time`, it will tell us how much times it took to execute the line in question.
- If we run a line of code and it takes quite a long time, we can put `%prun` in front. `%prun m.fit(x, y)`. This will run a profiler and tells us which lines of code took the most time, and maybe try to change our preprocessing to help speed things up.

### Start the tutorial

Now you can start the lab by heading to the notebook [trigger_word_detection.ipynb](trigger_word_detection.ipynb). To do so simply run `jupyter-notebook` in the command line, access the provided link in your browser and open `trigger_word_detection.ipynb`.