### Pitch detection algorithms

Autocorrelation-based C++ pitch detection algorithms with **O(nlogn)** running time:

* McLeod pitch method - [paper](http://miracle.otago.ac.nz/tartini/papers/A_Smarter_Way_to_Find_Pitch.pdf) - [visualization](./misc/mcleod)
* YIN - [paper](http://audition.ens.fr/adc/pdf/2002_JASA_YIN.pdf) - [visualization](./misc/yin)

### Build and install

Using this project should be as easy as `make && sudo make install` on Linux with a modern GCC - I don't officially support other platforms.

This project depends on [ffts](https://github.com/anthonix/ffts). To run the tests, you need [googletest](https://github.com/google/googletest), and run `make -C test/ && ./test/test`. To run the bench, you need [google benchmark](https://github.com/google/benchmark), and run `make -C test/ bench && ./test/bench`.

### Usage

The code is lightly documented in the [public header file](./include/pitch_detection.h). Compile your code with `-lpitch_detection`.

The namespaces are `pitch` and `pitch_alloc`.

The `pitch` namespace functions are for automatic buffer allocation:

```c++
#include <pitch_detection.h>

//std::vector<double> audio_buffer with sample rate e.g. 48000

double pitch_yin = pitch::yin(audio_buffer, 48000);
double pitch_mpm = pitch::mpm(audio_buffer, 48000);
```

#### Manual memory allocation

If you want to detect pitch for multiple audio buffers of a uniform size, you can do more manual memory control with the `pitch_alloc` namespace:

```c++
#include <pitch_detection.h>

//buffers have fixed length e.g. 48000, same as sample rate

pitch_alloc::Mpm ma(48000);
pitch_alloc::Yin ya(48000);

for (int i = 0; i < 10000; ++i) {
        //std::vector<double> audio_buffer size 48000 sample rate 48000

        auto pitch_yin = pitch_alloc::yin(audio_buffer, 48000, &ya);
        auto pitch_mpm = pitch_alloc::mpm(audio_buffer, 48000, &ma);
}
```

#### Advantages of manual memory allocation

The auto allocation strategy performs hundreds of thousands of allocations:

![auto-alloc](./misc/membench/auto-alloc.png)

Manual allocation, as expected, performs less allocations by several orders of magnitude:

![manual-alloc](./misc/membench/manual-alloc.png)
